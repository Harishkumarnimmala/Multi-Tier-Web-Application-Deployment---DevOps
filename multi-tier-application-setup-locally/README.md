# 🧑‍💻 Website Setup on CentOS Server

Here we are going to set up a basic website on a CentOS virtual machine.

---

## 📋 Prerequisites
- JDK 17 or 21
- Maven 3.9
- MySQL 8

---

## 💻 Technologies Used
- Spring MVC
- Spring Security
- Spring Data JPA
- Maven
- JSP
- Tomcat
- MySQL
- Memcached
- RabbitMQ
- Elasticsearch

---

## 🗃️ Database
We use **MySQL DB**.

### 📂 SQL Dump File Location
- `/src/main/resources/db_backup.sql`

### 💡 To Import the Dump into MySQL:
```bash
mysql -u <user_name> -p accounts < db_backup.sql
```

Replace `<user_name>` with your MySQL user.

---

## 🖥️ VM Setup Overview (Vagrant + VMware)
This project consists of multiple VMs defined in the `Vagrantfile`.

| VM Name | Role        | Box                          | IP Address       |
|---------|-------------|-------------------------------|------------------|
| `db01`  | MySQL DB    | CentOS Stream 9 (ARM)        | `192.168.56.25`  |
| `mc01`  | Memcached   | CentOS Stream 9 (ARM)        | `192.168.56.24`  |
| `rmq01` | RabbitMQ    | CentOS Stream 9 (ARM)        | `192.168.56.23`  |
| `app01` | App Server  | CentOS Stream 9 (ARM)        | `192.168.56.22`  |
| `web01` | Nginx Frontend | Ubuntu ARM                | `192.168.56.21`  |

All VMs are configured with **private networking** and provisioned through VMware Desktop on ARM-based Macs.

---

## 🚀 Useful Commands

# 1. Vagrant commands
| Action                | Command                                       |
|-----------------------|-----------------------------------------------|
| Check existing VM's      | `vagrant global-status`                     |
| Delete existing VM's     | `vagrant destroy -f <vmid here>`     -->  `vagrant global-status --prune`  |
| To reboot VM's           | `vagrant reload`  |

# 1. Msql commands
| Action                | Command                                       |
|-----------------------|-----------------------------------------------|
| To login      | `mysql -u root -p`                     |
| Delete existing VM's     | `vagrant destroy -f <vmid here>`     -->  `vagrant global-status --prune`  |
| To reboot VM's           | `vagrant reload`  |


## to copy files from source repo go to the source repo path and use below command
cp -r . "/Users/harishkumarnimmala/Desktop/New Projects/Multi-Tier-Web-Application-Deployment---DevOps/multi-tier-application-setup-locally"

## By any chance if we copied .git from source repo first we need to delete and reinitalize it using below commands
rm -rf multi-tier-application-setup-locally/.git
git add multi-tier-application-setup-locally





---

## 🐞 Troubleshooting

- ❌ **Tomcat doesn't start**  
  wget https://archive.apache.org/dist/tomcat/tomcat-10/v10.1.26/bin/apache-tomcat-10.1.26.tar.gz     (This path is correct)

  1. Nginx unable t reach tomcat webpage due to firewall issue which is blocking port 8080 do below steps 
  sudo firewall-cmd --permanent --add-port=8080/tcp
  sudo firewall-cmd --reload



- ❌ **MySQL connection refused**  
  💡 error to give previlage access 
  `GRANT ALL PRIVILEGES ON accounts.* TO 'admin'@'%' IDENTIFIED BY 'admin123';`

  2. When db vm has issues to listening propably firewall blocks it we need to enable them 
  sudo firewall-cmd --permanent --add-port=3306/tcp
  sudo firewall-cmd --reload


- ❌ **App throws DB connection error**  
  💡 Confirm credentials in `application.properties` or `.env`.

---

## 📄 Notes

- Always run `mvn clean install` after changing source code.
- Use consistent IP ranges when modifying the `Vagrantfile`.
- On ARM-based Macs, prefer boxes from the Vagrant Cloud with ARM support.



## ISSUE 1: 502 Bad Gateway from NGINX
Problem: NGINX on web01 showed "502 Bad Gateway" instead of loading the app from Tomcat (app01).

Steps Taken:

Verified NGINX config (/etc/nginx/sites-enabled/default):
upstream vproapp {
server app01:8080;
}
server {
listen 80;
location / {
proxy_pass http://vproapp;
}
}

From web01, ping and curl app01:
ping app01
curl http://app01:8080

On app01, checked if Tomcat is running:
sudo ss -tuln | grep 8080

Verified Tomcat web root directory:
ls /usr/local/tomcat/webapps/ROOT

Re-deployed app:
systemctl stop tomcat
rm -rf /usr/local/tomcat/webapps/ROOT*
cp target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
chown -R tomcat:tomcat /usr/local/tomcat/webapps
systemctl start tomcat

Issue Fixed: Application UI accessible via NGINX.




## ISSUE 2: MySQL DB Not Reachable from App
Problem: App VM (app01) couldn't connect to DB on db01.

Steps Taken:

From app01, ping and test port 3306:
ping db01
nc -zv db01 3306 (failed with "No route to host")

On db01, checked DB port:
sudo ss -tuln | grep 3306

Firewall was blocking:
sudo firewall-cmd --list-all
sudo firewall-cmd --list-ports

Opened port 3306:
sudo firewall-cmd --permanent --add-port=3306/tcp
sudo firewall-cmd --reload

Re-tested:
nc -zv db01 3306
mysql -h db01 -u admin -p

In MySQL, verified user access:
SELECT host, user FROM mysql.user WHERE user = 'admin';

Issue Fixed: Application able to reach DB and login/register requests working.

Extra Notes:
/etc/hosts was properly managed by vagrant-hostmanager plugin.

Tomcat logs reviewed at /usr/local/tomcat/logs/catalina.<date>.log

User admin_vp was verified in DB, and password hash updated if needed.

Make sure .vagrant and VM binaries are excluded from .gitignore.

---

## 🙌 Author

Harish Kumar Nimmala  
[GitHub](https://github.com/Harishkumarnimmala) .

---

## My tokens
1. For db01 : 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 


