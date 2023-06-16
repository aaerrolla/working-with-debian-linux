# Useful Commands

 1. Add  user to a group & check if user is part of group.


```
sudo usermod -aG <groupname> <username>
```
Example:


check current groups a user belongs to

```
id
```
Below command will add user anjan to group lxd


```
sudo usermod -aG lxd anjan
```

Logout and Login  to see the new group taking effect.

or

run below command 

```
newgrp lxd
```

2. Check user anjan is part of  lxd group

```
id
```
Or

```
getent group lxd
```

3. using && 

```
egrep -q 'vmx|svm' /proc/cpuinfo && echo yes || echo no
```
