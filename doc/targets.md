# Targets

Using the [droid.yml](droid-yml) file you can define a set of **targets** that contain a sequence of tasks to perform.

This will allow you to type something like `droid mybuild`, instructing Droid to

* Download your project dependencies
* Download and compile assets
* Compress and minify images, scripts and stylesheets
* ...etc

All in a **single command**!

## Use cases
Using Droid targets has a couple of interesting use-cases:

* **Continuous Integration**: Automatically checkout, update, and build new versions of your application
* **Building project assets**: Use a single command to compile your .less, .sass etc files into optimized css files, and minify your javascripts
* **Automated testing**: Script your unit-tests, acceptance-tests, convention checkers, etc
* **Script daily chores**: As long as there are [commands](commands) for it, you can script it :)


## Simple `droid.yml` file with targets

Here's a simple example of a `droid.yml` file:

```yml
targets:
    mybuild:
        tasks:
            - name: Install composer dependencies
              "composer:install": prefer=dist
            - name: Install bower dependencies
              "bower:install": ~
    mycleanup:
        tasks:
            - name: Remove vendor directory
              "fs:rmdir": path="vendor" force=true
            - name: Remove bower directory
              "fs:rmdir": path="bower_components"
```

To run the `mybuild` target, simply run:

    $ droid mybuild

This will install both the composer and bower dependencies of your project.

To cleanup your working directory by removing the composer and bower files, simply run:

    $ droid mycleanup
    
## Tasks

In the example, you'll see that each target (`mybuild` and `mycleanup`) contain a list of tasks.

A task is simply one [command](commands) to execute, with it's arguments specified.

```yml
- name: Install composer dependencies
  "composer:install": prefer=dist
```

This task instructs droid to run the [composer:install](commands__composer__install) command, with arguments `prefer` set to `dist`.

The `name` allows you to specify a human-readable version of what your task does. This will be displayed on the console
when the task is being executed.

You can specify tasks in 2 ways:

### Short notation (standard)

```yml
- name: Install composer dependencies
  "composer:install": prefer=dist
```

### Dictionary notation

```yml
- name: Install composer dependencies
  "composer:install": { prefer: "dist" }
```

### Expanded/long notation (useful if you're passing many parameters)

```yml
- name: Install composer dependencies
  command: "composer:install"
  arguments:
      prefer: dist
```

## Variables

You can define variables that can be used as command arguments in your tasks.

Variables can be defined on projects, targets, hosts and hostgroups.

### Target variables

```yml
targets:
    echotest:
        variables:
            greeting: "Hello"
            
        tasks:
            - name: "Greet the user"
              "debug:echo" message="{{greeting}} dear user!" color="green"
```

In this example, we've defined the `greeting` variable on the `echotest` **target**.
When running the "Greet the user" task, the [debug:echo](commands__debug__echo) command is called with argument
`message` set to "Hello dear user". The message is using the `{{greeting}}` variable.


### Project variables

```yml
variables:
    greeting: "Hello"
    
targets:
    echotest:
        tasks:
            - name: "Greet the user"
              "debug:echo" message="{{greeting}} dear user!" color="blue"
```

In this example, we've defined the `greeting` variable on the **project**. This way you can use the variable
in all it's targets and tasks.

## Next steps

You are now familiar with using Droid for automation of your local machine.

You can take this to the next level by learning about inventories, so you can manage remote machines.

<center><a href="/inventory" class="btn btn-lg btn-success">Remote management with inventory</a></center>
