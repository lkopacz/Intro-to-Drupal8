#Thanks for your interests in learning Drupal 8. To setup your computer, please follow the instructions.

## Install Vagrant and VirtualBox

If you do not have either installed, go to the respective links to set them up: https://www.vagrantup.com/downloads.html and https://www.virtualbox.org/wiki/Downloads

If you have either of those installed, please make sure that they are both up to date!

## Download Scotchbox

To download Scotchbox you will need to have access to Git. If you don't please download git at the following link: https://git-scm.com/downloads

On the command line, type cd into the directory where you would like your sites stored and copy and paste `git clone https://github.com/scotch-io/scotch-box.git my-project`. Replace `my-project` with whatever you'd like to name this. I have named my repository `Drupal-8-Demo`

## Run Vagrant

`cd` into your new project clone and run `vagrant up`. Depending on your computer, this can take a bit of time, so be patient! If you set up your site successfully, you can go to 192.168.33.10 and see a scotchbox site!

## Setting up Database access

If you have a Mac, I would highly recommend downloading SequelPro https://sequelpro.com/. If you have a Windows, I hear Navicat is good as well https://www.navicat.com/download. Once you download that, navigate to the program and setup your database (SSH Connection) with the following credentials:

- Name: Scotchbox/Vagrant/Whaever you want to call it
- Host: 127.0.0.1
- Database Name: (leave blank)
- Database User: root
- Database Password: root
- SSH Host: 192.168.33.10
- SSH User: vagrant
- SSH Password: vagrant

Now you should be able to connect to MySQL. You can either create a database using your respective programs, or you can do it manually through your vagrant box. To set it up manually:

- Make sure you are in your vagrant box. From your vagrant directory type: `vagrant ssh` and press enter.
- Once you are in your box, type `mysql` and press enter
- To see what databases exist, type `SHOW DATABASES;` and press enter. Don't forget the semi-colon!
- You should see something like this:
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| scotchbox          |
+--------------------+
4 rows in set (0.01 sec)
```
- To create a new database type `CREATE DATABASE drupal_demo;` and press enter. You should get confirmation saying that 1 row was affected, or something similar.
- If you created your database successfully you will see it when you type in `SHOW DATABASES;`
- To exit out of MySQL type `exit`, and to exit out of the Vagrant box, type `exit`
- Congrats! Now let's set up Drupal!

# Setting up all the things.

- `cd` into the Vagrant directory and type ls to see what other files there are. You should see a `public` directory and a `Vagrantfile`
- Go to drupal.org and get a zip or tar of the website https://www.drupal.org/project/drupal/releases/8.2.6. Unzip it and put the unzipped directory in the same directory. Once you are done you should have public, drupal (or whatever you named it) and Vagrantfile, you can put as many sites in here as you'd like!

### Setting up Hosts

- In order to get Drupal site to work, we will have to go into our hosts file and update the names to make sure scotchbox can tell which site is which.
- We need to go back inside our box, so type `vagrant ssh`
- Once you are in the box type: `cd /etc/apache2/sites-available/` and press enter. If you type ls you should see three files: 000-default.conf, default-ssl.conf, and scotchbox.local.conf.
- We are going to need to make a copy of the scotchbox.local.conf file. To do this from the command line, type `sudo cp scotchbox.local.conf drupal-demo.dev.conf` or whatever you would like the new file to be named. After drupal-demo, I would recommend having a .dev or .local before the .conf to make it less confusing.
- We need to edit the new file we created. To do this we can type `sudo vi drupal-demo.dev.conf` and you will see our virtual hosts file.
- To edit this file, you will need to press "I" (for insert). All we need to do is change anything that has scotchbox.local to drupal-demo.dev (or whatever we named our site).
- In my case, ServerName should be drupal-demo.dev, ServerAlias should be www.drupal-demo.dev and DocumentRoot should be /var/www/drupal-demo where drupal-demo is what you named your DRUPAL project (not the vagrant project overall) that you placed alongside the public directory and the Vagrantfile.
- To save this file, press Esc, then type :wq and press enter.
- Now within your box type `sudo a2ensite drupal-demo.dev.conf` or whatever you named that file and press enter.
- After that it will prompt you to reload apache2, so type `service apache2 reload`
- This is all we need to do in the Vagrant box, so type `exit`
- Now we need to set up the configuration of our hosts file.
- Type `sudo vi /etc/hosts` from the command line.
- Press "I" to edit this file, and at the bottom of this file create a vagrant hosts area.
- Type 192.168.33.10 drupal-demo.dev Under the scotchbox configuration. Press Esc to and :wq to save.
- Now you should be able to go to drupal-demo.dev on your browser to start setting up your drupal site.

## Setting up Drupal

- You will start getting prompted through the screens.
- Choose Language, you can select English. For choose profile, You can do standard.
- Verify Requirements, you can click 'continue anyway'
- For the database configuration, you can type your database name and the user root and password root. Then when you Save and Continue, it will start installing your Drupal site.
- You can put in the information, but I would uncheck the email notifications, since this is a local site.
- Congrats, you have set up your first Drupal Site.
