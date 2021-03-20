# Example # 1

## What is this function about ?

```R
model <- function(start, stop, stoc, spec, dens,
                  b, i_mat, i_dur, ntype, ncov)
{
  # function's body
}
```

## Change names to convey meaning

```R
simulate <- function(time_start, time_stop, is_stochastic, mosquito_species,
                     mosquito_density, mosquito_to_human_prob, immunity_maternal,
					 immunity_duration, net_type, net_coverage)
{
  # function's body
}
```

## Tip # 1

"Have you met my good friend 'bSty_1800'?"

​	Use descriptive variable names.

## What are remaining problems with this?

```R
simulate <- function(time_start, time_stop, is_stochastic, mosquito_species,
                     mosquito_density, mosquito_to_human_prob, immunity_maternal,
					 immunity_duration, net_type, net_coverage)
{
  # function's body
}
```

- Parameters could be easily mixed up.
- It's not clear what types things are expected to be.
- If we add more parameters, things will get unwieldy!

## How can we make it better?

```R
SimulationParameters <- setClass("SimulationParameters",
                                  slots=list(time_start="numeric",
       						      time_stop="numeric",
       						      is_stochastic="logical"))
```

## How can we make it better?

```R
# doesn't work
simulation_parameters <- SimulationParameters(
    time_start=1990,
    time_stop=2018,
    is_stochastic=2
)

# works
simulation_parameters <- SimulationParameters(
    time_start=1990,
    time_stop=2018,
    is_stochastic=FALSE
)
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
```
## Validity checking

```R
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
