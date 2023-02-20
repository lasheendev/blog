---
title: " Oracle Cloud Free VPS: 4 CPUs | 24 GB of memory 🚀"
date: 2023-02-20
description: "How to get a free VPS from Oracle Cloud with 4 CPUs and 24 GB of memory"
draft: false
---

## A whopping 4 CPUs and 24 GB of memory for free!

Oracle offers a generous free tier for its cloud service. This is a great opportunity for you to try out Cloud Dev. In this post, I will show you how to get a free VPS from Oracle Cloud. 

I personally host NexCloud on there and it runs flawlessly 

## What can you do with it?
There will be an upcoming blog post about this topic. But, in short, you can:
-   [Host a Minecraft server](https://blogs.oracle.com/developers/post/how-to-set-up-and-run-a-really-powerful-free-minecraft-server-in-the-cloud)
-   Host a website
-   Host a blog like this one
-   Host your NextCloud instance
-   Setup a remote development and work machine that you can access from anywhere with VSCode

## Step 1: Create an account

You can create an account at [https://signup.cloud.oracle.com/](https://signup.cloud.oracle.com/). You will need to provide your email address and a credit card to verify your account. But don't worry, you will not be charged for anything. After that, you will be able to login to your account

## Step 2: Create a VM


After login into your account click Instances From the Oracle Cloud Compute menu,  then click Create an instance

Enter a name then click Edit in the Image and shape section.

Click the Oracle-provided image, then select the Ubuntu 20.04 image(or whichever you like), then click Select image.

Click Change shape, then increase the OCPU count to 4, and the amount of memory to 24 GB, which is the maximum available under the Always Free resources, then click Select shape.

Under Networking, create or select a Virtual Cloud Network (VCN) with default settings. Ensure that the Assign a public IPv4 address option is selected.

For the SSH option, you may create yourself using PuTTy or use the auto-generated one from Oracle, don't** forget to** save the SSH key** as you need it to remote to your server.

For the Boot Volume, you may input 200 GB as it is the highest possible you can use as an Always Free tier: 

## Step 3: Connect to VM

You may connect to your VM by SSH with the private key you generated from the last step with the user “ubuntu”.You OpenSSH in Windows/Mac/Linux to SSH to the server like below.
```bash
ssh -i <path to private key> ubuntu@<public ip address>
```

Or you can use PuTTy on Windows if you prefer a GUI. I found this guide for it [here](https://www.cuit.columbia.edu/putty)

## Step 4: Update your VM

After you are in, you may try `sudo apt update & apt upgrade -y` to update all software on your machine.

## Step 5: Flex your vm's power
```bash
sudo apt install neofetch
neofetch
```

Then maybe run a stress test


```bash
sudo apt install stress-ng
stress-ng --cpu 4 --vm 4 --vm-bytes 24G --timeout 10s
```


## And that is all. 
You now are the proud owner of your own VM. Just some notes to consider


### Idle Compute Instances **Important**

Idle Always Free compute instances may be reclaimed by Oracle. Oracle will deem virtual machine and bare metal compute instances as idle if, during a 7-day period, the following are true:

-   CPU utilization for the 95th percentile is less than 10%
-   Network utilization is less than 10%
-   Memory utilization is less than 10% _(applies to [A1 shapes](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm#Details_of_the_Always_Free_Compute_instance__a1_flex) only)_

Thank you for reading! If you have any questions, feel free to open an issue on my GitHub repo.       
