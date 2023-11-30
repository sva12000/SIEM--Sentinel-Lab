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

 The deployment is in progress while is in progress I do the next step crerate the Log analytic

 ![SentLab VM_H](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/7e496f6b-1e9e-4e6f-a510-33a265806efb)

<h1>Create Log Analytics Workspace</h1>
The purpose of creating a LAW is to kind of ingest logs from the virtual machine. The Windows events Logs. Also, we are going to create our custom log that contains Geographic information that allows us to know where the attackers are coming from. To do so we are going to create log analytics and log analytics workspace this is where those logs will be stored and Our SIEM Azure Sentinel will connect to the workspace to be able to display the geodata information on the Map. First, go to the Log Analytics page and create a LAW. Click the Create Log Analytics workspace button.

![SentLab LAW](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/defed965-4f77-4d70-a0c8-59d0562c8e74)

Log Analytics Workspace windows.

![SentLab LAW_A](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/5e106d25-2959-4e5f-bd6d-dd9ceb5cf408)

On the Resource group box, I select the Resource group I created honeypotlab on Instace details’s Name box I enter the name “HoneyPotOne select the Region Canada East, and click the Review+Create button.

![SentLab LAW_B](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/f3950520-242c-4c79-95aa-6a89cebdf6f1)

Validation passed and click on create
 
![SentLab LAW_C](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/b854bcd6-0722-43ee-b188-82a6d269b377)

<h1>The Log Analytics deployment is complete</h1>

![SentLab LAW_D](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/84e9dc1d-5c1e-4975-915d-8babe5a85d53)

<h1>Enable gathering VM Logs in the Security Center</h1>
Next type security center on the Azure portal search box and select Azure security center under Marketplace, to enable the ability for Sec Center to gather logs from the VM into the Workspace, On the next page click on Open Defender for Cloud. In the Security Center Overview page select Environment Settings then click the Log Analytics Workspace that we created turn Defender ON and turn SQL OFF.
 
![SentLab SecCenter](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/92ec9527-74af-4a7c-b4c9-9adc58dc5393)

Security Center Overview page > Environment Settings

![SentLab SecCenter_A](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/fbaf7622-ee4e-4cfd-ae00-30c904788f9d)

Click on the Azure Subscription to display the Log Analytics Workspace that we create

![SentLab SecCenter_B](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/1a90dfe1-25d8-4863-8a64-78c193270088)

Click on Log Analytics Workspace that we created and on the next page turn Defender on by selecting Servers ON turn SQL servers off and click save 

![SentLab SecCenter_C](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/fbd8c61e-9105-496d-ad20-d8a50e35ebf6)

On the Data Collection window select All Events and click save 
 
![SentLab SecCenter_D](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/60a3d5fb-5b89-4d05-826a-2a4f7663d315)

Back to the LAW page and click on the log Analytics we created

![SentLab SecCenter_E](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/621906e8-98cc-4c0e-b936-60c912bc0c20)

On the next window on the left panel click on VM virtual machines under Under Classic tab and on the next page click on the VM we created

![SentLab SecCenter_F](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/9be68948-9df3-4205-ba62-f8caaf45686c)

Click on the VM we created and on the next page select Connect 

![SentLab SecCenter_G](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/a40fefa9-7cc5-44b2-ba37-a922a451d875)

<h1>Setup Azure Sentinel</h1>
While connecting to VM I will open a new Azure portal tab and I am going to set up Sentinel. So type sentinel on the portal search box and select it this is the SIEM we are going to use to picture the data attack on.

![SentLab AZ Sentinel](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/79243f93-022e-411c-abc9-8ead74fcf4ac)

Next, click on the Connect Workspace button 

![SentLab AZ Sent_A](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/fd5a4f12-2cea-4d55-9058-5bfb18c071a7)

Select the created workspace HoneyPotOne and click the Add button.

![SentLab AZ Sent_B](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/42ed81a1-0569-4fd6-b909-1d4c2ea99dbd)

Let it run and will be back after. Search for Virtual machines on the Azure portal search box. On the virtual machines page click the created VM 
Log into VM with Remote Desktop

![SentLab AZ Sent_C](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/8c15dcbe-5470-46be-a6d6-a4ff8bc81bba)

On the next page select the Public IP address of the VM, as we are going to use it in a Windows machine to access the VM via RDP.

![SentLab AZ Sent_D](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/352a58ac-50d4-4025-b487-2ad9aa6e6fd5)

Next, click on the start menu on your computer type remote desktop and launch the RDP window And add the public IP address of the VM to access it.

![SentLab AZ Sent_E](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/7bf28e31-4c05-42ae-a586-6af521850808)

Type the user ID and Password and click  yes to accept the certificate warning
 
![SentLab AZ Sent_F](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/4c1615d5-fa04-4e64-aa9b-72b8d0d65986)

On the welcome page
 
![SentLab AZ Sent_G](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/bd342dfb-02ce-4749-a445-30c701373f5f)

On the Choose privacy settings for your device page Switch the Yes to NO

![SentLab AZ Sent_H](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/4153c8e8-eaf5-48b3-9599-d39df08a3d1d)

And click on the accept button 

![SentLab AZ Sent_i](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/cac2c780-98a2-4c60-bbc3-66036ade7848)

This is your VM screen

![SentLab AZ Sent_J](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/7a18686e-d562-420b-88e1-0346b7848367)

Inside your VM launch your Browser minimize it and click on the start menu to open Windows Event Viewer windows.
Observer Event Viewer Logs in.

![SentLab AZ Sent_K](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/0dd50509-05f4-4bd8-860e-d37020ad40c8)

On the start menu select event viewer, let’s look at some logs that we are going to use on the geo map.

![SentLab AZ Sent_L](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/26c78ae2-7d2b-4c47-be07-171feb67c125)

On the event, viewer log windows click the arrow Windows Log and select Security to display the security logs as shown on the below screen. Those are all the security events in that virtual machine. Concentrate on event ID 4625 Account failed to log on. So From my regular computer, I try to log in with the wrong user ID and Password to create Event ID failures on Windows Event Logs. we will see those failures when I look at the Windows event ID Security Log.

![SentLab AZ Sent_M](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/dfe23f64-e91b-4f7f-9e3c-abe87ef25361)

On this window, you can see Event ID 4625 showing the account name trying to log on and failure information “ with failure reason “Unknown user name or bad password. Also, you see the IP address of the machine that trying to get into our machine.

![SentLab AZ Sent_N](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/e5fcdac6-bcc4-432c-9e98-640e20c81f92)

This screenshot shows the IP address of the machine that tries to get into our VM. Which is the machine I use to try to connect to the VM. The Event Log will record all the IP Addresses that will try to log into this VM and will be seen on the Geo Map. Here you do not see any indication of where the IP address is located in the world.

![SentLab AZ Sent_O](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/22dd64ae-0193-4436-b2f9-06bb8d83e4ec)

To see the location I going to use PowerShell to program it, to take those IP addresses, and then use an IPG Location API.
 
![SentLab AZ Sent_P](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/85626729-dc6b-4cd4-b231-8341b43f1986)

<h>ipgeolocation</h1>
I am going to use this website “ https://ipgeolocation.io” What I going to do is post this IP address to the API and the API will return the Country name, state_province, city, latitude, longitude, time zone, etc. and I am going to use that data to create my own Custom Log and send that log to log analytics workspace in Azure and I am going to use Sentinel (SIEM) to kind of read that latitude and longitude or any other data to be used to plot out on the map the different attackers that are trying to breach into my VM. I will show you how to do that but first I will make sure to turn the firewall off in the VM so the VM can respond to ICMP echo requests so people can discover it on the internet faster.
Turn the Firewall Off in the VM
On VM click on start menu search and type wf.msc and click enter to open the Windows firewall  
 
![SentinelLab IP_Geolocation](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/d42811b6-31fd-4def-a063-ee9deaee692e)

Click Windows Defender firewall properties, in the properties windows under the Domain Profile tab set FireWall state: to OFF as well on the Private Profile tab and Public Profile tab all firewall states set to OFF and click on Apply and OK and minimize VM
 
![SentinelLab IP_Geolocation_A](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/9a41140f-b3f3-4c6a-92eb-aaef90da60c3)

Next, in my computer, I open a command line and I will ping my Virtual machine using dash t (-t) for perpetual ping and I am getting a reply from the VM

![SentinelLab IP_Geolocation_B](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/590c4d27-4009-403d-872b-fcabb54fa686)
 
<h1>Download PowerShell Script file saved on this repository</h1>
Open PowerShell ISE on your VM  

![SentinelLab PowerShell](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/bc86aba3-5e64-4928-a604-5ad75ea50f98)

When Powershell is opened we are going to click on the new file under the file and we are going to paste the downloaded programming code into the Powershell script window of the new file.
 
![SentinelLab PowerShell_A](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/cc2ddd72-0aa2-40a8-980c-d4fde27e1ba8)

Pasted PowerShell file

![SentinelLab PowerShell_B](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/b6de9908-ea8e-4749-90dd-38e6ec290ebd)

Save PowerShell file in the VM desktop as Log_Exporter
 
![SentinelLab PowerShell_C](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/f7f6e574-26f0-47fc-a9aa-cb0bfb46d23e)

<h1>Get Geolocation.io API Key</h1>
To convert the IP Address to Latitude and Longitude you need to get your own API_KEY by going to the https://ipgeolocation.io/ and clicking on sign up to get the API key.

![SentLab Geolocation_API_Key](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/0b093ed1-11cb-439d-abbf-5a6fb902cf95)

Sign in and copy the API-Key and past it on the PowerShell script

![SentLab Geolocation_API_Key_A](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/a8efbd81-87cf-46ca-b1ba-fb5e276ad1fd)

Pasted the API _Key from the Ipgeolocation.io into the PowerShell Log_Exporter file’s API_KEY section 
 
![SentLab Geolocation_API_Key_B](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/128e8fa6-fefa-487e-be93-2ff52ab14371)

<h1>Run Script to get Geo Data from Attackers</h1>
the event log that I looked at earlier. So what it does is run in perpetuity and capture the geo data and create a new log file, that will be output to C:\ProgramData\
The purple lines will be displayed every time a failed login is recorded in the event viewer. All this failure information is sent out to the API address 

![SentLab Run_Script](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/26e9761d-2222-4bc9-b51e-4f6e0d7c6eee)

if you go to the C:\programData\ now there is a log file in there that has been automatically created this fail logon file is named failed_rdp file with some sample records in it with the country and geodata that I will use later to train log analytics workspace to accept and parse out my custom log.
  
![SentLab Run_Script_A](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/f58955ee-5353-4a22-8c83-7f5b315fe0d7)

Open the failed_rdp file and the first few lines are like the sample data that I am going to train analytics workspace and then the rest are actual real failed logins the two lines at the end are my intentional login with a bad password and wrong user-id
“username:sergadmin1,sourcehost:178.128.225.19, state: Ontario, label: Canada”
 
![SentLab Run_Script_B](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/fcdc5309-3f67-4fc5-9123-2154b9bd7a1c)

<h1>Create a custom log in LAW Log Analytics Workspace</h1> 
First of all, in the Azure portal search box, I type Log Analytics to access the Log Analytics workspace page and click on the workspace name HoneyPotOne > Tables. Select Create and then New custom log (MMA-based). By default, all configuration changes are automatically pushed to all agents. In the next window click on the folder icon to add the file Log.
(This file is the failed_rdp file I saved on my VM desktop, open the file and copy the content minimize the VM> go to my  PC open Notepad, and paste the content on it > save as failed_rdp.log on my desktop. I will use this file to train the Log Analytics on what to search for in the log file.
Next back to Create a custom log page > click on the folder icon to add the file failed_rdp.log > click Next > click next again > select windows under type box > Type C:\ProgramData\failed_rdp.log in the Path box

![SentinelLab Custom_LAW](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/31926464-102c-4ef6-9be8-b84fef3bda99)

Type C:\ProgramData\failed_rdp.Log in the Path box and click next> Add a name to the custom log: FAILED_RDP_WITH_GEO and click the next button >  Click create > Click Review + Create

![SentinelLab Custom_LAW_A](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/785a9b0b-574f-4a01-a8ee-cf08c49bef9b)

Add a name and description to the custom log: Name it "FAILED_RDP_WITH_GEO".

![SentinelLab Custom_LAW_B](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/c827e82d-4833-4dbe-984a-26b40ce85d7b)

Click on Create

![SentinelLab Custom_LAW_C](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/eeb89ac9-08a2-4e89-b0b3-0ca09542f755)

Search for the custom log file name 

![SentinelLab Custom_LAW_D](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/b965f01f-7117-4b4b-8884-fe1bc95227f4)

Next Back to the Log Analytics workspace name page and click on Logs on the left panel > In the Query window type the file name “dailed_rdp_with_geo_CL in the box under the run button >and click run

![SentinelLab Custom_LAW_E](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/5739a5fb-200f-4f34-bb5b-98ddf511f993)

What you will see after clicking on run are the custom log entries. You may have to wait 10 – 15 minutes before you can see any logs. The first logs that you will see are our sample logs and then the final two are where I failed to log

![SentinelLab Custom_LAW_F](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/efc9b1ab-0e2e-4ed8-90c3-14eaf28da70d)

I came back to my VM and the lines at the bottom I  highlighted are my custom log entries if you come back to Azure portal Log analytics workspace> HoneyPotOne windows the highlighted raw data column contains the whole lines I highlighted previously on the VM file failed_rdp.log 

![SentinelLab Custom_LAW_G](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/365787d3-1c41-45dd-ad25-0c47d7f0fd15)

if you come back to Azure portal Log analytics workspace> HoneyPotOne windows the highlighted RawData column contains the whole lines I highlighted previously on the VM file failed_rdp.log 

![SentinelLab Custom_LAW_H](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/8b9c777a-63da-4263-adc6-6e10f91628ec)

Right now I can check SecurityEvent 

![SentinelLab Custom_LAW_i](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/1b4b49c8-83ca-4fd0-b9fa-a5aef25de0c6)

I can search for a specific eventID as this is the security event log from the VM sent into the log analytics workspace I can search for any eventID on the log for ei; SecurityEvent | where EventID == 4625 and click on the run button.  It will return all of the failed RDP logs. If you expand it you can see the user name, IP address, and other information of the attacker

![SentinelLab Custom_LAW_J](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/6ac8ee67-be5a-4360-8979-5ee57da97753)

<h1>Create a KQL query to fields/extract</h1> 	
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
|summarize event_count=count() by sourcehost, latitude, longitude, state, label, destinationhost, country | where destinationhost != "samplehost"    | where sourcehost != " "

![SentinelLab KQL_Query](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/dcdbaa56-f790-48e8-89fa-a1e1b3739b24)

<h1>Testing Extracts completed</h1>
<h1>Setup map in sentinel with Latitude and Longitude ( or Country)</h1>
To do that go to portal.azure.com and type sentinel in the search box.  In the sentinel windows click on the workspace “Honeypotone”, I see the general sentinel dashboard, I see the logs coming in, the failed rdp with failed RDP with the geolocation, and List the security events from the virtual machine; I am going to set my actual geo map in the workbook

![SentinelLab MAP](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/359027f2-340e-47b5-8d1f-9df5f77ae10e)

Click on workbooks in the left panel > Add workbook

![SentinelLab MAP_A](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/28f54752-3137-483c-9f1f-92ef67a347bd)

Click edit and remove these two widgets these are default widgets so click on the three dots beside the edit button and select remove.

![SentinelLab MAP_B](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/8e1c9ffe-02b1-4613-b2af-1350baf38bb4)

I have removed the two widgets and now I going to add a query.

![SentinelLab MAP_C](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/41b0a97e-7970-4247-b138-7d7d783667cd)

Past the query in the Log Analytics workspace Logs Query box.. see it in the next screenshot.

![SentinelLab MAP_D](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/e1ae58a3-bc00-4d05-87f4-cae94a3f173e)

After entering the query code and running and making sure it is running ok I Select Map under the Visualization tab to see the map. You will see the map settings on the right where you can plot the map by the country field or I can use the latitude and longitude field, so click the down arrow of the latitude and select the latitude, the same with the Longitude box and click on apply and it will show on the map a dot with the location of the Lat and Long.

-On the Metric Settings - label this by label and it will show the country dash IP address ---                                                   
 Under Metric Value select event_count and click apply and it will show the numbers of how many failed login recurrences from a particular country with an IP address. After doing it.   
 
-I click the save icon and name it, under the title box type “Failed RDP World Map” as name > under location box select my location, and click save button > done editing I will come back in a few minutes to see how many attackers are plotted on the map.

In the meantime I back to the VM the PowerShell script has to be run in the VM like a proof of concept implementation. There is a better way to go about extracting IPgeo data, doing this way makes sense to the representation of the task extracting geodata using the API ingesting into the log analytics workspace mapping it out in SENTINEL. So make sure this PowerShell is still running so when subsequent login fails they will be recorded, if the script Is not running all the events will go to the Windows event log but they won’t go into my custom log which is what we need for the extractionn of the geodata. Below with have Sentinel mapping all the attacks into the map regions by country or IP address. 

![SentinelLab MAP_E](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/899478f1-de96-4114-9e18-5578075e7321)

Fixing Map plot sizes under the Map Setting 

![SentinelLab MAP_F](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/ca4d5ffa-9bd8-4e38-8899-0585e4009e9e)

click Apply and next window you are done.

![SentinelLab MAP_G](https://github.com/sva12000/SIEM--Sentinel-Lab/assets/43139150/e34c4516-a902-4434-822f-02e0bcefc186)


Congratulations !!!! Project is Completed!!
