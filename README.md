govCMS 4 Docker
===============

This Dockerfile can be used to spin up a govCMS instance.


A. Commands - Create base govCMS env without default sites
-------

////unused -- docker pull mysql:5.6.25
# Instantiate a new MySQL container to house your database.

////unused -- docker run --name testdb -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=drupal -e MYSQL_USER=drupal -e MYSQL_PASSWORD=drupal -d mysql:5.6.25 -v /C/Hunter/mysql-data:/var/lib/mysql

# Create the govCMS container to pull in the codebase and run the webserver - linking it to the db container.

docker build -t govcms .

////unused --  docker run -p 8888:80 --name govcms_server --link testdb:testdb govcms
docker run -p 8888:80 --name govcms_server govcms
```
There is no need to use build.properties at this period


B. Install default govCMS website with an external mysql
-------

1. Installation can either be done using Phing/Drush from the command line or through the GUI in a web browser.
2. The probelm when I try to use command line in Dockerfile is that it always fail, so I have to use GUI in web browser
3. access http://localhost:8888
4. followed guide to install everything include the example website and give the settings of external mysql
5. access http://localhost:8888 then you will see the govcms


C. Make a usable docker image - this image will include the mysql settings. If you change the mysql IP or port, you need to redo B step and C step
### Command line (preferred)
docker ps
docker commit 6798e77672c1 brotherhui/govcms:externalmysql_version
docker push brotherhui/govcms:externalmysql_version


D. Other commands:
#Connect to the container
docker exec -ti govcms /bin/bash

#Run the install task
phing -f build/phing/build.xml install
```

### Browser based

* Find the IP address of your container

```
# When using a mac.
boot2docker ip
```

* Access the install pages at \<ip address\>:8888
* Fill in database information:
  * DB Username: drupal
  * DB Password: drupal
  * DB Name: drupal
  * DB Host: testdb
* Run through the rest of the install procedure as usual.
