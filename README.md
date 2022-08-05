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
  


