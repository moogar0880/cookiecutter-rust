# {{cookiecutter.project_name}}

{{cookiecutter.project_short_description}}

[![Build Status](https://travis-ci.org/{{cookiecutter.github_username}}/{{cookiecutter.project_name}}.svg?branch=master)](https://travis-ci.org/{{cookiecutter.github_username}}/{{cookiecutter.project_name}})
[![crates.io](https://img.shields.io/crates/v/{{cookiecutter.project_name}}.svg)](https://crates.io/crates/{{cookiecutter.project_name}})

## Getting started

This project requires Go to be installed. On OS X with Homebrew you can just run `brew install rust`.

Running it then should be as simple as:

```console
$ make
$ ./bin/{{cookiecutter.project_name}}
```

### Testing

``make test``
