#Choix de l'OS
FROM ubuntu:latest

#MAJ
RUN apt-get update -y && apt-get upgrade -y
RUN apt-get install wget zip unzip nano -y
#RUN hostname wt1-11.ephec-ti.be

#Config system hostname & hosts
COPY hostname /etc/hostname
COPY hosts /etc/hosts

#Installation de mailutils
RUN apt-get install mailutils -y

#Installation de apache php
RUN apt-get install apache2 -y 
RUN apt-get install php -y

#Installation de postfix
RUN apt-get install postfix -y

#Installaion de dovecot
RUN DEBIAN_FRONTEND=noninteractive apt-get install dovecot-imapd dovecot-pop3d -y

#Installation de SquirrelMail
RUN wget https://sourceforge.net/projects/squirrelmail/files/stable/1.4.22/squirrelmail-webmail-1.4.22.zip
RUN unzip squirrelmail-webmail-1.4.22.zip
RUN mv squirrelmail-webmail-1.4.22 /var/www/html/
RUN chown -R www-data:www-data /var/www/html/squirrelmail-webmail-1.4.22/
RUN chmod 755 -R /var/www/html/squirrelmail-webmail-1.4.22/
RUN mv /var/www/html/squirrelmail-webmail-1.4.22/ /var/www/html/squirrelmail

#Ajoute directory et permissions de www-data
RUN mkdir /var/local/squirrelmail
RUN mkdir /var/local/squirrelmail/data
RUN chown -R www-data:www-data /var/local/squirrelmail/

#Config files
COPY main.cf /etc/postfix/main.cf
COPY config.php /var/www/html/squirrelmail/config/config.php
COPY mailname /etc/mailname
COPY init.sh /init.sh

#Open ports
EXPOSE 25 80 587 2525 465 25025 

#Mount
VOLUME ["/var/spool/postfix", "/var/spool/mail", "/var/www/html"]

#EntryPoint qui lance les services
ENTRYPOINT ["/init.sh"]
