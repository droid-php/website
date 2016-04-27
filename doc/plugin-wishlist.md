# Plugin Wishlist

[Extending Droid with Plugins](plugin-overview) is fun and easy!

We're keeping a wishlist of plugins we're looking forward to adding. If you're interested in helping out, we'd be happy to merge any pull requests or list urls to your plugins!

New to writing Droid plugins? Check out our step-by-step [tutorial](plugin-tutorial).

## Conventions

The following list describes the plugins and commands we’ll need for droid.
The droid commands are seperated into repositories per functionality. For example `droid-service` handles service start, stop, reload, restart, and `droid-fs` handles filesystem commands like mkdir, copy, etc.

For each command, it lists the commandname (i.e. fs:copy) and a list of arguments. Arguments are always required unless specified otherwise.
Additionally it lists a set of options indicated as `--force|-f`. This shows a long option name (--force), and a shortcut (-f). Some options require parameters (like --username=joe) and others are just a flag (like --force).

Each command should return exitcode 0 on success, and other status codes on errors.

Output should alway go through $output->write or writeln (never echo, print_r, etc!) and error messages should go through the error output:

    $errOutput = $output instanceof ConsoleOutputInterface ? $output->getErrorOutput() : $output;
    $output->writeln('<info>standard message</info>');
    $errOutput->writeln('<error>error message</error>');

Please submit all changes by forking the existing repository, and sending a pull-requests.
We can help to create new repositories when needed.


## droid (commands in the main droid-php/droid repo)

* core:exec
    * cmd: Command to execute
    * --env: csv of environment variables to set. Example “HELLO=world, A=b”
    * --dir: working directory for executing the cmd
* core:download (using guzzle)
    * url: URL to download from
    * filename: filename to save
    

## droid-fs (existing repo, needs more commands)

* fs:rename Move/rename a file or directory to another place
    * src: source file or directory to move
    * dst: destination to move it to
    * --force|f: Force delete, always return exit code 0, even if file does not exist
* fs:copydir Copy a directory (optionally recursively)
    * src: source directory
    * dst: destination directory
    * --recursive: recursively copy sub-directories
    * --owner: name of the owner of the copied directory (like `chown`)
    * --group: name of the group of the copied directory (like `chgrp`)
    * --filemask: file permission mask (like 0640 - use octdec etc)
    * --dirmask: directory mask (like 0700 - use octdec etc)
* fs:remove Delete a file or directory (optionally recursively)
    * src: file or directory to delete
    * --force|f: Force delete, always return exit code 0, even if file does not exist
    * --recursive|r: recursive delete
* fs:symlink Symlink one file/dir to another
    * src: source file or directory to move
    * dst: destination to move it to
    * --force:f: If the target file already exists, then unlink it so that the link may occur.
    
## droid-db (using PDO)
* db:create-database
    * connection: connection string like mysql://username:password@host:port/dbname
    * --force|-f: Force creation, always return exit code 0, even when db already exists.
* db:drop-database
    * connection: connection string like mysql://username:password@host:port/dbname
    * --force|-f: Force creation, always return exit code 0, even when db didn’t exist. 	
* db:query
    * connection: connection string like mysql://username:password@host:port/dbname
    * query: sql query to execute. For example: `DELETE FROM log WHERE create_at<01-01-2016`
    
## droid-composer (existing repo, needs more commands)
* composer:install Call `composer install`
    * --optimize|-o: Pass --optimize to composer
    * --no-dev: Pass --no-dev
    * --working-dir|-d: If specified, use the given directory as working directory.
    * --no-interaction|-n: Do not ask any interactive question
    * --ignore-platform-reqs: Ignore platform requirements (php & ext- packages).
    * --prefer-dist
    * --prefer-source
* composer:update Call `composer install`
    * --optimize|-o: Pass --optimize to composer
    * --no-dev: Pass --no-dev
    * --working-dir|-d: If specified, use the given directory as working directory.
    * --no-interaction|-n: Do not ask any interactive question
    * --ignore-platform-reqs: Ignore platform requirements (php & ext- packages).
    * --prefer-dist
    * --prefer-source
* composer:dumpautoload Call `composer dumpautoload`
    * --optimize|-o: Pass --optimize to composer
    * --no-dev: Pass --no-dev
    * --working-dir|-d: If specified, use the given directory as working directory.
    * --no-interaction|-n: Do not ask any interactive question
    
## droid-bower (existing repo, needs more commands)
* bower:install Call `bower install` (exists, but needs more args+options)
    * --allow-root: pass --allow-root to bower
    * --production|-p: Do not install project devDependencies
    * --force-latest|-F: Force latest version on conflict
    * --force|-f: Makes various commands more forceful
* bower:update Call `bower update`
    * --production|-p: Do not install project devDependencies
    * --force-latest|-F: Force latest version on conflict

## droid-ssh: (existing repo, needs more commands)
* scp:put Use \phpseclib\Net\SCP::put to upload a file Add all arguments and flags from ssh:exec (simply extend from BaseSshCommand)
    * src: source filename
    * dst: destination filename
    * --no-interaction|-n: Do not ask any interactive question
* scp:get Use \phpseclib\Net\SCP::get to upload a file Add all arguments and flags from ssh:exec (simply extend from BaseSshCommand)
    * src: source filename
    * dst: destination filename
    * --no-interaction|-n: Do not ask any interactive question
    
## droid-slack
Use https://github.com/maknz/slack  Check the README.md for options, descriptions and defaults.
* slack:message Send a Slack message
    * message: message contents to send
    * --endpoint: 
    * --username: the default username that messages will be sent from
    * --icon: the default icon messages will be sent with, either :emoji: or a URL to an image
    * ...any other options the slack library takes in `settings`.
    
## droid-objectstorage
Use github.com/linkorb/objectstorage. Use 2.0 IMPORTANT: Don’t use the ObjectStorage\Utils class. It will be removed.

* objectstorage:upload Uploads a file to object storage
    * source: Local file to upload
    * destination: remote filename
    * --storage: s3, gridfs, pdo or file 
    * --s3-bucket: bucket name in case of s3 adapter
    * --s3-access-key: Access key
    * --s3-secret-key: Secret key
    * --pdo-connection: PDO connection string, see droid-db for details
    * --pdo-tablename: tablename to use
    * --file-path: Path in case of --storage `file`
    * --encryption-key: key to use for EncryptionAdapter
    * --encryption-iv: initialization vector to use for EncryptionAdapter
    * --gridfs-server: connection string for gridfs
    * --gridfs-db
    * --bzip2-level: level to use for Bzip2Adapter

* objectstorage:download Downloads a file from object storage
    * source: Remote filename to download
    * destination: Local filename to download
    * --storage: s3, gridfs, pdo or file 
    * --s3-bucket: bucket name in case of s3 adapter
    * --s3-access-key: Access key
    * --s3-secret-key: Secret key
    * --pdo-connection: PDO connection string, see droid-db for details
    * --pdo-tablename: tablename to use
    * --file-path: Path in case of --storage `file`
    * --encryption-key: key to use for EncryptionAdapter
    * --encryption-iv: initialization vector to use for EncryptionAdapter
    * --gridfs-server: connection string for gridfs
    * --gridfs-db
    * --bzip2-level: level to use for Bzip2Adapter

# droid-apt (TODO: needs more commands for keys and repositories):
* apt:key-add: Add an apt-key
* apt:repository-add: Adds an apt repo

## droid-docker (TODO)
Select best PHP library, and use library where possible, shell out to console when needed. Decide what to do with helpers like docker-compose etc.

## droid-herald
## droid-smtp
## droid-xtheme
## droid-bluecode
## droid-blazon
## droid-bisight
## droid-sms
## droid-libcloud
Provision, start/stop, etc

## droid-mailchimp
Mailchimp:pushlist csv?




    
