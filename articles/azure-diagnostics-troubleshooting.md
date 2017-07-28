<properties
    pageTitle="Troubleshooting Azure Diagnostics"
    description="Troubleshoot problems when using Azure diagnostics in Azure Cloud Services, Virtual Machines and "
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Azure Diagnostics Troubleshooting
Troubleshooting information relevant to using Azure Diagnostics. For more information on Azure diagnostics, see [Azure Diagnostics Overview](azure-diagnostics.md#cloud-services).

## <a name="azure-diagnostics-is-not-starting"></a>Azure Diagnostics is not Starting

Diagnostics is comprised of two components: A guest agent plugin and the monitoring agent. You can check the log files **DiagnosticsPluginLauncher.log** and **DiagnosticsPlugin.log** for information on why diagnostics fails to start.  
  
In a Cloud Service role, log files for the guest agent plugin are located in: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

In an Azure Virtual Machine, log files for the guest agent plugin are located in: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
The last line of the log files will contain the exit code.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

The plugin returns the following exit codes:

Exit Code|Description
---|---
0|Success.
-1|Generic Error.
-2|Unable to load the rcf file.<p>This is an internal error that should only happen if the guest agent plugin launcher is manually invoked, incorrectly, on the VM.
-3|Cannot load the Diagnostics configuration file.<p><p>Solution: This is the result of a configuration file not passing schema validation. The solution is to provide a configuration file that complies with the schema.
-4|Another instance of the monitoring agent Diagnostics is already using the local resource directory.<p><p>Solution: Specify a different value for **LocalResourceDirectory**.
-6|The guest agent plugin launcher attempted to launch Diagnostics with an invalid command line.<p><p>This is an internal error that should only happen if the guest agent plugin launcher is manually invoked, incorrectly, on the VM.
-10|The Diagnostics plugin exited with an unhandled exception.
-11|The guest agent was unable to create the process responsible for launching and monitoring the monitoring agent.<p><p>Solution: Verify that sufficient system resources are available to launch new processes.<p>
-101|Invalid arguments when calling the Diagnostics plugin.<p><p>This is an internal error that should only happen if the guest agent plugin launcher is manually invoked, incorrectly, on the VM.
-102|The plugin process is unable to initialize itself.<p><p>Solution: Verify that sufficient system resources are available to launch new processes.
-103|The plugin process is unable to initialize itself. Specifically it is unable to create the logger object.<p><p>Solution: Verify that sufficient system resources are available to launch new processes.
-104|Unable to load the rcf file provided by the guest agent.<p><p>This is an internal error that should only happen if the guest agent plugin launcher is manually invoked, incorrectly, on the VM.
-105|The Diagnostics plugin cannot open the Diagnostics configuration file.<p><p>This is an internal error that should only happen if the Diagnostics plugin is manually invoked, incorrectly, on the VM.
-106|Cannot read the Diagnostics configuration file.<p><p>Solution: This is the result of a configuration file not passing schema validation. So the solution is to provide a configuration file that complies with the schema. You can find the XML that is delivered to the Diagnostics extension in the folder *%SystemDrive%\WindowsAzure\Config* on the VM. Open the appropriate XML file and search for **Microsoft.Azure.Diagnostics**, then for the **xmlCfg** field. The data is base64 encoded so you’ll need to [decode it](http://www.bing.com/search?q=base64+decoder) to see the XML that was loaded by Diagnostics.<p>
-107|The resource directory pass to the monitoring agent is invalid.<p><p>This is an internal error that should only happen if the monitoring agent is manually invoked, incorrectly, on the VM.</p>
-108    |Unable to convert the Diagnostics configuration file into the monitoring agent configuration file.<p><p>This is an internal error that should only happen if the Diagnostics plugin is manually invoked with an invalid configuration file.
-110|General Diagnostics configuration error.<p><p>This is an internal error that should only happen if the Diagnostics plugin is manually invoked with an invalid configuration file.
-111|Unable to start the monitoring agent.<p><p>Solution: Verify that sufficient system resources are available.
-112|General error


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnostics Data is Not Logged to Azure Storage
Azure diagnostics stores all data in Azure Storage.

The most common cause of missing event data is incorrectly defined storage account information.

Solution: Correct your Diagnostics configuration file and re-install Diagnostics.
If the issue persists after re-installing the diagnostics extension then you may have to debug further by looking through the any monitoring agent errors. Before event data is uploaded to your storage account it is stored in the LocalResourceDirectory.

For Cloud Service Role the LocalResourceDirectory is:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

For Virtual Machines the LocalResourceDirectory is:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

If there are no files in the LocalResourceDirectory folder, the monitoring agent is unable to launch. This is typically caused by an invalid configuration file, an event that should be reported in the CommandExecution.log.

If the Monitoring Agent is successfully collecting event data you will see .tsf files for each event defined in your configuration file. The Monitoring Agent logs its errors in the file MaEventTable.tsf. To inspect the contents of this file you can use the tabel2csv application to convert the .tsf file to a comma separated values(.csv) file:

On a Cloud Service Role:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

*%SystemDrive%* on a Cloud Service Role is typically D:

On a Virtual Machine:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

The above commands generates the log file *maeventtable.csv*, which you can open and inspect for failure messages.    


## <a name="diagnostics-data-tables-not-found"></a>Diagnostics data Tables not found
The tables in Azure storage holding Azure diagnostics data are named using the code below:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Here is an example:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

That will generate 4 tables:

Event|Table Name
---|---
provider=”prov1” &lt;Event id=”1” /&gt;|WADEvent+MD5(“prov1”)+”1”
provider=”prov1” &lt;Event id=”2” eventDestination=”dest1” /&gt;|WADdest1
provider=”prov1” &lt;DefaultEvents /&gt;|WADDefault+MD5(“prov1”)
provider=”prov2” &lt;DefaultEvents eventDestination=”dest2” /&gt;|WADdest2