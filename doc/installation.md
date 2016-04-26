# Installation

You can install Droid in a couple of ways:

## 1. Standalone PHAR file (recommended)

```
wget http://droidphp.com/assets/droid.phar -O /usr/local/bin/droid
chmod 755 /usr/local/bin/droid
```

## 2. Install as a project dependency

If you plan to use Droid as a task-runner in one of your projects, you can use *composer* to install
Droid as a dependency.

Simply add the following line to your `composer.json`

```json
"require": {
    "droid/droid": "~1.0",
    "droid/droid-plugins": "~1.0"
}
```
This will install the Droid application, and a set of default plugins.

Then update your composer dependencies by running:
```sh
composer update
```

You can now access Droid by running:

```sh
vendor/bin/droid
```


## Next steps

You now have droid installed on your local machine. Continue reading to learn how to use it!

<center><a href="/usage" class="btn btn-lg btn-success">Using Droid</a></center>
