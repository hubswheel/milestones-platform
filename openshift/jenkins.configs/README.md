# Configs for running jobs in Jenkins

## General

Jenkins does not provide a mechanism to export and import jobs.
There is a plugin that will copy jobs between running Jenkins instances
but nothing to export, archive or version job configs. The configs in
this folder are obtained by copying the files from the file system of
the Jenkins server.

## Getting the files from Jenkins

These files come from the Jenkins service running in a pod for the 
current project. Jenkins stores its files in directory
**/var/lib/jenkins/jobs/**

They were obtained using `oc rsync` with this general format:
```
oc rsync \
    jenkins-instance-id:/var/lib/jenkins/jobs/YourJobName/config.xml \
    /path/to/learn-infra/jenkins.configs/YourJobName
```

For example, this is the command that was used to copy
a config file from a running instance:

```
oc rsync \
    jenkins-2-qstth:/var/lib/jenkins/jobs/postgresqldump/config.xml \
    /Users/wr011/sites/milestones/milestones-platform//openshift/jenkins.configs/postgresqldump
```

## Moving files to Jenkins

1. Copy files to running Jenkins instance using `oc rsync`.
```
oc rsync \
    /path/to/learn-infra/jenkins.configs/YourJobName/ \
    jenkins-instance-id:/var/lib/jenkins/jobs/YourJobName
```

2. Use the Jenkins console to reload configuration from Disk.
   * From the Jenkins home page click on *Manage Jenkins*.
   * Click the link *Reload Configuration from Disk*.


## By the way ...

Avoid using spaces in the names of your jobs. You can do it but it 
makes the `oc rsync` command harder to read because you have to 
escape the space character or quote the file path.
