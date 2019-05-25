# ChipCerto

Panel developed for CHAN_DONGLE Adminstaration

### INSTALLATION :

Dependencies:
        Asterisk 1.8 or higher
        Chan_dongle


### STEP BY STEP

### edit  /etc/asterisk/dongle.conf like following: 
(You can edit configs as required.)
```sh
[general]
interval=5

[defaults]
rxgain=0
txgain=0
autodeletesms=no
resetdongle=yes
u2diag=0
usecallingpres=yes
callingpres=allowed_passed_screen
disablesms=no
language=en
smsaspdu=yes
mindtmfgap=0
mindtmfduration=0
mindtmfinterval=0
callwaiting=no
initstate=start
dtmfmode=auto
disallow=all
allow=alaw,ulaw,gsm
context=chipcerto_in
disable=no
```
### Add permision to /etc/asterisk/manager.conf
```sh
[magnus]
secret = magnussolution
deny=0.0.0.0/0.0.0.0
permit=127.0.0.1/255.255.255.0
read = system,call,log,verbose,agent,user,config,dtmf,reporting,cdr,dialplan
write = system,call,agent,user,config,command,reporting,originate
```

###Create configuration files. (Just execute the commands below)

```sh
echo '' > /etc/asterisk/chipcerto_dongle.conf
echo '[redirectchipcerto]
exten => _55341011.,1,Dial(dongle/r5/041${EXTEN:8})
    same => n,hangup()      

[chipcerto]
exten => _0ZX[6-9]X.,1,NoOp(######CONSULTA DA PORTABILIDADE######)
same => n,Dial(SIP/portabilidadecelular/${EXTEN})
same => n,CONGESTION(0)
same => n,Hangup()' > /etc/asterisk/chipcerto_extensions.conf
```
```sh
echo '' > /etc/asterisk/chipcerto_extensions_in.conf
```
```sh
echo '
[portabilidadecelular] 
 type = peer 
 fromdomain = sip.portabilidadecelular.com 
 host = sip.portabilidadecelular.com 
 port = 5060 
 defaultuser = user 
 username = user 
 fromuser = user 
 secret = pass 
 context = redirectchipcerto' > /etc/asterisk/chipcerto_sip.conf
```
```sh
echo '
[operadoras] 
     55341 = Tim 
     55321 = Claro 
     55331 = Oi
     55320 = Vivo 
     553 = Outras Operadoras 
[rotas] 
     55341 = r5,041,11 
     55321 = Modulo01,021,11 
     55314 = r3,031,11 
     55320 = r3,015,11 
     553 = Modulo10,015,11 
[portabilidade] 
     type = sip 
     username = user 
     password = pass 
[access] 
     username = admin 
     password = magnus 
' > /etc/asterisk/chipcerto.conf
```

### INCLUDE FILES IN ASTERISK

```sh
echo '#include chipcerto_sip.conf' >> /etc/asterisk/sip.conf
echo '#include chipcerto_extensions.conf ' >> /etc/asterisk/extensions.conf
echo '#include chipcerto_extensions_in.conf ' >> /etc/asterisk/extensions.conf
echo '#include chipcerto_dongle.conf ' >> /etc/asterisk/dongle.conf
 ``` 

### DOWNLOAD THE PANEL. within the web directory. 
 /var/www/html

 ``` sh
git clone https://github.com/atefsaeed2010/ChipCerto.git
 ``` 
6 - Give read/write permision on directory /etc/asterisk to user apache.
if you created asterisk user during installation, vyou can change user and group of apache.
Use these commands:

on Centos:
 ``` sh
sed -i "s/User apache/User asterisk/" /etc/httpd/conf/httpd.conf
sed -i "s/Group apache/Group asterisk/" /etc/httpd/conf/httpd.conf
``` 

on Debian:
 ``` sh
sed -i 's/User User ${APACHE_RUN_USER}/User asterisk/' /etc/apache2/apache2.conf
sed -i 's/Group User ${APACHE_RUN_GROUP}/Group asterisk/' /etc/apache2/apache2.conf 
```
### restart APACHE
Centos
```sh
service httpd restart
```
Debian
``` sh
service apache2 restart
```

### Copy the AGI to the asterisk folder
Centos
```sh
cp -rf /var/www/html/ChipCerto/phpagi/* /var/lib/asterisk/agi-bin
cp -rf /var/www/html/ChipCerto/phpagi/ChipCertoCredito.gsm /var/lib/asterisk/sounds
```

Debian
```sh
cp -rf /var/www/ChipCerto/phpagi/* /var/lib/asterisk/agi-bin
cp -rf /var/www/ChipCerto/phpagi/ChipCertoCredito.gsm /var/lib/asterisk/sounds
```

### PERMISSION OF AGI
```sh
chmod +x /var/lib/asterisk/agi-bin/portabilidadecelular.php
```

### Permission to create PHP sessions
```sh
chown -R asterisk:asterisk /var/lib/php/session/
chown -R asterisk:asterisk /etc/asterisk
```

>Access your dashboard at http://your_ip/ChipCerto

>USERNAME: admin | PASSWORD: magnus

Use https://github.com/atefsaeed2010/ChipCerto/issues to report errors.

support: atefsaeed2010@gmail.com
