# Splunk-In-The-Cloud-Setup
How-to on setting up splunk in Azure

###############################################
#            SPLUNK IN THE CLOUDS             #
#                  Brett H.                   #
###############################################

#Edit: The VMs can be hosted in AWS, VMWare, Virtualbox, or whatever your heart desires.

Optional:
1. Create a resource group. This will help to see the over all cost of running it.

Requried:

1.0. Create a a VM that will host splunk enterprise as well as the web app. I will be using CentOS based 7.9
2.0. While the deployment of the VM is in progress lets head to https://www.splunk.com/en_us/download/splunk-enterprise.html?locale=en_us
  2.1. Use your information to get a 60 day trial. Once at the download page click on linux and click download on the .rpm file.
  2.2. Cancel the download and copy the wget download command in the top right corner for later!
3.0. Time to login to the super cool VM and Download/install splunk!!!
  3.1. Use the command "ssh user@IP address" to login to your VM.
  3.2. Once you are logged in cd into the /opt folder by doing "cd /opt"
  3.3. Now we need to install wget to be able to run the command to install Splunk to do that run "sudo yum install wget"
  3.4. Now we will run the wget command received from Splunk doing "sudo wget <Splunk Command>"
  3.5. Do "ls" just to confirm that it downloaded
  3.6. Now that it is in the /OPT dir, lets download it using "sudo yum install <File that was downloaded>"
  3.7. After its downloaded we will go to the next step!
4.0. Setting up the Web Services!
  4.1. Run the command "sudo /opt/splunk/bin/splunk start --accept-license" This will start the splunk instance and auto accept the license!
  4.2. Throught this process it will ask for user/pass
  4.3. It will say that your Web Interface is hosted at YourVMName:8000
  4.4. You are not out of the woods yet! Lets allow the firewall rule first.
  4.5. If this was a Localhosted box you would need to run firewall-cmd --permanent --add-port=8000/tcp however... Its Azure see step 4.6
  4.6. Go back into azure and locat your VM, click network, and add an inboud TCP connection for port 8000.
  4.7. That's it!!!!!!!!!!!! You should now have a splunk dashboard.
  
  Summary: Today we setup an azure VM that hosts Splunk Enterprise. The next step I will be showing is setting up forwards and configuring splunk to accept logs!
  
  
###############################################
#           Forwarders in the Clouds          #
#                  Brett H.                   #
###############################################
  
  
1.0. In the Splunk Enterprise web interface navigate to Settings>Forwarding and receiving.
  1.1. Under the Receive data and click "Add New" and input port "9997" and save it.
  1.2. Now this splunk instance is listening on port 9997 
  1.3. We are going to navigate to the apps section in the splunk instance and then manage apps
  1.4. Click browse more apps in the top right and type linux. For this tutorial we will grab sys logs from another linux VM!
  1.5. We will install the splunk addon for unix and linux and input your Splunk Email and password to be able to download.
  1.6. Create another VM. It can be another CentOS, Ubuntu, or any other linux VM. I will be using Ubuntu 20.04 LTS gen 2.
  1.7. Navigate to https://www.splunk.com/en_us/download/universal-forwarder.html to download the .deb instead get the wget command.
2.0. Installing the Forwarder onto the VM
  2.1. SSH into the VM that will be used to forward the sys logs.
  2.2. Run "cd /opt" to switch into the directory we will install the forwarder on.
  2.3. Download wget by running "sudo apt-get install wget" 
  2.3. Paste the "sudo wget <Splunk install command> That you received from the splunk forwarder download of the .deb file.
  2.4. Once its done installing run "sudo dpkg -i splunkforwarder-9.0.0.1-9e907cedecb1-linux-2.6-amd64.deb"
  2.5. Now run "sudo /opt/splunkforwarder/bin/splunk start --accept-license" it will prompt to create a user and password
  2.6. Now run "cd /opt/splunkforwarder/bin/" so we can work in the bin dir in the splunk dir
3.0. Connecting Splunk Universal Forwarder up to the Splunk Enterprise.
  3.1. Run "sudo ./splunk set deploy-poll <Ip address of splunk enterprise>:8089" (port 8089 is the mngmt port) and input your credentials
  3.2. now run "sudo ./splunk restart"
  3.3. Just like when we downloaded splunk enterprise we had to add firewall rules. so in 3.4 we will do just that!
  3.4. On the Splunk Enterprise instance we will allow an inboud rule from port 8089 and 9997.
  3.5. Go back to splunk enterprise and naviagte to Settings>Forwarder Management. Mine shows up as No-Fun-Tech-Comp1-Forwarder which is the name of the VM I made
  3.6. Note: If it does not show up restart the forwarder instance again and refresh the splunk enterprise page.
  3.7. Now that we have the connection we will forward the data from the Sys log folder in the Ubuntu Instance.
4.0 Forwarding the Syslogs from the Ubuntu instance into Splunk Enterprise
  4.1. We will simply run this command on the universal forwarder ubuntu instance "sudo ./splunk add forward-server <Ipaddress of Splunk Enterprise:9997"
  4.2. It will ask for your user and password and then will say "added forwarding to <Splunk Enterprise IP>
  4.3. Since this is just a lab we are going to use /var/log
  4.4. run "sudo ./splunk add monitor /var/log" and then do a restart "sudo ./splunk restart"
  4.5. Now we will chnage over to splunk enterprise and see if we are receiving the logs!
  4.6
  

