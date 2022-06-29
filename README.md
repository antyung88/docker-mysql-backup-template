# docker-mysql-backup-template
Additional MySQL Container with Health Checks and Periodic Backups in docker-compose

# Usage
```
git clone https://github.com/antyung88/docker-compose-mysql-backup.git && cd docker-compose-mysql-backup
```
Edit the following for backup intervals and backups prune intervals
```
    # Database backups prune interval (replace with yours). Default is 7 days.
    # find /backup -type f -mtime +7 | xargs rm -f

    # Backup interval (replace with yours). Default is 1 day.
    # sleep 24h
    command: sh -c 'sleep 30m
             && while true; do
             mysqldump
             -h db
             -p database
             -u user
             --no-tablespaces
             --password=$$MYSQL_PASSWORD | gzip > /backup/backup-$$(date "+%Y-%m-%d_%H-%M").gz
             && find /backup -type f -mtime +7 | xargs rm -f;
             sleep 24h; done'
```
Deploy service:
```
docker-compose up -d
```
