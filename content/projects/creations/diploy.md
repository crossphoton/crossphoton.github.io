---
title: Diploy
date: 2021-08-21T05:55:05.034Z
link: https://github.com/crossphoton/diploy
image: https://opengraph.githubassets.com/1d3473d1156be0f85d280e243b2e46333f0968778c7ec3b7e45d5626590c4f0d/crossphoton/diploy
description: Deployment utility for linux servers.
tags:
  - linux
  - webhooks
  - deployment
  - sqlite
  - golang
  - cobra
fact: Made out of personal frustration xD.
weight: 2
sitemap:
  weight: 0
---
This is a utility to manage deployments on a linux server \[Might be compatible with other OSs too, didn't test]. It provides an http server for webhooks and CLI for management.

### GITHUB README BELOW

## DIPLOY

A utility to manage your linux deployments.

## How does it work

- You start a server using `diploy server`
- **diploy** exposes a http server for instructions [take care of *firewall* !!]
- You create a `diploy.yml` with below format file and run `diploy add` in the same directory.
- Now you get endpoits for each configuration you add with the below format.
- Use these as webhooks with Github... or just manually.
- Or just use the CLI for manual work.

## diploy.yml

```
name: <Application Name>    // These should be unique across installation
update:                     // Specify command to update
                            // codebase (optional [default `git pull`])
  command: git pull         // Command/Script to run to update
  type: command             // Whether a script (run using /bin/sh) or command
build:                      // To build stuff
  command: echo This is build
  type: command
run:                        // To start the application
  command: echo This is run
  type: command
```
## Installation
- Use the command `curl -s https://raw.githubusercontent.com/crossphoton/diploy/main/scripts/install.sh | /bin/sh` (This starts the setup too)
- Use `go get github.com/crossphoton/diploy` and build it
- Clone the repository (or [download ZIP](https://github.com/crossphoton/diploy/archive/refs/heads/main.zip))

## endpoints
All requests are POST requests.

For now there is no authentication in this. (See TODO)

#### Start processes
start:
  - update codebase: `/start/update/{name}`
  - build application: `/start/build/{name}`
  - run application: `/start/run/{name}`

#### Stop processes for a given application
stop:
    `/stop/{name}`
  
#### Run a sequence of operations
  `/seq/<a>+<b>+<c>/{name}`

##### Examples:
- `/seq/stop+update+build+run/golang-app`
- `/seq/restart/nodejs-app`
- `/seq/update+restart/website`


### CLI Usage

```
Usage:
  diploy [command]

Available Commands:
  add         add a configuration
  help        Help about any command
  remove      remove a configuaration by name
  server      Start diploy server
  start       start a service with name
  stop        stop a service with name

Flags:
  -h, --help          help for diploy
      --logs string   specify logs location (default "./diploy")

Use "diploy [command] --help" or "diploy help [command]" for more information about a command.
```

**Extra:** Use `diploy server setup` to setup a systemd file.

### Caveats
- Processes started with **diploy** will also stop if diploy is stopped.

### Cleanup
You can use the script if defaults were used during setup

`curl -s https://raw.githubusercontent.com/crossphoton/diploy/main/scripts/cleanup.sh | /bin/sh`
### Troubleshooting
- If problems with database: Specify logs location using `--logs` flag
- If problems with connection: Specify server address using `--addr` flag
### Todo
See the dedicated [TODO](./TODO.md) file.

### Stuff Used

- gorm + sqlite
- gorilla/mux
- cobra
- go-yaml