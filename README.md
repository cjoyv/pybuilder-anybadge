[![GitHub Workflow Status](https://github.com/soda480/pybuilder-anybadge/workflows/build/badge.svg)](https://github.com/soda480/pybuilder-anybadge/actions)
[![Code Coverage](https://codecov.io/gh/soda480/pybuilder-anybadge/branch/main/graph/badge.svg)](https://codecov.io/gh/soda480/pybuilder-anybadge)
[![Code Grade](https://www.code-inspector.com/project/20103/status/svg)](https://frontend.code-inspector.com/project/20103/dashboard)
[![PyPI version](https://badge.fury.io/py/pybuilder-anybadge.svg)](https://badge.fury.io/py/pybuilder-anybadge)

# pybuilder-anybadge #

A pybuilder plugin that generates badges for your project using `anybadge`, for more information refer to the [anybadge pypi page](https://pypi.org/project/anybadge/).

To add this plugin into your pybuilder project, add the following line near the top of your build.py:
```python
use_plugin('pypi:pybuilder_anybadge')
```

**NOTE** if you are using Pybuilder version `v0.11.x`, then specify the following version of the plugin:
```python
use_plugin('pypi:pybuilder_anybadge', '~=0.1.1')
```

### Pybuilder anybadge properties ###

The pybuilder task `pyb anybadge` will use anybadge to generate badges for your project by processing reports produced from various plugins; the badges that are currently supported are:
- **complexity** - requires the [pybuilder_radon](https://pypi.org/project/pybuilder-radon/) plugin. Generate badge using cyclomatic complexity score of your most complicated function.
- **severity** - requires the [pybuilder_bandit](https://pypi.org/project/pybuilder-bandit/) plugin. Generate badge using number of security vulnerabilities discovered by severity.
- **coverage** - requires the `coverage` plugin. Generate badge for overall unit test coverage.
- **python** - Generate badge for version of Python being used

The plugin will write the respective badges to the `docs/images` folder. The following plugin properties are available to further configure badge generation.

Name | Type | Default Value | Description
-- | -- | -- | --
anybadge_exclude | str | '' | Comma delimited string of badges to exclude from processing, valid values are 'complexity', 'severity', 'coverage' and 'python'
anybadge_add_to_readme | bool | False | Specify if plugin should add badges to the README.md file (see below for example). **Note** the plugin will add the badge references but you must commit/push the changes (including svg files in the docs/images folder)
anybadge_complexity_use_average | bool | False | Use overall average complexity as score when generating complexity badge

The plugin properties are set using `project.set_property`, the following is an example of how to set the properties:

```Python
project.set_property('anybadge_exclude', 'severity,coverage')
project.set_property('anybadge_add_to_readme', True)
project.set_property('anybadge_complexity_use_average', False)
```

The following badges were generated for this project using the `pybuilder_anybadge` plugin:

![coverage](https://raw.githubusercontent.com/soda480/pybuilder-anybadge/main/docs/images/coverage.svg)
![severity](https://raw.githubusercontent.com/soda480/pybuilder-anybadge/main/docs/images/severity.svg)
![complexity](https://raw.githubusercontent.com/soda480/pybuilder-anybadge/main/docs/images/complexity.svg)
![python](https://raw.githubusercontent.com/soda480/pybuilder-anybadge/main/docs/images/python.svg)

### Development ###

Clone the repository and ensure the latest version of Docker is installed on your development server.

Build the Docker image:
```sh
docker image build \
-t \
pybanybadge:latest .
```

Run the Docker container:
```sh
docker container run \
--rm \
-it \
-v $PWD:/pybuilder-anybadge \
pybanybadge:latest \
/bin/sh
```

Execute the build:
```sh
pyb -X
```