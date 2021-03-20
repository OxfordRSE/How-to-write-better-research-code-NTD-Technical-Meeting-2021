# Example # 2 -- maybe this is better for breakout room?

```R
carehomes_rt_mean_duration_weighted_by_infectivity <- function(step, pars)
{  
  dt <- pars$dt

  matricise <- function(vect, n_col) {
    matrix(rep(vect, n_col), ncol = n_col, byrow = FALSE)
  }

  n_vacc_classes <- ncol(pars$rel_susceptibility)

  n_groups <- pars$n_groups

  n_time_steps <-
    length(sircovid_parameters_beta_expand(step, pars$p_H_step))

  ## compute probabilities of different pathways
  p_C <- matricise(pars$p_C, n_vacc_classes) * pars$rel_p_sympt
  p_C <- outer(p_C, rep(1, n_time_steps))

  p_H <- matricise(pars$psi_H, n_vacc_classes) * pars$rel_p_hosp_if_sympt
  p_H <- outer(p_H, sircovid_parameters_beta_expand(step, pars$p_H_step))

  p_ICU <- outer(matricise(pars$psi_ICU, n_vacc_classes),
                 sircovid_parameters_beta_expand(step, pars$p_ICU_step))
  p_ICU_D <- outer(matricise(pars$psi_ICU_D, n_vacc_classes),
                   sircovid_parameters_beta_expand(step, pars$p_ICU_D_step))
  p_H_D <- outer(matricise(pars$psi_H_D, n_vacc_classes),
                 sircovid_parameters_beta_expand(step, pars$p_H_D_step))
  p_W_D <- outer(matricise(pars$psi_W_D, n_vacc_classes),
                 sircovid_parameters_beta_expand(step, pars$p_W_D_step))
  p_G_D <- outer(matricise(pars$psi_G_D, n_vacc_classes),
                 sircovid_parameters_beta_expand(step, pars$p_G_D_step))

  prob_H_R <- p_C * p_H * (1 - p_G_D) *
    (1 - p_ICU) * (1 - p_H_D)
  prob_H_D <- p_C * p_H * (1 - p_G_D) *
    (1 - p_ICU) * p_H_D
  prob_ICU_W_R <- p_C * p_H * (1 - p_G_D) *
    p_ICU * (1 - p_ICU_D) * (1 - p_W_D)
  prob_ICU_W_D <- p_C * p_H * (1 - p_G_D) *
    p_ICU * (1 - p_ICU_D) * p_W_D
  prob_ICU_D <- p_C * p_H * (1 - p_G_D) *
    p_ICU * p_ICU_D

  ## Compute mean duration (in time steps) of each stage of infection,
  ## weighed by probability of going through that stage
  ## and by relative infectivity of that stage

  ## Note the mean duration (in time steps) of a compartment for
  ## a discretised Erlang(k, gamma) is k / (1 - exp(dt * gamma))

  mean_duration_I_A <- pars$I_A_transmission * (1 - p_C) *
    pars$k_A / (1 - exp(- dt * pars$gamma_A))

  mean_duration_I_P <- pars$I_P_transmission * p_C *
    pars$k_P / (1 - exp(- dt * pars$gamma_P))

  mean_duration_I_C_1 <- pars$I_C_1_transmission * p_C *
    pars$k_C_1 / (1 - exp(- dt * pars$gamma_C_1))

  mean_duration_I_C_2 <- pars$I_C_2_transmission * p_C *
    pars$k_C_2 / (1 - exp(- dt * pars$gamma_C_2))

  mean_duration_G_D <- pars$G_D_transmission * p_C * p_H *
    p_G_D * pars$k_G_D / (1 - exp(- dt * pars$gamma_G_D))

  mean_duration_hosp <- pars$hosp_transmission * (
    prob_H_R * pars$k_H_R / (1 - exp(- dt * pars$gamma_H_R)) +
      prob_H_D * pars$k_H_D / (1 - exp(- dt * pars$gamma_H_D)) +
      (prob_ICU_W_R + prob_ICU_W_D + prob_ICU_D) * pars$k_ICU_pre /
      (1 - exp(- dt * pars$gamma_ICU_pre)))

  mean_duration_icu <- pars$ICU_transmission * (
    prob_ICU_W_R * pars$k_ICU_W_R / (1 - exp(- dt * pars$gamma_ICU_W_R)) +
      prob_ICU_W_D * pars$k_ICU_W_D / (1 - exp(- dt * pars$gamma_ICU_W_D)) +
      prob_ICU_D * pars$k_ICU_D / (1 - exp(- dt * pars$gamma_ICU_D)))

  mean_duration <- mean_duration_I_A + mean_duration_I_P + mean_duration_I_C_1 +
    mean_duration_I_C_2 + mean_duration_G_D + mean_duration_hosp +
    mean_duration_icu

  ## Account for different infectivity levels depending on vaccination stage

  mean_duration <- mean_duration *
    outer(pars$rel_infectivity, rep(1, n_time_steps))

  ## Multiply by dt to convert from time steps to days
  mean_duration <- dt * mean_duration

  mean_duration
}
```

## Duplicated code

```R
mean_duration_I_A <- pars$I_A_transmission * (1 - p_C) *
    pars$k_A / (1 - exp(- dt * pars$gamma_A))

mean_duration_I_P <- pars$I_P_transmission * p_C *
    pars$k_P / (1 - exp(- dt * pars$gamma_P))

mean_duration_G_D <- pars$G_D_transmission * p_C * p_H *
    p_G_D * pars$k_G_D / (1 - exp(- dt * pars$gamma_G_D))
```

Simplified using:

```R
discretised_erlang_pdf <- function(k, gamma, dt)
{
  k / (1 - exp(- dt * gamma))
}
```

## Taking function outside makes it testable

```R
matricise <- function(vect, n_col)
{
  matrix(rep(vect, n_col), ncol = n_col, byrow = FALSE)
}

carehomes_rt_mean_duration_weighted_by_infectivity <- function(step, pars)
{
	# do stuff without defining matricise
}
```

