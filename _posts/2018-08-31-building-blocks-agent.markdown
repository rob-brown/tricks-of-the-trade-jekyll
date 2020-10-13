---
layout: post
title:  "Building Blocks: Agent"
author: Robert Brown
date:   2018-08-31 21:24:00 -0600
categories:
---
I've long been an advocate of simple building blocks that are trivial to understand and implement. Despite their small size, they enable simpler development. Many that I have written are published on my [GitHub account](https://github.com/rob-brown). Today I want to share my most used building block: `Agent`.

`Agent` comes from [Elixir's module of the same name](https://hexdocs.pm/elixir/Agent.html). It is a thread-safe encapsulation around arbitrary state. Since I use `Agent` all the time in Elixir, I found myself wanting for it in other languages.

So, let's implement an agent in Swift. Here is the basic type definition and constructor.

```
import Foundation

public final class SimpleAgent<State> {
    private var state: State
    private let queue: DispatchQueue

    public init(state: State) {
        self.state = state
        self.queue = DispatchQueue(label: "pro.tricksofthetrade.SimpleAgent", qos: .userInitiated)
    }
}
```

We can see that `SimpleAgent` holds some generic state. It also has a `DispatchQueue`. This is where the thread safety comes in. I'm going to assume you are familiar enough with Grand Central Dispatch (GCD).

Before continuing, you should note that it's best that `State` be immutable. I prefer to do this with a `struct` that only has `let` properties that are also immutable. Having immutable data makes it impossible to mutate the data in an unsafe way.

An agent effectively has two operations: get and set. Let's start with getting data.

Fetching the data does the following:

1. Synchronously run a closure on the dispatch queue.
2. Call the given closure with the current state.
3. The closure returns the subset of data it wants.
4. Return the result to the caller.

And here is the code.

```
public func fetch<Result>(closure: ((State) -> Result)) -> Result {
    var result: Result!
    queue.sync {
        result = closure(self.state)
    }
    return result
}
```

Note that an implicitly unwrapped optional must be used here because the Swift compiler is not yet smart enough to know that `result` is always written to before it is read.

Now for updating the state. It works similarly to the `fetch` function.

1. Asynchronously run a closure on the dispatch queue.
2. Call the given closure with the current state.
3. The closure returns the new data to store.
4. Update the internal state.

And here is the code.

```
public func update(closure: @escaping (State) -> State) {
    queue.async {
        self.state = closure(self.state)
    }
}
```

The code could just as easily use `queue.sync`. There's no need to wait around for the update to complete, so `queue.async` is typically the desired behavior.

There is one more operation we need. There are times we want to both get and update the data. It is not safe for the caller to fetch the data, change it, then set it. That can lead to race conditions. So, let's build an operation that does both at the same time.

```
public func fetchAndUpdate<Result>(closure: (State) -> (Result, State)) -> Result {
    var result: Result!
    queue.sync {
        let (returnValue, newState) = closure(self.state)
        self.state = newState
        result = returnValue
    }
    return result
}
```

That's it. In total, `SimpleAgent` is 36 lines of code, including blank lines. Any Swift developer should be able to look at this code and understand it within a few minutes. Plus you now have a powerful building block for building bigger building blocks such as actors, state machines, and futures. Or it can be used as is.
