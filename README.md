Ansible Playbook deploying Django-CMS
=====================================

### This instructions will
- Add new AWS ssh key;
- Configure Security Groups;
- Create pulic EC2 and private RDS instances;
- Install Nginx, uwsgi, sqlclient, etc, upload configs;
- Create new unix user;
- Create virtualenv and install Django-CMS under newly created user;
- Copy init-script and run uwsgi application as a system service (systemd or upstart).

### Credentials
Assuming that you have set your AWS credentials in environment variables like so...
```sh
$ export AWS_ACCESS_KEY_ID='YOUR_AWS_API_KEY'
$ export AWS_SECRET_ACCESS_KEY='YOUR_AWS_API_SECRET_KEY'
```
...and have your SSH keys present in
```sh
~/.ssh/id_rsa.pub
~/.ssh/id_rsa
```

### Configuration:
Edit the `hosts.yml` file inside the `group_vars` directory:
```yaml
# User

username: Petro
user_email: Petro@poshta.domain
pub_key_file: ~/.ssh/id_rsa.pub # For EC2 and new unix-user
user_password: 123456789 # CMS-Password

# Application

app_name: Django-CMS
project_url: kurwa.space
project_dir: "/home/{{ username }}/projects/{{ project_url }}"
uwsgi_port: 9000 # Not used by default. Using unix-socket instead

#AWS

region: eu-central-1
vpc_id: vpc-a77a00ce
vpc_subnet_id: subnet-95f283ee

# EC2

ec2_instance_type: t2.micro
ec2_image: ami-87564feb # Ubuntu Server 14.04

# DB in RDS or elsewhere

rds_instance_type: db.t2.micro
backup_retention_period: 7
mysql_provider: "{{ rds.instance.endpoint }}" #change to hostname if needed
mysql_connection: "mysql://{{  mysql_user }}:{{ mysql_user_password }}@{{ mysql_provider }}/{{ mysql_database }}"

mysql_user: Petro
mysql_user_password: 123456789
mysql_database: django_cms
mysql_root_user: root
mysql_root_password: 123456789

```
