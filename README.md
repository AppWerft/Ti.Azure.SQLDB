# Ti.Azure.SQLDB

This is an Axway Titanium module for connecting to Azure SQL DB.

## Prerequisites

First step is to install az.

```
brew install az
```

### Potential pitfalls

#### File system rights
```
Error: An unexpected error occurred during the `brew link` step
The formula built, but is not symlinked into /usr/local
```

In this case you can follow this [receipt](https://gist.github.com/dalegaspi/7d336944041f31466c0f9c7a17f7d601). In short:

```
sudo chown -R $(whoami) $(brew --prefix)/*
sudo install -d -o $(whoami) -g admin /usr/local/Frameworks
```
...or just switch to MacPorts 😂

#### No module named pkg_resources

After starting von az on CLI you can run in this issue. In this case you can reinstall python by

```
brew reinstall python
```

Now you can test installation of az with tho command:

```
Rainers-MacBook-Pro-3:Liebherr fuerst$ az

     /\
    /  \    _____   _ _  ___ _
   / /\ \  |_  / | | | \'__/ _\
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome to the cool new Azure CLI!
```

## Creating azure account and database
	