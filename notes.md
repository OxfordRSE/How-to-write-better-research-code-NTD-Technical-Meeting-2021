

# Motivation


## There are several ways in which a software can be "good"

-   Performance
-   Usability
-   Readability
-   Maintainability / Extendability
-   Potential for reuse

These metrics don't necessarily go along with each other.
Ex: Performance vs readability


## First aim: software sustainability

Software sustainability is key to research reproducibility.

The authors or a third-party, perhaps familiar with the field, but with no specific
knowledge of the software should be able to

-   Compile/install/run the software (**Usability**)
-   Read and understand the source code (**Readability**)
-   Update the program (e.g. fix a bug or update a dependency). It
    the program breaks, it should be clear why. (**Maintainability**)


## Software as a research output

Similarly to a publication, research software enables future work. 

Most of the time by the author themselves (or next generation) or
other researchers.  In this context, reusability and extendability
matter.

-   **Reusability:** Software can be used for another project and/or its
    code (or part of) can be easily integrated to another program's
    code.  Typical hindrance to reusability are hard-coded parameters
    and file paths, global data and lack of modularity.
-   **Extendability:** Software can be adapted to new problems, requiring 
    additional features. Extendability often implies reusability.

A consequence of a lack of reusability and extendability is
researchers having to write software from scratch when starting a
new project.  Such time-consuming task is often justified by the
necessity of having a clear understanding of what the software does.


## Quality software accelerates research

If software quality matters from an research ethics point of view,
it also does from a practical point of view.

-   Development is faster
    -   Efficient navigation
    -   Less error-prone
    -   Bugs are easier to fix
-   Code can be understood months or years after development
-   Code can be reused in future projects
-   Code is easier to test
-   Collaboration is more effective (collaborative development,
    on-boarding of new group members)
-   Enhanced credibility of research and impact


## Hurdles in the way

-   Too few resources and examples.
-   Lack of training.
-   Low expectations, lack of quality standard.

As a result, improving software quality takes both time and energy.

To goal of today is to illustrate a few simple approaches that you
can apply right away to improve the quality of your software.
We focus on

-   Readability
-   Maintainability and extendability
-   Reusability


## A word about performance

As a rule of thumb, performance should never be the primary concern
when writing software.

-   Most of the time, performance will not be an issue. Modern
    compiler/interpreters do a great job at optimising our codes under
    the hood.
-   If performance is an issue, a program program that is both readable and 
    maintainable will be far easier to optimise.

