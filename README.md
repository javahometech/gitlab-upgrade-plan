# GitLab Upgrade Plan
### Upgradation path, upgrading gitlab from 12.8.1-ce.0

https://raaaimund.github.io/tech/2018/10/04/backup-and-restore-gitlab-on-docker/

```
upgrade to 12.10.14-ce.0
upgrade to 13.0.14-ce.0 --> 13.1.11-ce.0 --> 13.8.8-ce.0 --> 13.12.15-ce.0
upgrade to 14.0.12-ce.0 --> 14.4.2-ce.0
```

### Backup Gitlab local host
```bash
   docker exec -t <container-name> gitlab-rake gitlab:backup:create
   docker cp <container-name>:/var/opt/gitlab/backups/1636708049_2021_11_12_14.4.2_gitlab_backup.tar .
   docker cp ec2-user_web_1:/etc/gitlab/gitlab-secrets.json .
```

### Restore Gitlab from local host

```bash
    # Stop the processes that are connected to the database
    docker exec -it <name of container> gitlab-ctl stop puma
    docker exec -it <name of container> gitlab-ctl stop sidekiq

    # Verify that the processes are all down before continuing
    docker exec -it <name of container> gitlab-ctl status

    # Run the restore. NOTE: "_gitlab_backup.tar" is omitted from the name
    docker exec -it <name of container> gitlab-backup restore BACKUP=11493107454_2018_04_25_10.6.4-ce
   
    docker cp gitlab-secrets.json ec2-user_web_1:/etc/gitlab/gitlab-secrets.json
    
    # Restart the GitLab container
    docker restart <name of container>

    # Check GitLab
    docker exec -it <name of container> gitlab-rake gitlab:check SANITIZE=true
```
