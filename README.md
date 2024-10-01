Terminal dump for course : 62-52.1 Paramétrage de progiciels de gestion intégrés

hearc:hearc

# Installer psql

```bash
$ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -$ sudo apt-get update
$ sudo apt-get -y install postgresql-13
```

Créer le fichier de conf pour Openbravo

```bash
$ sudo nano /etc/postgresql/13/main/conf.d/00-openbravo-minimum.conf
```

Ajouter les lignes suivantes

```bash	
lc_numeric = 'en_US.UTF-8'
max_locks_per_transaction = 128`
```

Autoriser les connexions locales

```bash
$ sudo nano /etc/postgresql/13/main/postgresql.conf
```	

Modifier la ligne suivante

```bash
listen_addresses = '*'
```

```bash	
$ sudo nano /etc/postgresql/13/main/pg_hba.conf
```

Ajouter la ligne suivante dans IPv4 local connections

```bash
host all all 0.0.0.0/0 md5
```

Et dans IPv6 local connections

```bash
host all all ::0/0 md5
```

Redémarrer le service

```bash
$ sudo /etc/init.d/postgresql stop
$ sudo /etc/init.d/postgresql start
```

Créer un utilisateur avec le password 'dev'

```bash
$ sudo su - postgres -c psql
\password
➔ <enter password twice>
\quit
```

## Installer PgAdmin4

```bash
$ sudo apt-get install pgadmin4
```

## Installer Java 11

```bash
$ sudo apt-get install openjdk-11-jdk
```

Set JAVA_HOME

```bash
$ sudo su
$ echo 'export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64' > /etc/profile.d/base-variables.sh
$ exit
```

Vérifier l'installation

```bash	
$ java -version
```

## Installer Tomcat 8.5

tomcat est 

```bash
$ sudo mv tomcat /opt
```

Set les variables d'env

```bash
$ sudo su
$ echo 'export CATALINA_OPTS="-Xms512M -Xmx1152M -XX:PermSize=64M -Djava.awt.headless=true"' > /etc/profile.d/tomcat.sh
$ echo 'export CATALINA_HOME=/opt/tomcat' >> /etc/profile.d/base-variables.sh
$ echo 'export CATALINA_BASE=/opt/tomcat' >> /etc/profile.d/base-variables.sh
$ exit
```

On démarre le serveur Tomcat

```bash
$ /opt/tomcat/bin/startup.sh
```

## Installer ant

```bash	
$ sudo apt-get install ant
```

```bash
$ sudo su
$ echo 'export ANT_OPTS="-Xmx1152M"' > /etc/profile.d/ant.sh
$ exit
```

```bash
$ ant -version
```

## Installer Openbravo

```bash
$ sudo mkdir /opt/openbravo
$ sudo mkdir /opt/openbravo/attachments
$ sudo chown -R hearc /opt/openbravo
$ sudo mkdir /workspace
$ sudo chown -R hearc /workspace
$ mkdir /workspace/OBDev
$ cp -R -p openbravo /workspace/OBDev
```

```bash
$ cd /workspace/OBDev/openbravo
$ ant setup
```
Il faut répondre aux questions comme suit. En italique, vous trouverez le texte
affiché par le fichier de commandes, et en normal gras, les réponses que vous
devez donner.

```bash
Buildfile: /workspace/OBDev/openbravo/build.xml
init:
setup.compile:
[mkdir] Created dir: /workspace/OBDev/openbravo/build/classes
[javac] Compiling 2 source files to /workspace/OBDev/openbravo/build/classes
setup:
[echo] Launching configuration application...
----------------------------------------------------------------------------
Welcome to the Openbravo ERP Setup Wizard.
----------------------------------------------------------------------------
Please read the following License Agreement...
Press [Enter] to continue:
<Entrée>
A) Openbravo Public License
-> Bla bla de license, accepter en faisant <Entrée> plusieurs fois jusqu’à…
Do you accept this license? [y/n]:
y
----------------------------------------------------------------------------
Please choose one option.
----------------------------------------------------------------------------
[1] Step-by-step configuration.
[2] Default configuration.
[3] Exit without saving.
Choose an option:
1
Please select date format:
[0] DDMMYYYY
[1] MMDDYYYY
[2] YYYYMMDD
Please, choose an option [0]:
<Entrée> (valeur par défaut)
-------------------------
Your choice DDMMYYYY
-------------------------
Please select date separator:
[0] -
[1] /
[2] .
[3] :
Please, choose an option [0]:
<Entrée> (valeur par défaut)
-------------------------
Your choice -
-------------------------
Please select time format:
[0] 12h
[1] 24h
Please, choose an option [1]:
<Entrée> (valeur par défaut)
-------------------------
Your choice 24h
-------------------------
Please select time separator:
[0] :
[1] .
Please, choose an option [0]:
<Entrée> (valeur par défaut)
-------------------------
Your choice :
-------------------------
Please introduce Attachments directory:
Please, introduce here: [/opt/openbravo/attachments]:
<Entrée> (valeur par défaut)
-------------------------
Your choice /opt/openbravo/attachments
-------------------------
Please introduce Context name:
Please, introduce here: [openbravo]:
<Entrée> (valeur par défaut)
-------------------------
Your choice openbravo
-------------------------
Please introduce Web URL:
Please, introduce here: [@actual_url_context@/web]:
<Entrée> (valeur par défaut)
-------------------------
Your choice @actual_url_context@/web
-------------------------
Please introduce Context URL :
Please, introduce here: [http://localhost:8080/openbravo]:
<Entrée> (valeur par défaut)
-------------------------
Your choice http://localhost:8080/openbravo
-------------------------
Please introduce Authentication class:
Please, introduce here: []:
<Entrée> (valeur par défaut)
-------------------------
Your choice
-------------------------
Please select Database:
[0] Oracle
[1] PostgreSQL
Please, choose an option [1]:
<Entrée> (valeur par défaut)
-------------------------
Your choice PostgreSQL
-------------------------	
Please introduce SID:
Please, introduce here: [openbravo]:
<Entrée> (valeur par défaut)
-------------------------
Your choice openbravo
-------------------------
Please introduce System User:
Please, introduce here: [postgres]:
postgres
-------------------------
Your choice dev
-------------------------
Please introduce System Password:
Please, introduce here: [syspass]:
dev
-------------------------
Your choice dev
-------------------------
Please introduce DB User:
Please, introduce here: [tad]:
openbravo
-------------------------
Your choice openbravo
-------------------------
Please introduce DB User Password:
Please, introduce here: [tad]:
dev
-------------------------
Your choice dev
-------------------------
Please introduce DB Server Address:
Please, introduce here: [localhost]:
<Entrée> (valeur par défaut)
-------------------------
Your choice localhost
-------------------------
Please introduce DB Server Port:
Please, introduce here: [5432]:
<Entrée> (valeur par défaut, mais vérifiez que c’est le bon port dans PGAdmin4)
-------------------------
Your choice 5432
-------------------------
----------------------------------------------------------------------------
Do you agree with all options that you have configured?
----------------------------------------------------------------------------
[1] Accept.
[2] Back to preview configuration.
[3] Exit without saving.
Choose an option:
1

```

On installe/compile/construit avec ant (10min environ)

```bash
$ ant install.source
```

## Tester l'installation

```bash	
$ sudo /opt/tomcat/bin/startup.sh
```

Ouvrir dans un navigateur 'localhost:8080/openbravo'

```bash
$ sudo /opt/tomcat/bin/shutdown.sh
```