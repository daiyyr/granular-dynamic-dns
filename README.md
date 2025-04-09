# granular-dynamic-dns
Regularly update R53 records to point to local machine(s) public IP. Each machine use a different AWS user which can only update a specific DNS


# New machine setup
- Update workflows/deploy-dns-updaters.yaml, add new machine nick name to MachineNameList e.g. home1,machine2,machine3. The Github workflow should be triggered to create the new user with policies
- Go to aws console to get AWS Secret Access Key and AWS Access Key ID for that user
- SSH to the new machine, install aws cli and configure AWS CLI with the above AWS credentials. Depending on the OS, you may need to use the relevant package manager to install the AWS CLI. Home Assistant Operating System for Raspberry Pi is based on Alpine Linux, so we use apk:

```
apk add aws-cli
aws config
```

- copy the folder scripts/dynamic-dns to /opt/
- add a crontab record:

```
crontab -e

* * * * * /opt/dynamic-dns/update-dns.sh >> /opt/dynamic-dns/update-dns.log 2>&1
```
