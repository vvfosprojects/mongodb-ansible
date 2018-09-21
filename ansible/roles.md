# Roles
Some roles come with default variables. Take a look at `staging.hosts` for working examples of how these are used in practice.

## base
Installs the base packages for each vm and hardens the OS. It also sets a default root password for the host.
Sets up dns search domain and preferred nameserver.

## mongodb
Configures a single server or a cluster with MongoDB.

##### Vars
* `replicaset_enabled`: set to `true` to enable cluster mode when you want install multi-host MongoDB (ReplicaSet)
* `optimize_OS_mongodb`: set to `true` to enable system optimization for MongoDB instances
* `replicaset_name`: set the name of replicaset. es: 'gl_rs_01'

## second_disk
Handy role to configure and mount a second drive, since this is the most common case.

##### Vars
* `var_name`: the second disk to manage (default: `vdb`).
* `mount_point`: the directory to mount. Please note that the roles manages existing data, by temporarily mounting the disk in a temporary location, copying existing data and remounting the disk in the final point (default: `/var/lib/docker`).
* `use_lvm`: wheter this disk should be managed by LVM (default: `false`).
