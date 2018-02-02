Install
=======

Download [archive](https://golang.org/dl/) & extract to `/usr/local`, creating
a Go tree in `/usr/local/go`:

    $ tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz

Add the environment variable to user profile or system-wide:

    export PATH=$PATH:/usr/local/go/bin


Installing to a custom location
-------------------------------

The Go binary distributions assume they will be installed in `/usr/local/go`,
but it is possible to install the Go tools to a different location. In this
case you must set the GOROOT environment variable to point to the directory in
which it was installed:

    export GOROOT=$HOME/go1.X
    export PATH=$PATH:$GOROOT/bin


Workspaces
----------

A workspace is a directory hierarchy with three directories at its root:

- `src` contains Go source files,
- `pkg` contains package objects, and
- `bin` contains executable commands.

The `GOPATH` environment variable specifies the location of workspace:

    export GOPATH=$HOME/go

To print the effective current `GOPATH` or the default location if the variable
is unset:

    $ go env GOPATH

For convenience, add the workspace's bin subdirectory to your PATH:

    export PATH=$PATH:$(go env GOPATH)/bin

More info about `GOPATH`:

    $ go help gopath

