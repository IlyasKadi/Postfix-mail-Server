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
              <li><a href="#Installation-the-Postfix-server">Install the Postfix server</a></li>
              <li><a href="#Checking-the-repository-and-identification-of-the-configuration-files">Check the repository /etc/postfix and identify the configuration files.</a></li>
                  <ul> 
                      <li><a href="#role-of-main-cf">Explain the role of the main.cf configuration file.</a></li>
                      <li><a href="#role-of-master-cf">Explain the role of the master.cf configuration file.</a></li> 
                  </ul>
              <li><a href="#Installation-of-the-couriers-pop-and-imap">Install the courier-pop and courier-imap.</a></li>    
              <li><a href="#creation-of-the-database">Create a database (in mysql database) which will correspond and interact with Postfix.</a></li>
              <li><a href="#Create-of-the-user">Create a user who will be associated with this database.</a></li>
              <li><a href="#Test-the-configuration">Test configuration with a mail client (telnet, thunderbird, …).</a></li>
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

`sudo apt-get install postfix`

During installation, you will be asked to choose the type of mail configuration, choose “Internet Site”.

<div align="center">
    <img src="images/Postfix-conf0.png">
</div>

Now enter the fully qualified domain name that you want to use for send and receive emails.

<div align="center">
    <img src="images/Postfix-conf1.png">
</div>

Once Postfix installed, it will automatically start and creates a new /etc/postfix/main.cf file. You can verify the status of the service using the following commands.

`sudo systemctl status postfix`

<div align="center">
    <img src="images/Postfix-status.png">
</div>


<div align="center">
    <img src="![Postfix-conf0](https://user-images.githubusercontent.com/80456274/149037136-7393c375-f92a-4635-9367-97f6500b8c9f.png)">
</div>
## Checking-the-repository-and-identification-of-the-configuration-files
### role-of-main-cf
### role-of-master-cf
## Installation-of-the-couriers-pop-and-imap
## creation-of-the-database
## Create-of-the-user
## Test-the-configuration


<p align="right">(<a href="#top">back to top</a>)</p>



Out Team - [AIT EL KADI Ilyas](https://github.com/IlyasKadi) - [AZIZ Oussama](https://github.com/ATAMAN0)

Project Link: [https://github.com/IlyasKadi/Postfix-mail-Server](https://github.com/IlyasKadi/Postfix-mail-Server)

<p align="right">(<a href="#top">back to top</a>)</p>
