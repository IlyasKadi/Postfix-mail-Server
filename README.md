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
This is the architecture of the data base that we will create :

![DBdesugn](https://user-images.githubusercontent.com/80456274/150172608-f8937c1c-e87b-49c9-afd4-5f2a9a6b0b16.jpg)


Let's first restart mysqlserver (if it exists of course otherwise an installation is required).
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

We have already create the database (mailserver) :

![mailserver_db_shown](https://user-images.githubusercontent.com/80456274/150167331-bc445417-905d-4f2e-b828-7f4e8749a9ac.jpg)

Now we will connect to the (mailserver) DB, and those are the table created :

![mailserver_tables](https://user-images.githubusercontent.com/80456274/150167605-9134e7fe-9c74-4885-8010-ae103e11ca8b.jpg)

### Creation-of-the-users
This is the code for the table of the users
```mysql
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

```mysql
INSERT INTO virtual_Users (domain_name,email,password,fullname,department) VALUES ('hvthang.xyz','test1@hvthang.xyz',TO_BASE64(UNHEX(SHA2('test1', 512))),'Test 1','Test');

INSERT INTO virtual_Users (domain_name,email,password,fullname,department) VALUES ('hvthang.xyz','test2@hvthang.xyz',TO_BASE64(UNHEX(SHA2('test2', 512))),'Test 2','Test');

```
> UNHEX() function performs the opposite operation of HEX() wich returns a string representation of a hexadecimal value of a decimal or string value specified as an argument.

> Base64 encoding schemes are commonly used when there is a need to encode binary data that needs be stored and transferred over media that are designed to deal with textual data.


Users_table :

![users_table](https://user-images.githubusercontent.com/80456274/150171859-fd61bfc7-b602-414b-8378-2ee2bbfbf094.jpg)


## Installation-of-dovecot-pop-imap
To install Dovecot and its modules

```sh
apt install dovecot-core dovecot-imapd dovecot-pop3 dovecot-lmtpd dovecot-mysql-y
```

```sh
systemctl start dovecot
```

```sh
systemctl status dovecot
```

![dovecot_status](https://user-images.githubusercontent.com/80456274/150186592-91c8fd6e-377f-4824-b73a-e083d0b716fc.jpg)

Now for the configuration of Dovecot server :
```sh
cd /etc/dovecot
ls
```

![etc_dov_ls](https://user-images.githubusercontent.com/80456274/150186934-1a55ce2b-bb0e-4401-b379-4d0ec1e28163.jpg)

In the conf-file dovecot.conf:
We will add the protocols imap pop3 lmtp (Local Mail Transfer Protocol (LMTP) is an alternative to (Extended) Simple Mail Transfer Protocol)
```sh
# Enable installed protocols
!include_try /usr/share/dovecot/protocols.d/*.protocol
protocols = imap pop3 lmtp
```
In the conf file dovecot-sql.conf.ext:
We will add the driver and we will connect this file to the mailserver DB that we created before, and we will specify the password query and the format in which the password is stored in pssds database for the query:

```sh
driver = mysql
connnect = host=127.0.0.1 dbname =mailserver user =mailuser password=2445
password_query = SELECT email as user,  password FROM virtual_Users WHERE email='%u' and status_id=1;
defaukt_pass_shceme = SHA512
```
> We wil add serveral settings to the dovecot conf files to link between all the configs together.

Next we are going enable smpt for authenticated users and authentication to dovecot in the main_conf file of postfix server `main.cf` in `etc/postfix`

```sh
smtp_sasl_type = dovecot
smtp_sasl_path = private/auth
smtp_sasl_auth_enable = yes
smtpd_sasl_auth_enable = yes
broken_sasl_auth_clients = yes
smtpd_sasl_authenticated_header = yes

```
We're almost there all we need is to restart the postfix and the dovcecot servers
```sh
systemctl restart dovecot
systemctl restart postfix
```
And last but not least we make sure that ufw is disabled otherwise we aloow the port 25,110 and 145 for SMTP,POP and IMAP
## Test-the-configuration
To test if everything is OK, we will use telnet to send a mail from one user to another:
First we are going use SMTP to send a message :

![telnet_25](https://user-images.githubusercontent.com/80456274/150193543-7460028b-a3fb-4373-973a-c46292b56800.jpg)

Then we will use dovecot to login and check if the message is in the mailbox :

![ehlo_ouss_to_ilyas](https://user-images.githubusercontent.com/80456274/150195615-6442376b-efc9-4710-aa31-7d990e4b1eb1.jpg)
> retr 6.. number 6 is the 6th message sent ( yes there was 5 tests before :| )

Everything is working just FIIIINE -__-


<p align="right">(<a href="#top">back to top</a>)</p>



Out Team - [AIT EL KADI Ilyas](https://github.com/IlyasKadi) - [AZIZ Oussama](https://github.com/ATAMAN0)

Project Link: [https://github.com/IlyasKadi/Postfix-mail-Server](https://github.com/IlyasKadi/Postfix-mail-Server)

<p align="right">(<a href="#top">back to top</a>)</p>
