<properties
    pageTitle="Containers solution in Log Analytics | Microsoft Azure"
    description="The Containers solution in Log Analytics helps you view and manage your Docker container hosts in a single location."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>



# <a name="containers-preview-solution-log-analytics"></a>Containers (Preview) solution Log Analytics

This article describes how to set up and use the Containers solution in Log Analytics, which helps you view and manage your Docker container hosts in a single location. Docker is a software virtualization system used to create containers that automate software deployment to their IT infrastructure.

With the solution, you can see which containers are running on your container hosts and what images are running in the containers. You can view detailed audit information showing commands used with containers. And, you can troubleshoot containers by viewing and searching centralized logs without having to remotely view Docker hosts. You can find containers that may be noisy and consuming excess resources on a host. And, you can view centralized CPU, memory, storage, and network usage and performance information for containers.

## <a name="installing-and-configuring-the-solution"></a>Installing and configuring the solution

Use the following information to install and configure the solution.

Add the Containers solution to your OMS workspace using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).

There are two ways to install and use Docker with OMS:

- On supported Linux operating systems, install and run Docker and then install and configure OMS Agent for Linux
- On CoreOS, install and run Docker and then configure the OMSAgent to run inside a container

Review the supported Docker and Linux operating system versions for your container host on [GitHub](https://github.com/Microsoft/OMS-docker).

>[AZURE.IMPORTANT] Docker must be running **before** you install the [OMS Agent for Linux](log-analytics-linux-agents.md) on your container hosts. If you've already installed the agent before installing Docker, you'll need to reinstall the OMS Agent for Linux. For more information about Docker, see the [Docker website](https://www.docker.com).

You need the following settings configured on your container hosts before you can monitor containers.

## <a name="configure-settings-for-the-linux-container-host"></a>Configure settings for the Linux container host

After you've installed Docker, use the following settings for your container host to configure the agent for use with Docker. CoreOS doesn't support this configuration method.

### <a name="to-configure-settings-for-the-container-host---systemd-suse-opensuse-centos-7x-rhel-7x-and-ubuntu-15x-and-higher"></a>To configure settings for the container host - systemd (SUSE, openSUSE, CentOS 7.x, RHEL 7.x, and Ubuntu 15.x and higher)

1. Edit docker.service to add the following:

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. Add $DOCKER\_OPTS in &quot;ExecStart=/usr/bin/docker daemon&quot; in your docker.service file. Using the following example.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Restart the Docker service. For example:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-ubuntu-14x"></a>To configure settings for the container host - Upstart (Ubuntu 14.x)

1. Edit /etc/default/docker and add the following:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Save the file and then restart the Docker and OMS services.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>To configure settings for the container host - Amazon Linux

1. Edit /etc/sysconfig/docker and add the following:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Save the file and then restart the Docker service.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>Configure settings for CoreOS containers

After you've installed Docker, use the following settings for CoreOS to run Docker and create a container. You can use any supported version of Linux—including CoreOS, with this configuration method. You'll need your [OMS workspace ID and key](log-analytics-linux-agents.md).

### <a name="to-use-oms-for-all-containers-with-coreos"></a>To use OMS for all containers with CoreOS

- Start the OMS container that you want to monitor. Modify and use the following example.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Switching from using an installed agent to one in a container

If you previously used the directly-installed agent and want to instead use an agent running in a container, you must first remove OMSAgent. See [Steps to install the OMS Agent for Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md).

## <a name="containers-data-collection-details"></a>Containers data collection details

The Containers solution collects various performance metrics and log data from container hosts and containers using the OMS Agents for Linux that you have enabled and from OMSAgent running in containers.

The following table shows data collection methods and other details about how data is collected for Containers.

| platform | OMS Agent for Linux | SCOM agent | Azure Storage | SCOM required? | SCOM agent data sent via management group | collection frequency |
|---|---|---|---|---|---|---|
|Linux|![Yes](./media/log-analytics-containers/oms-bullet-green.png)|![No](./media/log-analytics-containers/oms-bullet-red.png)|![No](./media/log-analytics-containers/oms-bullet-red.png)|            ![No](./media/log-analytics-containers/oms-bullet-red.png)|![No](./media/log-analytics-containers/oms-bullet-red.png)| every 3 minutes|


The following table show examples of data types collected by the Containers solution:

| Data type | Fields |
| --- | --- |
| Performance for hosts and containers | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue,TimeGenerated, CounterPath, SourceSystem |
| Container inventory | TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContinerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Container image inventory | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Container log | TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Container service log | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |

## <a name="monitor-containers"></a>Monitor containers

After you have the solution enabled in the OMS portal, you'll see the **Containers** tile showing summary information about your container hosts and the containers running in hosts.

![Containers tile](./media/log-analytics-containers/containers-title.png)

The tile shows an overview of how many containers you have in the environment and whether they're failed, running, or stopped.

### <a name="using-the-containers-dashboard"></a>Using the Containers dashboard

Click the **Containers** tile. From there you'll see views organized by:

- Container Events
- Errors
- Containers Status
- Container Image Inventory
- CPU and Memory performance

Each pane in the dashboard is a visual representation of a search that is run on collected data.

![Containers dashboard](./media/log-analytics-containers/containers-dash01.png)

![Containers dashboard](./media/log-analytics-containers/containers-dash02.png)

In the **Container Status** blade, click to top area, as shown below.

![Containers status](./media/log-analytics-containers/containers-status.png)

Log Search opens, displaying information about the hosts and containers running in them.

![Log Search for containers](./media/log-analytics-containers/containers-log-search.png)

From here, you can edit the search query to modify it to find the specific information you're interested in. For more information about Log Searches, see [Log searches in Log Analytics](log-analytics-log-searches.md).

For example, you can modify the search query so that it shows all the stopped containers instead of the running containers by changing **Running** to **Stopped** in the search query.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Troubleshoot by finding a failed container

OMS marks a container as **Failed** if it has exited with a non-zero exit code. You can see an overview of the errors and failures in the environment in the **Failed Containers** blade.

### <a name="to-find-failed-containers"></a>To find failed containers

1. Click the **Container Events** blade.  
  ![containers events](./media/log-analytics-containers/containers-events.png)
2. Log Search opens, displaying the status of containers, similar to the following.  
  ![containers state](./media/log-analytics-containers/containers-container-state.png)
3. Next, click the failed value to view additional information such as image size and number of stopped and failed images. Expand **show more** to view the image ID.  
  ![failed containers](./media/log-analytics-containers/containers-state-failed.png)
4. Next, find the container that is running this image. Type the following into the search query.
  `Type=ContainerInventory <ImageID>`This displays the logs. You can scroll to see the failed container.  
  ![failed containers](./media/log-analytics-containers/containers-failed04.png)


## <a name="search-logs-for-container-data"></a>Search logs for container data

When you're troubleshooting a specific error, it can help to see where it is occurring in your environment. The following log types will help you create queries to return the information you want.

- **ContainerInventory** – Use this type when you want information about container location, what their names are, and what images they're running.
- **ContainerImageInventory** – Use this type when you're trying to find information organized by image and to view image information such as image IDs or sizes.
- **ContainerLog** – Use this type when you want to find specific error log information and entries.
- **ContainerServiceLog** – Use this type when you're trying to find audit trail information for the Docker daemon, such as start, stop, delete, or pull commands.

### <a name="to-search-logs-for-container-data"></a>To search logs for container data

- Choose an image that you know has failed recently and find the error logs for it. Start by finding a container name that is running that image with a **ContainerInventory** search. For example, search for`Type=ContainerInventory ubuntu Failed`  
    ![Search for Ubuntu containers](./media/log-analytics-containers/search-ubuntu.png)

  Note the name of the container next to **Name**, and search for those logs. In this example, it is `Type=ContainerLog adoring_meitner`.

**View performance information**

When you're beginning to construct queries, it can help to see what's possible first. For example, to see all performance data, try a broad query by typing the following search query.

```
Type=Perf
```

![containers performance](./media/log-analytics-containers/containers-perf01.png)



You can see this in a more graphical form when you click the word **Metrics** in the results.

![containers performance](./media/log-analytics-containers/containers-perf02.png)



You can scope the performance data you're seeing to a specific container by typing the name of it to the right of your query.

```
Type=Perf <containerName>
```

That shows the list of performance metrics that are collected for an individual container.

![containers performance](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Example log search queries

It's often useful to build queries starting with an example or two and then modifying them to fit your environment. As a starting point, you can experiment with the **Notable Queries** blade to help you build more advanced queries.

![Containers queries](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Saving log search queries

Saving queries is a standard feature in Log Analytics. By saving them, you'll have those that you've found useful handy for future use.

After you create a query that you find useful, save it by clicking **Favorites** at the top of the Log Search page. Then you can easily access it later from the **My Dashboard** page.

## <a name="next-steps"></a>Next steps

- [Search logs](log-analytics-log-searches.md) to view detailed container data records.
