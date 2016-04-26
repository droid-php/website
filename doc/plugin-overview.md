# Extending Droid with Plugins

Droid comes with a set of [commands](commands) that you can use out of the box.

But if the commands you're looking for are not (yet) available, then don't worry: Extending Droid with Plugins is easy.

## Built on Symfony Console Commands

All Droid commands are nothing more than standard [Symfony Console Commands](http://symfony.com/doc/current/components/console/introduction.html). The Symfony documentation is great,
and using this component is very easy.

Using standard Symfony classes and interfaces, you can define the commands and it's arguments and options.


## DroidPlugin class

Once you've created a repository with the Command(s) you'd like to add to Droid, you can do so by simply adding a `DroidPlugin` class to the root of your project's namespace. For example, if your `composer.json` contains the following line:

```yml
"autoload": {
        "psr-4": {
            "Acme\\Droid\\Plugin\\FooBar\\": "src/"
        }
    }
```

Then you'll need to create a DroidPlugin class similar to this:

```php
<?php

namespace Acme\Droid\Plugin\FooBar;

class DroidPlugin
{
    public function __construct($droid)
    {
        $this->droid = $droid;
    }
    
    public function getCommands()
    {
        $commands = [];
        $commands[] = new \Acme\Droid\Plugin\FooBar\Command\FooBarHelloCommand();
        $commands[] = new \Acme\Droid\Plugin\FooBar\Command\FooBarByeCommand();
        return $commands;
    }
}
```

Droid will automatically detect this class, and call `getCommands()` to learn which commands you'd like to add to Droid.

## Next steps

You are now familiar with the way Droid loads plugins.

Read on for a full step-by-step tutorial on creating and publishing your own Droid command!

<center><a href="/plugin-tutorial" class="btn btn-lg btn-success">Plugin Tutorial</a></center>
