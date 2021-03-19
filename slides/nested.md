# Tracking which individuals receive a vaccine

```{.R .numberLines}
apply_vaccine_to_population <- function(population, nvaccines, threshold_age) {
    for (individual in population) {
        if (nvaccines > 0) {
            if (indiv["age"] > threshold_age || indiv["isAtRisk"]) {
                if (indiv["hasFirstJab"]) {
                    inject_second_jab(indiv)
                    nvaccines <- nvaccines - 1
                } else {
                    inject_first_jab(indiv)
                }
            }
        } else {
            return("No more doses")
        }
    }
}
```

The nested if/else logic makes it easy to get lost! Let's "flatten" this

# Return early

```{.R .numberLines}
apply_vaccine_to_population <- function(population, nvaccines, threshold_age) {
    for (individual in population) {
        if (nvaccines > 0) {
            if (indiv["age"] > threshold_age || indiv["isAtRisk"]) {
                if (indiv["hasFirstJab"]) {
                    inject_second_jab(indiv)
                    nvaccines <- nvaccines - 1
                } else {
                    inject_first_jab(indiv)
                }
            }
        } else {
            return("No more doses")
        }
    }
}
```

# Return early

```{.R .numberLines data-line-numbers="|3|3,5-11"}
apply_vaccine_to_population <- function(population, nvaccines, threshold_age) {
    for (individual in population) {
        if (nvaccines == 0) { return("No more doses") }
 
        if (indiv["age"] > threshold_age || indiv["isAtRisk"]) {
            if (indiv["hasFirstJab"]) {
                inject_second_jab(indiv)
                nvaccines <- nvaccines - 1
            } else {
                inject_first_jab(indiv)
            }
        }
}
```

# Return early (again)

```{.R .numberLines}
apply_vaccine_to_population <- function(population, nvaccines, threshold_age) {
    for (individual in population) {
        if (nvaccines == 0) { return("No more doses") }
 
        if (indiv["age"] > threshold_age || indiv["isAtRisk"]) {
            if (indiv["hasFirstJab"]) {
                inject_second_jab(indiv)
                nvaccines <- nvaccines - 1
            } else {
                inject_first_jab(indiv)
            }
        }
}
```

# Return early (again)

```{.R .numberLines data-line-numbers="5"}
apply_vaccine_to_population <- function(population, nvaccines, threshold_age) {
    for (individual in population) {
        if (nvaccines == 0) { return("No more doses") }
 
        if (indiv["age"] > threshold_age || indiv["isAtRisk"]) next
        
        if (indiv["hasFirstJab"]) {
            inject_second_jab(indiv)
            nvaccines <- nvaccines - 1
        } else {
            inject_first_jab(indiv)
        }
}
```
# Extract condition in function

```{.R .numberLines data-line-numbers="|5"}
apply_vaccine_to_population <- function(population, nvaccines, threshold_age) {
    for (individual in population) {
        if (nvaccines == 0) { return("No more doses") }
 
        if (indiv["age"] > threshold_age || indiv["isAtRisk"]) next
        
        if (indiv["hasFirstJab"]) {
            inject_second_jab(indiv)
            nvaccines <- nvaccines - 1
        } else {
            inject_first_jab(indiv)
        }
}
```

# Extract condition in function

```{.R .numberLines data-line-numbers="|1-4|10"}
eligible_to_vaccine <- function(individual, threshold_age) {
    in_target_age_group = indiv["age"] > threshold_age
    return(in_target_age_group || individual["isAtRisk"])
}

apply_vaccine_to_population <- function(population, nvaccines, threshold_age) {
    for (individual in population) {
        if (nvaccines == 0) { return("No more doses") }
        
        if (!eligible_to_vaccine(individual, threshold_age)) next
        
        if (indiv["hasFirstJab"]) {
            inject_second_jab(individual)
            nvaccines <- nvaccines - 1
        } else {
            inject_first_jab(individual)
        }
    }
```

# Summary

:::::::::::::: {.columns}
::: {.column width="50%"}
```{.R .numberLines}
apply_vaccine_to_population <- function(population, nvaccines, threshold_age) {
    for (individual in population) {
        if (nvaccines > 0) {
            if (indiv["age"] > threshold_age || indiv["isAtRisk"]) {
                if (indiv["hasFirstJab"]) {
                    inject_second_jab(indiv)
                    nvaccines <- nvaccines - 1
                } else {
                    inject_first_jab(indiv)
                }
            }
        } else {
            return("No more doses")
        }
    }
}
```
:::
::: {.column width="50%"}
```{.R .numberLines}
eligible_to_vaccine <- function(individual, threshold_age) {
    in_target_age_group = indiv["age"] > threshold_age
    return(in_target_age_group || individual["isAtRisk"])
}

apply_vaccine_to_population <- function(population, nvaccines, threshold_age) {
    for (individual in population) {
        if (nvaccines == 0) { return("No more doses") }
        
        if (!eligible_to_vaccine(individual, threshold_age)) next
        
        if (indiv["hasFirstJab"]) {
            inject_second_jab(individual)
            nvaccines <- nvaccines - 1
        } else {
            inject_first_jab(individual)
        }
    }
}
```
:::
::::::::::::::
