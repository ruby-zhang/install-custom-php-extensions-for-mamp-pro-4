# Install custom PHP extensions for MAMP PRO 4

Example extension: gmp on a MAMP PRO with PHP 5.6.27 installation. For other versions it's just replacing the php version in the paths. Steps below are tested on MacOS Sierra (10.12.2).

### Prerequisites

Install Brew and MAMP PRO.

- http://brew.sh
- https://www.mamp.info/en/mamp-pro/

### Install desired extension with Brew
```
$ brew tap homebrew/php
```
```
$ brew search gmp
```
```
$ brew install php56-gmp
```

### Replace `$ php` by the MAMP php instance

By default `$ php`links to the preinstalled version on your Mac. That version is mostly different from the configured ones in MAMP. That's why I replaced it. Add the following line to your path in `~/.bash_profile` or `~/.zshrc` (if you use oh-my-zsh):
```
export PATH=/Applications/MAMP/bin/php/php5.6.27/bin:$PATH
```

### Link the extension with MAMP

The extension is now installed in a Brew folder, but not known by MAMP. The config file of the extension is in:

```
$ ls /usr/local/etc/php/5.6/conf.d
```

Here you should find the config file for gmp: "ext-gmp.ini". Copy the content. Combine it with the extension name to this:

```
[gmp]
extension="/usr/local/opt/php56-gmp/gmp.so"
```

Paste the snippet above in both the php.ini template (apache) and the real php.ini (cli). MAMP replaces parts of the real php.ini with the template accessible from the MAMP application. Look for this part:

```ini
;;;;;;;;;;;;;;;;;;;;;;
; Dynamic Extensions ;
;;;;;;;;;;;;;;;;;;;;;;
```

Paste the snippet INSIDE this block for the template linked from the MAMP app.
Paste the snippet OUTSIDE this block for the template in the real php.ini (/Applications/MAMP/bin/php/php5.6.27/conf/php.ini)


## Check if it works:

CLI: 
```
$ php -m | grep gmp
```
Server: Check if the extension is mentioned in a file containing: 
```php
<?php phpinfo(); ?>
```

