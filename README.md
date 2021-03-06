**Automated install for OpenNMS Horizon**

This implementation will use Ansible to login, install docker packages, clone a repo that contains the required docker structure and run docker-compose

I preffered this way with docker-compose instead of a 100% Ansible implementation because it will easy allow independent development and testing as well as a future split in 2 independent repositories.

**Prerequisites**
- One Ansible control node: a machine with Ansible installed and configured to connect to your Ansible hosts using SSH keys.
- make sure your ssh key is present on target inventory
`ssh-copy-id user@host`
- no password sudo.
For automated installs is recommended to have sudo with no password
https://linuxhandbook.com/sudo-without-password/
An user that is part of docker group can easy become root however there is also an option to have the password in a vault
- clone this repository on the control node
- create your hosts file by copying from hosts.sample then edit the ansible hosts file and replace [prod.host] with your target host fqdn or ip
`cd ansible && cp hosts.sample hosts && vi hosts`

**Usage**

- Install Docker:
`ansible-playbook -l opennms1 -i hosts docker.yml`

- Create structure, and start Postgress and Horizon:
`ansible-playbook -l opennms1 -i hosts horizon.yml`

- You can also run all install and start services in one shot:
`ansible-playbook -l opennms1 -i hosts main.yml`

**Test**

You should be able to login to target host over http http://prod.host:8980

TODO
- create an env deployment script that will take file.env.sample fileas and deploy them as file.env. In the process random a random DB password should be generated.
- create a tear down playbook
- make DB and Horizon independent. This can be easy achieved as is with 2 tasks using docker-compose commands.
- Fix log errors - horizon_1 postgres_1 | 2021-10-28 21:45:41.967 GMT [76] FATAL: role "postgres" does not exist

Issues
- opennms horizon seem to need datadir and etc writable for its user (10001). If not present and writable will exit with 127 code
