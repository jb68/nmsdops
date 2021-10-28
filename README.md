Prerequisets
- make sure your ssh key is present on target inventory
`ssh-copy-id julian@40.90.252.221`

For automated installs is reccomended to have sudo with no password
https://linuxhandbook.com/sudo-without-password/
An user that is part of docker group can easy become root however there is also an option to have the password in a vault
For this assigment the password needs to be entered?


Install Docker
- ansible-playbook -l opennms1 -i hosts docker.yml  --ask-become-pass


TODO
- Fix log errors - horizon_1  postgres_1  | 2021-10-28 21:45:41.967 GMT [76] FATAL:  role "postgres" does not exist

Issues
- opennms seem to need datadir and etc writable for its user. If not present and writable will exit with 127 code  