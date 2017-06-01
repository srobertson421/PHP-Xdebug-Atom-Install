# PHP-Xdebug-Atom-Install
Install instructions for xdebug installation and use with the php-debug atom package

## Requisites
Mac OSX
Bash terminal
[Homebrew](https://brew.sh/)
[Atom](https://atom.io)

## Installation

### Step 1
First we're going to want to install php along with xdebug. We'll be using brew package manager:

```bash
$ brew install homebrew/php/php56-xdebug
```

### Step 2
Once the install is finished we'll check some information. Go ahead and run the command:
```bash
php --ini
```

The output of that command should be something like this:

```bash
Configuration File (php.ini) Path: /usr/local/etc/php/5.6
Loaded Configuration File:         /usr/local/etc/php/5.6/php.ini
Scan for additional .ini files in: /usr/local/etc/php/5.6/conf.d
Additional .ini files parsed:      /usr/local/etc/php/5.6/conf.d/ext-xdebug.ini
```

If everything looks good there then continue on to [Step 4](). However if instead the output looks like this:

```bash
Configuration File (php.ini) Path: /etc
Loaded Configuration File:         (none)
Scan for additional .ini files in: (none)
Additional .ini files parsed:      (none)
```

then continue on to [Step 3]()

### Step 3
Since we're missing the config files for our php installation we'll have to go ahead an manually add in the reference. To do so we'll add an environment variable to our bash configuration. Typically you would end up editing either ```~/.bashrc``` ```~/.bash_profile``` or if you have zsh installed ```~/.zshrc```

You'll need to add this export line to whichever configuration file you're utilizing:

```bash
export PHPRC=/usr/local/etc/php/5.6/php.ini
```

and save the config file. The env variable PHPRC is used by PHP to direct to the default config file for PHP. In this case we're pointing it at */usr/local/etc/php/5.6/php.ini*. Remember that path as we'll be editing that file later.

Now source the config file and make sure the config is showing up:

```bash
source ~/.zshrc
php --ini
```

The output should be what we're looking for:

```bash
Configuration File (php.ini) Path: /usr/local/etc/php/5.6
Loaded Configuration File:         /usr/local/etc/php/5.6/php.ini
Scan for additional .ini files in: /usr/local/etc/php/5.6/conf.d
Additional .ini files parsed:      /usr/local/etc/php/5.6/conf.d/ext-xdebug.ini
```

### Step 4
Now that we have PHP pointing to the right config, we need to include some settings in the *.ini* file we mentioned earlier, typically located at: ```/usr/local/etc/php/5.6/php.ini```. Go ahead an open that file in your text editor.

We need to add the following lines to the *.ini* file in order for xdebug to be seen and used. These lines can be pasted anywhere in the config but I put mine around line 67:

```bash
zend_extension="/usr/local/Cellar/php56-xdebug/2.5.4/xdebug.so"
xdebug.remote_enable=1
xdebug.remote_host=127.0.0.1
xdebug.remote_connect_back=1    # Not safe for production servers
xdebug.remote_port=9000
xdebug.remote_handler=dbgp
xdebug.remote_mode=req
xdebug.remote_autostart=true
```

For the line **zend_extension=""** you'll want to make the value the path to your xdebug installation which as you remember was included with our brew install of php. If you followed this tutorial that path should be correct but if you'd like to be sure you can run:

```bash
brew info php56-xdebug
```

This command will show you the current install directory for the package we installed earlier. If you list out the contents of that directory you'll see a file titled **xdebug.so** which should be what the **zend_extentsion** points to. The other **xdebug** settings are for the Atom package we'll install later.

### Step 5

Next run the php cli tool from your terminal with the module flag:

```bash
php -m
```

This should output a long list of modules that are included with your php installation. You're looking for two modules specifically, both reference the xdebug library:

```
...
sysvshm
tokenizer
wddx
xdebug
xml
xmlreader
xmlrpc
xmlwriter
xsl
zip
zlib

[Zend Modules]
Xdebug
```

If you see the two modules, great! We're ready to keep going.

### Step 6
Now that our xdebug is coupled with PHP we're ready to link it up to the Atom php-debug package. To install that lets go ahead and open Atom. Once the program is open you'll want to navigate to the package installation menu. This can be see on Max OSX via the *Atom -> Preferences -> Install* menu.

Once you have the menu open, go ahead and search for the package **php-debug**


