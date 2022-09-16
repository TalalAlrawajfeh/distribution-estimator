# Distribution Estimator

This tool finds the distribution that best fits an input dataset.

## Requirements

To be able to use this tool, you need to have `python` installed with at least version `3.9`.
To install the required dependencies, run the following command:

```
[pip command] install -r requirements.txt
```

where `[pip command]` is replaced with `pip3` if you are working on a Unix/Linux based system or with `pip` if you are working on a Windows system.

## Run

To use the tool, run the following command:

```
[python command] -m src.estimation_tool [input file path]
```

where `[python command]` is replaced with `python3` if you are working on a Unix/Linux based system or with `python` if you are working on a Windows system and `[input file path]` is replaced with the path to the file containing integer values with each value contained in a separate line.

To try the existing sample, run:
```use
[python command] -m src.estimation_tool example_sample.txt
```

which will output:
```
Best discrete distribution fit result:
Zipfian(s = 0.8742652627639053, n = 3)
according to the test: Chi-Square Test
statistic: 0.0885003158140516
p-value: 0.9567145978140291
No suitable continuous distribution was found for the given data.
```

## Development

The functional part of the code is contained in the `src` directory. All unit tests are found in the `test` directory. 

To run the unit tests, execute the script `run_tests.sh`.

### Assumptions and Design Choice

The only assumption for this tool is that the input file must contain integers. The current supported distributions are:
1. Discrete Uniform Distribution.
2. Zipfian Distribution.
3. Beta Distribution.

The tool uses by default the Chi-Square test as a goodness of fit test for the discrete distributions and uses the Kolmogorov-Smirnov test for continuous distributions. 

One can plug-in any other distributions or statistical test by changing the defaults used in `estimation_tool.py`. All the distributions are defined in the `distribution.py` file and the statistical tests are defined in the `statistical_testing.py` file.

In the `distribution.py` file, there is a parent class for all the possible distributions which is called `Distribution` and there are the `ContinuousDistribution` and the `DiscreteDistribution` subclasses and each defined distribution is a subclass of either one.

In the `statistical_testing.py` file, there is a parent class for all possible goodness of fit tests which is called `GoodnessOfFitTest`. There are two subclasses of this class which are `ContinuousDistributionGoodnessOfFitTest` and `DiscreteDistributionGoodnessOfFitTest` for testing continuous distributions and discrete distributions respectively.

In addition to the above, one can define a resampling method for the data and replace the default resampler which just shuffles the data. You can find the base class for resamplers and the default resampler in the `sampling.py` file.

All generic functions and utilities are contained in the `generic_utils.py`.

The tool fits the sample data on both the supported continuous and discrete distributions and runs the chosen goodness of fit test for each class of distributions. If a distribution can't be used to fit the data or if the fit fails for some reason it is ignored. Afterwards, the best distribution is chosen among each class and the results are printed to the terminal.
