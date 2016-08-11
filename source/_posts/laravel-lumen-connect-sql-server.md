---
title: Laravel / Lumen Connect With SQL Server on MAC
date: 2016-08-11 12:43:02
tags: 
- laravel 
- lumen 
- sql 
- server 
- mac
---

Connecting to Microsoft SQL Server from a mac, can be a pain sometimes, luckily for us, there is something called Hombrew which relieves the pain of it (With the right tutorial).

## Installing Homebrew

So first we are going to install hombrew on our machine by running the following command: 

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Install FreeTDS and PHP

Next we'll need the freeTDS driver, we just need to run : 
```
brew install freetds
```

Now for PHP we need homebrew to be aware of some formulas, so we run the following commands one by one:
```
brew tap homebrew/dupes
brew tap homebrew/versions
brew tap homebrew/homebrew-php
```
And finally we install the PHP with MSSQL compiled into it:
```
install php56 --with-mssql
```

## Laravel / Lumen configuration
In your .env file you need to tell laravel/lumen which driver to use. As always it takes care of the underlying PDO configuration so you just need to write sqlsrv on the connection (And of course fill the corresponding server configuration) :
```
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomKey!!!

DB_CONNECTION=sqlsrv
DB_HOST=host_ip
DB_PORT=port
DB_DATABASE=database_name
DB_USERNAME=username
DB_PASSWORD=password

CACHE_DRIVER=memcached
QUEUE_DRIVER=sync
```

After doing all that, you can query SQL withouth a problem, YAY!
