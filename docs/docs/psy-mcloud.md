# Psychedelic Magento Cloud

Adobe Commerce Cloud formerly known as Magento Cloud is a Platform as a Service (PaaS) environment for magento 2x family of ecommerce framework. The PaaS is hosted by Adobe on Platform.sh. Customers are given access to the hosted server via restricted SSH acecss. There is only a common SSH user for all user account's created under magento-cloud. The SSH user do not have a dedicated `$HOME` directory. The "Psychedelic MCloud" is a group of custom scripts created to create an overlay over these restrctions and give bash history and other integrations.

## Source

The Psychedelic Magento Cloud can be found at:

[Psychedelic Magento Cloud Source](https://github.com/anishcorratech/psychedelic-mcloud)

## Details

Psychedelic MCloud contains customizations for:

- bash history
- tmux integration
- mysql backups
- ngrok integration (partial)

### Bash

The bash customization is for enabling bash history, when invoked from the terminal multiplexer (tmux).

### tmux

The tux customization contains the basic `tmux.conf` file and a custom `shell` to enable bash overrides.

The `tmux.conf` try to use a custom shell from the file `shell` from the above bash bash customization.
The `tmux.conf` also have support for mouse support for various versions of tmux found in new-generation and old-generation cloud servers.

#### Initiate tmux

```
tmux -f var/corra/tmux/tmux.conf new -s corra_devops_01
```

### MySQL Scripts

There are two mysql customizations in this directory.
The `myconnect` is used to connect to mysql by directly reading `app/etc/env.php`.
Also `mydbdump` is a mysql backup script.

### ngrok

This is used for media sync using ngrok
