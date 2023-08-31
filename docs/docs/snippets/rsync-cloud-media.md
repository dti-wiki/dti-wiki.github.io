# Rsync Media files in Magento Cloud

We may have to sync magnto media files from one server to another in magento cloud.

Magento cloud does not give us full permissions on their servers, we are left with restricted access.

## Warning

Make necessary backups of the media files on the destination remote server.

```
BKPTIM=$(date +"%Y-%m-%d_%H-%M-%S")
```

```
TKT="JIRA-123"
```

```
cd pub/
tar -zcf ../var/$TKT.media.backup.$BKPTIM.tar.gz media
```

## Procedure

Create a temporary SSH key file and add the SSH Public key in magento cloud project prortal under SSH Keys.


```
rsync -Pavz -e "ssh -i var/path/to/private_key_rsa" pub/media/* remote_ssh_user@remote.destination.server:/path/to/the/magento/pub/media/
```
