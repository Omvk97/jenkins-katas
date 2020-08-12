# Create and setup a Jenkins server

## Initial start
TESTING
In order for us to start using Jenkins, we need a Jenkins server.
Most Linux distributions have it in their own package repository, or you can download it directly from [Jenkins.io](https://jenkins.io/download/).

In this exercise, we are going to spin up a Jenkins instance through [Docker](https://www.docker.com/) and docker-compose.

Make sure that you do not have anything listening on port 8080, in case there are any existing docker container from other exercises. (*docker ps will help you see if any container is occupying the port*)

## Tasks

* Fork the repository to your own github account.
* Clone the forked repository on your machine. (If you are using a provided instance, clone the same repository down to your machine as well).
* On the instance `cd` into the repository folder

* Set the admin password to something only you know. It is stored in the folder `secret` and is called admin.txt. You can use nano for changing the content of the file.

* Run `docker-compose up -d` to run the jenkins docker image
* Examine that the container is starting by issuing a `docker-compose ps` and see that the state of the container is `up` like the below example

```bash
$ docker-compose ps
           Name                          Command               State                                    Ports
-----------------------------------------------------------------------------------------------------------------------------------------------
jenkins-katas_jenkins_1   /sbin/tini -- /usr/local/b ...   Up      0.0.0.0:50000->50000/tcp, 0.0.0.0:8080->8080/tcp, 0.0.0.0:8443->8443/tcp
```

## Navigate to jenkins, is it working?

You should now be able to navigate to your Jenkins instance! Go to `http://<your-hostname>:8080` and you will be presented with a screen like the one below. If you're trying this on you own computer using docker, you'll use `localhost` as `<your-hostname>` otherwise for the purpose of this excercise, use the provided public hostname/ip.

![Welcome page](../img/welcome2.png)

Click login, and use the username `admin` and your own generated password.