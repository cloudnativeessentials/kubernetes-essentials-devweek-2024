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

---
`<ip>`   <br/>

35.94.83.232
35.87.161.85
35.85.248.87
35.85.245.120
35.83.249.213
44.234.33.39
34.223.103.190
34.223.90.132
35.84.28.244
35.88.47.144
44.226.204.209
35.80.3.108	
35.81.151.33
35.83.249.82
18.246.45.215
35.88.45.129
44.242.138.82
35.84.184.137
35.94.81.147
44.234.48.3
