# Configuring the infrastructure with Ansible

## 1. Create the servers
All machines should be Centos 7.4+.  
Size the cluster following the table below:

Service     | Instance Name | Quantity | Volume size (GB)   | Flavor
-------     | ------------- | -------- | ----------------   | ------
MongoDB RS  | mongo-rs      | 3        | 25 (+ 100 GB\*)    | 4 CPU, 8GB
MongoDB Delayed RS  | mongo-rs      | 1        | 25 (+ 100 GB\*)    | 4 CPU, 8GB
MongoDB Arb | mongo-arb     | 1        | 25                 | 1 CPU, 1GB

\*: Additional storage needs to be created and attached before running Ansible. All secondary volumes are expected be configured as `/dev/vdb`.

## 2. Configure Ansible Inventory and Playbook
A sample `mongo.hosts.example` inventory has been provided to jumpstart the cluster. Replace all the placeholders and provide machines FQDN or IP addresses.

## 3. Running Ansible

### Using Ansible
A lot of roles contain secrets that ought to be unlocked with a "vault password". You can avoid typing it every time by writing it into a plaintext file (i.e., `vault-secret`) and passing it to Ansible during each run (SUPER RECOMMENDED).

Playbooks are run with the following syntax:
```
ansible-playbook -i <inventory>.hosts  <playbook>.yml --limit <host group> --timeout 30 --key-file ~/.ssh/<ssh key> --vault-password-file vault-secret
```

* `--vault-password-file` wants a plaintext file containing the vault password; it's an alternative to `--ask-vault-pass`;
* `--limit` flag is used to configure just a subset of the hosts (otherwise, you would reconfigure EVERYTHING each time and it would take long...).
* `--key-file` is the SSH private key to enter the instances. It must be valid for all hosts in your playbook run. It's an alternative to IPA authorized logins, common on DAF, and necessary for first runs on new servers. On IPA driven logins, provide a user with `-u` flag.
* `--timeout` sets a timeout for the single server to respond;

### Actually running the commands
```
time ansible-playbook -i mongo.hosts mongo.yml --timeout 30 --key-file ~/.ssh/vvf-staging --vault-password-file vault-secret
```

## Roles' references
Roles are documented [here](./roles.md).
