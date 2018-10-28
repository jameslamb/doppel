# doppel

`doppel` is an integration testing framework for testing API similarity across languages.

# R example

Test an R package

```{shell}
PKG=
./analyze.R --pkg rsixygen --output_dir $(pwd)
cat rsixygen.json | jq .
```

Test a python package

```{shell}
PKG=feather
./analyze.py --pkg ${PKG} --output_dir $(pwd)
cat python_${PKG}.json | jq .
```

# Usage in CI

The example below describes how to use this project in your CI environment (e.g. Travis, Jenkins, Appveyor).

```{shell}
mkdir -p $(pwd)/test_data
./analyze.R --pkg argparse --output_dir $(pwd)/test_data
./analyze.py --pkg argparse --output_dir $(pwd)/test_data
```


```{shell}
doppel \
    --verbose \
    -pkgs "optparse[r],optparse[python]"
```

# Design Principles

A `test_failure` always results in a non-zero exit code.

# What is the value of keeping the same public interface?

This project tests API consistency in libraries across languages.

Why is this valuable?

* For developers:
    * less communication overhead implementing changes across languages
    * forcing function to limit growth in complexity of the public API
* For users:
    * no need to re-learn the API when switching languages
    * form better expectations when switching languages

# Ideas

* Degree of similarity should be configurable
    * should be able to say "only functions"
    * should be able to say "only classes"
    * should be able to whitelist "extras" and have them not count against the final test
* Handling of language-specific features
    * constructor names
    * kwargs
    * active bindings (R)
    * magic methods (Python)
    * S3 methods (R)
* should handle within-package inheritance in classes
* should handle cross-package inheritance with classes
* should have its own reliable CI (this will be fun to set up...)
* checks:
    * differing number of classes
    * differing number of functions
    * 'specific_function' exists in one but not other
    * 'specific class' exists in one but not other
    * differing number of public methods for 'specific_class'
    * 'specific_method' exists in one but not other
    * differing number of public members for class in one but not other
    * 'specific_member' exists in one but not other
    * different number of args for a particular function or method
    * 'specific_arg' is in the signature of 'specific_function_or_method' in one but not the other

# References

1. [Writing Command-Line tools with Click](https://dbader.org/blog/python-commandline-tools-with-click)
2. [Python entrypoints explained](https://amir.rachum.com/blog/2017/07/28/python-entry-points/)