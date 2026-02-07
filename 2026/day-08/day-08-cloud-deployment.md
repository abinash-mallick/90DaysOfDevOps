# Day 08 — Deploying a Real Web Server on the Cloud

## Goal

Deploy a **real web server on the cloud** and learn practical server management.

## Tasks

- Launch a cloud instance (AWS EC2 or Utho)
- Connect via SSH
- Install Nginx
- Configure security groups for web access (port 80)
- Extract and save logs
- Verify the webpage from the internet

## Launching the EC2 Instance

Launched an EC2 instance by visiting the AWS Console.

![aws](img/Picture1.png)

## Connecting via SSH

Navigate to the folder where the private key is stored and run:

```bash
ssh -i <pemkey> ubuntu@44.243.47.62
```

## Installing Nginx

Run the following commands:

```bash
sudo apt-get update
sudo apt install nginx -y
```

![nginx](img/Picture2.png)

## Configuring Security Groups

Add inbound rule for **port 80** in the Security Group.

![SG](img/Picture3.png)

## Extracting Logs

Check nginx status:

```bash
systemctl status nginx
```

![nginx](img/Picture4.png)

Extract logs to file:

```bash
journalctl -u nginx > nginx.log
```

![service](img/Picture5.png)

Using **tee** to append logs:

```bash
journalctl -u nginx | tee -a nginx.log
```

![aws](img/Picture6.png)

## Verifying Webpage Accessibility

Open your browser and visit:

```
http://<ip_addressofec2>:80
```

![aws](img/Picture7.png)
