# pyenv Chef Cookbook

[![CircleCI](https://circleci.com/gh/darwin67/chef-pyenv.svg?style=svg)](https://circleci.com/gh/darwin67/chef-pyenv)
[![Chef Version](https://img.shields.io/cookbook/v/pyenv.svg?style=flat-square)](https://supermarket.chef.io/cookbooks/pyenv)
[![Maintainability](https://api.codeclimate.com/v1/badges/693934e931aa1c52bfa0/maintainability)](https://codeclimate.com/github/darwin67/chef-pyenv/maintainability)

## Description

Manages [pyenv][pyenv] and its installed Pythons.

Several custom resources are defined to facilitate this.

**WARNING** As of `v1.0.0`, this cookbook no longer provide any recipes. Custom resources are provided instead.

**NOTE** The following distros have [known issues][openssl-issues] regarding building python `3.7` which are all related to the OpenSSL library versions.

* RHEL6
* Debian 8

[openssl-issue]: https://github.com/pyenv/pyenv/wiki/Common-build-problems#error-the-python-ssl-extension-was-not-compiled-missing-the-openssl-lib

## Requirements

### Chef

This cookbook requires Chef 14.0+.

### Platform family

* Debian derivatives (debian, ubuntu)
* Fedora
* RHEL derivatives (RHEL, CentOS, Amazon Linux, Oracle, Scientific Linux)
* openSUSE and openSUSE leap

## Usage

Examples installtions are provided in `test/fixtures/cookbooks/test/recipes`

A `pyenv_system_install` or `pyenv_user_install` is required to be set so that pyenv knows which version you want to use, and is installed on the system.

## Pip

Used to install a Python package into the selected pyenv environment.

```ruby
pyenv_pip 'requests' do
  virtualenv  # Optional: if passed, pip inside provided virtualenv would be used (by default system's pip)
  version     # Optional: if passed, the version the python package to install
  user        # Optional: if passed, the user to install the python module for
  options     # Optional: if passed, pip would install/uninstall packages with given options
  requirement # Optional: if true passed, install/uninstall requirements file passed with name property
  editable    # Optional: if true passed, install package in editable mode
end
```

## Global

```ruby
pyenv_global '3.6.1' do
  user # Optional: if passed sets the users global version. Do not set, to set the systems global version
end
```

If a user is passed in to this resource it sets the global version for the user, under the users root_path (usually `~/.pyenv/version`), otherwise it sets the system global version.

## Plugin

Installs a pyenv plugin.

```ruby
pyenv_plugin 'virtualenv' do
  git_url     # Git URL of the plugin
  git_ref     # Git reference of the plugin
  environment # Optional: pass environment variables to git resource
  user        # Optional: if passed installs to the users pyenv. Do not set, to set installs to the system pyenv.
end
```

## Rehash

```ruby
pyenv_rehash 'rehash' do
  user # Optional: if passed rehashes the user pyenv otherwise rehashes the system pyenv
end
```

## Python

```ruby
pyenv_python '3.6.1' do
  user         # Optional: if passed, the user pyenv to install to
  environment  # Optional: pass environment variable to git resource
  pyenv_action # Optional: the action to perform, install, remove etc
  verbose      # Optional: print verbose output during python installation
end
```

Shorter example `pyenv_python '3.6.1'.`

## Script

Runs a pyenv aware script.

```ruby
pyenv_script 'foo' do
  code          # Script code to run
  pyenv_version # pyenv version to run the script against
  environment   # Optional: Environment to setup to run the script
  user          # Optional: User to run as
  group         # Optional: Group to run as
  path          # Optional: Path to search for commands
  returns       # Optional: Expected return code
end
```

## System install

Installs pyenv to the system location, by default `/usr/local/pyenv`

```ruby
pyenv_system_install 'foo' do
  git_url      # URL of the plugin repo you want to checkout
  git_ref      # Optional: Git reference to checkout
  environment  # Optional: pass environment variable during pyenv installation
  update_pyenv # Optional: Keeps the git repo up to date
end
```

## User install

Installs pyenv to the user path, making pyenv available to that user only.

```ruby
pyenv_user_install 'vagrant' do
  git_url     # Optional: Git URL to checkout pyenv from.
  git_ref     # Optional: Git reference to checkout e.g. 'master'
  environment # Optional: pass environment variable during pyenv installation
  user        # Which user to install pyenv to (also specified in the resources name above)
end
```

## System-Wide Mac Installation Note

This cookbook takes advantage of managing profile fragments in an
`/etc/profile.d` directory, common on most Unix-flavored platforms.
Unfortunately, Mac OS X does not support this idiom out of the box,
so you may need to [modify][mac_profile_d] your user profile.

## Development

* Source hosted at [GitHub][repo]
* Report issues/Questions/Feature requests on [GitHub Issues][issues]

Pull requests are very welcome! Make sure your patches are well tested.

## License and Author

* Copyright 2014, [Shane da Silva][sds]
* Copyright 2017, [Darwin D. Wu][darwin]

```
http://www.apache.org/licenses/LICENSE-2.0
```

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

[pyenv]: https://github.com/yyuu/pyenv
[mac_profile_d]: http://hints.macworld.com/article.php?story=20011221192012445
[repo]: https://github.com/darwin67/chef-pyenv
[issues]: https://github.com/darwin67/chef-pyenv/issues

[sds]: https://github.com/sds
[darwin]: https://github.com/darwin67
