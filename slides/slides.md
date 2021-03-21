---
title: Practical approaches to writing better software
author:
  - Ben Lambert
  - Thibault Lestang
affiliation: Department of Computer Science, University of Oxford
---

# Quality research software

## First aim: sustainable software

Long after the project is "done", it is possible to

-   Compile/install/run the software (**Usability**)
-   Read and understand the code (**Readability**)
-   Update the program (**Maintainability**)
-   Reuse the code/program (**Reusability**)

This applies to other researchers... and *yourself* in 6 months!

::: notes

Software can be "good" in many ways, but in a research context, sustainability is
the most important measure of software quality.

sustainable software is...

The reason sustainability is so important is because it is vital to
reproducibility and transparancy.  If peers can't run your software in years after
youyr paper is published, they will ahve a hard time reproduce your
work. If your code is very difficult to understand, it's going to be
very difficult for anyone to get insights into the actual
implementation of your work. That can be very problematic, especially
for results with societal impact.

- It can contain Markdown
- like this list

:::

## Software is a research output

It should be possible to build upon existing research software (**Extensibility**)

Typical hindrance to reusability and extensibility are hard-coded parameters and file paths and low modularity.

```python
file_handle = open("/Users/Sam/phdthesis/data/survey.dat")
```

::: notes

Software sustainability matters because, very much like a paper, software is a research output.

There are few things in science that can enable future research as much as well-written research software. Whether it's the future of *your* research or somebody else's.

Software is "soft", it's easy to share, it's easy to build upon - compared to a physical experimental setup for instance.

If we consider research as a collective human effort, software plays a key role in enabling this collaboration.
:::

# Quality software accelerates your research

## Software that's easier to work with

-   Bugs are easier to catch and easier to fix.
-   Source code is easy to navigate.
-   Source code is easy to modify.

## Software that can be built upon

-   Code can be understood months or years after development.
-   Software can be reused across projects, instead of starting from scratch.
-   Software can be extended to new situations.

## Software that makes your research more impactful

Open source, quality research software increases credibility and visibility of research.

## Hurdles in the way of improving software quality

-   Lack of exposure to good examples.
-   Too few resources relevant to researchers.
-   Lack of training.
-   Assumption that the code will never be read.

::: notes
If research software quality has such a positive impact, which isn't everyone
writing amazing software

"my code will not be read": if you assume this, it's likely that it will not be.
But if you write readable software, you code is more likely to be read (new students/post-docs, collaborators). Also, **you will read your code many times**!
:::

# It's not as difficult as you might think!

## Simple approach you can apply today

-   Choose descriptive names.
-   Avoid nested logic (`for`/`if`/`else`).
-   Reduce the size of functions'parameter lists.
-   Break long functions into smaller functions.
-   Follow a style guide.

By keeping these in mind as you code, you can make your software a lot
better already.

## Code Smells

There are many ways to write good software.

There are fewer ways to write bad software.

*Code smells* are recognised patterns of code likely to hinder understanding and make it hard to work with.

Code smells are actually named (Fowler, 2000):

-   Long Parameter List
-   Duplicated Code
-   Global Data

## Which one of these is not a real code smell?

* Large class
* Feature envy
* Inappropriate intimacy
* Angry quiche
* Freeloader
* Refused bequest

## Why learn how to smell?

Code smells have well-defined remedies.

Learning to classify code as whiffing of a particular odour means you can apply these.

# How to improve your smell?

## Empathy and writing

* Steven Pinker argues that empathy is key to good academic writing
* Even if writing solo, you can reduce the empathy gap with readers
* You can ask a future you if they understand what you mean
* (Better) you can ask others if they understand

## Empathy and coding

* Writing good software also requires empathy
* You can also use the same approaches to lower the empathy gap
* Repeatedly ask future you what they think about code and refactor
* (Much much better) you can ask others if they understand

## Coding collaboratively

* There are a number of approaches to working collaboratively
* Coding in pairs
* One off code reviews between you and your colleagues
* (Best) code collaboratively:
  * Produces better quality software
  * You also learn other approaches
  * Github is a great ground for collaborative development

## Style guides (another way to improve)

## Further exposure

* Reading references:
* Video references:
* "Better Codehub" badges on Github: semi-automated checking of software
* Courses offered by OxRSE (see below)

## Oxford Research Sofware Engineering

* Who are we?
* What do we offer?
  * Example projects
  * Example courses
* How to get in touch?
