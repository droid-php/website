# Automate your projects with droid.yml files

In the [previous steps](usage), you've seen how you can use Droid to perform ad-hoc commands.

While this is cool, the real power of Droid comes when configure it for automation.

## droid.yml

When you start droid, it looks for a `droid.yml` file in your project directory.

Optionally, you can specify an alternative configuration file using `--droid-config`. For example:

    droid --droid-config=/etc/my-droid-file.yml
    
The config file allows you to [define targets](targets) and [define inventory](inventory)


<center><a href="/targets" class="btn btn-lg btn-success">Automation with targets</a></center>

<center><a href="/inventory" class="btn btn-lg btn-success">Remote management with inventory</a></center>
