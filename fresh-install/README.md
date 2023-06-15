#### After Fresh Install of the Debain
The first thing you want to do after fresh install of debain is to make your primary user as sudo
1. Adding user to sudo

    Right way to do it is , create a file by user name under /etc/sudoers.d
    ```
    echo "foo ALL=(ALL) ALL" > /etc/sudoers.d/foo
    ```
    foo : user, this can be group name also
    ALL : host , here ALL means any host ,  you can mention specific host if you want like localhost
    (ALL): run as user ,  here ALL means any user , you can provide a specific user like root
    ALL: run any command  ,  run all commands ,  you can restrict by providing list of commands sperated by , 
    
    Validate 
    ```
    cat /var/log/boot.log
    ```
    Vs
    ```
    sudo cat /var/log/boot.log
    ```
2. Installing Essential packages for development and building kernel and c, c++

  
  update apt repo
  
  ```
  sudo apt update
  ```
  Install 
  
  ```
  sudo apt install build-essentials
  ```
  
3. Other Essential Tools & packages 
  
  Install  curl , git, vim , jq, tree, 
  
  ```
  sudo apt install curl git vim jq  tree
  ```
  

