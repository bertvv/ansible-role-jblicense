# Ansible role `jblicense`

An Ansible role for setting up a [JetBrains License Server](https://www.jetbrains.com/license_server/). Specifically, the responsibilities of this role are to:

- Install the Java JRE
- Install the JetBrains License Server
- Create a systemd service configuration file and enable the service
- Configure firewalld

## Requirements

You need to download a [Java JRE/JDK installer](http://www.oracle.com/technetwork/java/javase/downloads/index.html) first and place it in the `files/` folder. Adapt the variables `jblicense_jre*` according to the version you're installing (see Role Variables below).

## Role Variables

| Variable                  | Default                          | Comments (type)                                                              |
| :---                      | :---                             | :---                                                                         |
| `jblicense_host_name`     | jblicense.localdomain            | The host name of the license server. It should resolve to an IP address.     |
| `jblicense_jre_package`   | server-jre-8u60-linux-x64.tar.gz | The name of the Java JRE installation package.                               |
| `jblicense_jre_version`   | 1.8.0_60                         | The version of the Java JRE                                                  |
| `jblicense_listen_port`   | 1111                             | The port number on which to listen.                                          |
| `jblicense_local_subnets` | []                               | A list of subnets (in CIDR notation) that are considered "local". See below. |

When `jblicense_local_subnets` is set, only hosts from these subnets are allowed to request a license from the server. If not, *all* hosts are allowed. In the latter case, make sure your license server is not accessible over the public Internet!

## Dependencies

No dependencies.

## Example Playbook

See the [test playbook](tests/test.yml)

## Testing

The `tests/` directory contains tests for this role in the form of a Vagrant environment. After running `vagrant up`, you should be able to access the license server from your web browser at "http://192.168.56.11:1111". It will show the registration page.

The directory `tests/roles/jblicense` is a symbolic link that should point to the root of this project in order to work. To create it, do

```ShellSession
$ cd tests/
$ mkdir roles
$ ln -frs ../../PROJECT_DIR roles/jblicense
```

You may want to change the base box into one that you like. The current one is based on Box-Cutter's [CentOS Packer template](https://github.com/boxcutter/centos).

The playbook [`test.yml`](tests/test.yml) applies the role to a VM, setting role variables.

## Contributing

Issues, feature requests, ideas are appreciated and can be posted in the Issues section. Pull requests are also very welcome. Preferably, create a topic branch and when submitting, squash your commits into one (with a descriptive message).

## License

BSD

## Author Information

Bert Van Vreckem (bert.vanvreckem@gmail.com)

