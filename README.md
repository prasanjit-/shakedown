# Shakedown 
DC/OS test harness with containerised version.

## Overview

*A shakedown is a period of testing or a trial journey undergone by a ship, aircraft or other craft and its crew before being declared operational.

For DCOS Shakedown testing, you can go with either the "Shakedown as Docker Container" or "Shakedown as a native Installation" option.

## Shakedown as Docker Container

- Pull the docker image -
```docker pull servergurus/dcos-shakedown```

- Run `shakedown` commands via the docker container -
```docker run -it servergurus/dcos-shakedown <command_name>```
   For example, ```docker run -it servergurus/dcos-shakedown shakedown --help```
   
```
Usage: shakedown [OPTIONS] [TESTS]...

  Shakedown is a DC/OS test-harness wrapper for the pytest tool.

Options:
  -u, --dcos-url TEXT             URL to a running DC/OS cluster.
  -f, --fail [fast|never]         Sepcify whether to continue testing when
                                  encountering failures. (default: never)
  -m, --timeout INTEGER           Seconds after which to terminate a running
                                  test
  --ssh-user TEXT                 Username for cluster ssh authentication
  -i, --ssh-key-file PATH         Path to the SSH keyfile to use for
                                  authentication.
  -q, --quiet                     Suppress all superfluous output.
  -k, --ssl-no-verify             Suppress SSL certificate verification.
  -o, --stdout [pass|fail|skip|all|none]
                                  Print the standard output of tests with the
                                  specified result. (default: fail)
  -s, --stdout-inline             Display output inline rather than after test
                                  phase completion.
  -p, --pytest-option TEXT        Options flags to pass to pytest.
  -t, --oauth-token TEXT          OAuth token to use for DC/OS authentication.
  -n, --username TEXT             Username to use for DC/OS authentication.
  -w, --password TEXT             Password to use for DC/OS authentication.
  --no-banner                     Suppress the product banner.
  --version                       Show the version and exit.
  --help                          Show this message and exit.
  ```
   
Container repo: `https://hub.docker.com/r/servergurus/dcos-shakedown/`

## Further Usage

`shakedown --dcos-url=http://dcos.example.com [options] [path_to_tests]`

- `--dcos-url` is required.
- tests within the current working directory will be auto-discovered unless specified.
- arguments can be stored in a `~/.shakedown` [TOML](https://github.com/toml-lang/toml) file (command-line takes precedence)
- `shakedown --help` is your friend.


### Running in parallel

Shakedown can be run against multiple DC/OS clusters in parallel by setting the `DCOS_CONFIG_ENV` environmental variable to a unique file, eg:

`DCOS_CONFIG_ENV='shakedown-custom-01.toml' shakedown --dcos-url=http://dcos.example.com [options] [path_to_tests]`   


## Shakedown as a native Installation 

Shakedown requires Python 3.4+.

### Installing from PyPI

The recommended Shakedown installation method is via the PyPI Python Package Index repository at [https://pypi.python.org/pypi/dcos-shakedown](https://pypi.python.org/pypi/dcos-shakedown).  To install the latest version and all required modules:

`pip3 install dcos-shakedown`

dcos-shakedown has a number of dependencies which need to be available.  One of those dependencies, the cryptography module requires a number of OS level libraries in order to install correctly which include: `build-essential libssl-dev libffi-dev python-dev`.  For environments other than linux please read [Stackoverflow](http://stackoverflow.com/questions/22073516/failed-to-install-python-cryptography-package-with-pip-and-setup-py). On a new ubuntu environment the following should install dcos-shakedown.

* `apt-get update`
* `apt-get install python3 python3-pip build-essential libssl-dev libffi-dev python-dev`
* `pip3 install dcos-shakedown`

### Development and bleeding edge

To pull and install from our `master` branch on GitHub:

```
git clone https://github.com/dcos/shakedown.git
cd shakedown
pip3 install -r requirements.txt && pip3 install -e .
```

Or if you do not wish to pin to a version of `dcos-cli`:

```
pip3 install -r requirements-edge.txt && pip3 install -e .
```

### Setting up a new Shakedown virtual environment

If you'd like to isolate your Shakedown Python environment, you can do so using the [virtualenv](https://pypi.python.org/pypi/virtualenv) tool.  To create a new virtual environment in `$HOME/shakedown`:

```
pip3 install virtualenv
virtualenv $HOME/shakedown
source $HOME/shakedown/bin/activate
pip3 install dcos-shakedown
```

This virtual environment can then be activated in new terminal sessions with:

`source $HOME/shakedown/bin/activate`


## Helper methods

Shakedown is a testing tool as well as a library.  Many helper functions are available via `from shakedown import *` in your tests.  See the [API documentation](API.md) for more information.


## License

Shakedown is licensed under the Apache License, Version 2.0.  For additional information, see the [LICENSE](LICENSE) file included at the root of this repository.
