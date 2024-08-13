# Debugging

- Rubber duck problem solving
  - When you set out to ask a question, you should
    - Describe what's happening in sufficient detail that we can follow along
    - Provide the necessary background for us to understand what's going on, even if we aren't experts in your particular area
    - Tell us why you _need_ to know the answer
      - What led you here? Is it idle curiosity or is this somehow blocking you on a project?
      - We don't need your whole life story, just give us some context her
    - Share your research on your problem; what have you found so far?
      - Why didn't it work? And if you didn't do any research … should you even be asking?
      - If you're inviting us to spend our valuable time helping you, it's only fair that you put in a reasonable amount of your valuable time into crafting a decent question
  - In the process of writing up a thorough, detailed question for Stack Overflow or another Stack Exchange site, you can figure out the answer to your own problem
  - Sometimes asking the right question seems like half the problem
  - The critical part of rubber duck problem solving is to totally commit to asking a thorough, detailed question of this imaginary person or inanimate object
  - If you aren't willing to put the effort into fully explaining the problem and how you've attacked it, you can't reap the benefits of thinking deeply about your own problem before you ask others to

---

- Five whys
  - Iterative technique used to explore the cause-and-effect relationships underlying a particular problem
  - Primary goal of "five whys" is to determine the root cause of a defect or a problem by repeating the question "why?" five times, each time directing the "why" to the answer of the previous "why"
  - Assertion is that the fifth "why" asked should reveal the root cause of the problem
  - Five iterations of asking why is generally sufficient to get to a root cause
  - The key idea of the method is to encourage the troubleshooter to avoid assumptions and logic traps and instead trace the chain of causality in direct increments from the effect through any layers of abstraction to a root cause that still has some connection to the original problem
  - The real root cause should point toward a process that is not working well or does not exist
  - Untrained facilitators without this understanding of the method often observe that answers obtained seem to point towards classical answers such as not enough time, not enough investments or not enough resources
  - Therefore practitioners of the method recommend instead of asking why? to ask why did the process fail?
- Example
  1. Why? – The battery is dead.
  2. Why? – The alternator is not functioning.
  3. Why? – The alternator belt has broken.
  4. Why? – The alternator belt was well beyond its useful service life and not replaced.
  5. Why? – The vehicle was not maintained according to the recommended service schedule (a root cause).

---

- Taking the right attitude when debugging matters
- **Inspect, don't squash**
  - Don't try to fix a bug you run into as fast as possible
  - If you can, leave the bug in place, figure out exactly what's gone wrong, and then fix it after you've understood what happened
- **Being stuck is temporary**
  - Sometimes you can demoralized when debugging and it feels like you'll never make progress
  - Remind yourself that you've fixed a lot of bugs before, and you'll probably fix this one too
- **Trust nobody and nothing**
  - Sometimes bugs come from surprising sources.
  - Maintain an open mind and persist in your investigation until you can confirm or rule out your hypothesis
- **It’s probably your code**
  - 95% of the time something is going wrong with your program, it’s because you did something silly
  - It’s important to look for the problem in your own code first before trying to blame some external source
- **Don’t go it alone**
  - 

---

- What do you check in the first minute when detecting performance issues?
  - First 60 seconds of an optimized performance investigation at the command line using standard Linux tools
  - In 60 seconds you can get a high level idea of system resource usage and running processes using ten commands
  - Look for errors and saturation metrics – both easy to interpret, then look into resource utilization
  - Saturation is where a resource has more load than it can handle, and can be exposed either as the length of a request queue, or time spent waiting
  - Some of the following commands require the `sysstat` package installed
  - The metrics these commands expose will help you complete some of the USE Method: a methodology for locating performance bottlenecks
  - This involves checking utilization, saturation, and error metrics for all resources (CPUs, memory, disks, e.t.c.)
  - Also pay attention to when you have checked and exonerated a resource, as by process of elimination this narrows the targets to study, and directs any follow on investigation
- **`uptime`** :arrow_right: `$ 23:51:26 up 21:31, 1 user, load average: 30.02, 26.43, 19.02`
  - Quick way to view the load averages, which indicate the number of tasks (processes) wanting to run
  - 

---

## Credits

- [Rubber Duck Problem Solving](https://blog.codinghorror.com/rubber-duck-problem-solving/) by Jeff Atwood
- [Five whys](https://en.wikipedia.org/wiki/Five_whys)
- [A debugging manifesto](https://jvns.ca/blog/2022/12/08/a-debugging-manifesto/) by Julia Evans
- [Linux Performance Analysis in 60,000 Milliseconds](https://netflixtechblog.com/linux-performance-analysis-in-60-000-milliseconds-accc10403c55) by the Netflix Technology Blog
- 

## Open questions

- Debugging code locally
- Profiling memory and performance
- Debugging microservices
- Debugging memory leaks
- 
