# Inventory

In the [previous steps](targets) you've learned how to automate tasks and targets on your local machine.

In this section we'll be talking about using Droid to manage remote machines

## Use cases

Using Droid on remote machines has a couple of very interesting use-cases:

* **Application deployment**: Execute the checkout, update, and build targets on your production servers in parallel
* **Configuration management**: Use Droid to configure your servers, from a clean OS installation to a fully functional production server.

## Defining your inventory

The **inventory** is a list of hosts (servers, virtual machines, etc) and optionally groups (a list of hosts that fulfill a similar role)

### Defining hosts

You can configure your hosts in the [droid.yml](droid-yml) file.

Example:
```yaml
hosts:
    web1:
        address: web1.example.com
        auth: agent
        username: droid
        variables:
            os: debian
    "web2.example.com":
        auth: key
        variables:
            os: redhat
    
groups:
    web:
        hosts:
            - web1
            - web2.example.com
        variables:
            server: apache2
        
```

In this example, we're defining 2 hosts: `web1` and `web2.example.com`.

#### address

For each host, you can specify an `address`. If you don't specify the address (as seen for `web1`), the address will be the same as the name of the host (as seen in `web2.example.com`)

#### username

For each host you can specify the username that Droid will use to connect. If you don't specify this, the username will
be `droid`

#### auth

Droid is using ssh to connect to the remote hosts. You can configure how you'd like to authenticate. Read more about [authentication](authentication).

#### variables

You can define variables at the host level. These variables may be used in your [targets](targets) later on.

### Groups

You can define groups of hosts. In the example above, we're defining a group called `web` that contins the `web1` and `web2.example.com` hosts.

After defining a group, you can easily execute tasks in parallel on a group of servers.

Additionally you can define variables on the group that may be used in your [targets](targets) later on.


## Executing remote tasks and targets

After defining your inventory, you can now define [targets](targets) that can be executed remotely. This is done by specifying the `hosts` that the target should be executed on.

For example:

```yaml
targets:
    remotetest:
        name: "Create app directory"
        hosts: web
        tasks:
            - "fs:mkdir": path="/my-app" force=true
```

You can execute this by running:

    $ droid remotetest
    
Droid will now connect to all hosts in the `web` group (`web1` and `web2.example.com`) and execute the specified tasks
there in parallel. In this case, Droid will create the `/my-app` directory on both hosts.


## How does this work?

Droid can be packaged as a single binary phar file  (see [installation](installation)).

When you're running targets against remote hosts, Droid will use ssh to check if the droid binary is available on the remote host. It verifies if it's the same binary as you have on your local machine by validating the sha1 checksum of both files.

If there is no Droid binary on the remote host, or if the binary is different from your local binary, Droid will first upload your local version to the remote host using scp.

After this, standard ssh command execution is used to execute the commands that have been defined in your local `droid.yml` file, on the remote server, using the remote Droid binary.

The next time you run Droid, the remote version already exists, and will match the checksum of your local version, so the upload/scp command will be skipped. Once you update your local Droid binary, the next time you run a remote task, Droid will detect the change, and upload your newer binary to the remote host before executing any commands.
