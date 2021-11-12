# GitLab Upgrade Plan
### Upgradation path, upgrading gitlab from 12.8.1-ce.0
```
upgrade to 12.10.14-ce.0
upgrade to 13.0.14-ce.0 --> 13.1.11-ce.0 --> 13.8.8-ce.0 --> 13.12.15-ce.0
upgrade to 14.0.12-ce.0 --> 14.4.2-ce.0
```

### Backup Gitlab to S3
```bash
/opt/tools/gitlab/docker_compose/backup_gitlab.sh <gitlab_container_name> <gitlab_s3_bucket>
```

### Restore Gitlab from S3

```bash
   aws s3 cp s3://<s3-bucket>/path/to/backup/file .
   docker cp <gitlab-backup-file> dockercontainer:/<backup-location>
```

```bash
    # Stop the processes that are connected to the database
    docker exec -it <name of container> gitlab-ctl stop puma
    docker exec -it <name of container> gitlab-ctl stop sidekiq

    # Verify that the processes are all down before continuing
    docker exec -it <name of container> gitlab-ctl status

    # Run the restore. NOTE: "_gitlab_backup.tar" is omitted from the name
    docker exec -it <name of container> gitlab-backup restore BACKUP=11493107454_2018_04_25_10.6.4-ce

    # Restart the GitLab container
    docker restart <name of container>

    # Check GitLab
    docker exec -it <name of container> gitlab-rake gitlab:check SANITIZE=true
```
