---
title:  "Thinking Like a Hacker"
date:   2020-06-08 13:25:14 -0400
categories: personal
tags: personal
cover: /assets/images/personal/thinkinghacker/thinkinghacker-logo.png
---

A few years ago, I was working with a optometry clinic to migrate an industry standard Auto Refractometer software on another machine using a modern operating system. In order to successfully complete the migration process, licence keys are required to register the Auto Refractometer device. Unfortunately, the licence key was stored on a corrupted floppy disk format and a digital copy was never made. The manufacturer informed us about device support is no longer available and suggested to pay $5K USD for a newer machine with a yearly support contract. Ultimately, I could have just walked away from this situation and pass the hefty bill to the client but the hacker mentality kicked in. In a few days, I was able to decrypt the database, and reverse engineered the licence key activation process. It feels good when accomplishing something without the possibility of reaching the end goal. In hindsight, it turns out that there's a lot more to being a hacker than hacking.

## Mental Models

Identification of system weaknesses require logical reasoning and the ability to think systematically for actionable paths and possible conclusions. It can be a daunting task to think about where to begin due to the perceived skill and ambiguity tolerance. Developing mental models can help you become better at dissecting, explaining, and predicting behaviours of a system. A mental model can be thought of as a hacker's internal representation of the components and operating rules of an extremely complex software and hardware system.

## First Principles Thinking

First principle thinking is about deconstructing the problem into fundamental truths that can't be deduced any further. This process removes assumptions, conventional thinking, and reasoning by analogy which can lead into a path of incorrect truth.

A way to think about this is distinction, a hacker vs script kiddie. A hacker does all the research and development from scratch. A script kiddie uses pre-made tools with some minor tweaks. Both groups can achieve the same result but if the tool doesn't work as expected, which group will know how to diagnose the issue? The hacker because they know the limitations and possibilities of the problem to fix any issues that may arise.

If we never learn how to deconstruct the problem, test the assumptions, reconstruct it again with new ideas, we end being trapped by following what others assumption and procedures. You must've heard it before when you asked a question that couldn't be answered: "We've always have done this way and it works". This is where first principles can cut through the dogma and blind spots to see what the problem really is about.

I've broken down into 3 steps for first principles thinking.
1. Identify the problem.
2. Break down the problem into smaller chunks.
3. Dive deep into the smaller chunks of problem by asking why until you reach the core of first principles.

First principles thinking unveils the knowledge gaps and your current understanding of the problem. It requires a lot of time and effort to uncover the core elements. Using other mental models to augment first principles can be a powerful skill to learn and adapt when facing unknown unknowns.

## Inversion Thinking

Inversion thinking can be applied to a problem when you need a fresh perspective. What I mean by fresh perspective is probably not what you are thinking of. I'm talking about what is the worst case scenario for what I'm trying to solve and work backwards to the original problem. Focusing on the other side of the problem can put a spotlight on errors and obstacles that were not obvious at first.

How do you ask inverted questions? Ask the opposite question. For example, I've created a diagram that I made that is related to Information Security. Deploying an information security solution is complex and sometimes marketed as a magic bullet to solve some security woes. More often than not, misconfigurations and half baked implementations do happen which can possibly lead to a security breach. Before deploying a complex security solution, consider what other elements will increase the attack surface when implemented correctly or incorrectly. Imagining what are the worst case scenarios.

![inversionthinking.png](/assets/images/personal/thinkinghacker/thinkinghacker-inversionthinking.png){:height="600px"}

Seeing a different perspective of the problem by thinking forward and backward might help arise creative solutions more quickly. Inversion thinking challenges your own current beliefs and avoidance of what doesn't work to save time and headache from diving deeper into rabbit holes. We all have been there if you competed in capture-the-flag events.

## Abstraction Laddering

Abstraction laddering can be used to break down a problem into abstract and concrete answers. Similar to inversion thinking, we gain new perspective on a question with levels of abstraction to move beyond the initial problem statement. As an example in the story told in the beginning, abstraction laddering can be applied as such:

3) Why - To hack the licence management table.

2) Why - To see if I could unlock potential additional premium features.

1) Why - To test security of the product.

**Problem: Extract password from encrypted MDF database.**

1) How - Perform brute force dictionary attacks.

2) How - Exploit weak crypto vulnerabilities.

3) How - Decrypt the MDF database by viewing process threads.

![abstractionladdering.png](/assets/images/personal/thinkinghacker/thinkinghacker-abstractionladdering.png){:height="600px"}

As you move through the ladder, you begin to "see the forest from the trees". Sometimes we become too involved with the details of the problem and forget to look at the situation as a whole.

Abstraction laddering model does not require to start from the middle and work your way through for both directions. When branching out into either direction, new questions will become apparent and answers will determine if the problem isn't a problem at all. A word of caution, if branches are becoming too spread out, you may want to consider using other mental models to help clarify the problem before using abstraction laddering model.

## Future Updates 

I'll be updating this post from time to time with new mental models and the addition of covering the topic of motivation of a hacker to complete this post about how to think like a hacker.