# Docker Deep Dive

---

## Andrew Pruski

### SQL Server DBA, Microsoft Data Platform MVP, & Certified Kubernetes Administrator

<a href="https://twitter.com/dbafromthecold">@dbafromthecold</a><br>
www.dbafromthecold.com<br>
<a href="https://github.com/dbafromthecold">github.com/dbafromthecold.com</a>

---

## Session Aim

To provide a deeper knowledge of the Docker platform

---

## Agenda

- Isolation<br>
- Networking<br>
- Persisting data<br>
- Custom images<br>
- Docker Compose<br>

---

# Isolation

---

## Container Isolation

>Containers isolate software from its environment and ensure that it works uniformly 
>despite differences for instance between development and staging
https://www.docker.com/resources/what-container

---

## Control Groups

Ensures a single container cannot consume all<br>
resources of the host<br>
Implements resource limiting of:-
- CPU
- Memory

---

## Namespaces

Control what a container can see<br>
Used to control:-<br>
- Hostname within the container
- Processes that the container can see
- Mapping users in the container to users on the host

---

## File system

- Containers cannot see the entire host's filesystem<br>
- They can only see a subset of that filesystem<br>
- The container root directory is changed upon start up

---

# Demo

---

# Networking

---

## Default networks

<img src="images/docker_default_networks.png" style="float: right"/>

- bridge<br>
- host<br>
- none<br>

---

## Bridge network

Default network<br>
Represents _docker0_ network in the host network stack<br>
Containers communicate by IP address<br>
Supports port mapping 

---

## User defined networks

Docker provide multiple drivers<br>
Enables DNS resolution of container names to IP addresses<br>
Can be connected to more than one network<br>
Connect/disconnect from networks without restarting<br>

---

# Demo

---

# Persisting data

---

## Options for persisting data

- Bind mounts<br>
- Data volume containers<br>
- Named volumes

---

# Demo

---

# Custom images

---

## Building your own image

- Custom images built from a file containing instructions<br>
- Known as a dockerfile<br>
- Customise the image to grant permissions to directories<br>
- Add databases to SQL Server, ready to go when container starts<br>

---

## Dockerfile

<pre><code data-line-numbers="1|3|5-8|10|12|14">FROM mcr.microsoft.com/mssql/server:2019-CU5-ubuntu-18.04

USER root

RUN mkdir /var/opt/sqlserver
RUN mkdir /var/opt/sqlserver/sqldata
RUN mkdir /var/opt/sqlserver/sqllog
RUN mkdir /var/opt/sqlserver/sqlbackups

RUN chown -R mssql /var/opt/sqlserver

USER mssql

CMD /opt/mssql/bin/sqlservr
</pre></code>

---

# Demo

---

# Docker Compose

<pre><code data-line-numbers="1|2|3-8|9|10-13|14|15">docker run -d
--publish 15789:1433
--env SA_PASSWORD=Testing1122
--env ACCEPT_EULA=Y
--env MSSQL_AGENT_ENABLED=True
--env MSSQL_DATA_DIR=/var/opt/sqlserver/sqldata
--env MSSQL_LOG_DIR=/var/opt/sqlserver/sqllog
--env MSSQL_BACKUP_DIR=/var/opt/sqlserver/sqlbackups
--network sqlserver
--volume sqlsystem:/var/opt/mssql
--volume sqldata:/var/opt/sqlserver/sqldata
--volume sqllog:/var/opt/sqlserver/sqllog
--volume sqlbackup:/var/opt/sqlserver/sqlbackups
--name sqlcontainer1
mcr.microsoft.com/mssql/server:2019-CU5-ubuntu-18.04
</pre></code>

---

## What is Compose?

>Compose is a tool for defining and running multi-container Docker applications.
>With Compose, you use a YAML file to configure your application’s services.
>Then, with a single command, you create and start all the services from your configuration.](docs.docker.com/compose

---

# Demo

---

## Resources

https://github.com/dbafromthecold/DockerDeepDive<br>
http://tinyurl.com/y3x29t3j/summary-of-my-container-series<br>
https://github.com/dbafromthecold/SqlServerAndContainersGuide

<p align="center">
<img src="images/dockerdeepdive_qr_code.png" />
</p>
