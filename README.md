
<!-- README.md is generated from README.Rmd. Please edit that file -->

# r2lambda

<!-- badges: start -->
<!-- badges: end -->

The goal of `{r2lambda}` is to make it easier to go from an `R` script
to a deployed `AWS Lambda` service.

## Requirements

- Assumes the AWS command line interface and Docker are installed.

## Installation

You can install the development version of `{r2lambda}` like so:

``` r
# install_packages("remotes")
remotes::install_github("discindo", "r2lambda")
```

## Setup

`r2lambda` assumes environmental variables for connecting to AWS
services are available in the `R` session. This is typically done via an
`.Renviron` file that should include:

    ACCESS_KEY_ID = "YOUR AWS ACCESS KEY ID"
    SECRET_ACCESS_KEY = "YOUR AWS SECRET ACCESS KEY"
    PROFILE = "YOUR AWS PROFILE"
    REGION = "YOUR AWS REGION"

Additionally, for Docker to be able to authenticate with AWS Elastic
Container Registry to push images, we also need to set up a `.aws`
folder with two files, `config` and `credentials`, the following
content:

- `config` (defining a AWS profile):

<!-- -->

    [default]
    region = YOUR AWS REGION
    output = json

- `credentials` (defining AWS secrets):

<!-- -->

    [default]
    aws_access_key_id = YOUR AWS ACCESS KEY ID
    aws_secret_access_key = YOUR AWS SECRET ACCESS KEY

More details:
<https://docs.aws.amazon.com/sdkref/latest/guide/file-format.html>

## Run

``` r
runtime_function <- "parity"
runtime_path <- system.file("parity.R", package = "r2lambda")
dependencies <- NULL

# Might take a while, its building Docker image and pushing it to a remote repository
deploy_lambda(
  tag = "lambda-test",
  runtime_function = runtime_function,
  runtime_path = runtime_path,
  dependencies = dependencies
  )

invoke_lambda(
  function_name = "lambda-test",
  invocation_type = "RequestResponse",
  payload = list(number = 2),
  include_logs = FALSE
)

#> Lambda response payload: 
#> {"parity":"even"}
```
# r2lambda
