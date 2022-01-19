<div id="top"></div>

<!-- PROJECT LOGO -->
<br />
<div align="center">
    <img src="images/logo.png" alt="Logo" width="700" height="400">
  <h2 align="center">Project 4</h2>
  <h3 align="center">Postfix mail server</h3>
</div>



<!-- TABLE OF CONTENTS -->

  <summary>Table of Contents</summary>
  <ol>
     <li><a href="#Problem-definition">Problem definition</a></li>
    <li><a href="#Requirements">Requirements</a></li>
    <li>
      <a href="#Part-I">Part I : Postfix configuration</a>
         <ul>
              <li><a href="#addition-the-mail-server-to-the-dns">Add a mail server to your DNS</a></li>       
              <li><a href="#Installation-the-Postfix-server">Install the Postfix server(SMTP)</a></li>
              <li><a href="#Checking-the-repository-and-identification-of-the-configuration-files">Check the repository /etc/postfix and identify the configuration files.</a></li>
                  <ul> 
                      <li><a href="#role-of-main-cf">Explain the role of the main.cf configuration file.</a></li>
                      <li><a href="#role-of-master-cf">Explain the role of the master.cf configuration file.</a></li> 
                  </ul>
             <li><a href="#Creation-of-the-database">Create a database (in mysql database) which will correspond and interact with Postfix.</a></li>
                     <ul> 
                          <li><a href="#Creation-of-the-users">Create users that will be associated with this database.</a></li>
                     </ul>
             <li><a href="#Creation-of-the-users">Create users that will be associated with this database.</a></li>
             <li><a href="#Installation-of-dovecot-pop-imap">Install the Dovecote (POP3/IMAP).</a></li>     
             <li><a href="#Test-the-configuration">Test configuration with a mail client (telnet, thunderbird).</a></li>
           </ul>
        </li>    
   </ol>



# Problem definition:

>  The electronic mail, e-mail or email, is a service for the transmission of
> messages sent electronically via a computer network to the electronic mailbox of a
> recipient chosen by the sender.
> 
>  Postfix is a popular open-source Mail Transfer Agent (MTA) that can be used
> to route and deliver email on a Linux system. Several enterprises desire to use their
> own mail server for different purposes. Whether it is for security purposes or to be
> able to exchange messages locally, Postfix is always a fitting solution.


# Requirements

* Linux,
* Mysql,
* Postfix,

# Part-I

## addition-the-mail-server-to-the-dns
## Installation-the-Postfix-server

```sh
sudo apt-get install postfix
```

During installation, you will be asked to choose the type of mail configuration, choose “Internet Site”.

> We can select the No configuration option if we want to keep the default Postfix settings. The Internet site allows us to send and receive emails using SMTP. Therefore, we select the second option as shown in the following screenshot.

![postfix__i](https://user-images.githubusercontent.com/80456274/150162090-bfe1d679-b597-4cf8-8a30-a896a10a0a10.png)

Now enter the fully qualified domain name that you want to use for send and receive emails. In our case, it is `ataman.me`.

![post_ataman](https://user-images.githubusercontent.com/80456274/150162162-80efceba-b930-4c34-a868-acfc899c2095.png)

Once Postfix installed, it will automatically start and creates a new /etc/postfix/main.cf file. You can verify the status of the service using the following commands.

```sh
sudo systemctl status postfix
```

![postfix_status](https://user-images.githubusercontent.com/80456274/150162115-56e1d88f-051b-4081-9447-c5ac8d29bf2d.jpg)




## Checking-the-repository-and-identification-of-the-configuration-files
### role-of-main-cf
![main_cf__1](https://user-images.githubusercontent.com/80456274/150164344-e0f2f147-48f5-4c98-a750-41eee7a9d4d3.jpg)


### role-of-master-cf

## Creation-of-the-database
Let's first restart mysqlserver (if it exists of course otherwise a installation is required).
```sh
sudo apt-get install mysql-server 
```
```sh
systemctl restart mysql
```
Checking the server
```sh
systemctl status mysql
```

![db_status](https://user-images.githubusercontent.com/80456274/150165111-8363e7f0-8037-4938-a72f-3155f4acd885.jpg)

MYSQL server is ready
```sh
mysql
```
![mariadb](https://user-images.githubusercontent.com/80456274/150166446-91370d8f-1637-4bbd-a3e2-414ec34ec109.jpg)

We have already create a database (mailserver) with the tables
![mailserver_db_shown](https://user-images.githubusercontent.com/80456274/150167331-bc445417-905d-4f2e-b828-7f4e8749a9ac.jpg)

Now we will connect to the (mailserver) DB, and those are the table created :
![mailserver_tables](https://user-images.githubusercontent.com/80456274/150167605-9134e7fe-9c74-4885-8010-ae103e11ca8b.jpg)

### Creation-of-the-users
This is the code for the table of the users
```mysql
### Table Virtual Users
CREATE TABLE virtual_Users (
	domain_name VARCHAR(100) not null,
	email VARCHAR(100) NOT NULL,
	password VARCHAR(106) NOT NULL,
	fullname VARCHAR(50) NOT NULL,
	department VARCHAR(50) NOT NULL,
	status_id INT NOT NULL DEFAULT 1,
PRIMARY KEY (email),
FOREIGN KEY (domain_name) REFERENCES virtual_Domains(domain_name) ON DELETE CASCADE,
FOREIGN KEY (status_id) REFERENCES virtual_Status(status_id) ON DELETE CASCADE
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
Then we will ad two users: oussama & ilyas, and for the password it is encrypted with a Secure Hash Algorithm : SHA2.
> UNHEX() function performs the opposite operation of HEX() wich returns a string representation of a hexadecimal value of a decimal or string value specified as an argument.
> Base64 encoding schemes are commonly used when there is a need to encode binary data that needs be stored and transferred over media that are designed to deal with textual data.

```mysql
INSERT INTO virtual_Users (domain_name,email,password,fullname,department) VALUES ('hvthang.xyz','test1@hvthang.xyz',TO_BASE64(UNHEX(SHA2('test1', 512))),'Test 1','Test');

INSERT INTO virtual_Users (domain_name,email,password,fullname,department) VALUES ('hvthang.xyz','test2@hvthang.xyz',TO_BASE64(UNHEX(SHA2('test2', 512))),'Test 2','Test');

```



## Installation-of-dovecot-pop-imap

## Test-the-configuration


<p align="right">(<a href="#top">back to top</a>)</p>



Out Team - [AIT EL KADI Ilyas](https://github.com/IlyasKadi) - [AZIZ Oussama](https://github.com/ATAMAN0)

Project Link: [https://github.com/IlyasKadi/Postfix-mail-Server](https://github.com/IlyasKadi/Postfix-mail-Server)

<p align="right">(<a href="#top">back to top</a>)</p>
