### Working with ufw

1. Install ufw

```
sudo apt update && sudo apt install ufw -y
```
2. Check status of ufw firewall

```
sudo ufw status
```
3. Enable ufw

```
sudo ufw enable
```

4. Set default rule 

deny all incoming traffic & enable all outgoing traffic,
good enough for a personal use linux.

```
sudo ufw default allow outgoing
sudo ufw default deny incoming
```


