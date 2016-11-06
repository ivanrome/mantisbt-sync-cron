# mantisbt-sync-cron

Cron job container from [webwurst/docker-go-cron](https://github.com/webwurst/docker-go-cron) to launch [mantisbt-sync-core](https://github.com/jrrdev/mantisbt-sync-core) jobs


The following environment variables must be provided :

* BASE_URL=http://mantisbt-sync-core:8080/batch
* SCHEDULE : based on [robfig/cron](https://github.com/robfig/cron) syntax. See [here](https://github.com/robfig/cron/blob/master/doc.go)
* COMMAND=/mantis-sync-cron/tasks.sh <job>, where <job> is one of : syncEnumsJob, syncProjectsJob, syncIssuesJob
* MANTIS_USERNAME : user name used to connect to MantisBT. Left it empty if anonymous access is used
* MANTIS_PASSWORD : password used to connect to MantisBT. Left it empty if anonymous access is used
* MANTIS\_PROJECT_ID : MantisBT project id

The following configuration will run issues sync job every 6 hours :

```YAML
mantisbt-sync-cron:
    image: jrrdev/mantisbt-sync-cron:latest
    environment:
        - BASE_URL=http://mantisbt-sync-core:8080/batch
        - SCHEDULE=@every 6h
        - COMMAND=/mantis-sync-cron/tasks.sh syncIssuesJob
	- MANTIS_USERNAME=
	- MANTIS_PASSWORD=
	- MANTIS_PROJECT_ID=1
    ports:
        - "18080"
    links:
        - mantisbt-sync-core
```

** Note : the cron container can only launch one command. If multiple jobs scheduling is required, just add one container per job**