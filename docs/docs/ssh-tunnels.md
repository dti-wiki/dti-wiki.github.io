# SSH Tunneling #

OpenSSH support some tunneling features, which helps achieve security use cases such as remote web service access without exposing ports on the internet, accessing servers behind NAT, exposing local ports to the internet.

## What is SSH tunneling? ##

SSH tunneling is a method to transport additional data streams within an existing SSH session. SSH tunneling helps achieve security use cases such as remote web service access without exposing port on the internet, accessing server behind NAT, exposing local port to the internet.

## Access a remote service securely over SSH Tunnel ##

There is a service running on remote server and it is not exposed to internet. Now you need to access this service without exposing the service to internet. SSH Tunnel can help in this situation. For security reasons we should only bind services to local interfaces. Or if really required bind it to interface connected to private network. Binding the services on internet interfaces is not recommended.

### Scenario ###

We have a production cloud server in AWS and it runs a web-server on port 9191 and the port 9191 is not accessible from internet.

We also have a dev cloud server in Rackspace and we need to download a file named "/dynamically_generate_pdf_files.tar.bz2" from the AWS production web-server (on port 9191) to the Rackspace dev server.

But the problem is that the port 9191 of the AWS production web-server is not accessible from internet, so Rackspace dev server cannot directly download the file.

Now, the only connection option between AWS production web-server and Rackspace dev server is an SSH connection.

### Procedure ####

Connect to the AWS production web-server over SSH

start the web-server service on the AWS production web-server

Lets use python one-liner web-server:

```
python3 -m http.server 9191
```

Now, let the python one-liner web-server run for the entire duration of the procedure.

Connect to the Rackspace dev server over SSH

then open a SSH connection to AWS production web-server tunneling to the port 9191 on AWS production web-server from the Rackspace dev server.

```
ssh -i /path/to/rsa_private_key -N -L 9191:127.0.0.1:9191 user@AWS_production_web-server
```
Then do:

```
curl http://127.0.0.1:9191/
```
