# Snap Firefox cannot create user data directory

This issue is only reported on firefox installed from snap.
And this issue will only present itself if the user is having a custom $HOME directory.

Usually snap is configuring Ubuntu AppArmor with DEFAULT HOME DIRECTORY path for ex: `/home/some_username` .

But in your case if you are using custom AD integration in your Ubuntu then you may be having custom home directory like `/home/local/AD_DOMAIN/username` .

## Possibile Fix

If you use domain with realm, so our path home is not `/home` , and instead home path like `/home/MYDOMAINCOMPANY/`. 

You need to edit your `/etc/apparmor.d/tunables/home.d/ubuntu` and add below line:

```
@{HOMEDIRS}+=/home/MYDOMAINCOMPANY/
```

After save, you need to restart some services:

```
sudo systemctl restart apparmor.service snapd.apparmor.service snapd.service snapd.socket
```

This should fix the issues with firefox.


