# Local Development using Docker

BlocksOffice dockerized for development in local environments. 

## Prerequisites

**OS X** - Docker for Mac (Edge Channel)

https://download.docker.com/mac/edge/Docker.dmg

**Linux (Ubuntu)** - Docker Community Edition. 

https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-using-the-repository

**Windows 10**

Coming soon...

## Container Details

**Symfony** - Amazon Linux 2017.03/w Nginx + PHP-FPM

**MySQLServer** - 5.7.19

**Varnish** - 4.0.4

**Memcached** - 1.51

## Getting Started (First Time Installation)

1.) Make sure docker is running.

```docker run hello-world```

2.) Navigate to your locally checked out version of BlocksOffice.

```cd ~/sites/blocksoffice```

3.) Install network aliases (Requirement on OS X only).

```./dockerfiles/osx_init ```

4.) Add hostfile entries.

```127.0.0.2 tickets.sadlerswells.docker tickets.jw3.docker tickets.signature-theatre.docker tickets.glyndebourne.docker tickets.sagegateshead.docker tickets.artsiowa.docker tickets.malthousetheatre.docker tickets.attpac.docker tickets.ticketdfw.docker tickets.mtc.docker tickets.melbournerecital.docker tickets.mso.docker tickets.cheltenhamfestivals.docker tickets.kingsplace.docker tickets.theatreroyal.docker tickets.roundhouse.docker tickets.laphil.docker tickets.hollywoodbowl.docker tickets.royalalberthall.docker tickets.lincolncenter.docker tickets.drphillipscenter.docker```

## Basic Usage

1.) Navigate to your local clone of the BlocksOffice repository.

```cd ~/sites/blocksoffice```

2.) Branch off from a master branch to a feature or support branch.

```git checkout -b support/20001```

**OS X Users Only**

3.) (OS X) Fire up the BlocksOffice containers (This takes a few minutes first time you do it but is near instantaneous thereafter)

```docker-compose up -d```

**Linux Users Only**

3.) (Linux) Fire up the BlocksOffice containers (This takes a few minutes first time you do it but is near instantaneous thereafter)

```./dockerfiles/linux_docker_compose_up```

4.) Run BlocksOffice composer updates if you haven’t before.

```./dockerfiles/composer_update```

5.) Import a BO database into MySQL if you currently don’t have one. 

MySQL on your docker container can be accessed via the following credentials:

**Host** - 127.0.0.2

**Username** - root

**Password** - root

A SQL dump file can be found at the S3 link below which contains a copy of every single current BO installation (you need to be connected to the MM VPN to access it). 

https://s3.amazonaws.com/docker-application-files/blocksoffice/dbs.sql.zip

To import it into your MySQL container via a single liner, run the following command:

```curl -O https://s3.amazonaws.com/docker-application-files/blocksoffice/dbs.sql.zip && unzip dbs.sql.zip && docker exec -it symfony bash -c "yum install -y mysql && mysql -h mysqlserver -u root --password=root < /var/www/blocksoffice/dbs.sql"```

If you don’t need every single BO client database installed and you’d like a quicker download/import, hit up a developer and they should be able to help you out.

6.) Install BlocksOffice assets and run schema updates if you haven’t before.

```./dockerfiles/assets_schema_update```

7.) Comment out line 54-57 in web/app_dev.php. (BlocksOffice hacks...)

8.) Access sites using the .docker extension via the app_dev.php framework. 

![alt text](https://s3.amazonaws.com/docker-application-files/readme-assets/Screen+Shot+2017-10-12+at+16.49.34.png "Screenshot 1")

## Advanced Usage

### Running docker containers can be viewed by typing the following:

```docker ps```

### You can connect to a container via its name using the docker exec command.

```docker exec -it symfony bash```

Processes on the symfony container are managed by Supervisor. If you want to restart the processes being managed you should connect to the symfony containers shell and run the following:

```supervisorctl restart all```

### Clearing the Varnish cache.

```./dockerfiles/varnish_clear```

This will purge the entire cache and can be considered the equivalent of restarting varnish on a standard ec2 instance. For more granular cache clearing functionality connect to the varnish container and refer to the varnishadm documentation found here - https://varnish-cache.org/docs/3.0/tutorial/purging.html

### Connecting PHPStorm to Docker.

1.) Open preferences and navigate to Languages & Frameworks -> PHP -> Servers. Alter the settings as shown in the screenshot below and apply.

![alt text](https://s3.amazonaws.com/docker-application-files/readme-assets/Screen+Shot+2017-10-06+at+17.48.13.png "Screenshot 2")

2.) Navigate to Run -> Edit Configurations.

![alt text](https://s3.amazonaws.com/docker-application-files/readme-assets/Screen+Shot+2017-10-06+at+17.50.00.png "Screenshot 3")

3.) Add a new **PHP Remote Debug** configuration.

![alt text](https://s3.amazonaws.com/docker-application-files/readme-assets/Screen+Shot+2017-10-06+at+17.50.26.png "Screenshot 4")

4.) Enter the following details into the configuration and then click apply/ok.

![alt text](https://s3.amazonaws.com/docker-application-files/readme-assets/Screen+Shot+2017-10-06+at+17.52.50.png "Screenshot 5")

5.) You will now be able to turn on the xdebug listener in PHPStorm and intercept connections to your local Docker environment.















