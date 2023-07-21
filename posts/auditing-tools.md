---
title: Some code auditing tools!
date: 2023-07-20
---

## Hello!
Recently I've been reading up on code-auditing. Here are some different tools that I read are effective in assisting code-auditing. 

This is all very new to me, so for my own purposes I am documenting what softwares exist out there so I can go explore and test them out. I will probably end up writing more blog posts about what I like/dislike about some of these.


## Source Code navigators

Source code navigators are very similar to IDEs.(integrated development environments, like vscode) Instead they focus on readability and making the code easy to follow rather than building out the code. Here are some source code navigators below.


Free:

- Source Navigator(Linux/Windows)URL http://sourcenav.sourceforge.net/ 
- Cscope (Linux) 	 			URL http://cscope.sourceforge.net/ 
- Ctags(vim extension)  			URL http://ctags.sourceforge.net/ 

Paid: 

- Understand  URL www.scitools.com/

- Code Surfer (Linux/Windows)   URL www.grammatech.com/products/codesurfer/ 



## Debuggers 
Debuggers help understand code paths and put stop points in those paths.  Here are some debuggers that exist. Debuggers can offer lots of features like Kernel Debugging, memory searching, scripting/automation methods, conditional breakpoints, thread support and quick-assembling, and network debugging. 


Free:
- GNU Debugger (GDB)  (Linux/Windows)  URL www.gnu.org/ 
- OllyDbg (Windows) URL www.ollydbg.de/ 

Paid: 
- SoftICE  (Windows) URL www.compuware.com 


## Binary Navigation Tools
Since some applications don’t give you the available source code you can read the program binary by reading in the assembly code. Binary navigation tools help make this less difficult.
They offer lots of cool stuff like built in annotation, markers, graphing, structure definition and automation/scripting methods. 


Paid:
- IDA Pro (Linux/Windows) URL www.datarescue.com 
- BinNavi :    (Windows and Linux) -    URL www.sabre-security.com

## Fuzz-testing Tools
Fuzz testing tools help find bugs that you miss during code-auditing and can help with time constraints in a pinch. 

- SPIKE: UNIX and Window  - URL www.immunitysec.com


## Bye!
These are some of the tools I read that help get started on code auditing. I’m excited to get started with some of these in my free time. Peace out!



