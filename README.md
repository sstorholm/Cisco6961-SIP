# Cisco6961-SIP
# How to flash a Cisco 6961 with SIP

Flashing a Cisco CP-6961 phone (or 6941/6921 for that matter) with SIP firmware is easy, but it requires some preparatory work.

# 1. Obtain Firmware

You'll need to download the appropriate SIP firmware. It's available from Cisco, but you'll need a login to access the support portal. 
The latest version as of writing (and forever as the phone is EOL by now) is SIP 9.4.1.3SR3. The file you'll need to acquire is named cmterm-69xx-SIP-9-4-1-3SR3.zip
Extract this zip into a directory.

# 2. Create the SEPXXXXXXXXXXXX.cnf.xml configuration file

The phone will look for a couple of files from the TFTP server when it tries to register. The third one it'll try to find is the SEPXXXXXXXXXXXX.cnf.xml file. The name of the file need to follow the format SEP[MAC_address_of_phone].cnf.xml. So look up the MAC address of you're phone, and create the document necessary. An example configuration file is found in this repository.

# 3. Set up a TFTP server

Use a TFTP program of your choosing to share the folder containing the contents of the firmware zip-archive as well as the SEPXXXXXXXXXXXX.cnf.xml file on port tcp69. 

# 4. Enable Alternative TFTP Server

By default, the phone expects a DHCP option (69 or 150) with the address of the TFTP server hosting it's configuraiton files. This is a bit hard to setup on a home network or in a lab, so we'll circumwent this by enabling the Alternate TFTP server option on the phone.

## 4.1. Boot the phone

## 4.2. Configure TFTP Settings

Go to Settings -> 4. Administrative Settings -> 1. Network Setup -> 1. IPv4 Setup (or press the settings button and press 4-1-1 on the dial pad)
Select 6. Enable Alternate TFTP and set it to Yes
Select 7. TFTP Server 1 and set it to the IP of your desktop machine running the TFTP software. 

# 5. Wait

Now we just need to wait, the phone will start pulling in the files from the TFTP server as soon as we configure the Alternate TFTP server as long as it isn't registered with a PBX.

If you follow along in the logs of the TFTP software, you'll see that the phone tries to pull first a file called CTLSEPXXXXXXXXXXXX.tlv and then a file called ITLSEPXXXXXXXXXXXX.tlv. 
As we don't have these, the phone moves on to the file we created. As soon as it pulls that file, it'll parse the contents of that file and start downloading the new firmware and applying it. 
Note that you can supply settings in this file as well, so if your PBX is up and running the phone will connect to it as soon as it's done rebooting. 

