

# Datadog Support Engineer - At Home Task

## Table of Contents

+ [Introduction](#introduction)
+ [Setting Up Your Application Stack](#setting-up-your-application-stack)
  - [Vagrant Ubuntu and VirtualBox Installation](#vagrant-ubuntu-and-virtualbox-installation)
  - [MYSQL Installation](#mysql-installation)
+ [Data Collection](#data-collection)
  - [Datadog Sign Up and Vagrant Integration](#datadog-sign-up-and-vagrant-integration)</br>
&ensp;&ensp;Bonus Question: [What is an Agent?](#what-is-an-agent)
  - [Host Tags](#host-tags)
  - [MySQL Integration](#mysql-integration)
  - [Custom Agent Check](#custom-agent-check)
  - [Database Integration Screenboard](#database-integration-screenboard)
+ [Visualizing Your Data](#visualizing-your-data)</br>
&ensp;&ensp;Bonus Question: [What is the Difference Between Timeboards and Screenboards?](#timeboard-and-screenboard)
  - [Dashboard Cloning](#dashboard-cloning)
  - [Custom Agent Check Timeboard](#custom-agent-check-timeboard)
+ [Alerting Your Data](#alerting-your-data)
  - [Dashboard Snapshot and Notification](#dashboard-snapshot-and-notification)
  - [Events Monitoring](#events-monitoring)
  - [Monitoring Downtime](#monitoring-downtime)
  

</br>
</br>
</br>
</br>
</br>
</br>

# Introduction

With Datadog entering the IT industry in Australia, it is essential to hire competent and passionate employees to be the foundation of the company as it builds it's local client base. This activity is part of the hiring process and is intended to be done at home.

This activity will primarily focus on helping you install three topics:
  1. Application Suite Set-up
  2. Datadog Integration
  3. Monitoring and Alerts


# Setting Up Your Application Stack

To start with, you need to set-up the application stack in your Mac OS X. It will consist of a Vagrant-Ubuntu operating system hosted on a VirtualBox VM with MySQL as the database server.

## 	Vagrant Ubuntu and VirtualBox Installation

  1. To download the Vagrant installer, go to the [vagrant downloads site](https://www.vagrantup.com/downloads.html) and choose the installer for Mac OS X.
  2. Open the downloaded DMG file and double-click on vagrant.pkg.
![vagrant_install1b](https://user-images.githubusercontent.com/30991348/29323031-008e9aac-8223-11e7-9551-0a6899621b85.png)
  3. The steps are straightforward, the only parameter that you may want to change is in the Destination Select step which is sometimes skipped while clicking next. When that happens, click on the Change Install Location during the Installation Type step:</br>
![vagrant_intstall2b](https://user-images.githubusercontent.com/30991348/29323032-009acfd4-8223-11e7-8ba1-9375f5cf3594.png)
  4. You would see the message below if the installation is a success. If you encounter any issues, please check the amount of diskspace available on your computer or if your operating system’s compatibility.</br>

  5. We will also be using VirtualBox for VM provisioning. To install, go to https://www.virtualbox.org/wiki/Downloads  and click on the OS X hosts option.
  6. The installer works exactly the same as with Vagrant. Follow steps 2-3.
  7. You would see the message below once you installation successfully completes.
![virtualbox_success](https://user-images.githubusercontent.com/30991348/29323033-009e9c18-8223-11e7-93cf-c6f839236aa3.png)
 The commands below are simple vagrants that you may use:
 ```
 vagrant up      #start/create a Vagrant instance
 vagrant ssh     #connect to Vagrant instance
 vagrant halt    #shutdown the Vagrant instance
 vagrant destroy #terminate/delete the Vagrant instance
 ```


## MYSQL Installation
&ensp;&ensp;Follow the steps below to install MySQL on the Vagrant OS.
  1. Open your MAC Terminal and create/start a vagrant virtual machine by executing the command:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`vagrant up`
  2. Your Vagrant VM is now up and running. Connect to it by executing the command:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`vagrant ssh`
  3. Before MySQL installation, make sure the system is updated using the commands:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo apt-get update`<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo apt-get upgrade` <--_NOTE:this will take a few minutes to download and install_
  4. To install MySQL, run the command:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo apt-get install mysql-server`
  5. The MySQL server should be automatically up and running after the installation. To verify, check the mysql background process and try logging in:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`ps –ef | grep mysql`
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`mysql –u root -p` <--_NOTE:you will be prompted to use the MySQL root credentials that you created during its installation_
__insert screenshot__


# Data Collection

Now that you have set-up your application suite, you can now proceed with your system's integration with Datadog. 

### Datadog Sign Up and Vagrant Integration
  1. Go to [datadog homepage](https://www.datadoghq.com) and click on __GET STARTED FOR FREE__.
  2. Fill-in the form that will pop-up with your details and __Sign up__:
  	__INSERT SCREENSHOT__
  3. In the next step, you can answer the surveys about the softwares and services that you are currently using. Answer the survey and click on __Next__.
  4. Lastly, you will be required to install a datadog agent in your host server. Select Ubuntu from the left side menu to get the instructions:
    	__INSERT SCREENSHOT__
  5. Install curl before executing the script from the Datadog instructions by running the command below in the vagrant terminal:<br />
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo apt-get install curl`
  6. Copy the datadog installation command and run it in the terminal:
```
DD_API_KEY=c08db2089f1d3ea2ee9f6238c2e87d12 bash -c "$(curl -L https://raw.githubusercontent.com/DataDog/dd-agent/master/packaging/datadog-agent/source/install_agent.sh)" DD_INSTALL_ONLY=true
```
_NOTE:Replace the DD_API_KEY with the key that will be generated for your account_
  Running this command would automatically start the datadog agent data collection for the Vagrant server.
  7. After a few seconds, Datadog will receive the data from your host and you can now click on the Finish button in the lower right to complete the sign-up.
    	__INSERT SCREENSHOT__<br />

Below are basic datadog commands that you can enter in the terminal:
```
sudo /etc/init.d/datadog-agent start 	#start the datadog agent processes
sudo /etc/init.d/datadog-agent stop 	#stop the datadog agent processes
sudo /etc/init.d/datadog-agent restart 	#restart the datadog agent processes
sudo /etc/init.d/datadog-agent info 	#displays information on the data being collected by the agent
sudo /etc/init.d/datadog-agent info -v 	#displays a more detailed(verbose) information on the agent
```

## What is an Agent?

An agent is a program installed in the host server as part of a parent program to remotely connect and interact with the host. Datadog’s agent in the server will collect data and send it to the Datadog application for monitoring.

## Host Tags
This exercise will show you how to put tags on your host/s. It is useful to distinguish your hosts from each other especially when you are using multiple servers.
	
### Via Config file
  1. From your Vagrant terminal, go to the datadog configuration directory via command:</br>
	&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`cd /etc/dd-agent`
  2. Open and edit the configuration file - datadog.conf</br>
	&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo vi datadog.conf`
  3. On line 30-31, you will find a comment and a template for adding host tags:</br>
	```
	30 # Set the host's tags (optional)
	31 # tags: mytag, env:prod, role:database
	```
  4. Copy the sample line excluding the comment and update it with your tag, update and save the file.
		__INSERT SCREENSHOT__
  5. Restart the datadog agent via command:</br>
	&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo /etc/init.d/datadog-agent restart`
  6. You can now find the tags in the UI:</br>
 
</br>&ensp;&ensp;&ensp;&ensp;Host Map
__INSERT SCREENSHOT__

### Via Website
  1. From the left side menu, mouse over on __Infrastructure__ and click on __Infrastructure List__.
  2. On the upper right corner, click on __Update Host Tags__.
  __INSERT SCREENSHOT__
  3. Click on __Edit Tags__ and enter.
  
</br>&ensp;&ensp;&ensp;&ensp;Host Map
    	__INSERT SCREENSHOT__


 
## 	MySQL Integration
Next, integrate your MySQL to send your database metrics to Datadog. Replace the __[datadog db password]__ with your own from the commands below whenever applicable.
  1. On the left side menu of the Datadog UI, mouse over on __Integrations__ and click on __Integrations__.
  2. Type in MySQL in the search box and click on __Available__.
  __INSERT SCREENSHOT__
  3. Click on __Generate Password__ for convenience. This will update the command lines with the same password for your convenience.
  __INSERT SCREENSHOT__
  4. Copy the commands and execute in the vagrant terminal where you have installed the MySQL:<br />
```
sudo mysql -e "CREATE USER 'datadog'@'localhost' IDENTIFIED BY '[datadog db password]';"
sudo mysql -e "GRANT REPLICATION CLIENT ON *.* TO 'datadog'@'localhost' WITH MAX_USER_CONNECTIONS 5;"
sudo mysql -e "GRANT PROCESS ON *.* TO 'datadog'@'localhost';"
sudo mysql -e "GRANT SELECT ON performance_schema.* TO 'datadog'@'localhost';"
```
_NOTE: You may encounter the error “ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)”, when you do, append the command lines with –u root –p. You will be prompted with the root password.<br />
Ex. &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`sudo mysql -e "CREATE USER 'datadog'@'localhost' IDENTIFIED BY '[datadog password]';" –u root –p`_<br />
  5. Verify the changes using the commands:
  ```
  mysql -u datadog --password=<datadog db password> -e "show status" | \
  grep Uptime && echo -e "\033[0;32mMySQL user - OK\033[0m" || \
  echo -e "\033[0;31mCannot connect to MySQL\033[0m"
  mysql -u datadog --password=[datadog db password] -e "show slave status" && \
  echo -e "\033[0;32mMySQL grant - OK\033[0m" || \
  echo -e "\033[0;31mMissing REPLICATION CLIENT grant\033[0m"
  ```
  Result:
 
__insert screenshot__
  ```
  mysql -u datadog --password=[datadog db password] -e "SELECT * FROM performance_schema.threads" && \
  echo -e "\033[0;32mMySQL SELECT grant - OK\033[0m" || \
  echo -e "\033[0;31mMissing SELECT grant\033[0m"
  mysql -u datadog --password=[datadog db password] -e "SELECT * FROM INFORMATION_SCHEMA.PROCESSLIST" && \
  echo -e "\033[0;32mMySQL PROCESS grant - OK\033[0m" || \
  echo -e "\033[0;31mMissing PROCESS grant\033[0m"
  ```
  Result:
  __insert screenshot__
 
  6. Now we just have to create the MySQL configuration file for the Agent.  Go to directory __/etc/dd-agent/conf.d__ and create a file named mysql.yaml.
  ```
  cd /etc/dd-agent/conf.d
  sudo vi mysql.yaml
  ```
  7. Copy the configuration from the UI and paste it in mysql.yaml.
  ```
  init_config:

  instances:
  - server: localhost
    user: datadog
    pass: [datadog db password]
    tags:
        - optional_tag1
        - optional_tag2
    options:
        replication: 0
        galera_cluster: 1
   ```
 
  8. Save the file and restart the datadog agent:
   `sudo /etc/init.d/datadog-agent restart`
  9. Execute the info command below:
   `sudo /etc/init.d/datadog-agent info`

You should be able to see this under Checks:
__insert screenshot__
 
## Custom Agent Check

To create a custom check, you need to create two components:
  - Integration Config file - a config file in __/etc/dd-agent/conf.d__ directory with a .yaml extension.
  - Custom Check Script     – a python script in the __/etc/dd-agent/checks.d__ directory similarly named as the config file but with a .py extension. _Ex. myscript.yaml, myscript.py_
  
 Follow the steps below to create a custome check that generates a random number(from 0-1) metric and send it to Datadog named as "test.random.support".
  1. Create a file named cc_random.yaml into the __conf.d__ directory and type in:
  ```
  init_config:

  instances:
    [{}]
  ```
  2. Create a file named cc_random.py into the __checks.d__ directory and type in:
  ```
  import random

  from checks import AgentCheck

  class HelloCheck(AgentCheck):
    def check(self, instance):
        self.gauge('test.support.random', random.random())
  ```
  
  __insert screenshot__
  
  3. Restart the datadog agent:
   `sudo /etc/init.d/datadog-agent restart`
  4. execute the Datadog info command and you should see that the custom check is now included under Checks:
   `sudo /etc/init.d/datadog-agent info`
   __insert screenshot__
 
## Visualizing your Data
  
### Timeboard and Screenboard
In Datadog, you can create two kinds of dashboards - the Timeboard and the Screenboard. The difference of these boards are their main purpose.
  * The Timeboard is primarily for internal use like investigations and alerts. You can set-up monitoring based from these graphs. You can also share snapshots of a graph to internal teams via annotations. 
  * The Screenboard’s primary purpose is for public viewing. It has features solely for making viewing aesthetically pleasing and intuitive such as including widgets, images and Hostmap on the screen.

Both of these type of dashboard can be created by:
  1. From the left side menu of the Datadog UI, mouse over on __Dasboards__ and click on __New Dashboard__.
  2. Populate the dashboard name, pick the type of dashboard you need and click on __New Timeboard/New Screenboard__.
   __insert screenshot__
   
### Custom Agent Check Timeboard
  1. After choosing timeboard, you will be prompted to set-up your newly created dashboard.
  2. You can choose any of the objects available but for now, drag __Time Series__ into the blank space on the lower part of the page:
__insert screenshot__
  3. the Graph Editor box will pop-up to configure the graph:
     1. Set visualization to __Timeseries__ and choose the __test.support.random metric__.
     __insert screenshot__
     2. Set the graph title then click on Save and Finish Editing. Your graph will look like this:
     __insert screenshot__

### Database Integration Screenboard
  1. After choosing screenboard, you will be prompted to set-up your newly created dashboard.
  2. You can choose any of the objects available but for now, drag __Graph__ into the blank space on the lower part of the page.
  3. the __Graph Editor__ box will pop-up to configure the graph:
     1. Set visualization to __Time Series__.
     2. Choose the metrics from MySQL and also the test.support.random metric. You can also click on __Add Metric__ to include more.
 
     3. Set the display preferences and Widget Title and click Done.
    __insert screenshot__
     4. Click on __Save Changes__ and you now have your database screenboard:
   __insert screenshot__
  
### Dashboard Cloning
You can also create copies of your dashboard by cloning it. To create a clone you just need to:
  1. Click on the gear icon on the upper right of the dashboard page and choose __Clone Dashboard__:
    __insert screenshot__
  2. A pop-up box will appear so you can set the name of the dashboard clone. Click on __Clone__.
  3. A new dashboard patterned after your existing dashboard is now created:
   __insert screenshot__
 
## Alerting Your Data

### Dashboard Snapshot and Notification
You can send snapshots with annotations using the timeboard graphs.
  1. Hover you mouse over the graph and a camera icon will appear on the upper right corner of the graph. Click on this icon:
     __insert screenshot__
  2. The mouse cursor will be changed to a pencil and you can use this to draw a box and emphasize certain areas in the graph.
  3. You can also type in your comment in the dialog box below and send an email via annotation:

			__insert screenshot__
			
			__insert screenshot__


### Events Monitoring
Setting up a Monitor for the Test.Support.Random Metric
  1. On the left side menu of the Datadog UI, mouse over on Monitors and select __New Monitor__.
  __insert screenshot__
  2. Select __Metric__ as the __Monitor type__.
  3. Make sure the detection method is set at __Threshold Alert__ since we want to be alerted when the random number breaches the 0.90 mark.
  4. Next on Define the metric, make sure that you have selected __test.support.random__ from our host(vagrant_system) and select __Multi Alert__ for each host so the alert will be applied to all of your existing and to be created hosts.
  __insert screenshot__
  5. Modify the email subject and body:
  __insert screenshot__
  6. Finally, set the notifications and permissions and click __Save__.
Email received:
  __insert screenshot__

### Monitoring Downtime

You can schedule monitoring downtimes for when you have a planned outage or you just don’t want to receive any alerts during off-business hours.
To set it up, follow the instructions below:

  1. On the left side menu of the Datadog UI, mouse over __Monitors__, click on __Manage Downtime__, then click on __Schedule Downtime__.
  __insert screenshot__
  2. Choose a monitor. Choose the random number threshold monitoring:
  3. Configure the schedule as seen below so the monitor will stop at 7pm and resume at 9am on a daily basis:
  __insert screenshot__
  4. Lastly, add a message to notify your team for the reason for the monitoring outage/downtime.
  __insert screenshot__
  
&ensp;&ensp;&ensp;&ensp;Your monitoring downtime is now set-up! You'll be able to receive these emails notifications as configured:

Downtime Start:
  __insert screenshot__
  
 Monitoring End:
  __insert screenshot__
