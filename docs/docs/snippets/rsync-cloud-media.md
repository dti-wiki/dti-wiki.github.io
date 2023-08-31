# Rsync Media files in Magento Cloud

We may have to sync magento media files from one server to another in magento cloud.

Magento cloud does not give us full permissions on their servers, we are left with restricted access.

## Warning

Make necessary backups of the media files on the destination magento cloud server.

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

Create a temporary SSH key file on the source magento cloud server.

Add the SSH Public key in magento cloud project portal under SSH Keys.

Login to source magento cloud server and then run:

```
rsync -Pavz -e "ssh -i var/path/to/private_key_rsa" pub/media/* remote_ssh_user@remote.destination.server:/path/to/the/magento/pub/media/
```
