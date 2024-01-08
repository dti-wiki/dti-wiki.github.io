# Magento 2x Cloud DB Authentication

## Set Time

```
BKPTIM=$(date +"%Y-%m-%d_%H-%M-%S")
```

## Ticket Number

Optionally set ticket number if required.

```
TKT="JIRA-123"
```

## Set Authentication variables:

```
export DB_NAME=$(grep [\']db[\'] -A 20 app/etc/env.php | grep dbname | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/['][,]//");

export MYSQL_HOST=$(grep [\']db[\'] -A 20 app/etc/env.php | grep host | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/['][,]//");

export DB_USER=$(grep [\']db[\'] -A 20 app/etc/env.php | grep username | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/['][,]//");

export MYSQL_PWD=$(grep [\']db[\'] -A 20 app/etc/env.php | grep password | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/[']$//" | sed "s/['][,]//");
```

## Connect to MySQL with above set authentication variables.

```
mysql -h $MYSQL_HOST -u $DB_USER --password=$MYSQL_PWD $DB_NAME -A
```

## Database Dump

```
mysqldump -h $MYSQL_HOST -u $DB_USER --password=$MYSQL_PWD $DB_NAME --opt --single-transaction --skip-lock-tables | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | gzip > var/backups/$DB_NAME.${BKPTIM}.sql.gz
```

If you want to add the ticket name to the dump as well.

```
mysqldump -h $MYSQL_HOST -u $DB_USER --password=$MYSQL_PWD $DB_NAME --opt --single-transaction --skip-lock-tables | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | gzip > var/backups/$TKT.$DB_NAME.${BKPTIM}.sql.gz
```

## Other Useful Flags

In certian MySQL servers from verion 5.7 and above, we may get permission denied for some nonsensical privileges.

you can use the below mysql CLI flags to spuress those errors.

```
--set-gtid-purged=OFF --no-tablespaces
```
