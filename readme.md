# Docker container connections via OpenSSH

![ssh containers](<ssh containers.jpeg>)

To establish the remote login with SSH protocol between multiple containers, OpenSSH can be used. The OpenSSH-server utility is installed on the server/master container and the OpenSSH-client utility is over the remote machines.

The easiest connection configurations are as follows:

1. Pull the Ubuntu container images from the docker registry.

```
docker pull ubuntu:latest
```

2. Run the container in detached mode
```
docker run -d --name master ubuntu tail -f /dev/null
```

tail -f /dev/null is used to keep the container running as the Ubuntu container will stop after all processes are executed.

3. Execute the bash shell 
```
docker exec -it master bash
```

4. Update the image & install OpenSSH-server & nano (text editor)
```
apt-get update && apt-get install openssh-server nano -y
```

5. Edit the OpenSSH configuration at /etc/ssh/sshd_config
```
nano /etc/ssh/sshd_config

# edit the permission to root login under Authentication, then save the file & review the changes

PermitRootLogin yes
``` 

7. Restart the ssh service & check status
```
service ssh start && service ssh status
```

8. Reset the default root login password
```
passwd root
```

9. Exit
```
exit
```

10. Grab the IP address of the master container
```
docker inspect master | grep IP 
```

11. Configure the other containers which will access the master 
```
docker run -d --name remote-container01 ubuntu tail -f /dev/null
```

12. Update & install client utility 
```
docker exec -it remote-container01 bash 

apt-get update && apt-get install openssh-client -y
```


13. Connect via remote ssh 
```
ssh root@<IP_of_master_ontainer>
```

To stop the containers use,
```
docker stop <conatiner_name>
```
