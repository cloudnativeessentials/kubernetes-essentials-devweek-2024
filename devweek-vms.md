SSH into a VM with
`ssh ec2-user@<IP>`

Type `yes` when prompted

Expected output:
```shell
The authenticity of host '<ip> (<ip>)' can't be established.
ED25519 key fingerprint is SHA256:<SHA>.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '<ip>' (ED25519) to the list of known hosts.
Register this system with Red Hat Insights: insights-client --register
Create an account or view all your systems at https://red.ht/insights-dashboard
```

Each VM can have only 1 SSH login.
If you receive the following message then try another IP:
```shell
Register this system with Red Hat Insights: insights-client --register
Create an account or view all your systems at https://red.ht/insights-dashboard
There were too many logins for 'ec2-user'.
Last login: Wed Feb 21 04:40:40 2024 from <source-ip>
Connection to <ip> closed.
```


| IP          | 
| ----------- | 
| 10.10.10.3  | 
| 10.10.10.4  | 