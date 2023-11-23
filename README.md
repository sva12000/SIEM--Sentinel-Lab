<h1>Mapping Failed RDP to IP Geolocation</h1>

# Description

<b> A Poweshell script is used to analyze the Windows Event Log data regarding failed RDP attacks that is used by thrid party API to gather geographic information on the attackers geolocation. 
</br>
<br/>
In this lab Azure Sentinel (SIEM) is connected to a live virtual machine that is used as a Honey pot.
You will wacht in realtime all the attacks done aganst this virtual machine also know as (RDP Brute Force). The PowerShell script will extract the attackers Geolocation data and plot it on Azure Sentinel Map.

<h1>Pictures of subscription virtual machine, Log Analytics workspace, Sentinel, and Map with results of attacks.</h1>

![SentinelLab Pic1](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/4c1b4c60-6047-41d6-92dc-f91a3a9e4422)


<br />
<br />
<h1>PROJECT: Hands On - SIEM Azure SENTINEL MAP with Live Data</h1>                                                                                                    

<a>Create Virtual Machine<a/>
 
![SentinelLab Pic2](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/a1d2b12a-d63e-49a5-a7b3-7007e32aa44b)

![SentinelLabVM](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/d2189db9-bce8-4958-bb20-87f301fb2ca2)

![SentLabVMc](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/996b53f6-1943-477c-a388-ad8846aae2fd)

![SentLab VM_d](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/93f67b9d-1d73-4a2a-a439-aab5fc109f49)

![SentLab VM_E](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/33a375a0-dcde-49fd-9cc2-292bdd35647b)

Allow all to get into the Firewall, By creating and setting up NSG Network security group 
 ( select Advance and click on Create New under the configure network security group box.
 
 ![SentLab VM_F](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/e7dd0db5-e5ed-4f55-a7ad-038608243085)

 <h1>Create a Network Secuity Group (NSG)</h1>

On the next window remove the default inbound rules by clicking the recycle bin and creating a new rule

![SentLab NSG](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/cdd49e6d-9ab9-4272-b046-2550548badd9)

 
For this project, we are going to create our new INBOUND RULES that will allow everything into the VM coming from the public Internet. To do So click on Add an inbound rule > in the Destination port ranges box delete the port number and add start sign (*) for any protocol > Action Allow > Priority change it to 100 > Name box (add a name that reflects the function of the NSG) ei; “Danger_Allow_In” and click Add

![SentLab NSG_A](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/15194e9d-3689-4494-a4b9-41fa20652ae1)

A new Inbound rule named  “Danger_Allow_In” is created that allows all traffic from the internet into the VM.
CLICK OK and in the next window click Review + Create button

![SentLab NSG_B](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/e95b98af-6573-4030-b4cb-5a930552f03e)

click the Review + create button to continue configuring the VM

![SentLab NSG_C](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/91ada2c7-449a-4e14-a229-5ae1f1d6696e)

 Next Click Create 

 ![SentLab VM_G](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/4d339051-7184-4cfc-a204-3979e94358fd)

 The deployment is in progress while is in progress I do the next step Log analytic

 ![SentLab VM_H](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/7e496f6b-1e9e-4e6f-a510-33a265806efb)


 
Create Log Analytics Workspace:
The purpose of creating a LAW is to kind of ingest logs from the virtual machine. The Windows events Logs. Also, we are going to create our custom log that contains Geographic information that allows us to know where the attackers are coming from. To do so we are going to create log analytics and log analytics workspace this is where those logs will be stored and Our SIEM Azure Sentinel will connect to the workspace to be able to display the geodata information on the Map. First, go to the Log Analytics page and create a LAW. Click the Create Log Analytics workspace button.
 
 
On the Resource group box, I select the Resource group I created honeypotlab on Instace details’s Name box I enter the name “HoneyPotOne select the Region Canada East, and click the Review+Create button
 
Validation passed and click on create
 
 
Enable gathering VM Logs in the Security Center
Next type security center on the Azure portal search box and select Azure security center under Marketplace, to enable the ability for Sec Center to gather logs from the VM into the Workspace, On the next page click on Open Defender for Cloud. In the Security Center Overview page select Environment Settings then click the Log Analytics Workspace that we created turn Defender on and turn SQL off
 

Security Center Overview page > Environment Settings
 
Click on the Azure Subscription to display the Log Analytics Workspace that we create
 
Click on Log Analytics Workspace that we created and on the next page turn Defender on by selecting Servers ON turn SQL servers off and click save 
 
On the Data Collection window select All Events and click save 
 


Back to the LAW page and click on the log Analytics we created
 
On the next window on the left panel click on VM virtual machines under Under Classic tab and on the next page click on the VM we created
 
Click on the VM we created and on the next page select Connect 
 
Setup Azure Sentinel
While connecting to VM I will open a new Azure portal tab and I am going to set up Sentinel. So type sentinel on the portal search box and select it this is the SIEM we are going to use to picture the data attack
 
Next, click on the Connect Workspace button 
 
Select the created workspace HoneyPotOne and click the Add button
 
Lets it run and will be back after. Search for Virtual machines on the Azure portal search box. On the virtual machines page click the created VM 
Log into VM with Remote Desktop
 
 on the next page select the Public IP address of the VM, as we are going to use it in a Windows machine to access the VM via RDP.
 
Next, click on the start menu on your computer type remote desktop and launch the RDP window 
And add the public IP address of the VM to access it
  
Type the user ID and Password and click  yes to accept the certificate warning
 

On the welcome page
 

On the Choose privacy settings for your device page Switch the Yes to NO
 
And click on the accept button 
This is your VM screen
 
Inside your VM launch your Browser minimize it and click on the start menu to open Windows Event Viewer windows.
Observer Event Viewer Logs in
 
On the start menu select event viewer, let’s look at some logs that we are going to use on the geo map
 
On the event, viewer log windows click the arrow Windows Log and select Security to display the security logs as shown on the below screen. Those are all the security events in that virtual machine. Concentrate on event ID 4625 Account failed to log on. So From my regular computer, I try to log in with the wrong user ID and Password to create Event ID failures on Windows Event Logs. we will see those failures when I look at the Windows event ID Security Log.
 
On this window, you can see Event ID 4625 showing the account name trying to log on and failure information “ with failure reason “Unknown user name or bad password. Also, you see the IP address of the machine that trying to get into our machine.
 
This screenshot shows the IP address of the machine that tries to get into our VM. Which is the machine I use to try to connect to the VM. The Event Log will record all the IP Addresses that will try to log into this VM and will be seen on the Geo Map. Here you do not see any indication of where the IP address is located in the world.
To see the location I going to use PowerShell to program it, to take those IP addresses, and then use an IPG Location API.
 
 
I am going to use this website “ https://ipgeolocation.io” What I going to do is post this IP address to the API and the API will return the Country name, state_province, city, latitude, longitude, time zone, etc. and I am going to use that data to create my own Custom Log and send that log to log analytics workspace in Azure and I am going to use Sentinel (SIEM) to kind of read that latitude and longitude or any other data to be used to plot out on the map the different attackers that are trying to breach into my VM. I will show you how to do that but first I will make sure to turn the firewall off in the VM so the VM can respond to ICMP echo requests so people can discover it on the internet faster.
Turn the Firewall Off in the VM
On VM click on start menu search and type wf.msc and click enter to open the Windows firewall  
 

Click Windows Defender firewall properties, in the properties windows under the Domain Profile tab set FireWall state: to OFF as well on the Private Profile tab and Public Profile tab all firewall states set to OFF and click on Apply and OK and minimize VM
 


Next, in my computer, I open a command line  and I will ping my Virtual machine using dash t (-t) for perpetual ping and I am getting a reply from the VM

 

Download PowerShell Script
Open PowerShell ISE on your VM  
When Powershell is opened we are going to click on the new file under the file and we are going to paste the downloaded programming code into the Powershell script window of the new file.
 
 
Save in the VM desktop as Log_Exporter
 


Get Geolocation.io API Key
To convert the IP Address to Latitude and Longitude you need to get your own API_KEY by going to the https://ipgeolocation.io/ and clicking on sign up to get the API key.
 
Sign in and copy the API-Key and past it on the PowerShell script
 
Pasted the API _Key from the Ipgeolocation.io into the PowerShell Log_Exporter file’s API_KEY section 
 
 
Run Script to get Geo Data from Attackers
the event log that I looked at earlier. So what it does is run in perpetuity and capture the geo data and create a new log file, that will be output to C:\ProgramData\
The purple lines will be displayed every time a failed login is recorded in the event viewer. All this failure information is sent out to the API address 
  

if you go to the c:\programData\ now there is a log file in there that has been automatically created this fail logon file is named failed_rdp file with some sample records in it with the country and geodata that I will use later to train log analytics workspace to accept and parse out my custom log.
  

Open the failed_rdp file and the first few lines are like the sample data that I am going to train analytics workspace and then the rest are actual real failed logins the two lines at the end are my intentional login with a bad password and wrong user-id
“username:sergadmin1,sourcehost:178.128.225.19, state: Ontario, label: Canada”
 

Create a custom log in LAW 
First of all, in the Azure portal search box, I type Log Analytics to access the Log Analytics workspace page and click on the workspace name HoneyPotOne > Tables. Select Create and then New custom log (MMA-based). By default, all configuration changes are automatically pushed to all agents. In the next window click on the folder icon to add the file Log.
(This file is the failed_rdp file I saved on my VM desktop, open the file and copy the content minimize the VM> go to my  PC open Notepad, and paste the content on it > save as failed_rdp.log on my desktop. I will use this file to train the Log Analytics on what to search for in the log file.
Next back to Create a custom log page > click on the folder icon to add the file failed_rdp.log > click Next > click next again > select windows under type box > Type C:\ProgramData\failed_rdp.log in the Path box
 
Type C:\ProgramData\failed_rdp.Log in the Path box and click next> Add a name to the custom log: FAILED_RDP_WITH_GEO and click the next button >  Click create > Click Review + Create
 
Add a name and description to the custom log: FAILED_RDP_WITH_GEO
 
Click on Create
 
Search for the custom log file name 
 
Next Back to the Log Analytics workspace name page and click on Logs on the left panel > In the Query window type the file name “dailed_rdp_with_geo_CL in the box under the run button >and click run
 
What you will see after clicking on run are the custom log entries. You may have to wait 10 – 15 minutes before you can see any logs. The first logs that you will see are our sample logs and then the final two are where I failed to log
 
I can see my failed log-in

I came back to my VM and the lines at the bottom I  highlighted are my custom log entries if you come back to Azure portal Log analytics workspace> HoneyPotOne windows the highlighted raw data column contains the whole lines I highlighted previously on the VM file failed_rdp.log 
 
if you come back to Azure portal Log analytics workspace> HoneyPotOne windows the highlighted RawData column contains the whole lines I highlighted previously on the VM file failed_rdp.log 
 
Right now I can check SecurityEvent 
 
I can search for a specific eventID as this is the security event log from the VM sent into the log analytics workspace I can search for any eventID on the log for ei; SecurityEvent | where EventID == 4625 and click on the run button.  It will return all of the failed RDP logs. If you expand it you can see the user name, IP address, and other information of the attacker
 
Create a KQL query to fields/extract 	
The Next thing is to Extract the fields from RawData to make their fields so I can have the RawData divided into Column fields like Latitude column, Longitude column, country column field, and so on. To do that I will
Write a KQL query to transform ingested data
1.	View the data in the target custom table in Log Analytics:
a.	In the Azure portal, select Log Analytics workspaces > your Log Analytics workspace > Logs.
b.	Run a basic query on the custom logs table to view table data.
2.	Use the query window to write and test a query that transforms the raw data in your table.
For information about the KQL operators that transformations support, see Structure of Transformation in Azure Monitor.
 Note
The only columns that are available to apply transforms against are TimeGenerated and RawData. Other columns are added to the table automatically after the transformation and are not available at the time of transformation. The _ResourceId column can't be used in the transformation.
Example
The sample uses basic KQL operators to parse the data in the RawData column into nine new columns, called user name, timestamp, latitude, longitude, sourcehost, state, label, destination and country 
•	The extend operator adds new columns.
•	The where operator:
Kusto Query Language
FAILED_RDP_WITH_GE0_CL
| extend  username = extract(@"username:([^,]+)", 1, RawData),
          timestamp = extract(@"timestamp:([^,]+)", 1, RawData),
          latitude = extract(@"latitude:([^,]+)", 1, RawData),
          longitude = extract(@"longitude:([^,]+)", 1, RawData),
          sourcehost = extract(@"sourcehost:([^,]+)", 1, RawData),
          state = extract(@"state:([^,]+)", 1, RawData),
          label = extract(@"label:([^,]+)", 1, RawData),
          destinationhost = extract(@"destinationhost:([^,]+)", 1, RawData),
          country = extract(@"country:([^,]+)", 1, RawData)
|summarize event_count=count() by sourcehost, latitude, longitude, state, label, destinationhost, country | where destinationhost != "samplehost"                                                                                                                                                                    | where sourcehost != " 
Testing Extracts completed
Setup map in sentinel with Latitude and Longitude ( or Country)
To do that go to portal.azure.com and type sentinel in the search box.  In the sentinel windows click on the workspace “Honeypotone”, I see the general sentinel dashboard, I see the logs coming in, the failed rdp with failed RDP with the geolocation, and List the security events from the virtual machine; I am going to set my actual geo map in the workbook
 
I click on workbooks in the left panel > Add workbook
 
Click edit and remove these two widgets these are default widgets so click on the three dots beside the edit button and select remove
 
I have removed the two widgets and now I going to add a query
 
Next Windows I will paste the query in the Log Analytics workspace Logs Query box.. see it in the next screenshot.
 
After entering the query code and running and making sure it is running ok I Select Map under the Visualization tab to see the map. You will see the map settings on the right where you can plot the map by the country field or I can use the latitude and longitude field, so click the down arrow of the latitude and select the latitude, the same with the Longitude box and click on apply and it will show on the map a dot with the location of the Lat and Long.
-On the Metric Settings - label this by label and it will show the country dash IP address.                           ---Under Metric Value select event_count and click apply and it will show the numbers of how many failed login recurrences from a particular country with an IP address. After doing it.                                                                     -I click the save icon and name it,  under the title box type “Failed RDP World Map” as name > under location box select my location, and click save button > done editing I will come back in a few minutes to see how many attackers are plotted on the map.
In the meantime I back to the VM the PowerShell script has to be run in the VM like a proof of concept implementation. There is a better way to go about extracting IPgeo data, doing this way makes sense to the representation of the task extracting geodata using the API ingesting into the log analytics workspace mapping it out in SENTINEL. So make sure this PowerShell is still running so when subsequent login fails they will be recorded, if the script Is not running all the events will go to the Windows event log but they won’t go into my custom log which is what we need for the extractionn of the geodata. Below with have Sentinel mapping all the attacks into the map regions by country or IP address. 
 
Fixing Map plot sizes under the Map Setting
 
Done!
