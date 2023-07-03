---
title: Address Space Layout Randomization(ASLR) A technique I found pretty cool!
date: 2023-07-03
---


I never really considered where applications load resources when executed and how this could be targeted for attack if they were loaded into the same address each time. So, when I read about ASLR I thought it was very cool and I want to give a quick high level overview on what I learned about ASLR.


## What is ASLR?

By default, programs may allocate various components, such as the stack and heap, at predictable memory locations when they are executed. This predictable allocation can make it easier for attackers to attempt buffer overflow attacks. However, address space layout randomization (ASLR) techniques counter this vulnerability by randomizing the memory locations where the application loads its resources and components at load time. Each time the application is run, the resources that were previously allocated at a specific address, such as 0xA1234567, would be placed at a different, randomized address like 0xB7654321


###  How does it protect your environment?
ASLR provides protection to your environment by randomizing the addresses of application resources. This randomization makes it significantly more challenging for attackers to pinpoint specific memory addresses for carrying out memory exploitation attacks.


## How to implement ASLR in your operating system?

In Windows 10, you can turn on ASLR in the settings under **Exploit protection**. By default, it's off but can be enabled. However, the effectiveness of this Windows 10 feature may vary.


In Linux, ASLR is turned on by default with a value of 2. Setting it to zero would turn it off if, for some reason, you wanted to do that.


```sysctl -w kernel.randomize_va_space=2```

### Drawbacks

One downside to ASLR is that some people report it can cause problems with certain drivers for some programs. ASLR can also cause errors in databases and servers. For example, in DB2, it can lead to odd behavior due to how maps share memory across processes. ASLR techniques can interfere with **addressing**(when an OS assigns unique ids or memory locations to data for program accesss/manipulation).

While ASLR is a very neat technique to enhance application safety, it's important to keep in mind that ASLR techniques alone won't ensure complete security. It should be combined with other memory protection techniques to add more layers of safety. These drawback exists because ASLR is limited to the number of memory addresses on the system.