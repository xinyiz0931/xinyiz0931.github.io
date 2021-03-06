## ROS Multiple Machines via SSH

2021/5/26

Let's assume we have two computers, their hostnames are `server` and `client`. We want to run ROS master on computer `server`, and connect with this ROS master on computer `client`. Once this communication is establish, we can use computer `client` to echo rostopic or call rosservice, or other things. 

### 1. Establish physical connections

This is very simple, we may use a Network switch or router to connect computers. 

### 2. Verify username, hostname and IP of your computers

You can find username and hostname on ubuntu by terminal, you can see some thing like 

```
xinyi@server
```

`xinyi` would be your username and `server` would be your hostname. Also, you can find the host name by


```
vi /etc/hostname
```


and you can find the IP by


```
ifconfig
```

In our case, we find one computer with the hostname `server` and IP `10.0.1.1`, another with the hostname `client` and IP `10.0.1.2`. 

Next thing, on computer `server` you can run `ping 10.0.1.2` while on computer `client` you can run `ping 10.0.1.1` to check if their can communicate each other. 

### 3. Link the hostname and IP

Open `vi /etc/hosts` you may see things like 

```
127.0.0.1 localhost              
127.0.1.1 server
```


Append another computer IP and name on it.       
On computer `server`, you add `10.0.1.2 client`.               
On computer `client`, you add `10.0.1.1 server`.                 

### 4. Install and start ssh

Install using 


```
sudo apt install openssh-server
```

and check ssh server is started or not by 

```
sudo systemctl status ssh
```

### 5. Verify ssh connection

By using `ssh username@ip_address` or `ssh hostname`, you can connect other computer. We need to ensure both our computers can run this command.         
On computer `server`, you run `ssh username@10.0.1.2`, or `ssh username@client`.               
On computer `client`, you run `ssh username@10.0.1.1`, or `ssh username@server`.    

### 6. Change ROS master name and IP

Only on the client computer, open `~/.bashrc` by 

```
sudo vi ~/.bashrc
```

Add this to the file: 

```
ROS_MASTER_URI=http://10.0.1.1:11311            
ROS_HOSTNAME=server
```

And finally 

```
source ~/.bashrc
```

### 7. Test

Now, everything is ready.         
On computer `server`, launch your ros master. And on computer `client`, try 

```
rospotic list
```

to check if it can connect. 

Notice that it is a permanent solution. It can work on multiple computers rather than only two. 




