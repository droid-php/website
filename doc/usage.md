# Using Droid

Cool, you are ready to try out Droid? We're sure you're going to love it.

We suggest you check out the [installation](/installation) page first.

## List the available commands

```sh
$ droid list
Droid version 1.0.0

Usage:
  command [options] [arguments]

Options:
  -h, --help            Display this help message
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  config            Show project configuration
  help              Displays help for a command
  inventory         Show inventory configuration
  list              Lists commands
 bower
  bower:install     Bower install
 composer
  composer:install  Composer install
 dbtk
  dbtk:schema-load  Load schema into database
 debug
  debug:echo        Echo a message to the console
```
*shortened the output for brevity*

This commmand will list the available Droid commands. You'll notice that there are several 'global' commands, such as `config`, `help`, `inventory`, `list`, etc.

Additionally, you'll find several plugin-specific commands, such as `composer:install` or `debug:echo`

## Learning more about the available commands 
To learn more about these commands, use the `--help` flag:

```sh
$ droid debug:echo --help
Usage:
  debug:echo [options] [--] <message>

Arguments:
  message               Message to output

Options:
  -c, --color=COLOR     Set color of the output
  -u, --uppercase       Uppercase the output
Help:
 Echo a message to the console
```

You'll find that the `debug:echo` command takes a `message` argument, and optionally a color and uppercase flag.

## Running commands

You can call this command like this:

```sh
$ droid debug:echo "hello world"
hello world
$ droid debug:echo "hello world" -u
HELLO WORLD
```

## Next steps

You are now familiar with using Droid to call ad-hoc commands.

Read on to start using the real power of Droid through **automation**

<center><a href="/droid-yml" class="btn btn-lg btn-success">Configuring Droid for automation</a></center>
