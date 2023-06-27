# Useful Commands

1. Add  user to a group.

```
sudo usermod -aG <groupname> <username>
```
Example  
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

2. Check if user is part of group.

    id command without any parameters lists all groups to which current user belongs to  

```
id
```
to list groups of specific user use  id command with user name
    
```
id <user-name>
```

Example

```
id irc
```

3. Check if a user is part of a group

Example 1 to list users belong to group lxd

```
getent group lxd
```

Example 2 , to list users in netdev group

```
getent group netdev
```


4. Using Logical AND && ,  OR ||

&& sometimes this is very handy,where you want to execute next command if only previous command results in  success.  

|| executes irrespective of status of previous command.

Example 

Below   if  egrep  is success then only   yes is printed else no  

```
egrep -q 'vmx|svm' /proc/cpuinfo && echo yes || echo no
```

5. Effective usage of dmesg.

    dmesg is very useful command to troubleshoot issues related to firmware , custom kernel modules , and any linux startup errors 

    General usage

    ```
    sudo dmesg
    ```

    is very verbose and takes time to get to the message what you looking for.

    alternative 

    ```
    sudo dmesg | grep -i error
    ```

    ok , but still not good 

    but dmesg has options that allow us to look what exactly we need, it allows us to print only message that belongs to emergency, alert , critical, error, warnings ,notice , info , debug.

    the option  is -l  you can provide emerg, alert, crit,err, warn, notice, info, debug 

    Example : to print all warnings 

    ```
    sudo dmesg -l warn
    ```

    logs can be further filtered based on facilities , facilities will tell if log from user space or kernel space , mail , daemon , auth , syslog , lpr , news 

    option is  -f 

    Example  to list the only warnings from kernel 

    ```
    sudo dmesg -l warn -f kern
    ```
    
    Screen cast. 


[<img src="https://img.youtube.com/vi/hoJq1jGXclY/hqdefault.jpg" width="600" height="300"
/>](https://www.youtube.com/embed/hoJq1jGXclY)
