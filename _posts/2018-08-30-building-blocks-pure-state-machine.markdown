---
layout: post
title:  "Building Blocks: Pure State Machine"
date:   2018-08-30 22:12:00 -0600
categories:
---
Previously I wrote about my [most common building block: `Agent`](#posts/building-blocks-agent). Today I'm going to use `Agent` to make a bigger building block, namely a pure [state machine](https://en.wikipedia.org/wiki/Finite-state_machine). It's implementation is inspired by [Elm](http://elm-lang.org) and [Andy Matuschak](https://gist.github.com/andymatuschak/d5f0a8730ad601bcccae97e8398e25b2).

Before continuing, note that "pure" means the state machine is mathematically pure. Or in other words given the same outputs it will *always* produce the same outputs. This makes testing the state machine trivial.

Let's start with the basic type definition and constructor.

```
import Foundation

public final class SimplePureStateMachine<State, Action, Command> {
public typealias ActionHandler = (State, Action) -> (State, Command)

private let state: SimpleAgent<State>
private let handler: ActionHandler

public init(initialState: State, handler: @escaping ActionHandler) {
    self.state = SimpleAgent(state: initialState)
    self.handler = handler
}
}
```

The state is stored in the [agent created previously](#posts/building-blocks-agent). This ensures updates are thread safe.

The most notable part of the type definition is `ActionHandler`. It's simply a function. As input, it receives the current state and an action (sometimes called an event or message). The action represents an operation to be performed on the state. I typically implement actions as a Swift `enum`. Here's an example:

```
public enum BluetoothAction {
case bluetoothPoweredOn
case bluetoothPoweredOff
case startScan
case stopScan
case scanTimedOut
}
```

As output, the `ActionHandler` returns new state and a command (sometimes called an effect). The command represents any side effects the state machine would like performed on its behalf. Since the state machine is pure, it can't do things like performing network requests or access a database. Some external entity will receive the commands, perform them, and report back to the state machine with an action.

For those interested, this type of state machine is known as a [Mealy machine](https://en.wikipedia.org/wiki/Finite-state_machine#Transducers).

Now that you are thoroughly lost in the theory, let's actually implement the rest of the state machine. It turns out there is only one function left to implement: `handleAction`.

```
public func handleAction(_ action: Action) -> Command {
return state.fetchAndUpdate {
    let (newState, command) = self.handler($0, action)
    return (command, newState)
}
}
```

That's it. All the real work is done by `SimpleAgent` and the `ActionHandler`. This leaves `SimplePureStateMachine` weighing in at a whopping 24 lines, including blank lines.

Before I conclude, I'll demonstrate `SimplePureStateMachine`. That will help clear up the mystery behind `ActionHandler`. For this example, I will make a simple coin-operated turnstile.

```
enum TurnstileState {
case locked
case unlocked
}

enum TurnstileAction {
case insertToken
case admitPerson
}

enum TurnstileCommand {
case lock
case unlock
case ejectToken
case notifySecurity
}

let turnstile = SimplePureStateMachine<TurnstileState, TurnstileAction, TurnstileCommand>(initialState: .locked) { state, action in
switch (state, action) {
case (.locked, .insertToken):
    return (.unlocked, .unlock)

case (.locked, .admitPerson):
    return (.locked, .notifySecurity)

case (.unlocked, .admitPerson):
    return (.locked, .lock)

case (.unlocked, .insertToken):
    return (.unlocked, .ejectToken)
}
}

turnstile.handleAction(.insertToken)   // Command: unlock
turnstile.handleAction(.admitPerson)   // Command: lock
turnstile.handleAction(.admitPerson)   // Command: notifySecurity
turnstile.handleAction(.insertToken)   // Command: unlock
turnstile.handleAction(.insertToken)   // Command: ejectToken
```

Again, the great win with a pure state machine is the logic can be tested without any side effects such as inadvertedly alerting security. Also, the state machine doesn't care how the side effects are implemented. This leaves us free to change that detail however and whenever we want without chaning the state machine. Now that's modularity.
