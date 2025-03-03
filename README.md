Wishlist
========

<p>
    <a href="https://github.com/charmbracelet/wishlist/releases"><img src="https://img.shields.io/github/release/charmbracelet/wishlist.svg" alt="Latest Release"></a>
    <a href="https://pkg.go.dev/github.com/charmbracelet/wishlist?tab=doc"><img src="https://godoc.org/github.com/golang/gddo?status.svg" alt="GoDoc"></a>
    <a href="https://github.com/charmbracelet/wishlist/actions"><img src="https://github.com/charmbracelet/wishlist/workflows/build/badge.svg" alt="Build Status"></a>
    <a href="https://nightly.link/charmbracelet/wishlist/workflows/nightly/main"><img src="https://shields.io/badge/-Nightly%20Builds-orange?logo=hackthebox&logoColor=fff&style=appveyor"/></a>
</p>

The SSH directory ✨

![screencast](https://user-images.githubusercontent.com/245435/148814423-2a3b05b4-fb79-4ff9-a475-1942d6ffb363.gif)

With Wishlist you can have a single entrypoint for multiple SSH endpoints, whether they are [Wish](https://github.com/charmbracelet/wish) apps or not.

As a server, it can be used to start multiple SSH apps within a single package and list them over SSH.
You can list apps provided elsewhere, too.

You can also use the `wishlist` CLI to list and connect to servers in your `~/.ssh/config` or a YAML config file.

## Installation

Use your fave package manager:

```bash
# macOS or Linux
brew install charmbracelet/tap/wishlist

# Arch Linux (btw)
yay -S wishlist-bin (or wishlist)

# Windows (with Scoop)
scoop install wishlist
```

Or download a pre-compiled binary or package from the [releases][releases] page.

Or just build it yourself (requires Go 1.16+):

```bash
git clone https://github.com/charmbracelet/wishlist.git
cd wishlist
go build ./cmd/wishlist/
```

[releases]: https://github.com/charmbracelet/wishlist/releases


## Usage

### CLI

#### Remote

If you just want a directory of existing servers, you can use the `wishlist` CLI and a YAML config file. You can also just run it without any arguments to list the servers in your `~/.ssh/config`.
To start wishlist in server mode, you'll need to use the `serve` subcommand:

```sh
wishlist serve
```

Check the [example config file](/_example/config.yaml) file as well as `wishlist server --help` for details.

#### Local

If you want to explore your `~/.ssh/config`, you can run wishlist in local mode with:

```sh
wishlist
```

Note that not all options are supported at this moment. Check the [commented example config](/_example/config) for reference.

### Library

Wishlist is also available as a library which allows you to start several apps within the same process.
Check out the `_example` folder for a working example.

## Auth

* if ssh agent forwarding is available, it will be used
* otherwise, each session will create a new ed25519 key and use it, in which case your app will be to allow access to any public key
* password-based auth is not supported

### Agent forwarding example

```sh
eval (ssh-agent)
ssh-add -k # adds all your pubkeys
ssh-add -l # should list the added keys

ssh \
  -o 'ForwardAgent=yes' \             # forwards the agent
  -o 'UserKnownHostsFile=/dev/null' \ # do not add to ~/.ssh/known_hosts, optional
  -p 2222 \                           # port
  foo.bar \                           # host
  -t list                             # optional, app name
```

You can also add this to your `~/.ssh/config`, for instance:

```sshconfig
Host wishlist
	HostName foo.bar
	Port 2222
	ForwardAgent yes
	UserKnownHostsFile /dev/null
```

## Running it

Wishlist will read and store all its information in a `.wishlist` folder in the current working directory:

- the server keys
- the client keys
- known hosts
- config files

Config files may be provided in either YAML or SSH Config formats:

- [example YAML](/_example/config.yaml)
- [example SSH config](/_example/config)

The config files are tried in the following order:

- the `-config` flag in either YAML or SSH config formats
- `.wishlist/config.yaml`
- `.wishlist/config.yml`
- `$HOME/.ssh/config`
- `/etc/ssh/ssh_config`

The first one that is loaded and parsed without errors will be used.
This means that if you have your common used hosts in your `~/.ssh/config`, you can simply run `wishlist` and get it running right away.
It also means that if you don't want that, you can pass a path to `-config`, and it can be either a YAML or a SSH config file.

### Using the binary

```sh
wishlist
```

### Using Docker

```sh
mkdir .wishlist
$EDITOR .wishlist/config.yaml # either an YAML or a SSH config
docker run \
  -p 2222:22 \
  -v $PWD/.wishlist:/.wishlist \
  docker.io/charmcli/wishlist:latest
```

## Acknowledgments

The gif above shows a lot of [Maas Lalani’s](https://github.com/maaslalani) [confeTTY](https://github.com/maaslalani/confetty).

## Feedback

We'd love to hear your thoughts on this project. Feel free to drop us a note!

* [Twitter](https://twitter.com/charmcli)
* [The Fediverse](https://mastodon.technology/@charm)
* [Slack](https://charm.sh/slack)

## License

[MIT](/LICENSE)

***

Part of [Charm](https://charm.sh).

<a href="https://charm.sh/"><img alt="The Charm logo" src="https://stuff.charm.sh/charm-badge.jpg" width="400"></a>

Charm热爱开源 • Charm loves open source
