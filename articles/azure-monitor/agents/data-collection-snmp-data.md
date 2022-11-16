---
title: Send SNMP traps to Azure Monitor Logs using Azure Monitor Agent
description: Learn how to collect SNMP traps using Azure Monitor Agent.  
ms.topic: how-to
ms.date: 06/22/2022
ms.reviewer: shseth

---

# Send SNMP traps to Azure Monitor Logs using Azure Monitor Agent
  
Simple Network Management Protocol (SNMP) is a widely-deployed management protocol for monitoring and configuring Linux devices and appliances.  
  
There are two ways to collect SNMP data: 

- **Polls** - The managing system polls an SNMP agent to gather values for specific properties.
- **Traps** - An SNMP agent forwards events or notifications to a managing system. 

Traps are most often used as event notifications, while polls are more appropriate for stateful health detection and collecting performance metrics.  
  
This article explains how to send SNMP traps to Azure Monitor Logs as syslog events or SNMP events using Azure Monitor Agent.


## Overview
  
- In this article, we describe how to set up an SNMP trap receiver that called most Linux distributions provide, called **snmptrapd**, from the [Net-SNMP](https://www.net-snmp.org/) agent. However, there are many SNMP trap receiver services you can use.

    The snmptrapd configuration procedure can vary slightly among Linux distributions.  For more information on snmptrapd configuration, including guidance on configuring for SNMP v3 authentication, see the [Net-SNMP documentation](https://www.net-snmp.org/docs/man/snmptrapd.conf.html).  


- SNMP identifies monitored properties using Object Identifier (OID) values, which are defined and described in vendor-provided Management Information Base (MIB) files.  

    It's important that the SNMP trap receiver you use can load MIB files for your environment, so that the properties in the SNMP trap message have meaningful names instead of OIDs.  
 
## Set up snmptrapd

To set up snmptrapd on a CentOS 7, Red Hat Enterprise Linux 7, Oracle Linux 7 server:

1. Install and enable snmptrapd: 

    ```bash
    #Install the SNMP agent
    sudo yum install net-snmp
    #Enable the service
    sudo systemctl enable snmptrapd
    #Allow UDP 162 through the firewall
    sudo firewall-cmd --zone=public --add-port=162/udp --permanent
    ```
1. To enable logging SNMP trap fields with their names, instead of by OIDs, place all MIB files in `/usr/share/snmp/mibs`, which is the default directory for MIB files. 

    Copy all MIB files to this directory for each device that sends SNMP traps. 

    > [!NOTE] Some vendors, maintain a single MIB for all devices, while others, have many hundreds of MIB files. For snmptrapd to load an MIB file correctly, it must load all dependent MIBs. Be sure to check the snmptrapd log file after loading MIBs to ensure that there are no missing dependencies in parsing your MIB files.  

1. Authorize community strings (SNMP v1 & v2 authentication strings): 
  
    1. Edit `snmptrapd.conf`: 
    
        ```bash
        sudo vi /etc/snmp/snmptrapd.conf  
        ```        

    1.  Ensure that the following line exists in your `snmptrapd.conf` file to allow all traps for all OIDs, from all sources, with a community string of *public*: 
    
        ```bash
        authCommunity log,execute,net public
        ```

## Configure snmptrapd to send traps to syslog or text file

There are two ways snmptrapd can send SNMP traps to Azure Monitor Agent: 

- Forward incoming traps to syslog, which Azure Monitor Agent collects if you [create a data collection rule to collect syslog data](../../sentinel/forward-syslog-monitor-agent.md). 

- Write the syslog messages to a file, which can be *tailed* and parsed by Azure Monitor Agent for collection. This option may be preferable, as you can send the SNMP traps as a new datatype rather than sending as syslog events.  
    
To edit the output behavior configuration of snmptrapd on Red Hat, CentOS, and Oracle Linux: 

1. Open the `/etc/snmp/snmptrapd.conf` file: 
    
    ```bash
    sudo vi /etc/sysconfig/snmptrapd
    ```    
1. Add this line to the `LOGGING` section of the file to format the logs for collection by Azure Monitor Agent:
    
    ```bash
    format2 snmptrap %a %B %y/%m/%l %h:%j:%k %N %W %q %T %W %v \n
    ```
    
    > [!NOTE]
    > snmptrapd logs both traps and daemon messages - for example, service stop and start - to the same log file. In the example above, we’ve defined the log format to start with the word “snmptrap” to make it easy to filter snmptraps from the log later on.  

1. Run the command line,     

    Here’s an example configuration:  

    ```bash        
    # snmptrapd command line options# '-f' is implicitly added by snmptrapd systemd unit file# OPTIONS="-Lsd"OPTIONS="-m ALL -Ls2 -Lf /var/log/snmptrapd"
    ```  
        
    The options in this example configuration are:  
    
        - `-m ALL` - Load all MIB files in the default directory.
        - `-Ls2` - Output traps to syslog, to the Local2 facility.
        - `-Lf /var/log/snmptrapd` - Log traps to the `/var/log/snmptrapd` file. 
        
For more information, see: 
- https://www.net-snmp.org/docs/man/snmpcmd.html for how to set output options. 
- https://www.net-snmp.org/docs/man/snmptrapd.html for how to set formatting options. 
    
## Collect SNMP traps using Azure Monitor Agent

If you configured snmptrapd to send events to syslog, follow the steps described in [Collect events and performance counters with Azure Monitor Agent](../agents/data-collection-rule-azure-monitor-agent.md). Make sure to select **Linux syslog** as the data source when you define the data collection rule for Azure Monitor Agent.

If you configured snmptrapd to write events to a file, follow the steps described in [Collect text and IIS logs with Azure Monitor agent](../agents/data-collection-text-log.md).

## Next steps

Learn more about: 
- [Best practices for cost management in Azure Monitor](../best-practices-cost.md).
- [Azure Monitor agent](azure-monitor-agent-overview.md).
- [Data collection rules](../essentials/data-collection-rule-overview.md).