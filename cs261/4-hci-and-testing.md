# 4 - Human-Computer Interaction and Testing

Unfortunately, while having a great backend makes for a good app, so does having a good frontend, and if your frontend is garbage, users will think that your whole app is garbage. Why does that sound like your average fireship quote?

## 50 Insecure Dependencies Found

We depend on software a lot. We rely on it for our finances, our social life, academia, and so fourth. We all know how frustrating it is when you can't submit an assignment because the portal for it is down, or your banking app is so poorly designed that it takes way too long to pay your month's rent. We may **reject** such systems for better ones. More terrifyingly, however, badly designed software can cost lives, as seen in the Therac-25 and the six people it killed. There are many such cases where bad software **affects people directly** and **incurs massive costs**. Furthermore, all it takes is a poorly designed database to **lose data**, which may be even more important than the software itself.

We also need to keep perspective in mind, as although we, the developers, have a good understanding of how reliable a system is, *this perspective is fundamentally different to that of the user*, who has their own **perceived reliability**. Your system may be very reliable but have an unresponsive user interface, making it look unreliable.

## *Lean on me, when you're not strong*

*Actually maybe don't, I don't consider myself very strong either ;-;*

We can measure how reliable our software is, using the following questions:

- If someone requests a service, how likely is it for the system to fail the request?
  - Example: If someone want to join a meeting on a conference platform, how often will it fail to connect when the room exists?
- How often do failures occur?
- How long can the system run without encountering a failure?
- How often is the system online, i.e. how often is it *available*?

Likewise, talking about dependability could refer to a few different things. namely:

- **Availability** - Often called "Uptime".
- **Reliability** - Does the system provide the service it advertises, and does it do it well?
- **Safety** - How likely is this to cause real-world damage or kill someone?
  - *Don't confuse this with security!*
- **Confidentiality** - Can this system hold sensitive data without fear of it falling into the wrong hands?
- **Integrity** - Can we be certain that only people with the right security clearance can modify important data?
- **Maintainability** - Is the system quick and easy to fix?
  - Think about servers and the ability to hot-swap components, for example.
- **Error Tolerance** - How easily can the system recover from user errors?

But keep in mind that this isn't a complete list, and each of these points may interlink or be broken down! 

## Epic Fail!!

We may start with a **fault**, which spurs an **error**, which cannot be handled by the system, resulting in a **failure**, all of which we may represent using a **Fault-Error-Failure cycle**. Such a failure may be caused by **hardware** failing (e.g. a CPU dying), an issue in the **software** we've written, or the failure may be **operational**, where the user has done something wrong, which may be referred to as an IBM error (Idiot Behind Machine), or in Brazilian Portuguese, a BIOS Error (Bicho Ignorante Operando o Sistema.). We IT people can't help but show so much contempt towards normies. And we wonder why they hate us.

### How to stop failing

Well, reading these notes is a good start.

But in the context of system development, we have three main options:

- **Fault Avoidance** - Develop the system with consideration so that we avoid situations where a fault could occur.
  - Example: Using IDE tools to produce reliable and correct code
- **Fault detection and correction** - Basically debugging.
- **Fault tolerance** - Develop the system with some safety harnesses which manage faults and errors, allowing the system to continue working
  - Think of try-catch statements and redundant systems.

As you can imagine, it's not possible to remove every single flaw from a system before you launch it. It ends up becoming too expensive, and issues will get more and more esoteric as you go. As such then, *it's often cheaper to release the software with some very minor issues and pay for the consequences*. I think game studios have gone a *liiitle* bit overboard with this idea though.

### Keep calm and carry on

Let's look a little more into how to recover from errors. In the worst case, an error will cause some aspect of the system to go down. We should, then, account for this and allow for these components to go down *gracefully*, i.e. allow **graceful degradation**, so the system can continue working as a whole. One way of doing this is creating **redundancy**, which allows a copy of the same component to carry on where the old on left off, and we may want these to be **diverse** so that they won't fail in the same way as the original one.

Remember that this needs to apply to the whole system: not just software or hardware! A software component failing could be just as bad as a server blowing up (literally or metaphorically, depending on where your data centre is located). You should also consider the significance of consequences: does failure mean someone won't be able to enjoy a particular show, or that they'll be blasted with ten times the lethal dose of radiation (oops!)? This is why the above is useful, it allows us to **contain software failures**. Of course, the issue is it's on *you*, the developer, to fix such failures.

### Trust the Process

You need your development process to be **dependable**, so that your client can be sure they can rely on your solution, and that it has properly been reviewed so that end-users can enjoy yet another rom-com or not suffer intense trauma during a cancer treatment (seriously, some of the Therac-25 stories sound like they came straight out of a film). But what defines a dependable process?

- **Documentable** - It is clear what the steps are in the process, and how to write documentation for each step.
- **Standardised** - Is it possible to apply this process to a wide range of situations? Is it possible to stick closely to the steps involved?
- **Auditable** - Can someone *not* working on the development understand what is going on?
- **Diverse** - Use multiple, different verification and review techniques to catch problems.
- **Robust** - Can we easily recover from problems in the process?

*Fortunately*, the techniques we covered prior should be able to prevent any problems with dependability, if done correctly. Also remember that hardware is just as important, so we ensure this is dependable too, through hot-swappable server, redundant hardware and so on.

## Ol' Reliable

There are three main categories that **dependable systems** can be in.

### Protection Systems

This applies to a system which monitors operation, using sensors and a control system to manage the main system/environment as it operates. With protection systems, **we use an additional set of protection sensors and a protection system, which can leave the system in a safe state before shutting it down**, should a problem occur. This isn't unique to software engineering: protection systems are also very important in electrical engineering for example: think about how we deal with power surges.

### Self-monitoring

Here, we focus on the *consistency of data*. A self-monitoring system (*surprise surprise*) monitors its own operation, **computing a set of inputs over multiple channels to ensure the same result is being achieved**. If these give differing outputs, then we assume a problem has occurred and hence take action. For optimality, these channels should use diverse software and hardware.

### N-Version Programming

If you ask N people the same open-ended question, assuming each person acts independently, what are the odds they will give the exact same answer? *Basically zero*. The same principle applies to software engineering. So, N-Version programming leverages this by **having multiple teams design the same software unit, executing in parallel on separate machines, comparing outputs using a voting system**. Of course, this requires lots of development costs, so this should be considered, only when the first two options are not viable.

### Diversity counts!

You may notice a pattern throughout this portion of the notes. **Diversity prevents problems!** Just as there are many ways to write a piece of software, there's many ways to cook a steak: searing, grilling, toaster, etc... One method may be more consistent, another more flavourful, one more... likely to give you food poisoning. But either way, diverse programming allows us to spot bugs more easily, preventing harm from being caused.

## Welcome to Test Chamber 4. You're doing quite well.

*Go play Portal 1 and 2 after you're done revising. They're good games.*
*(And Wheatley is voiced by a Warwick Grad!)*

Nobody wants to hear it, but **Testing** is very, very important. It prevents rockets from blowing up in the sky, radiotherapy worsening cancer (again?), and shareholders from losing millions of dollars (could be worse tbf). For a more "fun" example, a bug in the Zelda game Skyward Sword, essentially soft-locked the game and prevented some players from progressing to the end of the game. This is a clear issue from a lack of testing by quality assurance, but since then, Nintendo has gotten much better about testing (generally. hm.).

### Dang static electricity!

**Static testing** doesn't involve any execution of code at all. Instead, it entails looking directly at code and documentation, to ensure if in theory it is achieving the correct result. You could essentially describe this as program *verification* against the specification. We do both code and documentation as this challenges our *understanding* of what is being asked, ensuring we avoid problems quickly, saving ourselves headache down the line.

Static testing is useful, as we don't need a complete code base to do it, plus it's a good chance to refactor code, especially getting rid of abhorrent one-liners. About 90% of errors can be found through inspection, so it's worth your time! We may also use *linters* to help us spot issues, especially if you use visual studio code a lot.

### `39 out of 70 tests failed.`

Oops, sorry for scaring you there!

**Dynamic Testing** is where we actually run code against a set of test cases, to make sure it's doing the correct thing. This is especially important where concurrency or hardware are involved, as there are some factors which we may have previously failed to consider, such as race conditions or bad performance. Of course, we need explicit test cases and plans for this, so we use a set of test data, and ensure we know the correct outputs, *before we even think about writing any code*.

There are two types of dynamic testing, **White-box/Structural Testing** and **Black-box/Functional Testing**.

#### White Box Testing

With white-box testing, the focus is on the *structure* of the code. We may analyse a piece of code or an algorithm, and look at what variables are defined, and how they are used, abstracting this information into a flow diagram, allowing us to identify branches and loops. **We want to develop test cases that explore all branches and possibilities.** This ensures we cover the whole of the code.

#### Black Box Testing

For black box testing, we ensure a component's *functionality*, only caring about what we expect will happen when we put certain values in. **We create a set of test inputs, and determine their correct outputs, and test the code on these expected outcomes, all without looking at the code.** This type of testing includes unit tests, integration tests and system tests. Speaking of...

#### Units, Ints and Sys

- **Unit Testing** - Tests one specific component, i.e. a single file, class or thread of execution. We often automate these types of tests.
- **Integration Testing** - Starts putting the whole system together, ensuring components are interacting properly, highlighting interface misuse or misunderstanding, or timing errors.
- **System Testing** - Testing the system as a whole, including any off-the-shelf components we may want to use. There's often emergent behaviour here, that's not visible in integration testing.

### Let's take this for a test drive

**Test-driven Development** is a now widely-accepted paradigm, where we write tests before our code, and write new tests at every increment of code we make: start by identifying what could be added, write the test for it (which will of course fail), implement the functionality, test it, and keep modifying the increment until all tests pass. Rinse and repeat until you have the final code you're looking for.

Doing this allows us to better keep in mind what our code is actually meant to do, and allows us to keep in mind any common pitfalls, such as avoiding divisions by zero, or handling a null input.

## "How on earth did you do THAT??"

*Especially* nobody wants to hear it, but at some point, your client then needs to test your system to make sure it fits their needs and that it achieves what it was set out to achieve. Here's a fictional anecdote you may have heard before:

> A group of engineers design a robot bartender. They account for various different scenarios in which the robot may be asked to make drinks. They account for a customer asking the robot to make one drink, two drinks, three... infinite drinks, zero drinks, minus one drink, half a drink... you get the idea. After all of that testing, the team finally trials the robot in an actual bar.
>
> The first customer walks into the bar and asks the simple question: "Where's the bathroom?"
>
> The robot explodes.

Of course, the point of this is, as a software developer, you have to expect *everything*. The way someone may operate a program or computer may seem asinine or counter-intuitive, but you still need to keep it in mind. People's brains work in mysterious ways, *yours included*.

### Alpha Testing

The next time someone tells you they're an alpha, know that they're still in the works and you should do what you can to point out their flaws, as this is the point of **alpha testing**. This happens during development, and is important as it prevents bugs that could massively alter the course of development.

### Beta Testing

If, on the other hand, someone tells you they're a beta, they're mostly complete, but may have some minor flaws which you can help them to correct. **Beta testing** introduces the system to a wider, more open user base who can give their feedback, showing how the system works in a more complete form. Beta testing itself is often a form of marketing, think about the number of games you may have played while they were still in beta form!

### Acceptance Testing

*The final frontier...*

At the earliest stage of development, you and the client should have agreed on some **acceptance criteria**, which you will have gone away and converted into a **test plan**, from which you'll make a set of **acceptance tests**, mainly consisting of a *user journey*, describing what a user might intend to do, and what the expected outcome is. We then **run these tests** and then **negotiate the results** with the client. At long last, the client then gives their final judgement over whether or not they **accept or reject the system**. If they reject... get back to work. If they accept, *congratulations*! You've just built a piece of software that will (hopefully) be used in the real world and not kill anyone! Unless you're working for a military company, but I won't get into that hot mess.