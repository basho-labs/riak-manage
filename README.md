# riak-manage

A tool for managing Riak clusters.

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


## Getting help, creating a cluster
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

## Templates

Templates are just directories containing app.config, vm.args, and other
configuration files that get bootstrapped by cp and sed. Have a look at the
create command for all of the search and replace performed. To create your own
set of templates, copy the `default` directory and edit away. The `RIAK_MANAGE_`
variables will be replaced with values during create. To specify the use of a
template during create, use `--template NAME`. See `riak-manage help create`
for more.

## Writing commands

Contributing your own comands is pretty easy. An example command, called
`a-sample-cmd`, is present that shows how to get started. Just copy
`a-sample-cmd` to a file in the `commands` directory with the name
corresponding to what you wish the command to be, then implement the three
hooks as shown to perform the desired actions.

See the `node`, `riak`, and `join-all` commands for more examples.

