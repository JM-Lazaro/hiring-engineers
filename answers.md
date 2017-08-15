# Table of Contents
1. [Example](#example)
2. [Example2](#example2)
3. [Third Example](#third-example)
4. [Intro](#intro)

## Example
## Example2
## Third Example
## Intro

Table of Contents<br /> 
[Introduction](#introduction)<br /> 
[Setting Up the Application Stack](#setting-up-the-application-stack)<br /> 
&ensp;&ensp;2.1 [Vagrant Ubuntu and VirtualBox](#vagrant-ubuntu-and-virtualbox)<br /> 
&ensp;&ensp;2.2 [MYSQL](#mysql)<br /> 
[Data Collection](#data-collection)<br /> 
&ensp;&ensp;3.1 [Datadog Sign Up](#datadog-sign-up)<br /> 
[What is an Agent?](#what-is-an-agent?)<br /> 
&ensp;&ensp;3.2 [Adding Tags to Host](#adding-tags-to-host)<br /> 
&ensp;&ensp;&ensp;&ensp;3.2.1 [Via Website](#via-website)<br /> 
&ensp;&ensp;&ensp;&ensp;3.2.2 [Via Config file](#via-config-file)<br /> 
&ensp;&ensp;3.3 [MySQL Integration](#mysql-integration)<br /> 
&ensp;&ensp;3.4 [Custom Check](#custom-check)<br /> 
&ensp;&ensp;3.5 [Database Integration Screenboard](#database-integration-screenboard)<br /> 
&ensp;&ensp;3.6 [Timeboard vs. Screenboard](#timeboard-vs.-screenboard)<br /> 
&ensp;&ensp;3.7 [Dashboard Cloning](#dashboard-cloning)<br /> 
&ensp;&ensp;3.8 [Custom Agent Check Timeboard](#custom-agent-timeboard)<br /> 
[Alerts and Monitoring](#alerts-and-monitoring)<br /> 
&ensp;&ensp;4.1 [Dashboard Snapshot and Notification](#dashboard-sanapshot-and-notification)<br /> 
&ensp;&ensp;4.2 [Alerting Your Data](#alerting-your-data)<br /> 
&ensp;&ensp;4.3 [Setting up a Monitor for the Test Metric](#setting-up-a-monitor-for-the-test-metric)<br /> 
&ensp;&ensp;4.4 [Monitoring Downtime](#monitoring-downtime)<br /> 













# Introduction

The purpose of this document is to display the applicant’s ability to complete basic Datadog Support Engineer tasks as listed in the Github Task.



# Setting Up the Application Stack

Firstly, set-up the application stack in your Mac OS X. It will consist of a Vagrant Ubuntu operating system hosted on a VirtualBox VM with a

## 	Vagrant Ubuntu and VirtualBox

a.	Go to the vagrant downloads site -https://www.vagrantup.com/downloads.html and choose the installer for Mac OS X.
b.	Open the downloaded DMG file and double-click on vagrant.pkg.
<img width="1440" alt="vagrant_install1" src="https://user-images.githubusercontent.com/30991348/29299031-46102182-81af-11e7-929c-65c84744d7d2.png">
c.	Follow the instructions, which are very straightforward. The only option that you may want to change is in the Destination Select step which is sometimes skipped while clicking next. When that happens, click on the Change Install Location during the Installation Type step:
<img width="1440" alt="vagrant_intstall2" src="https://user-images.githubusercontent.com/30991348/29299034-46671212-81af-11e7-8880-aade8752b944.png">
d.	You would see the message below if the installation is a success. If you encounter any issues, please check the amount of diskspace available on your computer or if your operating system’s compatibility.
<img width="1440" alt="vagrant_intstall3" src="https://user-images.githubusercontent.com/30991348/29299033-46618f04-81af-11e7-8a62-c72ff7fc9118.png">
e.	We will also be using VirtualBox for VM provisioning. To install, go to https://www.virtualbox.org/wiki/Downloads  and click on the OS X hosts option.
f.	Open the downloaded installer and double-click on VirtualBox.pkg.
<img width="1440" alt="virtualbox_install1" src="https://user-images.githubusercontent.com/30991348/29299035-46687e36-81af-11e7-9cbb-d8611e89fb4c.png">
g.	You would see the message below if the installation is a success.
<img width="1440" alt="virtualbox_install4" src="https://user-images.githubusercontent.com/30991348/29299032-46482eec-81af-11e7-9bc2-ef1526239789.png">
 


## 	MYSQL

a.	Open your terminal and open a vagrant virtual machine by executing the command:<br />
vagrant up

 

b.	Your Vagrant VM is now up and running. Connect to it by executing the command:<br />
vagrant ssh

 






c.	Before MySQL installation, make sure the system is updated using the commands:<br />
<br />
sudo apt-get update
<br />sudo apt-get upgrade  <---- this will take a while to download and install

d.	To install MySQL, run the command:<br />
sudo apt-get install mysql-server

 


e.	The MySQL server should be automatically up and running after the installation. To verify, check the mysql background process using:<br />
ps –ef | grep mysql

 

Also, log-in using the root access credentials and exit by entering ‘quit’:<br />
mysql –u root -p

 













# Data Collection

Now that we have set-up our application suite, we will now proceed in integrating Datadog to our system.


## 	Datadog Sign Up

a.	Go to datadog homepage: https://www.datadoghq.com and click the GET STARTED FOR FREE button:

 

b.	Fill-in the form that will pop-up with your details and click Sign up:

 









c.	In the next step, answer the surveys about the current softwares and services that you are currently using. This step is optional:
 









d.	For the final step, install a datadog agent to your host system. Select Ubuntu from the left side menu:
 



e.	Install curl before executing the script from the Datadog instructions by running the command below in the vagrant terminal:<br />
sudo apt-get install curl

 

f.	Copy the datadog installation command and run it in the terminal:<br />
<br />
DD_API_KEY=c08db2089f1d3ea2ee9f6238c2e87d12 bash -c "$(curl -L https://raw.githubusercontent.com/DataDog/dd-agent/master/packaging/datadog-agent/source/install_agent.sh)" DD_INSTALL_ONLY=true
	 

g.	Running this command would automatically start the datadog agent data collection for the Ubuntu server:

 





h.	After a few seconds, Datadog will receive the data from your host and you can now click on the Finish button in the lower right to complete the sign-up.

 

 








	 

	






## 	Adding Tags to Host

### 		Via Website

a.	From the left bar menu, mouse over on Infrastructure and click on “Infrastructure List”.
b.	On the upper right corner, click on Update Host Tags
c.	Click on Edit Tags and enter. 
 
	***Special characters other than hyphen or underscore will be replaced with an underscore.

**Host Map

 

### 		Via Config file
	
a.	From your vagrant terminal, go to the datadog configuration directory via command:<br />
/etc/dd-agent

b.	Open and edit the configuration file - datadog.conf<br />
sudo vi datadog.conf

c.	On line 30-31, you will find a comment and a template for adding host tags:
 

d.	Copy the template, update and save the file.
 

e.	Restart the datadog agent via command:<br />
sudo /etc/init.d/datadog-agent restart

f.	You can now find the tags in the UI:
 
Host Map

 
## 	MySQL Integration

a.	Get the integration instructions for MySQL, go to Datadog UI.
b.	On the left side menu, mouse over on Integrations and click on Integrations.
c.	Type in MySQL in the search box and click on the Configure.
 

d.	Click on Generate Password for convenience. This will update the command lines with the same password for your convenience.
 

e.	Copy the commands and execute in the vagrant terminal where you have installed the MySQL:<br />
sudo mysql -e "CREATE USER 'datadog'@'localhost' IDENTIFIED BY 'AfpJBxoAbcwKQGY3V5zs7Vfj';"<br />
sudo mysql -e "GRANT REPLICATION CLIENT ON *.* TO 'datadog'@'localhost' WITH MAX_USER_CONNECTIONS 5;"<br />
sudo mysql -e "GRANT PROCESS ON *.* TO 'datadog'@'localhost';"<br />
sudo mysql -e "GRANT SELECT ON performance_schema.* TO 'datadog'@'localhost';"<br />

*** You may encounter the error “ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)”, when you do, append the command lines with –u root –p. You will be prompted with the root password.<br />
Ex. sudo mysql -e "CREATE USER 'datadog'@'localhost' IDENTIFIED BY 'AfpJBxoAbcwKQGY3V5zs7Vfj';" –u root –p
f.	Verify the changes using the commands from the UI:
 
			Result:
 
 
			Result:
 
g.	You now have to configure an Agent for MySQL. Go to directory /etc/dd-agent/conf.d and create a file named mysql.yaml.
h.	Copy the configuration from the UI and paste it in mysql.yaml.
 
i.	Save the file and restart the datadog agent using command:<br />
sudo /etc/init.d/datadog-agent restart
j.	Execute the info command below:<br />
sudo /etc/init.d/datadog-agent info

You should be able to see this under Checks:
 
## 	Custom Check

To create a custom check, you need to create an integration configuration file and the custom check script. 
a.	Integration config file - Create a config file in conf.d directory with a .yaml extension and contains:
 
b.	Custom Check Script – create a script in the checks.d directory similarly named as the config file but with a .py extension. 
c.	Below is a custom check agent script that sends a random number metric named ‘test.random.support’ to Datadog:
 

d.	Restart the datadog agent.<br />
sudo /etc/init.d/datadog-agent restart
e.	execute the Datadog info command and you should see that the custom check is now included under Checks:<br />
sudo /etc/init.d/datadog-agent info
 


















## 	Visualizing your Data

### 		Database Integration Screenboard

1.	To create a screenboard, go to the Datadog UI.
2.	From the left side menu, mouse over on Dasboards and click on New Dashboard.
3.	Fill in the dashboard name and click on New Screenboard.
4.	You can choose any of the objects available but for now, drag Graph into the blank space on the lower part of the page:
 
5.	the Graph Editor box will pop-up to configure the graph:
i.	Set visualization to Timeseries.
ii.	Choose the metrics from MySQL and also the test.support.random metric. You can also click on Add Metric to include more.
 



iii.	Finally, set the display preferences and Widget Title and click Done.
 
iv.	Click on Save Changes and you now have your database screenboard:
Link: https://p.datadoghq.com/sb/2cc1ef6fa-ddf64a7098 
 

### 		Dashboard Cloning
You can also create copies of your dashboard by cloning it. To create a clone you just need to:
a.	Click on the gear icon on the upper right of the dashboard page and choose Clone Dashboard:
 





b.	A pop-up box will appear so you can set the name of the dashboard clone. Click on Clone.
 
c.	A new dashboard patterned after your existing dashboard is now created:
Link - https://p.datadoghq.com/sb/2cc1ef6fa-5a7fc11516

 


















### 		Custom Agent Check Timeboard
a.	To create a screenboard, go to the Datadog UI.
b.	From the left side menu, mouse over on Dasboards and click on New Dashboard.
c.	Fill in the dashboard name and click on New Timeboard.
d.	You can choose any of the objects available but for now, drag Time Series into the blank space on the lower part of the page:
 

e.	the Graph Editor box will pop-up to configure the graph:
i.	Set visualization to Timeseries and choose the test.support.random metric.
 





ii.	Set the graph title then click on Save and Finish Editing. Your dashboard will look like this:
 



### 		Dashboard Snapshot and Notification
You can send snapshots with annotations using the timeboard graphs.
1.	Hover you mouse over the graph and a camera icon will appear on the upper right corner of the graph. Click on this icon.
2.	The mouse cursor will be change to a pencil and you can use this to draw a box in the graph and emphasize specific events in it.
3.	You can also type in your comment in the dialog box below and send an email via annotation:

 


			Email sent:
 































## 	Alerting your Data

Setting up a Monitor for the Test Metric
1.	On the left side menu, mouse over on Monitors and select New Monitor.
2.	Select Metric as the Monitor type.
3.	Make sure the detection method is set at Threshold Alert since we want to be alerted when the random number breaches the 0.90 mark.
4.	Next on Define the metric, make sure that you have selected test.support.random from our host(vagrant_system) and select Multi Alert for each host so the alert will be applied to all of your existing and to be created hosts.

 

5.	Modify the email subject and body:
 

6.	Finally, set the notifications and permissions and click Save.

 

Email received:

 





## 	Monitoring Downtime

You can schedule monitoring downtimes for when you have a planned outage or you just don’t want to receive any alerts during off-business hours.
To set it up, follow the instructions below:

a.	From the left side menu, mouse over Monitors and click on Manage Downtime, then click on Schedule Downtime:
 

b.	Choose a monitor. Choose the random number threshold monitoring:
 














c.	Configure the schedule as seen below so the monitor will stop at 7pm and resume at 9am on a daily basis:
 

d.	Lastly, add a message to notify your team for the reason for the monitoring outage/downtime.
 
e.	Your monitoring downtime is now set-up:
 



