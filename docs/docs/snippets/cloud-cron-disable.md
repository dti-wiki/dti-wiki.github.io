# Disable Cron on Magento Enterprise Cloud

Usually crons are by default enabled in the magento configuration. If you have to disable the magento cloud crons.

Please modify `app/etc/env.php` and add `'enabled' => 0,` under "cron" array as below.

```
  'cron' =>
  array (
    'enabled' => 0,
  ),
```

Monitor the cron logs :

```
tail -f $HOME/var/log/cron.log
```
