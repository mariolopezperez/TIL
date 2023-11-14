#### Mount filesystem via SSHFS (ro) and scan with Thor

```
 sshfs -o ro,reconnect,allow_other admin@<device_hostname>:/ /mnt/<Device_Name>
```

Scan with ThorScanner the locally mounted fs
```
 sudo ./thor-linux-64 --lab -p /mnt/<Device_Name> --virtual-map /mnt/<Device_Name>:/ -j <Device_Name>:
```
