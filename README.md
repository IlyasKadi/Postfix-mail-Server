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

<div align="center">
    <img src="images/Postfix-conf0.png">
</div>

Now enter the fully qualified domain name that you want to use for send and receive emails. In our case, it is `ataman.me`.

<div align="center">
    <img src="images/Postfix-conf1.png">
</div>

Once Postfix installed, it will automatically start and creates a new /etc/postfix/main.cf file. You can verify the status of the service using the following commands.

```sh
sudo systemctl status postfix
```

<div align="center">
    <img src="images/Postfix-status.png">
</div>



## Checking-the-repository-and-identification-of-the-configuration-files
### role-of-main-cf
### role-of-master-cf

## Creation-of-the-database
### Creation-of-the-users
## Installation-of-dovecot-pop-imap

## Test-the-configuration


<p align="right">(<a href="#top">back to top</a>)</p>



Out Team - [AIT EL KADI Ilyas](https://github.com/IlyasKadi) - [AZIZ Oussama](https://github.com/ATAMAN0)

Project Link: [https://github.com/IlyasKadi/Postfix-mail-Server](https://github.com/IlyasKadi/Postfix-mail-Server)

<p align="right">(<a href="#top">back to top</a>)</p>
