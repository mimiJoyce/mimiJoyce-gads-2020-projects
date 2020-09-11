# Google Cloud Fundamentals: Getting Started with Compute Engine

## Overview
In this lab, you will create virtual machines (VMs) and connect to them. You will also create connections between the instances

## Objectives
In this lab, you will learn how to perform the following tasks:

- Create a compute Engine Virtual Machine using the Google Cloud Platform (GCP) Console.

- Create a compute Engine Virtual Machine using the gcloud command-line interface.

- Connect between the two instances. 

## Steps:

## 1. Create a Virtual Machine Using the gcloud command line

1. In GCP console, on the top right toolbar, click Activate Cloud Shell button ![see image](https://cdn.qwiklabs.com/vdY5e%2Fan9ZGXw5a%2FZMb1agpXhRGozsOadHURcR8thAQ%3D)

2. Click Continue. ![see image](https://cdn.qwiklabs.com/lr3PBRjWIrJ%2BMQnE8kCkOnRQQVgJnWSg4UWk16f0s%2FA%3D)

3. To create a VM instance called my-vm1
```
gcloud compute instances create "my-vm-1" \ 
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default" \
--tags http
```

```c
gcloud compute firewall-rules create allow-http \
--action=ALLOW \
--destination=INGRESS \
--rules=http:80 \
--target-tags=http
```

## 2. Create another Virtual Machine Using the gcloud command line

1. To display a list of all the zones in the region to which Qwiklabs assigned you, 

```c
gcloud compute zones list | grep us-central1
```

2. Choose a zone from that list other than the zone to which Qwiklabs assigned you. For example, if Qwiklabs assigned you to region us-central1 and zone us-central1-a you might choose zone us-central1-b

3. To set your default zone to the one you just chose, enter this partial command gcloud config set compute/zone followed by the zone you chose.

   Your completed command will look like this:

```c
gcloud config set compute/zone us-central1-b
```

4. To create a VM instance called my-vm-2 in that zone, execute this command:

```c
gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"
```

Note: The VM can take about two minutes to launch and be fully available for use.

## 3. Connect between VM instances.

 Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:

1. Connect SSH to my-vm-2:
```c
gcloud compute ssh my-vm-2
```

2. ping my-vm-1 from my-vm-2
```c
ping -c 4 my-vm-1
```
3. Use the ssh command to open a command prompt on my-vm-1 from my-vm-2:
```c
ssh my-vm-1
```
If you are prompted about whether you want to continue connecting to a host with unknown authenticity, enter yes to confirm that you do.

4. At the command prompt on my-vm-1, install the Nginx web server:

```c
sudo apt-get install nginx-light -y
```

5. Use the nano text editor to add a custom message to the home page of the web server:
```c
sudo nano /var/www/html/index.nginx-debian.html
```
6. Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:
```c
Hi from joyceMimi
```
7. Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor.

8. Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command
```c
curl http://localhost/
```
The response will be the HTML source of the web server's home page, including your line of custom text

9. To exit the command prompt on my-vm-1, execute this command:
```c
exit
```

### You will return to the command prompt on my-vm-2

1. To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:

```c
curl http://my-vm-1/
```
 The response will again be the HTML source of the web server's home page, including your line of custom text.

 2. Now get the external IP of the my-vm-1 instance from this command:
 ```c
 gcloud compute instances list --zone us-central1-a
 ```
 3. Paste the copied ip address of my-vm-1 into a new browser tab and hit enter.

 Result:  The response will again be the HTML source of the web server's home page, including your line of custom text

```c				
from: Mimi Joyce Addingi
```