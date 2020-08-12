# Python-Amazon-MWS

Python-Amazon-MWS is a Python connector to [Amazon Marketplace Web Services][2]
(or MWS). It provides a simple way to build and send requests to MWS,
allowing access to all that MWS can do from your Python application.

**Join us on [Slack][1]!**

## Metrics

| Branch    | Coverage                                                                                                                                                                         | Testing                                                                                                | Version                                                                           |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| `master`  | [![codecov](https://codecov.io/gh/python-amazon-mws/python-amazon-mws/branch/master/graph/badge.svg)](https://codecov.io/gh/python-amazon-mws/python-amazon-mws/branch/master)   | N/A                                                                                                    | [![PyPI version](https://badge.fury.io/py/mws.svg)](https://badge.fury.io/py/mws) |
| `develop` | [![codecov](https://codecov.io/gh/python-amazon-mws/python-amazon-mws/branch/develop/graph/badge.svg)](https://codecov.io/gh/python-amazon-mws/python-amazon-mws/branch/develop) | ![CI Testing](https://github.com/python-amazon-mws/python-amazon-mws/workflows/CI%20Testing/badge.svg) | 1.0.0-dev (GitHub install required)                                               |

## Installation

Two versions are currently available:

- Installing `mws` from PyPI, you will have version **0.8.x**, which is built from our `master` branch.
  - This is a close match to the original package by czpython, with some small tweaks to add critical functionality.
  - Supports Python 2.7 and 3.4+.
- The updated **1.0.0-dev** version must be installed from this repo's `develop` branch.
  - This includes additional API coverage that may be missing in 0.8.x, as well as other new features.
  - Some methods have new or updated arguments compared to 0.8.x, and much of the original monolithic `mws` module has been broken down into separate components (such as the `mws.apis` collection of modules).
  - Supports Python 3.5+.

### Installing 0.8.x (PyPI)

> **Warning**: If you are using version 0.8.x in a production system, note that our eventual 1.0 release will be backwards-incompatible, and may break programs that depend on the 0.8.x version. We advise users pin their Pip-installed version in requirements as `mws~=0.8.9`.

Install the `mws` package using Pip:

```shell
pip install mws
```

Alternatively, you can install direct from this repo's `master` branch, like so:

```shell
pip install git+https://github.com/python-amazon-mws/python-amazon-mws.git@master#egg=mws
```

### Installing 1.0.x-dev (GitHub)

Our `develop` version can be installed directly from the repo using:

```shell
pip install git+https://github.com/python-amazon-mws/python-amazon-mws.git@develop#egg=mws
```

Note that code may be updated at any time as development continues, so please use at your own risk.

## Quickstart

Export your API credentials as environment variables in your shell.

```shell
export MWS_ACCOUNT_ID=...
export MWS_ACCESS_KEY=...
export MWS_SECRET_KEY=...
```

Now you can experiment with the API from within an interactive Python shell e.g.

```python
>>> import mws, os
>>> orders_api = mws.Orders(
...     access_key=os.environ['MWS_ACCESS_KEY'],
...     secret_key=os.environ['MWS_SECRET_KEY'],
...     account_id=os.environ['MWS_ACCOUNT_ID'],
...     region='UK',   # defaults to 'US'
... )
>>> service_status = orders_api.get_service_status()
>>> service_status
<mws.mws.DictWrapper object at 0x1063a2160>
>>> service_status.original
'<?xml version="1.0"?>\n<GetServiceStatusResponse xmlns="https://mws.amazonservices.com/Orders/2013-09-01">\n  <GetServiceStatusResult>\n    <Status>GREEN</Status>\n    <Timestamp>2017-06-14T16:39:12.765Z</Timestamp>\n  </GetServiceStatusResult>\n  <ResponseMetadata>\n    <RequestId>affdec68-05d2-4bc5-a8a4-bb40f307dd6b</RequestId>\n  </ResponseMetadata>\n</
GetServiceStatusResponse>\n'
>>> service_status.parsed
{'value': '\n    ', 'Status': {'value': 'GREEN'}, 'Timestamp': {'value': '2017-06-14T16:39:12.765Z'}}
>>> service_status.response
<Response [200]>
```

## Development

All dependencies for developing on `python-amazon-mws`, including testing and documentation building, can be installed using:

```shell
pip install -r requirements-dev.txt
```

### Tests

Tests are run with `pytest`. To run tests, simply install our dev requirements and then run:

```shell
pytest
```

See [pytest docs](https://docs.pytest.org/en/latest/usage.html#specifying-tests-selecting-tests)
for details on selecting specific tests, rather than the entire test suite, as needed.

We also perform coverage reporting using `pytest-cov`. You can generate a coverage report locally using:

```shell
pytest --cov=mws
```

The test suite and coverage reporting to Codecov are run automatically in the repo on pushes and pull requests, using GitHub Actions workflows. We test on latest versions of Python 3.5+, and on latest Ubuntu, Mac, and Windows OSes.

### Documentation

Docs are built using Sphinx.

To build docs locally, use `make`:

```shell
make html
```

The output HTML documentation will be in `docs/build/`.

To run a live reloading server serving the HTML documentation (on `localhost:8000` or `127.0.0.1:8000` by default):

```shell
make livehtml
```

#### On Windows

`make` may not be available on Windows. To build the docs locally, instead use `sphinx-build`:

```shell
sphinx-build -b source build
```

You can also run a live-reloading server using `sphinx-autobuild` (on `localhost:8000` or `127.0.0.1:8000` by default):

```shell
sphinx-autobuild source build
```

### Contributing

Please make pull requests to `develop`. Code coverage isn't necessary but encouraged where possible (especially for anything which might behave differently between Python 2/3).

## Support

For support using the package, please [join our Slack][1] and post in the `#help` channel.

For support using MWS itself, we advise using the [MWS documentation][2]

[1]: https://join.slack.com/t/pythonamazonmws/shared_invite/enQtOTcwNTAzNjI4OTc2LTQyMzk1YzIxNTU0MmE1MWE0ZDUzZjBhMjI2ODZhNTQ5Mjk3ZTUyOGFkODk1N2Q2NjczZjY2M2U3NzAzNDU4ZTc
[2]: http://docs.developer.amazonservices.com/en_US/dev_guide/index.html
[3]: https://github.com/czpython/python-amazon-mws
