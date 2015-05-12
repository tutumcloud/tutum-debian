tutum-debian
============

[![Deploy to Tutum](https://s.tutum.co/deploy-to-tutum.svg)](https://dashboard.tutum.co/stack/deploy/)

Simple Debian docker images with SSH access


Usage
-----

To create the image `tutum/debian` with one tag per Debian release,
execute the following commands on the tutum-debian folder:

    docker build -t tutum/debian:latest .
    docker build -t tutum/debian:squeeze squeeze/
    docker build -t tutum/debian:wheezy wheezy/
    docker build -t tutum/debian:jessie jessie/


Running tutum/debian
--------------------

To run a container from the image you created earlier with the `wheezy` tag
binding it to port 2222 in all interfaces, execute:

	docker run -d -p 2222:22 tutum/debian:wheezy

The first time that you run your container, a random password will be generated
for user `root`. To get the password, check the logs of the container by running:

	docker logs <CONTAINER_ID>

You will see an output like the following:

	========================================================================
	You can now connect to this Debian container via SSH using:

	    ssh -p <port> root@<host>
	and enter the root password 'U0iSGVUCr7W3' when prompted

	Please remember to change the above password as soon as possible!
	========================================================================

In this case, `U0iSGVUCr7W3` is the password allocated to the `root` user.

Done!


Setting a specific password for the root account
------------------------------------------------

If you want to use a preset password instead of a random generated one, you can
set the environment variable `ROOT_PASS` to your specific password when running the container:

	docker run -d -p 2222:22 -e ROOT_PASS="mypass" tutum/debian:wheezy


Adding SSH authorized keys
--------------------------

If you want to use your SSH key to login, you can use the `AUTHORIZED_KEYS` environment variable. You can add more than one public key separating them by `,`:

    docker run -d -p 2222:22 -e AUTHORIZED_KEYS="`cat ~/.ssh/id_rsa.pub`" tutum/debian:latest
