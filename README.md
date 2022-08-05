# Splunk-In-The-Cloud-Setup
How-to on setting up splunk in Azure

###############################################
#            SPLUNK IN THE CLOUDS             #
#                                             #
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
  
  
  


