# Container Init

At times, we think if only this container acted a little different that its normal.

say for example:

- we need to change the default user defined in `dockerfile` but at the same time we don't want to rebuild the dockerimage.
- install new packages in a docker image, yet we don't want to build a new image using the source of the original docker image.
- sometimes when you are lazy and you want to directly manipulate a docker image

## The Script

Then you may need "con-init" or `coninit.sh` a.k.a "Container Init"

This is a `coninit.sh` for alpine linux based containers.

``` bash
#!/bin/bash

UID=1000
GID=1000
USERNAME=anish

#install dependencies
apk update
apk add shadow sudo bash curl yarn jq

#Create User and Groups and Sudo Access
groupadd -g ${GID} ${USERNAME}
useradd ${USERNAME} -u ${UID} -g ${GID} -m -s /bin/bash
usermod -a -G wheel ${USERNAME}
echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/${USERNAME}

#Run infinitely
tail -f /dev/null
```

If we are using a debian/ubuntu container then the above (for alpine) won't work as expected. So in case of Debian based containers use below.

```
#!/bin/bash

CUID=1000
CGID=1000
USERNAME=anish

#install dependencies
apt update
apt install -y passwd sudo bash curl jq vim-tiny git

#Create User and Groups and Sudo Access
groupadd -g ${CGID} ${USERNAME}
useradd ${USERNAME} -u ${CUID} -g ${CGID} -m -s /bin/bash
UID_MAX=500000000 usermod -a -G adm,sudo ${USERNAME}
echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >>/etc/sudoers.d/${USERNAME}

#Run infinitely
tail -f /dev/null
```

## The Compose file

Now to use this `coninit.sh` we wil employ a `docker-compose.yml` file as below:

``` yaml
version: '3.8'
services:
  node-dev:
    image: node:16.16.0-slim
    container_name: node-dev
    working_dir: /srv
    environment:
      - VAR1="Foo"
      - VAR2="Bar"
    volumes:
      - ./con-init.sh:/root/con-init.sh
      - ./app:/srv
    command: sh /root/con-init.sh
```

Create a empty directory named "app" in the same directory which houses `docker-compose.yml` file.

you can put the code files in the newly created "app" directory.

Then do:

```
docker-compose up -d
```

now a new container named `node-dev` with start using the `node:16.16.0-slim` image with the changes we added in the `coninit.sh` file.

Now lets connect to the new container, assuming we created the user as `anish` in the `coninit.sh` file. If you used a different user replace `anish` with the username you used in the below command.

``` bash
docker-compose exec -u anish -it node-dev /bin/bash
```

