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
  💡 Check logs in `/var/log/tomcat/` and ensure port 8080 isn't in use.

- ❌ **MySQL connection refused**  
  💡 error to give previlage access 
  `GRANT ALL PRIVILEGES ON accounts.* TO 'admin'@'%' IDENTIFIED BY 'admin123';`

- ❌ **App throws DB connection error**  
  💡 Confirm credentials in `application.properties` or `.env`.

---

## 📄 Notes

- Always run `mvn clean install` after changing source code.
- Use consistent IP ranges when modifying the `Vagrantfile`.
- On ARM-based Macs, prefer boxes from the Vagrant Cloud with ARM support.

---

## 🙌 Author

Harish Kumar Nimmala  
[GitHub](https://github.com/Harishkumarnimmala) .

---

## My tokens
1. For db01 : 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 


