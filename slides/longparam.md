# An SIR simulation

```{#initcode data-line-numbers="|3"}
  library(deSolve)

  simulate_SIR <- function(S0, I0, R0, Tstart, Tf, timestep, gamma, beta, N, method="lsoda", atol=1e-6, rtol=1e-06)
  {
      dxdt <- function(t, y, parms) {
	  list(
	      derivatives=c(
		  dSdt=-beta*y[1]*y[2]/N,
		  dIdt=beta*y[1]*y[2]/N - gamma*y[2],
		  dRdt=gamma*y[2]
	      )
	  )
      }

      times = seq(from = 0, to = Tf, by = timestep)
      ode(c(S0, I0, 0), times, dxdt, method = "lsoda", atol, rtol)    
  }
```

# {data-auto-animate=""}

```{.R .numberLines data-id="code-animation"}
  library(deSolve)

  simulate_SIR <- function(N, S0, I0, R0, Tstart, Tf, timestep, gamma, beta, method="lsoda", atol=1e-6, rtol=1e-06)
  {
      dxdt <- function(t, y, parms) {
	  list(
	      derivatives=c(
		  dSdt=-beta*y[1]*y[2]/N,
		  dIdt=beta*y[1]*y[2]/N - gamma*y[2],
		  dRdt=gamma*y[2]
	      )
	  )
      }

      times = seq(from = 0, to = Tf, by = timestep)
      ode(c(S0, I0, 0), times, dxdt, method = "lsoda", atol, rtol)
  }
```

# {data-auto-animate=""}
```{.R .numberLines data-id="code-animation" data-line-numbers="5"}
  library(deSolve)

  simulate_SIR <- function(S0, I0, R0, Tstart, Tf, timestep, gamma, beta, method="lsoda", atol=1e-6, rtol=1e-06)
  {
      N <- S0 + I0 + R0
      dxdt <- function(t, y, parms) {
	  list(
	      derivatives=c(
		  dSdt=-beta*y[1]*y[2]/N,
		  dIdt=beta*y[1]*y[2]/N - gamma*y[2],
		  dRdt=gamma*y[2]
	      )
	  )
      }

      times = seq(from = 0, to = Tf, by = timestep)
      ode(c(S0, I0, 0), times, dxdt, method = "lsoda", atol, rtol)    
  }
```

# {data-auto-animate=""}

```{.R .numberLines data-id="group-parameters" data-line-numbers=""}
  simulate_SIR <- function(S0, I0, R0, Tstart, Tf, timestep, gamma, beta, method="lsoda", atol=1e-6, rtol=1e-06)
  {
      N <- S0 + I0 + R0
      dxdt <- function(t, y, parms) {
	  list(
	      derivatives=c(
		  dSdt=-beta*y[1]*y[2]/N,
		  dIdt=beta*y[1]*y[2]/N - gamma*y[2],
		  dRdt=gamma*y[2]
	      )
	  )
      }

      times = seq(from = 0, to = Tf, by = timestep)
      ode(c(S0, I0, 0), times, dxdt, method = "lsoda", atol, rtol)
```
We group together

- the parameters of the numerical model `beta`, `gamma` and `timestep`)
- the parameters for the solver (`method`, `atol` and `rtol`)

# {data-auto-animate=""}

```{.R .numberLines data-id="group-parameters" data-line-numbers=""}
simulate_SIR <- function(<mark>init_state</mark>, model_parms, solver_parms, Tstart, Tf)
{
  dxdt <- function(t, y, parms) {
	list(
	    derivatives=c(
		dSdt=-parms$beta*y[1]*y[2]/parms$N,
		dIdt=parms$beta*y[1]*y[2]/parms$N - parms$gamma*y[2],
		dRdt=parms$gamma*y[2]
	    )
	)
    }

    times = seq(from = Tstar, to = Tf, by = model_parms$timestep)
    parms = c(model_parms, N=sum(init_state))
    ode(init_state, times, dxdt, parms, solver_parms$method, solver_parms$atol, solver_parms$rtol)
}
```
We group together

- the parameters of the numerical model `beta`, `gamma` and `timestep`)
- the parameters for the solver (`method`, `atol` and `rtol`)

# A common pattern for all variable names {data-auto-animate=""}

```{.R .numberLines data-id="code-animation" data-line-numbers="1,6,7,13"}
simulate_SIR <- function(init_state, model_parms, solver_parms, min_time, max_time)
    {
      dxdt <- function(t, y, parms) {
	    list(
		derivatives=c(
		    dSdt=-parms$beta*y[1]*y[2]/parms$population_size,
		    dIdt=parms$beta*y[1]*y[2]/parms$population_size - parms$gamma*y[2],
		    dRdt=parms$gamma*y[2]
		)
	    )
	}

	times = seq(from = min_time, to = max_time, by = model_parms$timestep)
	parms = c(model_parms, population_size=sum(init_state))
	ode(init_state, times, dxdt, parms, solver_parms$method, solver_parms$atol, solver_parms$rtol)
    }
```

# Follow a style guide (tidyverse) {data-auto-animate=""}

```{.R .numberLines data-id="code-animation" data-line-numbers=""}
simulate_SIR <- function(init_state, model_parms, solver_parms, min_time, max_time) {
      dxdt <- function(t, y, parms) {
	list(
	  derivatives = c(
	    dSdt = -parms$beta * y[1] * y[2] / parms$population_size,
	    dIdt = parms$beta * y[1] * y[2] / parms$population_size - parms$gamma * y[2],
	    dRdt = parms$gamma * y[2]
	  )
	)
  }

      times <- seq(from = min_time, to = max_time, by = model_parms$timestep)
      parms <- c(model_parms, population_size = sum(init_state))
      ode(init_state, times, dxdt, parms, solver_parms$method, solver_parms$atol, solver_parms$rtol)
    }
```

# Summary

:::::::::::::: {.columns}
::: {.column width="50%"}
```{.R .numberLines}
  library(deSolve)

  simulate_SIR <- function(S0, I0, R0, Tstart, Tf, timestep, gamma, beta, N, method="lsoda", atol=1e-6, rtol=1e-06)
  {
      dxdt <- function(t, y, parms) {
	  list(
	      derivatives=c(
		  dSdt=-beta*y[1]*y[2]/N,
		  dIdt=beta*y[1]*y[2]/N - gamma*y[2],
		  dRdt=gamma*y[2]
	      )
	  )
      }

      times = seq(from = 0, to = Tf, by = timestep)
      ode(c(S0, I0, 0), times, dxdt, method = "lsoda", atol, rtol)    
  }
```
:::
::: {.column width="50%"}
```{.R .numberLines}
simulate_SIR <- function(init_state, model_parms, solver_parms, min_time, max_time) {
      dxdt <- function(t, y, parms) {
	list(
	  derivatives = c(
	    dSdt = -parms$beta * y[1] * y[2] / parms$population_size,
	    dIdt = parms$beta * y[1] * y[2] / parms$population_size - parms$gamma * y[2],
	    dRdt = parms$gamma * y[2]
	  )
	)
  }

      times <- seq(from = min_time, to = max_time, by = model_parms$timestep)
      parms <- c(model_parms, population_size = sum(init_state))
      ode(init_state, times, dxdt, parms, solver_parms$method, solver_parms$atol, solver_parms$rtol)
    }
```
:::
::::::::::::::

# An `SIR_init_state` class

```{.R .numberLines}
print.SIR_init_state <- function(obj) {
  print("Initial state for an SIR model with:")
  print(sprintf("   S0 = %f", obj$S))
  print(sprintf("   I0 = %f", obj$I))
  print(sprintf("   R0 = %f", obj$R))
}


init_state <- list(S = 998, I = 2, R = 0)
class(init_state) <- "SIR_init_state"

print(init_state)
```



