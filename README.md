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

- **forever_root**: The full path to Forever working directory. Defaults to `"/var/run/forever"`.
- **forever_user**: The username of the user to run Forever managed process as. Defaults to `"root"`.
- **forever_env**: Environment variables for the Upstart jobs, as a hash. Defaults to `{}`.
- **forever_node_bin_dir**: The full path to the directory containing the node and forever binaries. Defaults to
  `"/usr/local/bin"`.
- **forever_node_path**: Set the full path to the Node.js main node_modules directory. Defaults to
  `"/usr/local/lib/node_modules"`.
- **forever_applications**: A list of Node.JS application configurations, each configuration is a hash with the
  following properties:
    - **name**: The application name (must be a valid filename).
    - **file**: The application startup Javascript file path.
    - **pid_file**: Process ID file path. Defaults to `/var/run/{{name}}.pid`.
    - **log_file**: Log file path. Defaults to `/var/log/{{name}}.log`.
    - **min_uptime**: Minimum uptime, in milliseconds. Defaults to `5000`.
    - **spin_sleep_time**: Spin sleep time, in milliseconds. Defaults to `2000`.
    - **enabled**:  Whether or not the application should be enabled. Defaults to `true`.
    - **env**: Environment variables for the Upstart Job, as a hash. Defaults to `{}`.
    - **command**: The command to use to run the application file. Defaults to `node`.
- **forever_start_on**: The upstart event to start Forever managed processes at. Defaults to `"startup"`.
- **forever_stop_on**: The upstart event to stop Forever managed processes at. Defaults to `"startup"`.

Example Playbook
-------------------------

    - hosts: servers
      roles:
         - name: forever
           # Start forever process once Vagrant sync. folders are mounted.
           forever_start_on: vagrant-mounted
           # Set NOD_ENV environment for all Upstart managed processes.
           forever_env:
             NODE_ENV: production
           forever_applications:
           # A simple Node server, running on a fixed port.
           - name: foo-server
             file: "/home/node/foo/server.js"
           # A server requiring ES6 features (aka. Harmony), running on a configurable port.           
           - name koa-server
             file "/home/node/koaapp/server.js"
             command: node --harmony
             env:
               BIND_PORT: 8081

License
-------

Apache v2

Author Information
------------------

- vevmesteren <https://github.com/vevmesteren>
- Pierre Paul Lefebvre <https://github.com/PierrePaul>
- Pierre Buyle <buyle@pheromone.ca>
