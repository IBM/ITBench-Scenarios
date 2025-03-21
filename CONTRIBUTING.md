# Contributing to ITBench-Scenarios

ITBench-Scenarios accepts contributions through GitHub pull request. All commits in must be signed. A commit can be signed by using the following command:

```shell
git commit -m "my awesome commit" -s
```

or

```shell
cz commit -- --signoff
```

## Required Software

- [Python3](https://www.python.org/downloads/) (v3.12.z)

## Environment Set Up

1. Create a Python virtual environment.

```shell
python -m venv venv
```

2. Download the `detect-secrets`, `pre-commit`, tools

```shell
python -m pip install -r requirements-dev.txt
```

3. Install `pre-commit` to the repo. This only needs to be done once.

```shell
pre-commit install
```

4. Follow the instructions listed in the `README.md` of the domain being contributed to.
