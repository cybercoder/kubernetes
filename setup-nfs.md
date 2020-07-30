### expand lvm to maximum:

```
sudo lvm
lvm> lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
lvm> exit
```

#### resize the file system:
```
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
```

## Install NFS server:

```
sudo apt install nfs-kernel-server -y
```
### Create an NFS Export Directory
```
sudo mkdir -p /srv/kubedata
sudo chown -R nobody:nogroup /srv/kubedata
```
### open the export file:
```
  sudo nano /etc/exports
```
#### add the following line for each worker node:

```
/srv/kubedata 192.168.88.50(rw,sync,no_root_squash,no_subtree_check,no_all_squash,insecure)
```

### Install nfs-common on each worker node:

```
sudo apt install nfs-common -y
```

### Restart NFS service

```
sudo systemctl restart nfs-kernel-server
```
