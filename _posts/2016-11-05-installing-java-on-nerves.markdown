---
layout: post
title:  "Installing Java on Nerves"
author: Robert Brown
date:   2016-11-05 12:30:00 -0600
categories: elixir nerves java
---
At work we have hardware product that's mostly written in C running on a Raspberry Pi 3. I have been pushing to adopt [Nerves](http://nerves-project.org), an embedded platform that uses [Elixir](http://elixir-lang.org).

To prove Nerves's viability, I'm building a prototype. One of the requirements is to interact with a spectrometer that uses a Java library. Nerves doesn't come with Java out of the box. However, Nerves is built on [Buildroot](https://buildroot.org) which has a package for [JamVM](http://jamvm.sourceforge.net).

JamVM is an alternative JVM for embedded systems. To include it, you have to create a custom system image for your platform (ex. Raspberry Pi 3). I won't go into the details about building custom systems. You can read [Wendy Smoak's post about it](http://wsmoak.net/2016/10/17/building-using-custom-nerves-system.html).

JamVM runs Java 5. If that fits your requirements, then you can use the custom image. If you don't want to use a custom image or need a newer version of Java, keep reading.

I tried [OpenJDK](http://openjdk.java.net) first. That turned out to be a convoluted mess. Frankly, I still don't understand how [IcedTea](https://en.wikipedia.org/wiki/IcedTea), [HotSpot](http://openjdk.java.net/groups/hotspot/), [Classpath](https://en.wikipedia.org/wiki/GNU_Classpath), [Zero](http://openjdk.java.net/projects/zero/), [Jalimo](https://evolvis.org/plugins/mediawiki/wiki/jalimo/index.php/Main_Page), [OpenEmbedded](http://www.openembedded.org/wiki/OpenEmbedded), [CACAO](http://www.cacaojvm.org), and other projects relate to each other. Some projects appear abandoned. The others had poor documentation.

Then I switched to the official [Oracle Java](https://www.oracle.com/java/index.html). (Note, the [OTN license](http://www.oracle.com/technetwork/licenses/standard-license-152015.html) may not be compatible for your project.) The hard part here was figuring out which build to use. I tried a few and got [Java embedded to work](http://www.oracle.com/technetwork/java/embedded/embedded-se/downloads/javase-embedded-downloads-2209751.html).

The Java embedded download is different from the others. Rather than give out a pre-built binaries, it instead has a `jrecreate.sh` script. This lets you make a JRE that fits your needs. I made a full JRE like this:

```bash
./bin/jrecreate.sh --dest ~/Nerves/ejdk
```

This produced the necessary binaries for me to add to Nerves. They weigh in at 55 MB. Alternatively, I could have made a minimal build like this:

```bash
./bin/jrecreate.sh --profile compact1 --dest ~/Nerves/ejdk
```

That build is just 11 MB. If I need I'll switch to the smaller JRE later.

With the JRE built, it's pretty straightforward to add it to Nerves. Buildroot has a directory called `rootfs-additions`. This directory includes files you want copied into your root file system. Its contents will look familiar. You can include `bin`, `usr`, `lib`, `etc`, and other subdirectories you would expect from a standard Unix-based system.

First, create a local `rootfs-additions`. I did it like this:

```bash
mkdir config/rootfs-additions
cp ~/Nerves/ejdk/* config/rootfs-additions
```

My resulting directory looks like this:

```
firmware
└── config
    ├── config.exs
    └── rootfs-additions
        ├── bin
        |   ├── java
        |   ├── jjs
        |   ├── keytool
        |   └── ...
        └── lib
            ├── applet
            ├── arm
            ├── cmm
            └── ...
```

Next, we need to tell Nerves about our local `rootfs-additions`. In `config.exs` add these lines:

```elixir
config :nerves, :firmware,
  rootfs_additions: "config/rootfs-additions"
```

That's all there is to it. Nerves will now grab these files and include them in your firmware image.

Note, your project's `rootfs-additions` will overwrite any files of the same name that Nerves includes in its `rootfs-additions`. This is useful, for example, to overwrite the config file at `/etc/erlinit.config`.

Nerves has been a great platform to work with. It's a refreshing advance after having used C-based firmware for years. Nerves makes it easy to build firmware at a high level. Extending its capabilities is  simple too. I look forward to seeing how Nerves continues to evolve.
