#How to install Directus Suite

This project has been deployed on a VPS with quickly installation

[>> Full documentation here <<](https://docs.directus.io/getting-started/introduction.html)

##My configuration

![Generic badge](https://img.shields.io/badge/UBUNTU-18.04.1_LTS-ORANGE.svg) ![Generic badge](https://img.shields.io/badge/PHP-7.2-BLUE.svg) ![Generic badge](https://img.shields.io/badge/APACHE-2.4-RED.svg) ![Generic badge](https://img.shields.io/badge/MYSQL-5.7-LIGHTGREY.svg)

## Requierements

[>> Click here <<](https://docs.directus.io/getting-started/installation.html#requirements)

##Installation

###1/ Clone project

> git clone https://github.com/directus/directus.git

Enter in the folder of the project `directus` and run :

> composer install
###2/ Apache VirtualHost

Go to :

> cd /etc/apache2/sites-availables/

Create or add in your file ``.conf`` this :
```
<VirtualHost *:80>
     ServerAdmin admin@example.com
     ServerName example.com
     ServerAlias directus.example.com
     DocumentRoot /var/www/html/directus/public
     ErrorLog ${APACHE_LOG_DIR}/directus_error.log
     CustomLog ${APACHE_LOG_DIR}/directus_access.log combined
 
     <Directory /var/www/html/directus>
       AllowOverride All
     </Directory>
 </VirtualHost>
 ```
To edit if necessary (ServerName, ServerAlias, DocumentRoot, Directory, etc...)

Reload apache to take into account this new Virtual Host

> sudo service apache2 reload

###3/ Create database

Log in to your mysql and create a new database that will be called directus for example.

When your database is created, we need to generate the tables with a simple command from Directus :

> php bin/directus install:database

If you look after this command in your database, you will see appear several tables prefixed by ``directus``

You will need to connect this to the project, duplicate the ``api_sample.php`` file and rename it by ``api.php``

Edit what you need in ``api.php``

###4/ Create admin user

> php bin/directus install:install -e <admin_email> -p <admin_password>

Admin account created

###5/ Connect to the App

Run in your browser the url where is your Directus project and add ``/admin`` at the end

You will see a form to connect to the application

Log in with the email and password you used to create the admin user

You can now create your own table with your own data before use the API

##Use the API

###Get Authentication Token 

You will have two way to get to have one

####JWT Access Token

It's the best way to get data from Directus but the tokens expire really quickly.

Create `POST` HTTP request with this url `/[project]/auth/authenticate` 

Body contents :

```Body
{
    "email": "rijk@directus.io",
    "password": "supergeheimwachtwoord"
}
```

And you will get something like that:

```
{
    "data": {
        "token": "eyJ0eXAiOIJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNTU4NTEzODMxLCJ0eXBlIjoiZYXWV0aCIsImtleSI6Ijx0eXBlLWEtcHVibGljLWF1dGhlbnRpY2F0aW9uLWtleS1zdHJpbmc-IiwicHJvamVjdCI6Il8ifQ.JXvZIN180N5k1LZcWOxpTgH4jhDyraw2yqrkr_q2qQs"
    },
    "public": true
}
```

I advise you to save it in database

####StaticToken

Add value in ``token`` column in the ``directus_users`` table in the database directly. The token will never expire.

###Get all items in a collection

+ HTTP method : ``GET``
+ Url : ``http://example.com/_/items/{collection_name}``
+ Auhtorization : ``Bearer Token`` (use JWT Acces Token or StaticToken)

###Add item in a collection

+ HTTP method : ``POST``
+ Url : ``http://example.com/_/items/{collection_name}``
+ Auhtorization : ``Bearer Token`` (use JWT Acces Token or StaticToken)
+ Body : An object of the collection

###Get one item in a collection

+ HTTP method : ``GET``
+ Url : ``http://example.com/_/items/{collection_name}/{id}``
+ Auhtorization : ``Bearer Token`` (use JWT Acces Token or StaticToken)

###Update an item in a collection

+ HTTP method : ``PATCH``
+ Url : ``http://example.com/_/items/{collection_name}/{id}``
+ Auhtorization : ``Bearer Token`` (use JWT Acces Token or StaticToken)
+ Body : Only fields you want to update

###Delete one item in a collection

+ HTTP method : ``DELETE``
+ Url : ``http://example.com/_/items/{collection_name}/{id}``
+ Auhtorization : ``Bearer Token`` (use JWT Acces Token or StaticToken)

