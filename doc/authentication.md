# Authentication

When using Droid with [inventories](inventory), you can specify different ways of authentication when connecting to remote hosts.

Connections are established over standard SSH. You can use the following methods of authentication


## Username/Password

While not ideal, you can manage remote hosts that only support password-based authentication. You can configure password-based authentication in your [droid.yml](droid-yml) file as follows:

```yml
hosts:
    web1:
        address: web1.example.com
        auth: password
        username: root
        password: secret
```

## SSH keys

You can also authenticate using standard SSH keys. You can configure key-based authentication in your [droid.yml](droid-yml) file as follows:

```yml
hosts:
    web1:
        address: web1.example.com
        auth: key
        username: joe
        keyfile: /home/joe/.ssh/id_rsa
        keypass: othersecret
```

The `keyfile` is an absolute or relative path to a private key.
The `keypass` only needs to be specified in case the private key is encrypted using a passphrase.

## SSH Agent

The simplest way of authentication is by using ssh-agent. This way all the keys that are available in the current session can be used for authentication automatically. If you have not already done so, you can add a key to a running ssh-agent by running:

    ssh-add ~/.ssh/my_private_key

You can configure agent based authentication in your [droid.yml](droid-yml) file as follows:

```yml
hosts:
    web1:
        address: web1.example.com
        auth: agent
        username: joe
```
