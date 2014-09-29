Forever
=======

Ansible role to install [Forever](https://github.com/nodejitsu/forever) and and configure
[Upstart](http://upstart.ubuntu.com/) to ensure that the given [Node.js](http://www.nodejs.org/) scripts runs
continuously.

Requirements
------------

- Node.js and npm
- Upstart

Role Variables
--------------

- **forever_node_bin_dir**: The full path to the directory containing the node and forever binaries. Defaults to
  `"/usr/local/bin"`.
- **forvever_node_path**: Set the full path to the Node.js main node_modules directory. Defaults to
  `"/usr/local/lib/node_modules"`.
- **forever_applications**: A list of Node.JS application configurations, each configuration is a hash with the
  following properties:
    - **name**: The application name (must be a valid filename).
    - **file**: The application startup Javascript file path.
    - **pid_file**: Process ID file path. Defaults to `/var/run/{{name}}.pid`.
    - **log_file**: Log file path. Defaults to `/var/log/{{name}}.log`.
    - **min_uptime**: Minimum uptime, in milliseconds. Defaults to `5000`.
    - **spin_sleep_time**: Spin sleep time, in milliseconds. Defaults to `2000`.
    - **enabled**:  Whether or not the application should be enabled. Defaults to true.

Example Playbook
-------------------------

    - hosts: servers
      roles:
         - role: forever
           forever_applications:
           - name: foo-server
             file: "/home/node/foo/server.js"
           - name: bar-daemon
             file: "/home/node/bar/app.js"
             min_uptime: 8000

License
-------

Apache v2

Author Information
------------------

- vevmesteren <https://github.com/vevmesteren>
- Pierre Paul Lefebvre <https://github.com/PierrePaul>
- Pierre Buyle <buyle@pheromone.ca>
