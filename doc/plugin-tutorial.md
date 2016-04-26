# Plugin Tutorial

In the [previous](plugins) page, you've learned how plugins work.

In this page we'll be giving you a step-by-step tutorial on how to create your own first Droid plugin!

## Initializing the project directory / repository

First you'll need a directory to create your project files in:

    mkdir ~/my-droid-plugin
    cd ~/my-droid-plugin
    git init

All your plugin files will go in the `my-droid-plugin` directory. We've called `git init` so you can commit your files, and upload the to version control services such as GitHub or BitBucket.

## Create a composer.json file

We'll be using composer to manage our project's dependencies.
If you're not yet familiar with composer, please refer to the excellent documentation on [getcomposer.org](http://getcomposer.org)

Create a file called `composer.json` in the root of your project directory:

```json
{
    "name": "acme/droid-foobar",
    "description": "Droid plugin droid-foobar",
    "homepage": "http://www.github.com/droid-php/droid-fs",
    "keywords": ["droid"],
    "type": "library",
    "require": {
        "php": ">=5.3.0",
        "symfony/console": "~2.7"
    },
    "autoload": {
        "psr-4": {
            "Acme\\Droid\\Plugin\\FooBar\\": "src/"
        }
    },
    "license": "MIT"
}
```

Be sure to update the `name`, `description` and `psr-4` namespace to match your own names.

We'll be using the name **Acme** as an example name for your organization. **FooBar** will be the example name of your plugin.

Now that you've created your `composer.json` file, you can install the dependencies:

    composer install
    
You'll notice that the `vendor/` directory was created.

## Create a .gitignore file

In the root of your directory, create a file called `.gitignore`:

```
vendor/
``` 

This will tell git to ignore the vendor directory, as this should not be sent to version control.

## Create the console command

In your project directory, create a directory called `bin/`.
Next, create a file called `bin/droid-foobar` with the following PHP code:

```php
#!/usr/bin/env php
<?php

use Symfony\Component\Console\Application;

$loader = __DIR__ . '/../vendor/autoload.php';

if (!file_exists($loader)) {
    die(
        'You must set up the project dependencies, run the following commands:' . PHP_EOL .
        'curl -s http://getcomposer.org/installer | php' . PHP_EOL .
        'php composer.phar install' . PHP_EOL
    );
}

require $loader;

$application = new Application('Droid FooBar', '1.0.0');
$application->setCatchExceptions(true);
$application->add(new Acme\Droid\Plugin\FooBar\Command\FooBarHelloCommand());
$application->run();
```

## Create the actual command (FooBarHelloCommand)

In the root of your project, create a directory called `src/` and in there a directory called `Command/`.

Next, create a file called `src/Command/FooBarHelloCommand.php` with the following contents:

```php
<?php

namespace Acme\Droid\Plugin\Command;

use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputOption;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;

class FooBarHelloCommand extends Command
{
    public function configure()
    {
        $this->setName('foobar:hello')
            ->setDescription('Echo a message to the console')
            ->addArgument(
                'name',
                InputArgument::REQUIRED,
                'Name of the person to greet'
            )
            ->addOption(
                'uppercase',
                'u',
                InputOption::VALUE_NONE,
                'Uppercase the output'
            )
        ;
    }
    
    public function execute(InputInterface $input, OutputInterface $output)
    {
        $name = $input->getArgument('name');
        $message = 'Hello ' . $name . ', greetings from FooBar!';
        if ($input->getOption('uppercase')) {
            $message = strtoupper($message);
        }
        $output->writeLn($message);
    }
}
```

## Testing your command

You can now try out your command by running:

    bin/droid-foobar foobar:hello Joe
    
If all works well, the output will be:
    
    Hello Joe, greetings from FooBar!
    
You can try out the option you've added (`uppercase`) by running the command like this:

    bin/droid-foobar --uppercase foobar:hello Joe
    
You should now see the same message, but in all uppercase

You have now created a simple Symfony Console Command. To extend it's functionality, you'll simply edit the `execute` method of this class.

## Turn your Symfony Console Command into a Droid Plugin

There's one simple step left in order for your standard Symfony Console Command to become a Droid Plugin.

For Droid to know that your project is actually a Droid Plugin, you'll simply need to create a `DroidPlugin` class
in the root of your project's namespace. In this case, create a file called `src/DroidPlugin.php` with the following contents:

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
        // Optional: Add more commands here if your plugin supports multiple
        return $commands;
    }
}
```

## Using your Droid Plugin

You can now commit your code through Git, and push it to a code hosting service like GitHub, BitBucket, etc.
Make sure you add a proper [semver](http://semver.org/) tag, such as `v1.0.0`.

After that, go to [packagist.org](http://packagist.org) and submit the URL to your project for Packagist to index it.

To use your new plugin in one of your projects, simply edit the `composer.json` file of the project you'd like to use Droid and your plugin, and add the following lines to your `require` or `require-dev` section:
```json
"require": {
    "droid/droid": "~1.0",
    "acme/droid-foobar": "~1.0"
}
```

After running `composer update` you can now use your plugin like this:

    vendor/bin/droid foobar:hello Joe
    
