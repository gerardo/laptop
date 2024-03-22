Laptop
======

Laptop is a script to set up an macOS laptop for software development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.

Requirements
------------

Supported systems:

* macOS Sonoma (14.x) on Apple Silicon and Intel
* macOS Ventura (13.x) on Apple Silicon and Intel
* macOS Monterey (12.x) on Apple Silicon and Intel

Older versions may work but aren't regularly tested.
Bug reports for older versions are welcome.

Install
-------

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/gerardo/laptop/master/mac
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```

Optionally, [install gerardo/dotfiles][dotfiles].

[dotfiles]: https://github.com/gerardo/dotfiles

Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.

What it sets up
---------------

macOS tools:

* [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

Unix tools:

* [Git] for version control
* [The Silver Searcher] for finding things in files
* [Zsh] as your shell

[Git]: https://git-scm.com/
[The Silver Searcher]: https://github.com/ggreer/the_silver_searcher
[Zsh]: http://www.zsh.org/

GitHub tools:

* [GitHub CLI] for interacting with the GitHub API

[GitHub CLI]: https://cli.github.com/

Image tools:

* [ImageMagick] for cropping and resizing images

Programming languages, package managers, and configuration:

* [Python] and [Ruby] stable for writing general-purpose code
* [Bundler] for managing Ruby libraries
* [Rosetta 2] for running tools that are not supported in Apple silicon processors

[Python]: https://www.python.org/
[Bundler]: http://bundler.io/
[ImageMagick]: http://www.imagemagick.org/
[Ruby]: https://www.ruby-lang.org/en/

And some more! Check the [mac-components/](mac-components) directory.

It should take less than 15 minutes to install (depends on your machine).

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

Contributing
------------

This is a fork of [https://github.com/thoughtbot/laptop](thoughtbot/laptop), you might want to contribute to the source project.

License
-------

Laptop is © 2011-2017 thoughtbot, inc.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE
