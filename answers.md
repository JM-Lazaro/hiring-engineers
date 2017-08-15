

# Datadog Support Engineer - At Home Task

## Table of Contents

+ [Introduction](#introduction)
+ [Setting Up the Application Stack](#setting-up-the-application-stack)
  - [Vagrant Ubuntu and VirtualBox](#vagrant-ubuntu-and-virtualbox)
  - [MYSQL](#mysql)
+ [Data Collection](#data-collection)
  - [Datadog Sign Up](#datadog-sign-up)</br>
&ensp;&ensp;Bonus Question: [What is an Agent?](#what-is-an-agent?)
  - [Adding Tags to Host](#adding-tags-to-host)
  - [MySQL Integration](#mysql-integration)
  - [Custom Check](#custom-check)
  - [Database Integration Screenboard](#database-integration-screenboard)
  - [Timeboard vs. Screenboard](#timeboard-vs.-screenboard)
  - [Dashboard Cloning](#dashboard-cloning)
  - [Custom Agent Check Timeboard](#custom-agent-timeboard)
+ [Alerts and Monitoring](#alerts-and-monitoring)
  - [Dashboard Snapshot and Notification](#dashboard-sanapshot-and-notification)
  - [Alerting Your Data](#alerting-your-data)
  - [Setting up a Monitor for the Test Metric](#setting-up-a-monitor-for-the-test-metric)
  - [Monitoring Downtime](#monitoring-downtime)

</br>
</br>
</br>
</br>
</br>
</br>

# Introduction

The purpose of this document is to display the applicant’s ability to complete basic Datadog Support Engineer tasks as listed in the Github Task.



# Setting Up the Application Stack

Firstly, set-up the application stack in your Mac OS X. It will consist of a Vagrant Ubuntu operating system hosted on a VirtualBox VM with a

## 	Vagrant Ubuntu and VirtualBox

  1. Go to the [vagrant downloads site](https://www.vagrantup.com/downloads.html) and choose the installer for Mac OS X.
  2. Open the downloaded DMG file and double-click on vagrant.pkg.
&ensp;&ensp;<img width="1440" alt="vagrant_install1" src="https://user-images.githubusercontent.com/30991348/29299031-46102182-81af-11e7-929c-65c84744d7d2.png">
  3. Follow the instructions, which are very straightforward. The only option that you may want to change is in the Destination Select step which is sometimes skipped while clicking next. When that happens, click on the Change Install Location during the Installation Type step:
  4. You would see the message below if the installation is a success. If you encounter any issues, please check the amount of diskspace available on your computer or if your operating system’s compatibility.
  5. We will also be using VirtualBox for VM provisioning. To install, go to https://www.virtualbox.org/wiki/Downloads  and click on the OS X hosts option.
  6. Open the downloaded installer and double-click on VirtualBox.pkg.
  7. You would see the message below if the installation is a success.
 


## 	MYSQL

  1. Open your MAC Terminal and create/start a vagrant virtual machine by executing the command:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`vagrant up`
  2. Your Vagrant VM is now up and running. Connect to it by executing the command:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`vagrant ssh`
  3. Before MySQL installation, make sure the system is updated using the commands:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo apt-get update`<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo apt-get upgrade` <--_this will take a few minutes to download and install_
  4. To install MySQL, run the command:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo apt-get install mysql-server`
  5. The MySQL server should be automatically up and running after the installation. To verify, check the mysql background process and try logging in:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`ps –ef | grep mysql`
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`mysql –u root -p`

# Data Collection

Now that we have set-up our application suite, we will now proceed in integrating Datadog to our system.
<h3>Datadog Sign Up</h3>
<ol>
<li>Go to [datadog homepage](https://www.datadoghq.com) and click the GET STARTED FOR FREE button:</li>
<li>Fill-in the form that will pop-up with your details and click Sign up:</li>
<li>In the next step, answer the surveys about the current softwares and services that you are currently using. This step is optional:</li>
<li>For the final step, install a datadog agent to your host system. Select Ubuntu from the left side menu:</li>
<li>Install curl before executing the script from the Datadog instructions by running the command below in the vagrant terminal:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo apt-get install curl`</li>
<li>Copy the datadog installation command and run it in the terminal:<br />
<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`DD_API_KEY=c08db2089f1d3ea2ee9f6238c2e87d12 bash -c "$(curl -L https://raw.githubusercontent.com/DataDog/dd-agent/master/packaging/datadog-agent/source/install_agent.sh)" DD_INSTALL_ONLY=true`
</li>
<li>Running this command would automatically start the datadog agent data collection for the Ubuntu server:</li>
<li>After a few seconds, Datadog will receive the data from your host and you can now click on the Finish button in the lower right to complete the sign-up.</li>
</ol>
## 	Adding Tags to Host

### Via Website
  1. From the left bar menu, mouse over on Infrastructure and click on “Infrastructure List”.
  2. On the upper right corner, click on Update Host Tags
  3. Click on Edit Tags and enter.
  
</br>&ensp;&ensp;&ensp;&ensp;Host Map

### Via Config file
  1. From your vagrant terminal, go to the datadog configuration directory via command:</br>
	&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`cd /etc/dd-agent`
  2. Open and edit the configuration file - datadog.conf</br>
	&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo vi datadog.conf`
  3. On line 30-31, you will find a comment and a template for adding host tags:</br>
  4. Copy the template, update and save the file.
  5. Restart the datadog agent via command:</br>
	&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo /etc/init.d/datadog-agent restart`
  6. You can now find the tags in the UI:</br>
 
</br>&ensp;&ensp;&ensp;&ensp;Host Map

 
## 	MySQL Integration
  1. Get the integration instructions for MySQL, go to Datadog UI.
  2. On the left side menu, mouse over on Integrations and click on Integrations.
  3. Type in MySQL in the search box and click on the Configure.
  4. Click on Generate Password for convenience. This will update the command lines with the same password for your convenience.
  5. Copy the commands and execute in the vagrant terminal where you have installed the MySQL:<br />
```
sudo mysql -e "CREATE USER 'datadog'@'localhost' IDENTIFIED BY 'AfpJBxoAbcwKQGY3V5zs7Vfj';"<br />
sudo mysql -e "GRANT REPLICATION CLIENT ON *.* TO 'datadog'@'localhost' WITH MAX_USER_CONNECTIONS 5;"<br />
sudo mysql -e "GRANT PROCESS ON *.* TO 'datadog'@'localhost';"<br />
sudo mysql -e "GRANT SELECT ON performance_schema.* TO 'datadog'@'localhost';"<br />
```
_NOTE: You may encounter the error “ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)”, when you do, append the command lines with –u root –p. You will be prompted with the root password.<br />
Ex. &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo mysql -e "CREATE USER 'datadog'@'localhost' IDENTIFIED BY 'AfpJBxoAbcwKQGY3V5zs7Vfj';" –u root –p`_
  6. Verify the changes using the commands from the UI:
 
			Result:
 
 
			Result:
 
  7. You now have to configure an Agent for MySQL. Go to directory /etc/dd-agent/conf.d and create a file named mysql.yaml.
  8. Copy the configuration from the UI and paste it in mysql.yaml.
 
  9. Save the file and restart the datadog agent using command:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo /etc/init.d/datadog-agent restart`
  10. Execute the info command below:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo /etc/init.d/datadog-agent info`

You should be able to see this under Checks:
 
## 	Custom Check
To create a custom check, you need to create an integration configuration file and the custom check script. 
  1. Integration config file - Create a config file in conf.d directory with a .yaml extension and contains:
  2. Custom Check Script – create a script in the checks.d directory similarly named as the config file but with a .py extension. 
  3. Below is a custom check agent script that sends a random number metric named ‘test.random.support’ to Datadog:
  4. Restart the datadog agent.<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo /etc/init.d/datadog-agent restart`
  5. execute the Datadog info command and you should see that the custom check is now included under Checks:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo /etc/init.d/datadog-agent info`
 
## 	Visualizing your Data

### 		Database Integration Screenboard
  1. To create a screenboard, go to the Datadog UI.
  2. From the left side menu, mouse over on Dasboards and click on New Dashboard.
  3. Fill in the dashboard name and click on New Screenboard.
  4. You can choose any of the objects available but for now, drag Graph into the blank space on the lower part of the page:
 
  5. the Graph Editor box will pop-up to configure the graph:
     1. Set visualization to Timeseries.
     2. Choose the metrics from MySQL and also the test.support.random metric. You can also click on Add Metric to include more.
 
     3. Finally, set the display preferences and Widget Title and click Done.
 
     4. Click on Save Changes and you now have your database screenboard:

 

### 		Dashboard Cloning
You can also create copies of your dashboard by cloning it. To create a clone you just need to:
a.	Click on the gear icon on the upper right of the dashboard page and choose Clone Dashboard:
 
b.	A pop-up box will appear so you can set the name of the dashboard clone. Click on Clone.
 
c.	A new dashboard patterned after your existing dashboard is now created:

### 		Custom Agent Check Timeboard
  1. To create a screenboard, go to the Datadog UI.
  2. From the left side menu, mouse over on Dasboards and click on New Dashboard.
  3. Fill in the dashboard name and click on New Timeboard.
  4. You can choose any of the objects available but for now, drag Time Series into the blank space on the lower part of the page:

  5. the Graph Editor box will pop-up to configure the graph:
     1. Set visualization to Timeseries and choose the test.support.random metric.
     2. Set the graph title then click on Save and Finish Editing. Your dashboard will look like this:
 
### 		Dashboard Snapshot and Notification
You can send snapshots with annotations using the timeboard graphs.
  1. Hover you mouse over the graph and a camera icon will appear on the upper right corner of the graph. Click on this icon.
  2. The mouse cursor will be change to a pencil and you can use this to draw a box in the graph and emphasize specific events in it.
  3. You can also type in your comment in the dialog box below and send an email via annotation:

			Email sent:
 
## 	Alerting your Data

Setting up a Monitor for the Test Metric
  1. On the left side menu, mouse over on Monitors and select New Monitor.
  2. Select Metric as the Monitor type.
  3. Make sure the detection method is set at Threshold Alert since we want to be alerted when the random number breaches the 0.90 mark.
  4. Next on Define the metric, make sure that you have selected test.support.random from our host(vagrant_system) and select Multi Alert for each host so the alert will be applied to all of your existing and to be created hosts.
  5. Modify the email subject and body:
  6. Finally, set the notifications and permissions and click Save.
Email received:

### 	Monitoring Downtime

You can schedule monitoring downtimes for when you have a planned outage or you just don’t want to receive any alerts during off-business hours.
To set it up, follow the instructions below:

  1. From the left side menu, mouse over Monitors and click on Manage Downtime, then click on Schedule Downtime:
  2. Choose a monitor. Choose the random number threshold monitoring:
  3. Configure the schedule as seen below so the monitor will stop at 7pm and resume at 9am on a daily basis:
  4. Lastly, add a message to notify your team for the reason for the monitoring outage/downtime.
&ensp;&ensp;&ensp;&ensp;Your monitoring downtime is now set-up:
