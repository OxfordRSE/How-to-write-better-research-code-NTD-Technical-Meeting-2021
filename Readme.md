# Practical approaches to writing sustainable research software
NTD Modelling Consortium technical meeting 2021

## View the slides

- [Introduction](https://oxfordrse.github.io/NTDMC_tech_meeting_2021/slides.html#/title-slide)
- [Example 1: grouping parameters into objects](https://oxfordrse.github.io/NTDMC_tech_meeting_2021/example1.html#/example-1)
- [Breakout room example: An SIR simulation](https://oxfordrse.github.io/NTDMC_tech_meeting_2021/longparam.html#/an-sir-simulation)
- [Example 3: nested if/else and complex conditions](https://oxfordrse.github.io/NTDMC_tech_meeting_2021/nested.html#/nested-logic-and-complex-conditionals)
- [Example 4: A long function](https://oxfordrse.github.io/NTDMC_tech_meeting_2021/longfunction.html#/long-methods-and-why-they-occur)
- [Documentation strings and style guides](https://oxfordrse.github.io/NTDMC_tech_meeting_2021/slides.html#/in-code-documentation-and-style-guides)
- [How to improve your smell? Empathy, collaboration and exposure](https://oxfordrse.github.io/NTDMC_tech_meeting_2021/slides.html#/how-to-improve-your-smell)
- [More on the Oxford RSE group](https://oxfordrse.github.io/NTDMC_tech_meeting_2021/slides.html#/oxford-research-sofware-engineering)

If you have a question, want to know more and simply want to chat about the material, feel free to open an issue on this repository.

## References

### Books
 - [Refactoring, by Martin Fowler](https://martinfowler.com/books/refactoring.html)
   The book that introduced the concept of "code smell".
   See particularly chapter 3 that discusses smells.

### Talks

 - [useR 2018 - Code smells and fells, by Jenny Bryan](https://www.youtube.com/watch?v=7oyiPBjLAWY&t=2897s&ab_channel=RConsortium)
 - [Rails Conf 2016 - Get a Whiff of This by Sandi Metz](https://www.youtube.com/watch?v=PJjHfa5yxlU&ab_channel=Confreaks)
 
### Articles and blogs

 - [Code Smells by Jeff Atwood (Coding Horror)](https://blog.codinghorror.com/code-smells/)
   J. Atwood is behind stack overflow.
 - [How To Write Unmaintainable Code by Roedy Green](https://github.com/Droogans/unmaintainable-code)
   A classic.
   
### Other

 - [Smells to Refactorings Quick Reference Guide (pdf)](https://www.industriallogic.com/img/blog/2005/09/smellstorefactorings.pdf)
 - [Code Smells on Refactoring Guru](https://refactoring.guru/es/refactoring/smells)

## To build the slides:

Clone the repository with submodules:
```bash
git clone git@github.com:OxfordRSE/https://github.com/OxfordRSE/NTDMC_tech_meeting_2021.git --recurse-submodules
```

Make sure you have pandoc and CMake installed

```bash
sudo apt install pandoc
```

Build the slides:

```bash
cd slides
mkdir build
bash update_reveal_dist.sh
cd build && cmake ..
make
```
