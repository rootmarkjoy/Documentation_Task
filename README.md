# Documentation_Task

1. Hostname changed:-
```sh
hostnamectl set-hostname jai.hopto.org
hostnamectl
```

2. Added Swap 4.0Gi Memory:-
```sh
free -h
sudo swapon --show
fallocate -l 4G /swapfile
ls -lh /swapfile
chmod 600 /swapfile
ls -lh /swapfile
mkswap /swapfile
swapon /swapfile
swapon --show
```
