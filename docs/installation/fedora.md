<!--[metadata]>
+++
title = "Installation on Fedora"
description = "Instructions for installing Docker on Fedora."
keywords = ["Docker, Docker documentation, Fedora, requirements,  linux"]
[menu.main]
parent = "smn_linux"
+++
<![end-metadata]-->

# Fedora

Docker is supported on the following versions of Fedora:

- Fedora 20 
- Fedora 21
- Fedora 22 

This page instructs you to install using Docker-managed release packages and
installation mechanisms. Using these packages ensures you get the latest release
of Docker. If you wish to install using Fedora-managed packages, consult your
Fedora release documentation for information on Fedora's Docker support.

##Prerequisites

Docker requires a 64-bit installation regardless of your Fedora version. Also, your kernel must be 3.10 at minimum. To check your current kernel
version, open a terminal and use `uname -r` to display your kernel version:

    $ uname -r 
    3.19.5-100.fc20.x86_64

If your kernel is at a older version, you must update it.

Finally, is it recommended that you fully update your system. Please keep in
mind that your system should be fully patched to fix any potential kernel bugs. Any
reported kernel bugs may have already been fixed on the latest kernel packages 


## Install

You use the same installation procedure for all versions of Fedora,
only the package you install differs. Choose from these packages:

<style type="text/css">
  .tg  {border-collapse:collapse;border-spacing:0;}
   td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;vertical-align: top;}
   th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;vertical-align: top;}
</style>
<table class="tg">
  <tr>
    <th>Version</th>
    <th>Package name</th>
  </tr>
  <tr>
    <td>Fedora 20</td>
    <td>
        <p>
    <a href="https://get.docker.com/rpm/1.7.0/fedora-20/RPMS/x86_64/docker-engine-1.7.0-1.fc20.x86_64.rpm">
    docker-engine-1.7.0-1.fc20.x86_64.rpm</a>
        </p>
        <p>
    <a href="https://get.docker.com/rpm/1.7.0/fedora-20/SRPMS/docker-engine-1.7.0-1.fc20.src.rpm">
   docker-engine-1.7.0-1.fc20.src.rpm</a>
        </p>
    </td>
  </tr>
  <tr>
    <td>Fedora 21</td>
    <td>
    <p>
    <a href="https://get.docker.com/rpm/1.7.0/fedora-21/RPMS/x86_64/docker-engine-1.7.0-1.fc21.x86_64.rpm">
    docker-engine-1.7.0-1.fc21.x86_64.rpm</a>
        </p>
        <p>
    <a href="https://get.docker.com/rpm/1.7.0/fedora-21/SRPMS/docker-engine-1.7.0-1.fc21.src.rpm">
  docker-engine-1.7.0-1.fc21.src.rpm</a>
        </p>
    </td>
  </tr>
   <tr>
    <td>Fedora 22</td>
    <td>
    <p>
    <a href="https://get.docker.com/rpm/1.7.0/fedora-22/RPMS/x86_64/docker-engine-1.7.0-1.fc22.x86_64.rpm">
    docker-engine-1.7.0-1.fc22.x86_64.rpm</a>
        </p>
        <p>
    <a href="https://get.docker.com/rpm/1.7.0/fedora-22/SRPMS/docker-engine-1.7.0-1.fc22.src.rpm">
    docker-engine-1.7.0-1.fc22.src.rpm</a>
        </p>
    </td>
  </tr> 
</table>


This procedure depicts an installation on version 21. If you are installing on
20 or 22, substitute that package for your installation. 

1. Log into your machine as a user with `sudo` or `root` privileges.

2. Make sure you don't have an older version of Docker installed.

		$ yum list installed | grep docker
	  
	If you have an older version, remove it using the `yum -y remove <packagename>` command.

3. Download the Docker RPM to the current directory.
		
		$ curl -O -sSL https://url_to_package/docker-engine-1.7.0-0.1.fc21.x86_64.rpm

4. Use `yum` to install the package.

		$ sudo yum localinstall --nogpgcheck docker-engine-1.7.0-0.1.fc21.x86_64.rpm

5. Start the Docker daemon.

		$ sudo service docker start

6. Verify `docker` is installed correctly by running a test image in a container.

		$ sudo docker run hello-world
		Unable to find image 'hello-world:latest' locally
		latest: Pulling from hello-world
		a8219747be10: Pull complete 
		91c95931e552: Already exists 
		hello-world:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
		Digest: sha256:aa03e5d0d5553b4c3473e89c8619cf79df368babd18681cf5daeb82aab55838d
		Status: Downloaded newer image for hello-world:latest
		Hello from Docker.
		This message shows that your installation appears to be working correctly.

		To generate this message, Docker took the following steps:
		 1. The Docker client contacted the Docker daemon.
		 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
				(Assuming it was not already locally available.)
		 3. The Docker daemon created a new container from that image which runs the
				executable that produces the output you are currently reading.
		 4. The Docker daemon streamed that output to the Docker client, which sent it
				to your terminal.

		To try something more ambitious, you can run an Ubuntu container with:
		 $ docker run -it ubuntu bash

		For more examples and ideas, visit:
		 http://docs.docker.com/userguide/
 
## Create a docker group		

The `docker` daemon binds to a Unix socket instead of a TCP port. By default
that Unix socket is owned by the user `root` and other users can access it with
`sudo`. For this reason, `docker` daemon always runs as the `root` user.

To avoid having to use `sudo` when you use the `docker` command, create a Unix
group called `docker` and add users to it. When the `docker` daemon starts, it
makes the ownership of the Unix socket read/writable by the `docker` group.

>**Warning**: The `docker` group is equivalent to the `root` user; For details
>on how this impacts security in your system, see [*Docker Daemon Attack
>Surface*](/articles/security/#docker-daemon-attack-surface) for details.

To create the `docker` group and add your user:

1. Log into your system as a user with `sudo` privileges.

2. Create the `docker` group and add your user.

    `sudo usermod -aG docker your_username`

3. Log out and log back in.

    This ensures your user is running with the correct permissions.

4. Verify your work by running `docker` without `sudo`.

        $ docker run hello-world
				Unable to find image 'hello-world:latest' locally
				latest: Pulling from hello-world
				a8219747be10: Pull complete 
				91c95931e552: Already exists 
				hello-world:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
				Digest: sha256:aa03e5d0d5553b4c3473e89c8619cf79df368babd18681cf5daeb82aab55838d
				Status: Downloaded newer image for hello-world:latest
				Hello from Docker.
				This message shows that your installation appears to be working correctly.

				To generate this message, Docker took the following steps:
				 1. The Docker client contacted the Docker daemon.
				 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
						(Assuming it was not already locally available.)
				 3. The Docker daemon created a new container from that image which runs the
						executable that produces the output you are currently reading.
				 4. The Docker daemon streamed that output to the Docker client, which sent it
						to your terminal.

				To try something more ambitious, you can run an Ubuntu container with:
				 $ docker run -it ubuntu bash

				For more examples and ideas, visit:
				 http://docs.docker.com/userguide/
 
## Start the docker daemon at boot

To ensure Docker starts when you boot your system, do the following:

    $ sudo chkconfig docker on

If you need to add an HTTP Proxy, set a different directory or partition for the
Docker runtime files, or make other customizations, read our Systemd article to
learn how to [customize your Systemd Docker daemon options](/articles/systemd/).


## Uninstall

You can uninstall the Docker software with `yum`.  

1. List the package you have installed.

		$ yum list installed | grep docker
		yum list installed | grep docker
		docker-engine.x86_64                1.7.0-0.1.fc20
																																								 @/docker-engine-1.7.0-0.1.fc20.el6.x86_64

2. Remove the package.

		$ sudo yum -y remove docker-engine.x86_64 

	This command does not remove images, containers, volumes, or user-created
	configuration files on your host. 

3. To delete all images, containers, and volumes, run the following command:

		$ rm -rf /var/lib/docker

4. Locate and delete any user-created configuration files.
