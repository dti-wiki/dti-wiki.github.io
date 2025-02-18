# AppImage Error due to apparmor profiles

## The SUID sandbox helper binary was found, but is not configured correctly

For ex:

!!! info "Question?"

    After upgrading to 24.04, I get the "The SUID sandbox helper binary was found, but is not configured correctly." message when I try to run this Electron AppImage application file. The entire error looks like this (example for Obsidian app):

Error excerpt:

```
> ./Obsidian-1.4.13.AppImage


[21824:0430/094557.661436:FATAL:setuid_sandbox_host.cc(158)]
The SUID sandbox helper binary was found, but is not configured correctly.
Rather than run without sandboxing I'm aborting now. You need to make sure that /tmp/.mount_ObsidiOpBPaM/chrome-sandbox is owned by root and has mode 4755.
Trace/breakpoint trap (core dumped)
```

## FIX

Probably the best way to solve the problem is to create an apparmor profile which allows the application to make use of unprivileged usernamespaces.

Create the file named `/etc/apparmor.d/obsidianappimage` with the following content:

```
# This profile allows everything and only exists to give the
# application a name instead of having the label "unconfined"

abi <abi/4.0>,
include <tunables/global>

profile obsidianappimage /path/to/Obsidian-1.6.7.AppImage flags=(default_allow) {
  userns,

  # Site-specific additions and overrides. See local/README for details.
  include if exists <local/obsidianappimage>
}
```

Replace the `/path/to/Obsidian-1.6.7.AppImage` with the actual path to your appimage.

After saving the file run the below command to reload all apparmor profiles:

```
sudo systemctl reload apparmor.service
```

If this command does not work for you, simply reboot.

!!! warning "Do not move your AppImage file"

    Note: Moving the appimage to a different location later or changing it's name makes it neccessary to update your apparmor profile with the correct path and reload the apparmor profiles.

References : https://ubuntu.com/blog/ubuntu-23-10-restricted-unprivileged-user-namespaces
