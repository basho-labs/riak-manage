# riak-manage

The riak-manage toolset is a project to manage Riak KV clusters. [Riak](http://basho.com/riak/) is an open source, distributed database that focuses on high availability, horizontal scalability, and *predictable* latency.

**Notes on this Beta project**:
* This software should **not be used in a production environment**.
* This software is **not fully compatable with all new features of Riak 1.4.x**. Please refer to the issues page for a full description of upcoming features.
* If you'd like to shepard this project into the 2.x compatibility, open an issue stating so and you will be made a maintainer!

1. [Installation](#installation)
2. [Getting Started](#getting-started)
3. [Contributing](#contributing)
5. [License and Authors](#license-and-authors)

## Installation

Requirements: 

* Bash 3.x or 4.x
* POSIX grep, sed, awk, uniq, ln, cp, logger, du
* readlink (BSD or GNU)
* Riak (for local test clusters)
* OpenSSH (for planned remote cluster management)

To install from source, clone this repository or [download and extract a
tarball](https://github.com/basho/riak-manage/tarball/master) somewhere like
/opt/riak-manage or in your home directory. Then, create a symlink to the main
riak-manage script in a directory that is in $PATH such as /usr/bin, /usr/sbin,
or /usr/local/bin.

Packages may be forthcoming. The riak-manage script also supports being placed
in the bin or sbin directory of a prefix, and the riak-manage directory in the
lib directory of that same prefix. For example:

```
/usr/sbin/riak-manage
/usr/lib/riak-manage/{commands,templates}
```

## Getting Started

To get started with riak-manage, first export RIAK_CLUSTERS to a path:
```
export RIAK_CLUSTERS=$HOME/clusters
```

Then make sure that `riak` is in your path:
```
which riak
```
Neither of the above are strictly necessary, but setting them will allow you to
call all of the following commands without needing to do any additional
configuration.


### Getting help, creating a cluster
Now, execute each of the following and get familiar with riak-manage:

```
riak-manage
riak-manage help
riak-manage help node
riak-manage node help # help accomodates argument dyslexia
```
```
# Create a cluster
riak-manage mycluster create

# Start that cluster
riak-manage mycluster riak start

# Join it together with old style commnads
riak-manage mycluster join-all

OR

# If on 1.2 or newer, you might have to give -f
riak-manage mycluster join-all -f

OR

# Join the cluster using the cluster staging commands.
# Then use the node command to look at the plan and commit it.
riak-manage mycluster cluster-join-all
riak-manage mycluster node 1 riak-admin cluster plan
riak-manage mycluster node 1 riak-admin commit
```

You can continue creating clusters and riak-manage will figure out all of the
ports for the additional clusters so that nothing conflicts. This works for up
to 20 automatically managed clusters, then you are on your own.

### Templates

Templates are just directories containing app.config, vm.args, and other
configuration files that get bootstrapped by cp and sed. Have a look at the
create command for all of the search and replace performed. To create your own
set of templates, copy the `default` directory and edit away. The `RIAK_MANAGE_`
variables will be replaced with values during create. To specify the use of a
template during create, use `--template NAME`. See `riak-manage help create`
for more.

### Writing commands

Contributing your own comands is pretty easy. An example command, called
`a-sample-cmd`, is present that shows how to get started. Just copy
`a-sample-cmd` to a file in the `commands` directory with the name
corresponding to what you wish the command to be, then implement the three
hooks as shown to perform the desired actions.

See the `node`, `riak`, and `join-all` commands for more examples.

## Contributing

Basho Labs repos survive because of community contribution. Review the details in [CONTRIBUTING.md](CONTRIBUTING.md) in order to give back to this project.

## License and Authors
The riak-manage project is Open Source software released under the Apache 2.0 License. Please see the [LICENSE](LICENSE) file for full license details.

