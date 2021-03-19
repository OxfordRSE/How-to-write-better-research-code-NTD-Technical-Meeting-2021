# Software can be "good" in several ways

-   Performance
-   Usability
-   Readability
-   Maintainability / Extensibility
-   Potential for reuse

These metrics don't necessarily go along with each other.

# Software sustainability

Long after the project is "done", it is possible to

-   Compile/install/run the software (**Usability**)
-   Read and understand the source code (**Readability**)
-   Update the program (**Maintainability**)
-   Reuse all or part of the program in another project (**Reusability**)

This applies to both the original author(s) and other researchers.

# Software as a research output

Authors can reuse their own code, and colleagues can build upon it.

For this to be true, we want software that is 
- **Readable:**
- **Reusable:**
- **Extensible:**

Typical hindrance to reusability and extensibility are hard-coded parameters and file paths, global data and lack of modularity.
```python
file_handle = open("/Users/Sam/phdthesis/data/survey.dat")
```

# Quality software accelerates research

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

# Hurdles in the way of improving software quality

-   Lack of exposure to good examples.
-   Too few resources relevant to researchers.
-   Lack of training.
-   Assumption that the code will never be read.

# It's not as difficult as you might think!

There are simple steps you can take today
-   Choose descriptive names for variables and functions
-   Avoid nested logic (for/if/else)
-   Reduce the size of functions' parameter lists
-   Break long functions into smaller functions
-   Follow a style guide

# Code Smells

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

# Example # 1

## What does this code do?

```R
model <- function(start, stop, stoc,
                  spec, dens,
                  b, i_mat, i_dur,
                  ntype, ncov)
{
# do stuff
}
```

## Change names to convey meaning

```R
simulate <- function(time_start, time_stop,
		       					 is_stochastic, mosquito_species,
                     mosquito_density,
                     mosquito_to_human_prob,
                     immunity_maternal, immunity_duration,
                     net_type, net_coverage)
{
# do stuff
}
```

## Tip # 1

"Have you met my good friend 'bSty_1800'?"

​	Use descriptive variable names.

## What are remaining problems with this?

``` R
simulate <- function(time_start, time_stop,
                     is_stochastic, mosquito_species,
                     mosquito_density,
                     mosquito_to_human_prob,
                     immunity_maternal, immunity_duration,
                     net_type, net_coverage)
{
# do stuff
}
```

- Passing a long list of things that could easily be mixed up; especially if we don't always pass in named arguments
- It's not clear what types things are expected to be
- If we add more parameters, the thing can quickly get unwieldy

## How can we make it better?

```R
SimulationParameters <- setClass("SimulationParameters",
       slots=list(time_start="numeric",
       						time_stop="numeric",
       						is_stochastic="logical"))

# doesn't work
simulation_parameters <- SimulationParameters(
       time_start=1990,
       time_stop=2018,
       is_stochastic=2)

# works
simulation_parameters <- SimulationParameters(
       time_start=1990,
       time_stop=2018,
       is_stochastic=FALSE)
```

## Validity checking

```R
check <-function(object) {

  if(!object@net_type %in% c("net 1", "net 2"))
    return("A net must of type `net 1` or `net 2`.")

  coverage <- object@net_coverage
  if(coverage < 0 | coverage > 1)
    return("Net coverage must not be outside [0, 1].")
}

BednetParameters <- setClass("BednetParameters",
        slots=list(net_type="character",
        net_coverage="numeric"),
        validity = check)
                                   
# fails
bednet_parameters <- BednetParameters(
  			net_type = "net 3",
        net_coverage = 0.3)

# fails
bednet_parameters <- BednetParameters(
  			net_type = "net 2",
        net_coverage = 100)
```

## Simulation object

```R
Simulation <- setClass("Simulation",
         slots=list(simulation_parameters="SimulationParameters",
                    bednet_parameters="BednetParameters",
                    immunity_parameters="ImmunityParameters",
                    mosquito_parameters="MosquitoParameters"))

# ...don't show how to do but could create a "run" method
simulation <- Simulation(simulation_parameters=....)
result <- run(simulation)

# what parameters did I use to run my simulation?
simulation@simulation_parameters
```

## Tip # 2

"Oh you're going the same way as me? Why not ride-share?"

​	Repackage your long parameter lists into sets of objects that travel together.

"Can you prove you're of correct type? If not, you're not getting into this function."

​	Check for valid parameter values.

## Breakout exercise: SIR model

# Example # 2

```R
process_contact_matrices <- function(c_home, c_school, c_work, c_other)
{
    nce <- A - length(c_home[1, ])
    contact_home <- matrix(0, nrow=A, ncol=A)
    contact_school <- matrix(0, nrow=A, ncol=A)
    contact_work <- matrix(0, nrow=A, ncol=A)
    contact_other <- matrix(0, nrow=A, ncol=A)

    for (i in 1:(A - nce)){
      for (j in 1:(A - nce)){
        contact_home[i, j] <- c_home[i, j]
        contact_school[i, j] <- c_school[i, j]
        contact_work[i, j] <- c_work[i, j]
        contact_other[i, j] <- c_other[i, j]
      }
    }

    for (i in (A + 1 - nce):A){
      for (j in 1:(A - nce)){
        contact_home[i, j] <- c_home[(A - nce), j]
        contact_school[i, j] <- c_school[(A - nce), j]
        contact_work[i, j] <- c_work[(A - nce), j]
        contact_other[i, j] <- c_other[(A - nce), j]
      }
    }
  }
```

## Rewriting using 



# How to improve your smell?

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

# Oxford Research Sofware Engineering

* Who are we?
* What do we offer?
  * Example projects
  * Example courses
* How to get in touch?