# Appendix I - Principles for the Simple Server

This server will need to function both now and later. Spinning it up once is half the work; the other half is keeping it up and getting it back up when it goes does. And if you’re like me—someone who doesn’t work with computers everyday—there’s a good chance you’ll at least forget the tedious details if not the big ideas. By designing a server that works for the future you, it is more likely to be kept up for the long term.
This is not to say don't experiment with your server. You should. But do it with the goals above in mind.

___

The primary goals of the RockPro64 Simple Server are:
1. Straight-forward setup.
2. Minimized complexity.
3. Maintanable by your future self, assuming that the skills you learned during initial setup will have waned.

The KISS principle is also applicable:
> The KISS principle is best exemplified by the story of Johnson handing a team of design engineers a handful of tools, with the challenge that the jet aircraft they were designing must be repairable by an average mechanic in the field under combat conditions with only these tools. Hence, the "stupid" refers to the relationship between the way things break and the sophistication available to repair them.

Rule of Least Power (use the smallest software you need; bigger 


Finally, the Simple Server is also based on the design principles for the [Clock of the Long Now](https://longnow.org/clock/), which I've modified below:

**Longevity**: With occasional maintenance, the server should reasonably be expected to function for 10 years.

**Maintainability**: The server should be maintainable with minimal coding know-how.

**Transparency**: It should be possible to determine operational principles of the server and its code by close inspection.

**Evolvability**: It should be possible to improve the server with time.

**Scalability**: It should be possible to increase the storage capacity of the server. However, increasing I/O (Input/Output) capacity would likely necessitate a new design.

### Longevity:
- Go slow
- Avoid heat
- Avoid excessive fan speed and energy use
- Stay clean
- Stay dry
- Expect power outages
- Expect hardware failure
- Expect attacks
- Don't tempt hackers

### Maintainability and transparency:
- Use familiar hardware and code
- Allow inspection
- Rehearse backups and restarts
- Make it easy to buy spare parts
- Expect restarts
- Include the manual

### Scalability and Evolvabilty:
- Make all parts similarly
- Separate functions
- Provide simple interfaces
