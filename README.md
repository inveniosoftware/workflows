# Reusable GitHub Actions Workflows

This repository contains reusable GitHub Actions workflows that can be used in any repository.

## Workflows

### Python tests

This workflow will:

1. Checkout the repository at the commit the workflow is running on.
2. Set up Python.
3. (Optional) Run any pre-install steps (see [below](#pre-install)).
4. Install the package with the specified extras.
5. Run the tests using the `./run-tests.sh` script in the repository.

#### Inputs

| Name             | Description                   | Default            | Example                                      |
|------------------|-------------------------------|--------------------|----------------------------------------------|
| `python-version` | Python versions to test       | `["3.9", "3.12"]`  | `["3.10", "3.11"]`                           |
| `extras`         | Extra dependencies to install | `tests`            | `tests,oaipmh`                               |
| `db-service`     | Database service to use       | `["postgresql14"]` | `["postgresql14", "postgresql15", "mysql8"]` |
| `search-service` | Search service to use         | `["opensearch2"]`  | `["elasticsearch7", "opensearch2"]`          |

To use in your repository, add the following in your `.github/workflows/tests.yml` file under the `jobs` key:

```yaml
# ...

jobs:

  Python:
    uses: inveniosoftware/workflows/.github/workflows/tests-python.yml@master
    # If you want to override some of the inputs:
    # with:
    #   extras: "tests,oaipmh"
```

#### Pre-install

In case you want to run additional steps before Python dependencies are install, you
can create a `.github/actions/pre-install/actions.yml` file in your repository. This
file should contain a list of steps to run before the dependencies are installed.

For example if you need to install `graphviz`, you can create the following file:

```yaml
# .github/actions/pre-install/actions.yml
runs:
  using: composite
  steps:
    - uses: ts-graphviz/setup-graphviz@v1
```

### JS tests

Inputs:

| Name                             | Description                                                          | Default            | Example                                                           |
|----------------------------------|----------------------------------------------------------------------|--------------------|-------------------------------------------------------------------|
| `node-version`                   | Node versions to test                                                | `["18.x", "20.x"]` | `["20.x", "22.x"]`                                                |
| `js-working-directory`           | Working directory for JS tests. If unset, not tests will run         | Unset              | `./invenio_foobar/assets/semantic-ui/js/invenio_foobar`           |
| `translations-working-directory` | Working directory for translations tests. If unset no tests will run | Unset              | `./invenio_foobar/assets/semantic-ui/translations/invenio_foobar` |


To use in your repository, add the following in your `.github/workflows/tests.yml` file under the `jobs` key:

```yaml
# ...

jobs:

  JS:
    uses: inveniosoftware/workflows/.github/workflows/tests-js.yml@master
    with:
      js-working-directory: "./invenio_foobar/assets/semantic-ui/js/invenio_foobar"
      # If you you don't have translations to test, just skip this line
      translations-working-directory: "./invenio_foobar/assets/semantic-ui/translations/invenio_foobar"
```
