<properties 
pageTitle="Cloud Services Role config XPath cheat sheet | Microsoft Azure" 
description="The various XPath settings you can use in the cloud service role config to expose settings as an environment variable." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Expose role configuration settings as an environment variable with XPath

In the cloud service worker or web role service definition file, you can expose runtime configuration values as environment variables. The following XPath values are supported (which correspond to API values).

These XPath values are also available through the [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) library. 

## <a name="app-running-in-emulator"></a>App running in emulator

Indicates that the app is running in the emulator.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Code  | var x = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>Deployment ID

Retrieves the deployment ID for the instance.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Code  | var deploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>Role ID 

Retrieves the current role ID for the instance.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Code  | var id = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Update domain

Retrieves the update domain of the instance.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Code  | var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Fault domain

Retrieves the fault domain of the instance.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Code  | var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Role name

Retrieves the role name of the instances.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Code  | var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Config setting

Retrieves the value of the specified configuration setting.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Code  | var setting = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Local storage path

Retrieves the local storage path for the instance.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Code  | var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Local storage size

Retrieves the size of the local storage for the instance.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Code  | var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Endpoint protocol 

Retrieves the endpoint protocol for the instance.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Code  | var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protocol; |

## <a name="endpoint-ip"></a>Endpoint IP

Gets the specified endpoint's IP address.

| Type | Example |
| ----- | ---- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Code  | var address = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Endpoint port 

Retrieves the endpoint port for the instance.

| Type  | Example |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Code  | var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |





## <a name="example"></a>Example

Here is an example of a worker role that creates a startup task with an environment variable named `TestIsEmulated` set to the [@emulated xpath value](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Next steps

Learn more about the [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) file.

Create a [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) package.

Enable [remote desktop](cloud-services-role-enable-remote-desktop.md) for a role.
