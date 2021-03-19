# Extracting a function {data-auto-animate=""}

```{.R .numberLines data-id="extract-function" data-line-numbers="5-9|11-21"}
empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  #error checking
  # ...

  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)
  end_is_here   <- !is.null(rlang::get_expr(exposure_end))

  # Make sure that all the columns actually exist
  cols <- c(rlang::quo_text(date_of_onset),
            rlang::quo_text(exposure),
            rlang::quo_text(exposure_end))
  cols <- cols[cols != "NULL"]
  if (!all(cols %in% names(x))) {
    msg  <- "%s is not a column in %s"
    cols <- cols[!cols %in% names(x)]
    msg  <- sprintf(msg, cols, deparse(substitute(x)))
    stop(paste(msg, collapse = "\n  "))
  }
  
  # Grab the values from the columns
  doo   <- dplyr::pull(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)
  
  # ...
```

# Extracting a function {data-auto-animate=""}

```{.R .numberLines data-id="extract-function" data-line-numbers="1-12, 24-25|24"}
check_all_columns_exist <- function(x, date_of_onset, exposure, exposure_end) {
  cols <- c(rlang::quo_text(date_of_onset),
            rlang::quo_text(exposure),
            rlang::quo_text(exposure_end))
  cols <- cols[cols != "NULL"]
  if (!all(cols %in% names(x))) {
    msg  <- "%s is not a column in %s"
    cols <- cols[!cols %in% names(x)]
    msg  <- sprintf(msg, cols, deparse(substitute(x)))
    stop(paste(msg, collapse = "\n  "))
  }
}

empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  #error checking
  # ...
  
  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)
  end_is_here   <- !is.null(rlang::get_expr(exposure_end))

  # Make sure that all the columns actually exist
  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- dplyr::pull(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)
```

# Extracting a function {data-auto-animate=""}

```{.R .numberLines data-id="extract-function" data-line-numbers=""}
check_all_columns_exist <- function(x, date_of_onset, exposure, exposure_end) {
  cols <- c(rlang::quo_text(date_of_onset),
            rlang::quo_text(exposure),
            rlang::quo_text(exposure_end))
  cols <- cols[cols != "NULL"]
  if (!all(cols %in% names(x))) {
    msg  <- "%s is not a column in %s"
    cols <- cols[!cols %in% names(x)]
    msg  <- sprintf(msg, cols, deparse(substitute(x)))
    stop(paste(msg, collapse = "\n  "))
  }
}

empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  #error checking
  # ...
  
  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)
  end_is_here   <- !is.null(rlang::get_expr(exposure_end))

  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- dplyr::pull(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)
```

# Extracting a(nother) function {data-auto-animate="" data-auto-animate-unmatched="false"}

```{.R .numberLines data-id="extract-another-function" data-line-numbers="13-20|14,17-20"}
empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  #error checking
  # ...

  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)
  end_is_here   <- !is.null(rlang::get_expr(exposure_end))

  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- dplyr::pull(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)

  if (!inherits(doo, "Date")) {
    msg <- "date_of_onset must be a column of Dates. I found a column of class %s"
    stop(sprintf(msg, paste(class(doo), collapse = ", ")))
  }

  if (end_is_here) {
    # ...
```

# Extracting a(nother) function {data-auto-animate=""}

```{.R .numberLines data-id="extract-another-function" data-line-numbers="1-10,25"}
get_date_of_onset_value <- function(data, date_of_onset_var){
    doo   <- dplyr::pull(x, !! date_of_onset)
  
    if (!inherits(doo, "Date")) {
    msg <- "date_of_onset must be a column of Dates. I found a column of class %s"
    stop(sprintf(msg, paste(class(doo), collapse = ", ")))
    }

    return(doo)
  }
  
empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  #error checking
  # ...

  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)
  end_is_here   <- !is.null(rlang::get_expr(exposure_end))

  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- get_date_of_onset_value(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)

  if (end_is_here) {
    # ...
```

# Exctracting the construction of exposures {data-auto-animate="" data-auto-animate-unmatched="false"}

```{.R .numberLines data-id="extract-exposures" data-line-numbers="11-22"}
empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  
  # ...

  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- get_date_of_onset_value(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)

  if (end_is_here) {
    # We need to create the list for each date
    if (is.list(expos) || !inherits(expos, "Date")) {
      stop("if exposure_end is specified, then exposure must be a vector of Dates")
    }
    e     <- expos
    ee    <- dplyr::pull(x, !! exposure_end)
    expos <- vector(mode = "list", length = length(e))
    for (i in seq(expos)) {
      expos[[i]] <- seq(from = e[i], to = ee[i], by = "1 day")
    }
  }

  y <- compute_incubation(doo, expos)
  
  # ...
```

# Exctracting the construction of exposures {data-auto-animate=""}

```{.R .numberLines data-id="extract-exposures" data-line-numbers="1-13,25|2"}
exposure_for_each_date <- function(expos, data) {
  # We need to create the list for each date
  if (is.list(expos) || !inherits(expos, "Date")) {
    stop("if exposure_end is specified, then exposure must be a vector of Dates")
  }
  e     <- expos
  e    <- dplyr::pull(data, !! exposure_end)
  expos <- vector(mode = "list", length = length(e))
  for (i in seq(expos)) {
     expos[[i]] <- seq(from = e[i], to = ee[i], by = "1 day")
  }
  return(expos)
}

empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  
  # ...

  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- get_date_of_onset_value(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)

  if (end_is_here) expos <- exposure_for_each_date(expos, x)

  y <- compute_incubation(doo, expos)
  
  # ...
```

# The end isn't quite here {data-auto-animate=""}

```{.R .numberLines data-id="end-isnt-here" data-line-numbers="10,18"}
empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  #error checking

  # ...

  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)
  end_is_here   <- !is.null(rlang::get_expr(exposure_end))

  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- get_date_of_onset_value(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)

  if (end_is_here) expos <- exposure_for_each_date(expos, x)

  y <- compute_incubation(doo, expos)
```

# The end isn't quite here {data-auto-animate=""}

```{.R .numberLines data-id="end-isnt-here" data-line-numbers="17,18"}
empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  #error checking

  # ...

  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)

  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- get_date_of_onset_value(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)

  end_is_here   <- !is.null(rlang::get_expr(exposure_end))
  if (end_is_here) expos <- exposure_for_each_date(expos, x)

  y <- compute_incubation(doo, expos)
```

# The end isn't quite here {data-auto-animate=""}

```{.R .numberLines data-id="end-isnt-here" data-line-numbers="1-5,23"}
end_of_exposure <- function(exposure_end){
  return(
    !is.null(rlang::get_expr(exposure_end))
  )
}

empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  #error checking

  # ...

  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)

  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- get_date_of_onset_value(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)
  
  if (end_of_exposure(exposure_end)) expos <- exposure_for_each_date(expos, x)

  y <- compute_incubation(doo, expos)
```

# Exctracting the dataframe validation {data-auto-animate=""}

```{.R .numberLines data-id="dataframe-validation" data-line-numbers="2-9"}
empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {
  #error checking
  if (!is.data.frame(x)) {
    stop("x is not a data.frame")
  }

  if (ncol(x) == 0L) {
    stop("x has no columns")
  }

  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)
  
  # ...
```

# Exctracting the dataframe validation {data-auto-animate=""}

```{.R .numberLines data-id="dataframe-validation" data-line-numbers="1-10,14"}
check_if_valid_dataframe <- function(data) {

  if (!is.data.frame(x)) {
    stop("x is not a data.frame")
  }

  if (ncol(x) == 0L) {
    stop("x has no columns")
  }
}

empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {

  check_if_valid_dataframe(x)

  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)
  end_is_here   <- !is.null(rlang::get_expr(exposure_end))
  
  # ...
```

# Summary {data-auto-animate=""}

```{.R .numberLines data-id="full-function" data-line-numbers=""}
empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {

  check_valid_dataframe(x)

  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)
  end_is_here   <- !is.null(rlang::get_expr(exposure_end))

  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- get_date_of_onset_value(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)

  if (end_of_exposure(exposure_end)) expos <- exposure_for_each_date(expos, x)

  y <- compute_incubation(doo, expos)

  # check if incubation period is below 0
  if (any(y$incubation_period < 0)) {
    warning("negative incubation periods in data!")
  }

  return(y)
}
```

# Summary {data-auto-animate=""}

```{.R .numberLines data-id="full-function" data-line-numbers=""}
empirical_incubation_dist  <- function(x, date_of_onset, exposure, exposure_end = NULL) {

  check_valid_dataframe(x)

  # prepare column names for transfer
  exposure      <- rlang::enquo(exposure)
  date_of_onset <- rlang::enquo(date_of_onset)
  exposure_end  <- rlang::enquo(exposure_end)

  check_all_columns_exist(x, date_of_onset, exposure, exposure_end)

  # Grab the values from the columns
  doo   <- get_date_of_onset_value(x, !! date_of_onset)
  expos <- dplyr::pull(x, !! exposure)

  if (end_of_exposure(exposure_end)) expos <- exposure_for_each_date(expos, x)

  y <- compute_incubation(doo, exposure_for_each_date)

  # check if incubation period is below 0
  if (any(y$incubation_period < 0)) {
    warning("negative incubation periods in data!")
  }

  return(y)
}
```
