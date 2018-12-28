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
	
## Create a Service Principal
In order to access resources a Service Principal needs to be created in your Tenant. It is really convenient to do it via AZ CLI:

```
az ad sp create-for-rbac --name [APP_NAME] --password [CLIENT_SECRET]	
```
What is happening here is that you’re registering your application in order to be able to be recognized by Azure (more precisely: from the AD tenant that is taking care of your subscription). Exactly like when you register your application to access Twitter or Facebook in order to be able to read and write posts/tweets/user data and so on.

## Request the Access Token

As said before authentication used the OAuth2 protocol, and this means that we have to obtain a token in order to authenticate all subsequent request. We need to use the client_credential flow:

```
const Azure = require('ti.azuresqldb');
Azure.createAccessToken({
	appid : APP_ID,
	password : PASSWORD,
	tenantid : TENANT_ID
},onSuccess,onError);
```
In function `onSuccess` you can proceed the next steps. The AccessToken will stored in property `AZURE_BEARER` and will internally uses as Bearer header in http request.

All the three required information:

*    APP_ID
*    PASSWORD
*    TENANT_ID

can be obtained from the previous step. You already have the PASSWORD since you used it to create the Service Principal. The TENANT_ID and the APP_ID will be returned by the az ad sp create-for-rbac command you executed before. Otherwise you can execute the following az command to find it the tenant id:

```
az account list --output table --query '[].{Name:name, SubscriptionId:id, TenantId:tenantId}'
```

And the following to get the APP_ID:

```
az ad sp list
```

The result of the curl call will be an Authorization Token that looks like the following:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRac
[...]
hkSFwruPWvkE15zzleYir_SsSVveaRlMUq9q7GOEr87aGvOVB3QManIn_jIo1cnDCUJZ3WX7hcMvq0dLE8Ap1ZL_HQqOzLbJfpnSCDfs2X2pBmqB3JH5rzrCAzeL1mYL5TOgC8k3s1Z_vvTqxD2XrO7QOGhGfxqxxDWJAXiblUtafHg
```

## Save document

```
const Azure = require('ti.azuresqldb');
Azure.saveDocument({
    first_name : "Fritz",
    family_name : "Müller",
    age : 23
},onSuccess,onError);
```

## Getting documents

```
const Azure = require('ti.azuresqldb');
Azure.getAll({
},onSuccess,onError);
```