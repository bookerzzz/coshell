# coshell v0.1.2

A no-frills dependency-free replacement for GNU parallel, perfect for initramfs usage.

Licensed under GNU/GPL v2.

# How it works

An ``sh -c ...`` command is started for each of the input commands; environment and current working directory are preserved.
**NOTE:** file descriptors are not

All commands will be executed, no matter which one fails.
Return value will be the sum of exit values of each command.

# Self-contained

    $ ldd coshell
    	not a dynamic executable

Thanks to [Go language](https://golang.org/), this is a self-contained executable thus a perfect match for inclusion in an initramfs or any other project where you would prefer not to have too many dependencies.

# Installation

Once you run:

    go get github.com/gdm85/coshell

The binary will be available in your ``$GOPATH/bin``; alternatively, build it with:

    go build

Then copy the ``coshell`` binary to your PATH, ``~/bin``, ``/usr/local/bin`` or any of your option.

# Usage

Specify each command on a single line as standard input.

Example:

    echo -e "echo test1\necho test2\necho test3" | coshell

Output:

    test3
    test1
    test2

## deinterlace option

Order is not deterministic by default, but with option ``--deinterlace`` or ``-d`` all output will be buffered and afterwards
printed in the same chronological order as process termination.

## halt-all option

If `--halt-all` or `-a` option is specified then first process to terminate unsuccessfully (with non-zero exit code) will cause a
signal to be broadcast to all other processes; signal can be defined with `--signal` or `-s`.

## signal option

In case the halt-all option is enabled, the signal specified through `--signal` or `-s` will be used to terminate the other processes; ignored otherwise.

Valid values are specified in [cosh/signal.go](./cosh/signal.go).

## Other examples

See [examples/](examples/) directory.
