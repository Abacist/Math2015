| File Sharing Protocol Family                             
                                                           
 Server Test Suite User Guide                              |
|----------------------------------------------------------|
| *Windows is built to be the most interoperable platform* |
| ![](./media/image2.png)                                  |

<span id="_Toc282185720" class="anchor"><span id="_Toc282185832" class="anchor"><span id="_Toc361668067" class="anchor"><span id="_Toc425336618" class="anchor"></span></span></span></span>Introduction
========================================================================================================================================================================================================

This guide provides information about how to install, configure, and run File Sharing Test Suite in its environment. This suite is designed to test implementations of file sharing protocol family of protocols including Server Message Block (SMB) Protocol Version 2 and 3, File Server Remote VSS Protocol and Service Witness Protocol, The Distributed File System (DFS): Namespace Referral Protocol, Remote Shared Virtual Disk Protocol, as specified in the Microsoft document *\[MS-SMB2\], \[MS-FSRVP\] , \[MS-SWN\], \[MS-DFSC\], \[MS-FSA\], \[MS-SQOS\] and \[MS-RSVD\].* This guide provides information about using this test suite on the Microsoft® Windows® operating system and on operating systems that are not Windows based.

This suite tests only the protocol implementation behaviors that are observed on the wire. For detailed information about the design of this test suite, please see *FileSharing\_ServerTestDesignSpecification.docx*.

<span id="_Toc361668069" class="anchor"><span id="_Toc425336619" class="anchor"></span></span>License Information
=================================================================================================================

For licensing information, please see the End User License Agreement (EULA) that is provided with this test suite. The EULA is contained in the EULA.rtf file in the installation folder.

<span id="_Toc361668070" class="anchor"><span id="_Toc425336620" class="anchor"></span></span>Further Assistance
================================================================================================================

If you need further information about this test suite or assistance in troubleshooting issues related to this test suite, contact <dochelp@microsoft.com>.

<span id="_Toc283040536" class="anchor"><span id="_Toc361668071" class="anchor"><span id="_Toc425336621" class="anchor"></span></span></span>Quick Start Checklist
==================================================================================================================================================================

The following checklist summarizes the steps you need to complete to get the test suite up and running. The checklist also provides references to documentation that can help you get started.

![](./media/image3.png)Note

For workgroup environments, omit tasks that are related to the setup and configuration of domain controllers.

| **Check** | **Task**                                                                                 | **Topic**                                                                                                                                                                                                         |
|-----------|------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| □         | Download the test suite for the protocol implementation                                  | For a list of the files that the download package contains, please see section [8](#_Installed_Files_and_1).                                                                                                      |
| □         | Confirm that your test environment and computers meet the requirements of the test suite | For information about the requirements of the test suite, please see section [5](#_Requirements).                                                                                                                 |
| □         | Set up the Domain Controller (DC)                                                        | See section [7.2.1](#_Setup_the_Domain_1)[](#setup_other_computers).                                                                                                                                              |
| □         | Set up the system under test (SUT)                                                       | See section [7.1.2](#_Setup_the_SUT) or section [7.2.3](#_Setup_the_SUT_1) .                                                                                                                                      |
| □         | Set up the driver computer                                                               | See section [7.1.1](#_Setup_the_Driver_1) or section [7.2.2](#_Setup_the_Driver_2).                                                                                                                               |
| □         | Set up the network                                                                       | See section [6](#_Network_Setup_1).                                                                                                                                                                               |
| □         | Verify the connections from the driver computer to the SUT and other computers           | See section [6.3](#_Verify_Connectivity_from_1).                                                                                                                                                                  |
| □         | Install the software prerequisites                                                       | For information about software that must be installed on the computers in your test environment before the test suite is installed, please see section [5.7](#_Software_2)[](#z15189026c30248e59c67571a87444bd6). |
| □         | Configure the test suite settings                                                        | See section [9.1](#_Network_Configuration_2).                                                                                                                                                                     |
| □         | Run a BVT test                                                                           | Section [9.3.1](#_Run_Scenario0_Test)                                                                                                                                                                             |
| □         | Run test cases                                                                           | Section [9.3.2](#_Run_All_Test)                                                                                                                                                                                   |
| □         | Debug my own test cases                                                                  | Section [10](#_Debugging_Test_Cases)                                                                                                                                                                              |
| □         | Get the results of test runs                                                             | Section [9.3.3](#_Run_Specified_Test)                                                                                                                                                                             |

<span id="_Toc361668072" class="anchor"></span>

<span id="_Requirements" class="anchor"><span id="_Network_Configuration" class="anchor"><span id="_Toc283040538" class="anchor"><span id="_Toc361668073" class="anchor"><span id="_Toc425336622" class="anchor"></span></span></span></span></span>Requirements 
=================================================================================================================================================================================================================================================================

This section lists the requirements for the test environment used to run this test suite.<span id="_Toc283040539" class="anchor"><span id="_Toc361668074" class="anchor"></span></span>

Network Infrastructure
----------------------

-   A test network is required to connect the test computer systems.

-   It must consist of an isolated hub or switch.

-   It must not be connected to a production network or used for any other business or personal communications or operations.

-   It must not be connected to the internet.

-   IP addresses must be assigned for the test network.

-   Computer names should be assigned in the test network infrastructure.

-   User credentials used on the system must be dedicated to the test network infrastructure.

-   Network details including computer IP addresses, names and credentials will be saved in log files.

    -   Please refer to the Detailed Logging Support section.

> Please refer to the Privacy Statement and EULA for further information.

Environment
-----------

Run this test suite in a workgroup environment that contains the following computers (physical or virtual):

-   A driver computer running Microsoft® Windows 8.1 Enterprise.

-   A computer running either Microsoft® Windows Server 2016 Technical Preview 2, or a computer set up as SUT that is running non-Windows operating system.

Run this test suite in a domain environment that contains the following computers (physical or virtual):

-   A driver computer running Microsoft® Windows 8.1 Enterprise.

-   For scenarios involving cluster, two computers set up as failover cluster for a Windows-based SUT running Microsoft® Windows Server 2016 Technical Preview 2, or computers set up as cluster for SUT that is running non-Windows operating system.

    For scenarios not involving cluster, a computer running Microsoft® Windows Server 2016 Technical Preview 2, or a computer set up as SUT that is running non-Windows operating system. Either failover cluster node could be used as SUT for non-cluster involved scenarios here.

![](./media/image3.png)Note

For more information about scenarios involving cluster and otherwise, please check *FileSharing\_ServerTestDesignSpecification.docx* section 3 “Test Suite Design”.

-   A computer setup as Domain Controller (DC) running Microsoft® Windows Server 2016 Technical Preview 2 or non-Windows operating system.

-   A computer setup as SAN storage server running Microsoft® Windows Server 2016 Technical Preview 2, or device setup as SAN storage that is running non-Windows operating system.

<span id="_Test_Client_Computer" class="anchor"><span id="_Toc361668075" class="anchor"><span id="_Toc425336625" class="anchor"><span id="_Toc283040542" class="anchor"></span></span></span></span>Driver Computer 
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

The minimum requirements for the driver computer are as follows.

| **Requirement**  | **Description**                           |
|------------------|-------------------------------------------|
| Operating system | Microsoft Windows 8.1, Enterprise Edition |
| Memory           | 2 GB RAM                                  |
| Disk space       | 60 GB                                     |

<span id="_Toc425336626" class="anchor"><span id="_Toc361668076" class="anchor"><span id="_Toc296942782" class="anchor"></span></span></span>System Under Test (SUT)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------

The minimum requirements for the SUT are as follows.

| **Requirement**  | **Description**                                                                                                                                                  |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Operating system | For Windows platform: Microsoft Windows Server 2016 Technical Preview 2, Standard Edition, or a SUT implementation that is running non-Windows operating system. |
| Features         | For Windows platform: Failover Clustering                                                                                                                        |
| Role Services    | For Windows platform:                                                                                                                                            
                                                                                                                                                                                      
                    File Server                                                                                                                                                       
                                                                                                                                                                                      
                    DFS Namespaces                                                                                                                                                    
                                                                                                                                                                                      
                    File Server Resource Manager                                                                                                                                      
                                                                                                                                                                                      
                    File Server VSS Agent Service                                                                                                                                     |
| Memory           | 2 GB RAM                                                                                                                                                         |
| Disk space       | 60 GB                                                                                                                                                            |

Domain Controller (DC)
----------------------

The minimum requirements for the DC are as follows.

![](./media/image3.png)Note

The requirements in this section apply only to the Domain environment.

| **Requirement**  | **Description**                                                                                                                                       |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| Operating system | For Windows platform: Microsoft Windows Server 2016 Technical Preview 2, Standard Edition, or non-Windows operating system running DC implementation. |
| Services         | For Windows platform: Active Directory Domain Services (AD DS)                                                                                        |
| Memory           | 1 GB RAM                                                                                                                                              |
| Disk space       | 60 GB                                                                                                                                                 |

<span id="_Toc361668077" class="anchor"></span>

SAN Storage Server
------------------

The minimum requirements for the SAN storage server

![](./media/image3.png)Note

The requirements in this section apply only to the Windows-based computers in the test environment.

| **Requirement**  | **Description**                                                                                                                     |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Operating system | Microsoft Windows Server 2016 Technical Preview 2, Standard Edition, or a SAN storage that is running non-Windows operating system. |
| Services         | File Server                                                                                                                         
                                                                                                                                                         
                    iSCSI Target Server                                                                                                                  |
| Memory           | 1 GB RAM                                                                                                                            |
| Disk space       | 60 GB                                                                                                                               |

<span id="_Software_Requirements" class="anchor"><span id="_Software" class="anchor"><span id="_Software_1" class="anchor"><span id="_Toc361668079" class="anchor"><span id="_Toc283040544" class="anchor"></span></span></span></span></span>

<span id="_Software_2" class="anchor"><span id="_Toc425336629" class="anchor"></span></span>Software 
-----------------------------------------------------------------------------------------------------

All of the following software must be installed on the driver computer ***before*** the installation of this test suite. They have some dependencies on each other, it is recommended to install them in the order listed below so that they can work correctly.

1.  Install Microsoft® Visual Studio® 2012 or Test Agent.

2.  Install Spec Explorer.

3.  Install Microsoft Protocol Test Framework.

4.  Install Microsoft Message Analyzer.

5.  Install File Sharing Family Test Suite.

6.  Install Protocol Test Manager.

| **Required Software **                                                                                                                                                                                                          |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -   **Windows PowerShell** 3.0 is required.                                                                                                                                                                                     
                                                                                                                                                                                                                                  
 -   **Microsoft® Visual Studio® 2012 or Test Agent for Visual Studio 2012 **                                                                                                                                                     
                                                                                                                                                                                                                                  
     Execution of test cases requires vstest.console.exe utility which is included in either Visual Studio or Test Agent for Visual Studio.                                                                                       
                                                                                                                                                                                                                                  
 ![](./media/image3.png)Note                                                                                                                                                                                                      
                                                                                                                                                                                                                                  
 1.  Visual Studio provides additional debugging and test case authoring ability for you to understand and update the test better.                                                                                                
                                                                                                                                                                                                                                  
     You can download Visual Studio Ultimate 2012 Trial (30 days) from the website listed below:                                                                                                                                  
                                                                                                                                                                                                                                  
     <http://www.microsoft.com/en-us/download/details.aspx?id=30678>                                                                                                                                                              
                                                                                                                                                                                                                                  
     You can download Visual Studio 2012 Update 4 or later from                                                                                                                                                                   
                                                                                                                                                                                                                                  
     <http://www.microsoft.com/en-us/download/confirmation.aspx?id=39305>                                                                                                                                                         
                                                                                                                                                                                                                                  
 2.  By installing Visual Studio agent, you could run the cases by Protocol Test Manager or batch scripts without installing Visual Studio. <http://www.microsoft.com/en-us/download/details.aspx?id=38186>                       
                                                                                                                                                                                                                                  
 -   **Spec Explorer** (build 3.5.3146.0 or later)                                                                                                                                                                                
                                                                                                                                                                                                                                  
     Spec Explorer is a model based testing tool used by test suite to design model and generate test cases. It has some dependencies on VC++ 9.0 redistributable package which should be installed together.                     
                                                                                                                                                                                                                                  
 ![](./media/image3.png)Note                                                                                                                                                                                                      
                                                                                                                                                                                                                                  
 > You can download Spec Explorer from the website listed below:                                                                                                                                                                  
 >                                                                                                                                                                                                                                
 > <http://visualstudiogallery.msdn.microsoft.com/271d0904-f178-4ce9-956b-d9bfa4902745/>                                                                                                                                          
 >                                                                                                                                                                                                                                
 > You can download VC++ 9.0 redistributable package from the website listed below:                                                                                                                                               
 >                                                                                                                                                                                                                                
 > <http://www.microsoft.com/en-us/download/details.aspx?id=5582>                                                                                                                                                                 
                                                                                                                                                                                                                                  
 -   **Microsoft Protocol Test Framework** 1.0.2485.0                                                                                                                                                                             
                                                                                                                                                                                                                                  
     Microsoft Protocol Test Framework is a common framework to support protocol test suite.                                                                                                                                      
                                                                                                                                                                                                                                  
     ![](./media/image3.png)Note                                                                                                                                                                                                  
                                                                                                                                                                                                                                  
 > You can download Microsoft protocol Test Framework from the website listed below:                                                                                                                                              
 >                                                                                                                                                                                                                                
 > <https://connect.microsoft.com/site216/Downloads>                                                                                                                                                                              |
| **Optional Software **                                                                                                                                                                                                          |
| -   **Protocol Test Manager**                                                                                                                                                                                                   
                                                                                                                                                                                                                                  
 > Protocol Test Manager provides a graphical UI for easily configuring and running Microsoft Windows Protocol Test Suites. Microsoft® Visual Studio® 2012 or Test Agent must be installed before install Protocol Test Manager.  
                                                                                                                                                                                                                                  
 -   **Microsoft Message Analyzer**                                                                                                                                                                                               
                                                                                                                                                                                                                                  
 ![](./media/image3.png)Note                                                                                                                                                                                                      
                                                                                                                                                                                                                                  
 > You can download the latest Message Analyzer from the website listed below:                                                                                                                                                    
 >                                                                                                                                                                                                                                
 > <http://www.microsoft.com/en-us/download/details.aspx?id=44226>                                                                                                                                                                |

<span id="_Setup" class="anchor"><span id="_Network_Setup" class="anchor"></span></span>

<span id="_Network_Setup_1" class="anchor"><span id="_Toc361668080" class="anchor"><span id="_Toc425336630" class="anchor"></span></span></span>Network Setup
=============================================================================================================================================================

Run this test suite in a domain environment using either physical or virtual machines. This section describes the test environment using physical machines.

For information about configuring a virtual machine, please see <http://www.microsoft.com/virtualization/en/us/solution-appliance-test.aspx>. The configuration of virtual machines for use with this test suite is out of the scope of this guide.

Workgroup Environment
---------------------

The workgroup environment requires interactions between the following computers and server roles.

-   The driver computer runs the test cases by sending requests over the wire in the form of protocol messages.

-   The SUT runs an implementation of the protocol that is being tested. The SUT responds to the requests that the driver computer sends.

![](./media/image3.png)Note

The following diagram applies to both Windows based computers and Non-Windows based computers in the test environment.

| Machine Name/Access Point | NIC       | IPv4          | Subnet Mask   |
|---------------------------|-----------|---------------|---------------|
| Client01                  | External1 | 192.168.1.111 | 255.255.255.0 |
|                           | External2 | 192.168.1.112 | 255.255.255.0 |
| Node01                    | External1 | 192.168.1.11  | 255.255.255.0 |
|                           | External2 | 192.168.1.12  | 255.255.255.0 |

<span id="_Toc361668081" class="anchor"></span>

Domain Environment
------------------

The domain environment requires interactions between the following computers and server roles.

-   The driver computer runs the test cases by sending requests over the wire in the form of protocol messages.

-   The SUT runs an implementation of the protocol that is being tested. The SUT responds to the requests that the driver computer sends.

-   The domain controller (DC) responds to security authentication requests (logging in, checking permissions, etc.) within the domain.

-   Any additional computers and server roles are specified to provide functionality that is required to test the protocol implementation.

![](./media/image3.png)Note

The following diagram applies only to the Windows based computers in the test environment.

For scenarios involving cluster, driver computer in this test suite will simulate a physical machine to contact to Node01 and Node02 which are built as failover cluster for SUT. SAN storage server Storage01 is on an isolated network to provide backend storage for failover cluster.

For non-cluster scenarios, driver computer will simulate a physical machine than contacts either node which is acting as SUT.

The following figure shows the domain environment with recommended network configurations for reference.

![](./media/image3.png)Note

“Cluster01” is the Failover Cluster; “GeneralFS” is a File Server role provided by the Failover Cluster.

| Machine Name/Access Point | NIC       | IPv4          | Subnet Mask   | Default Gateway | DNS Server    |
|---------------------------|-----------|---------------|---------------|-----------------|---------------|
| DC01                      | External1 | 192.168.1.1   | 255.255.255.0 | &lt;empty&gt;   | 127.0.0.1     |
|                           | External2 | 192.168.2.1   | 255.255.255.0 | &lt;empty&gt;   | 127.0.0.1     |
| Client01                  | External1 | 192.168.1.111 | 255.255.255.0 | 192.168.1.1     | 192.168.1.1   |
|                           | External2 | 192.168.2.111 | 255.255.255.0 | 192.168.2.1     | 192.168.2.1   |
| Node01                    | External1 | 192.168.1.11  | 255.255.255.0 | 192.168.1.1     | 192.168.1.1   |
|                           | External2 | 192.168.2.11  | 255.255.255.0 | 192.168.2.1     | 192.168.2.1   |
|                           | Cluster   | 10.10.0.11    | 255.255.255.0 | &lt;empty&gt;   | &lt;empty&gt; |
|                           | iSCSI     | 192.168.0.11  | 255.255.255.0 | &lt;empty&gt;   | &lt;empty&gt; |
| Node02                    | External1 | 192.168.1.12  | 255.255.255.0 | 192.168.1.1     | 192.168.1.1   |
|                           | External2 | 192.168.2.12  | 255.255.255.0 | 192.168.2.1     | 192.168.2.1   |
|                           | Cluster   | 10.10.0.12    | 255.255.255.0 | &lt;empty&gt;   | &lt;empty&gt; |
|                           | iSCSI     | 192.168.0.12  | 255.255.255.0 | &lt;empty&gt;   | &lt;empty&gt; |
| Storage01                 | iSCSI     | 192.168.0.1   | 255.255.255.0 | &lt;empty&gt;   | &lt;empty&gt; |
| Cluster01                 | External1 | 192.168.1.100 |               |                 |               |
|                           | External2 | 192.168.2.100 |               |                 |               |
| GeneralFS                 | External1 | 192.168.1.200 |               |                 |               |
|                           | External2 | 192.168.2.200 |               |                 |               |

![](./media/image3.png)Note

> For Non-Windows, Node01, Node02 and Storage01 could be replaced by the implementation-specific SUT.

<span id="_Verify_Connectivity_from_1" class="anchor"><span id="_Toc361668082" class="anchor"><span id="_Toc425336633" class="anchor"></span></span></span>Verify Connectivity from the Driver Computer
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

After you install the environment, verify the connection from the driver computer to the SUT, and between all other computers in the test environment. You can perform following steps to test the connectivity between two Windows based computers. For further information, please see the administration guide for your operating system.

To check the connection from the driver computer

![](./media/image3.png)Note

Disable active firewalls in the test environment.

1.  Click the **Start** button, and then click **Run**.

2.  In the **Run** dialog box, type **cmd** and then click **OK**.

3.  At the command prompt, type **ping** followed by the hostname or IP address of the SUT, and then press **Enter**. The following example checks the connection to a SUT named as "NODE01":

&gt; ping NODE01

1.  Repeat these steps until you confirm connectivity between all computers in the test environment.

Do not proceed with the configuration of the test suite until connectivity is confirmed. Any issues with network connectivity must be resolved before you configure the test suite.

<span id="_Computer_Setup" class="anchor"><span id="_Toc361668083" class="anchor"><span id="_Toc425336634" class="anchor"><span id="_Toc283040545" class="anchor"><span id="_Ref282699723" class="anchor"></span></span></span></span></span>Computer Setup 
============================================================================================================================================================================================================================================================

This section explains how to set up the computers for the test environment.

1.  <span id="_Toc362208431" class="anchor"><span id="_Toc362208747" class="anchor"><span id="_Toc362260266" class="anchor"><span id="_Toc362260581" class="anchor"><span id="_Toc362725879" class="anchor"><span id="_Toc362726269" class="anchor"><span id="_Toc362208432" class="anchor"><span id="_Toc362208748" class="anchor"><span id="_Toc362260267" class="anchor"><span id="_Toc362260582" class="anchor"><span id="_Toc362725880" class="anchor"><span id="_Toc362726270" class="anchor"><span id="_Test_Client_Computer_1" class="anchor"><span id="_Setup_of_the_1" class="anchor"><span id="_System_Under_Test" class="anchor"><span id="_Setup_the_Driver" class="anchor"><span id="_Toc296942791" class="anchor"><span id="_Toc361668084" class="anchor"></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span> Workgroup Environment
    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

2.  ### <span id="_Setup_the_Driver_1" class="anchor"><span id="_Toc425336636" class="anchor"></span></span>Set up the Driver Computer

This section describes how to set up the driver computer.

![](./media/image6.png)Important

Microsoft Visual Studio 2012, Protocol Test Framework, and Spec Explorer must be installed on the driver computer before you run the test suite installer.

To set up the driver computer

1.  Configure IP address and set the computer name to “Client01”.

2.  Install the required and optional software described earlier.

3.  Open Windows PowerShell as Administrator privilege, and then type following command:

    *Set-ExecutionPolicy Unrestricted*

    Execute the same command on Windows PowerShell (x86)

4.  Copy MSI package and install it on the driver computer.

5.  When options are presented, select the option, **Install Test Suite on Driver Computer**.

6.  Create a folder named **FileSharingCaptureFileDirectory** under **C:\\**

    It’s used to save the capture files automatically captured by Microsoft Message Analyzer.

<!-- -->

1.  ### <span id="_Setup_the_SUT" class="anchor"><span id="_Toc425336637" class="anchor"></span></span>Set up the SUT

2.  #### Common Setup

<!-- -->

1.  Configure IP addresses on the SUT node and set the computer name to **Node01**.

2.  Turn off firewall.

    For Windows platform, please refer to [7.3.13](#_Toc362725923)

3.  Create local user accounts:

    1.  Admin User

        For Windows platform, please refer to [7.3.14](#_Create_Admin_User)

    2.  Non-Admin User

        For Windows platform, please refer to [7.3.15](#_Create_a_Non-Admin_1)

4.  Enable Guest account

    For Windows platform, please refer to [7.3.16](#_Enable_the_Local)

5.  The password for all accounts is set to “**Password01!**”

    For Windows platform, please refer to [7.3.15](#_Reset_Password_to)

<!-- -->

1.  #### Setup to test SMB2

2.  ##### Create a share SMBBasic 

It’s basic file share to test SMB2 cases.

1.  Create a share **SMBBasic** on node01.

2.  Share with read/write access to Admin User.

For Windows platform, please refer to section [7.3.1](#_Create_SMBBasic_share).

##### Create a share with Encrypt data enabled

It’s used to test Encryption feature of SMB2 protocol.

1.  Create share **SMBEncrypted** on node01.

2.  Share with read/write access to Admin User.

3.  Enable share folder with **Encrypt data** which indicates that the server requires messages to be encrypted for accessing this share.

For Windows platform, please refer to section [7.3.6](#_Create_SMBEncrypted_share).

1.  ##### Other configuration

    1.  ###### Create a share with Oplock Force Level 2 enabled

It’s used in some cases when testing **Oplock** feature of SMB2 protocol.

1.  Create a share **ShareForceLevel2** on node01.

2.  Share with read/write access to Admin User.

3.  Enable **FORCE\_LEVELII\_OPLOCK** on this share.

For Windows platform, please refer to section [7.3.7](#_Enable_FORCE_LEVELII_OPLOCK_on_1).

###### Create a share SameWithSMBBasic

It’s used for testing some negative cases of **AppInstanceId** feature of SMB2. If **AppInstanceId** is not supported, this section can be ignored.

Add a share named **SameWithSMBBasic**, this share should have the same local path as **SMBBasic** share.

Summary of the negative cases:

1.  The case creates an open under the share **SMBBasic**. There’s an **AppInstanceId** in the Create Request.

2.  The case connects the share **SameWithSMBBasic** and sends a second Create Request to SUT with the same **AppInstanceId**.

3.  Verify the first open is not force to close by the second create request because the second create request connects to a different share.

For Windows platform, please refer to section [7.3.4](#_Create_share_SameWithSMBBasic_1).

###### Create a share different from SMBBasic

It’s used for some negative cases (similar to section [7.1.2.2.3.2](#create-a-share-samewithsmbbasic)) when testing **AppInstanceId** feature of SMB2. If **AppInstanceId** is not supported, this section can be ignored.

1.  Create a share named **DifferentFromSMBBasic** on node01. The local path of this share should be different from **SMBBasic** share.

2.  Share with read/write access to Admin User.

    1.  ###### Create Symbolic Links

It’s used for some negative cases when testing **Create/Close** feature of SMB2. For details, please see section 3.3.5.9 in \[MS-SMB2\].

1.  Add a **Symbolic Link** to **SMBBasic**

In share **SMBBasic**, add a symbolic link named **Symboliclink**, which links to **SMBBasic**.

1.  Add a new folder **Sub** under **SMBBasic**, add another symbolic link named **Symboliclink2** under Sub folder, and link it to **SMBBasic**.

For Windows platform, please refer to section [7.3.5](#_Add_symbolic_link).

###### Create a share on volume supporting integrity

If integrity is not supported by file system, this section can be ignored.

The share is used to test IoCtl codes **FSCTL\_GET\_INTEGRITY\_INFORMATION** and **FSCTL\_SET\_INTEGRITY\_INFORMATION**.

1.  Create a share named **SMBReFSShare** on node01.

The file system of the volume containing the specified share should support the use of **integrity**.

1.  Share with read/write access to Admin User.

For Windows platform, please refer to section [7.3.8](#_Create_SMBReFSShare_for).

![](./media/image3.png)Note

Details about the two IoCtl codes can be found in \[MS-FSCC\].

###### Modify the Signing configurations

If Signing is not supported by file system, this section can be ignored.

The configuration is used to test the Signing function.

In order to run Signing cases, you need to do:

1.  Open the ptfconfig file whose path is as follow:

C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*Source\\Server\\TestCode\\Common\\TestSuite\\CommonTestSuite.deployment.ptfconfig

1.  If SigningRequired is enabled on SUT, set the value of property “IsRequireMessageSigning” to true in the ptfconfig file mentioned above.

2.  If SigningRequired is disabled on SUT, set the value of property “IsRequireMessageSigning” to false in the ptfconfig file mentioned above.

For Windows platform on how to enable or disable SigningRequired, please refer to section [7.3.21](#_Modify_the_Signing).<span id="_Setup_to_test_1" class="anchor"></span>

#### Setup to test DFSC

If DFS is not supported, this section can be ignored.

1.  Create DFS namespaces.

    Two Stand-alone namespaces: **SMBDfs** and **Standalone**

2.  Create a share **FileShare** on SUT.

    Share with read/write access to Admin User.

3.  Add one folder to namespace **SMBDfs.**

    Folder name is **SMBDfsLink**, link target is **\\\\node01\\FileShare**

4.  Add two folders to namespace **Standalone.**

    One is **DFSLink**, link target is **\\\\node01\\FileShare**

    The other is **Interlink**, link target is **\\\\node01\\SMBDfs\\SMBDfsLink**

For Windows platform, please refer to section [7.10](#_Setup_Distributed_File).

#### Setup to test RSVD

If \[MS-RSVD\] is not implemented, this section can be ignored.

Create two virtual hard disk files named **rsvd.vhdx** and **convert.vhdx**, as well as a virtual hard disk set file named **rsvd.vhds,** and put them in a share of SUT.

Testing RSVD is not applicable for workgroup environment of Windows platform.

#### Setup to test SQOS

If \[MS-SQOS\] is not implemented, this section can be ignored.

1.  Create a virtual hard disk file named **sqos.vhdx** and put it in a share of SUT.

2.  Create a new policy (with MinimumIoRate: 100 and MaximumIoRate: 200).

Save the policy id to the ptfconfig item: SqosPolicyId

Testing SQOS is not applicable for workgroup environment of Windows platform.

1.  <span id="_Toc362208436" class="anchor"><span id="_Toc362208752" class="anchor"><span id="_Toc362260271" class="anchor"><span id="_Toc362260586" class="anchor"><span id="_Toc362725884" class="anchor"><span id="_Toc362726274" class="anchor"><span id="_Toc425336638" class="anchor"></span></span></span></span></span></span></span>Domain Environment
    -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

2.  ### <span id="_Setup_the_Domain_1" class="anchor"><span id="_Toc425336639" class="anchor"></span></span>Set up the Domain Controller (DC)

3.  #### Configure IP addresses and set the computer name to “DC01”

4.  #### Promote DC

Promote the computer to be a Domain Controller.

For Windows platform, please refer to section [7.3.9.1](#install-dns-service-and-active-directory-domain-services-with-a-specific-domain).

#### Create Domain User accounts 

1.  Create a domain admin user account in Active Directory

2.  Create a domain user account named as **nonadmin** in Active Directory

    Make sure this account is not the admin account of SUT.

3.  Enable Guest account

4.  Create a domain group AzGroup01 in Active Directory

5.  Create a domain user AzUser01 within AzGroup01 in Active Directory

6.  Password of all the accounts is set to “**Password01!**”

For Windows platform, please refer to section [7.3.9](#_Set_up_a).

#### Setup to test Claimed Based Access Control (CBAC)

For Windows platform, please refer to section [7.3.9.9](#configure-claimed-based-access-control-cbac).

### <span id="_Setup_the_Driver_2" class="anchor"><span id="_Toc425336640" class="anchor"></span></span>Set up the Driver Computer

1.  Complete steps specified in section [7.1.1](#_Setup_the_Driver_1).

2.  Join the Driver Computer to the domain provided by the DC.<span id="_Toc362208439" class="anchor"><span id="_Toc362208755" class="anchor"><span id="_Toc362260274" class="anchor"><span id="_Toc362260589" class="anchor"><span id="_Setup_the_Domain" class="anchor"><span id="_Set_Up_the" class="anchor"><span id="_Toc362208441" class="anchor"><span id="_Toc362208757" class="anchor"><span id="_Toc362260276" class="anchor"><span id="_Toc362260591" class="anchor"><span id="_Toc362208442" class="anchor"><span id="_Toc362208758" class="anchor"><span id="_Toc362260277" class="anchor"><span id="_Toc362260592" class="anchor"><span id="_Toc362208443" class="anchor"><span id="_Toc362208759" class="anchor"><span id="_Toc362260278" class="anchor"><span id="_Toc362260593" class="anchor"><span id="_Toc362208445" class="anchor"><span id="_Toc362208761" class="anchor"><span id="_Toc362260280" class="anchor"><span id="_Toc362260595" class="anchor"><span id="_Toc362208446" class="anchor"><span id="_Toc362208762" class="anchor"><span id="_Toc362260281" class="anchor"><span id="_Toc362260596" class="anchor"><span id="_Toc362208447" class="anchor"><span id="_Toc362208763" class="anchor"><span id="_Toc362260282" class="anchor"><span id="_Toc362260597" class="anchor"><span id="_Toc362208449" class="anchor"><span id="_Toc362208765" class="anchor"><span id="_Toc362260284" class="anchor"><span id="_Toc362260599" class="anchor"><span id="_Toc362208450" class="anchor"><span id="_Toc362208766" class="anchor"><span id="_Toc362260285" class="anchor"><span id="_Toc362260600" class="anchor"><span id="_Toc362208451" class="anchor"><span id="_Toc362208767" class="anchor"><span id="_Toc362260286" class="anchor"><span id="_Toc362260601" class="anchor"><span id="_Toc362208453" class="anchor"><span id="_Toc362208769" class="anchor"><span id="_Toc362260288" class="anchor"><span id="_Toc362260603" class="anchor"><span id="_Toc362208454" class="anchor"><span id="_Toc362208770" class="anchor"><span id="_Toc362260289" class="anchor"><span id="_Toc362260604" class="anchor"><span id="_Toc362208455" class="anchor"><span id="_Toc362208771" class="anchor"><span id="_Toc362260290" class="anchor"><span id="_Toc362260605" class="anchor"></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span>

<!-- -->

1.  ### <span id="_Toc362208457" class="anchor"><span id="_Toc362208773" class="anchor"><span id="_Toc362260292" class="anchor"><span id="_Toc362260607" class="anchor"><span id="_Toc362725888" class="anchor"><span id="_Toc362726278" class="anchor"><span id="_Toc362208458" class="anchor"><span id="_Toc362208774" class="anchor"><span id="_Toc362260293" class="anchor"><span id="_Toc362260608" class="anchor"><span id="_Toc362725889" class="anchor"><span id="_Toc362726279" class="anchor"><span id="_Toc362208459" class="anchor"><span id="_Toc362208775" class="anchor"><span id="_Toc362260294" class="anchor"><span id="_Toc362260609" class="anchor"><span id="_Toc362725890" class="anchor"><span id="_Toc362726280" class="anchor"><span id="_Toc362208460" class="anchor"><span id="_Toc362208776" class="anchor"><span id="_Toc362260295" class="anchor"><span id="_Toc362260610" class="anchor"><span id="_Toc362725891" class="anchor"><span id="_Toc362726281" class="anchor"><span id="_Toc362208461" class="anchor"><span id="_Toc362208777" class="anchor"><span id="_Toc362260296" class="anchor"><span id="_Toc362260611" class="anchor"><span id="_Toc362725892" class="anchor"><span id="_Toc362726282" class="anchor"><span id="_Toc362208462" class="anchor"><span id="_Toc362208778" class="anchor"><span id="_Toc362260297" class="anchor"><span id="_Toc362260612" class="anchor"><span id="_Toc362725893" class="anchor"><span id="_Toc362726283" class="anchor"><span id="_Toc362208463" class="anchor"><span id="_Toc362208779" class="anchor"><span id="_Toc362260298" class="anchor"><span id="_Toc362260613" class="anchor"><span id="_Toc362725894" class="anchor"><span id="_Toc362726284" class="anchor"><span id="_Toc362208464" class="anchor"><span id="_Toc362208780" class="anchor"><span id="_Toc362260299" class="anchor"><span id="_Toc362260614" class="anchor"><span id="_Toc362725895" class="anchor"><span id="_Toc362726285" class="anchor"><span id="_Toc362208465" class="anchor"><span id="_Toc362208781" class="anchor"><span id="_Toc362260300" class="anchor"><span id="_Toc362260615" class="anchor"><span id="_Toc362725896" class="anchor"><span id="_Toc362726286" class="anchor"><span id="_Toc362208466" class="anchor"><span id="_Toc362208782" class="anchor"><span id="_Toc362260301" class="anchor"><span id="_Toc362260616" class="anchor"><span id="_Toc362725897" class="anchor"><span id="_Toc362726287" class="anchor"><span id="_Toc362208467" class="anchor"><span id="_Toc362208783" class="anchor"><span id="_Toc362260302" class="anchor"><span id="_Toc362260617" class="anchor"><span id="_Toc362725898" class="anchor"><span id="_Toc362726288" class="anchor"><span id="_Toc362208468" class="anchor"><span id="_Toc362208784" class="anchor"><span id="_Toc362260303" class="anchor"><span id="_Toc362260618" class="anchor"><span id="_Toc362725899" class="anchor"><span id="_Toc362726289" class="anchor"><span id="_Toc362208469" class="anchor"><span id="_Toc362208785" class="anchor"><span id="_Toc362260304" class="anchor"><span id="_Toc362260619" class="anchor"><span id="_Toc362725900" class="anchor"><span id="_Toc362726290" class="anchor"><span id="_Toc362208470" class="anchor"><span id="_Toc362208786" class="anchor"><span id="_Toc362260305" class="anchor"><span id="_Toc362260620" class="anchor"><span id="_Toc362725901" class="anchor"><span id="_Toc362726291" class="anchor"><span id="_Toc362208471" class="anchor"><span id="_Toc362208787" class="anchor"><span id="_Toc362260306" class="anchor"><span id="_Toc362260621" class="anchor"><span id="_Toc362725902" class="anchor"><span id="_Toc362726292" class="anchor"><span id="_Toc362208472" class="anchor"><span id="_Toc362208788" class="anchor"><span id="_Toc362260307" class="anchor"><span id="_Toc362260622" class="anchor"><span id="_Toc362725903" class="anchor"><span id="_Toc362726293" class="anchor"><span id="_Toc362208473" class="anchor"><span id="_Toc362208789" class="anchor"><span id="_Toc362260308" class="anchor"><span id="_Toc362260623" class="anchor"><span id="_Toc362725904" class="anchor"><span id="_Toc362726294" class="anchor"><span id="_Toc362208474" class="anchor"><span id="_Toc362208790" class="anchor"><span id="_Toc362260309" class="anchor"><span id="_Toc362260624" class="anchor"><span id="_Toc362725905" class="anchor"><span id="_Toc362726295" class="anchor"><span id="_Toc362208475" class="anchor"><span id="_Toc362208791" class="anchor"><span id="_Toc362260310" class="anchor"><span id="_Toc362260625" class="anchor"><span id="_Toc362725906" class="anchor"><span id="_Toc362726296" class="anchor"><span id="_Toc362208476" class="anchor"><span id="_Toc362208792" class="anchor"><span id="_Toc362260311" class="anchor"><span id="_Toc362260626" class="anchor"><span id="_Toc362725907" class="anchor"><span id="_Toc362726297" class="anchor"><span id="_Toc362208477" class="anchor"><span id="_Toc362208793" class="anchor"><span id="_Toc362260312" class="anchor"><span id="_Toc362260627" class="anchor"><span id="_Toc362725908" class="anchor"><span id="_Toc362726298" class="anchor"><span id="_Toc362208478" class="anchor"><span id="_Toc362208794" class="anchor"><span id="_Toc362260313" class="anchor"><span id="_Toc362260628" class="anchor"><span id="_Toc362725909" class="anchor"><span id="_Toc362726299" class="anchor"><span id="_Setup_the_SUT_1" class="anchor"><span id="_Toc361668092" class="anchor"><span id="_Toc425336641" class="anchor"></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span>Set up the SUT

2.  #### Common Set up 

<!-- -->

1.  Complete steps specified in section [7.1.2.1](#common-setup)

2.  Join the SUT to the domain provided by the DC.

3.  Setup the computer password for SUT.

    For Windows platform, please refer to section [7.3.22](#_Manually_Setup_Computer).

#### Setup to test SMB2

1.  Complete steps specified in section [7.1.2.2](#setup-to-test-smb2)

2.  Share all the shares with Domain Admin User.<span id="_Toc361668093" class="anchor"></span>

#### Setup to test DFSC

If DFS is not supported, this section can be ignored.

1.  Complete steps specified in section [7.1.2.3](#_Setup_to_test_1)

2.  Create one Domain-based namespace: **DomainBased**

3.  Add two folders to namespace **DomainBased**

    One is **DFSLink.** If there is node02, link target is **\\\\node02\\FileShare**, and otherwise link target is **\\\\node01\\FileShare**

    The other is **Interlink**, link target is **\\\\node01\\SMBDfs\\SMBDfsLink**

For Windows platform, please refer to section [7.3.10](#_Setup_Distributed_File).

#### Setup to test Server Failover/FSRVP/SWN

1.  Create a share named **SMBClustered**, set the property **Share.IsCA** to TRUE, set **Share.Type** to include **STYPE\_CLUSTER\_SOFS**, and make the share asymmetric.

    1.  **Share.IsCA** is set to TRUE when **SHI1005\_FLAGS\_ENABLE\_CA** bit in **shi1005\_flags** is set as documented in \[MS-SRVS\] section 2.2.4.29.

    2.  **STYPE\_CLUSTER\_SOFS** is one of “**Share Types**” documented in \[MS-SRVS\] section 2.2.2.4.

2.  Create a share named **SMBClusteredEncrypted** with the properties **Share.IsCA** and **Share.EncryptData** set to TRUE.

3.  Create a share named **SMBClusteredForceLevel2**, set the property **Share.ForceLevel2Oplock** to TRUE and set **Share.Type** to include **STYPE\_CLUSTER\_SOFS**.

<!-- -->

1.  **Share.ForceLevel2Oplock** is set to TRUE when **SHI1005\_FLAGS\_FORCE\_LEVELII\_OPLOCK** bit in **shi1005\_flags** is set as specified in \[MS-SRVS\] section 2.2.4.29.

2.  **STYPE\_CLUSTER\_SOFS** is one of “**Share Types**” documented in \[MS-SRVS\] section 2.2.2.4.

For Windows platform, please refer to section [7.3.11](#_Setup_SAN_Storage_1) and section [7.3.12](#_Setup_cluster_nodes).

#### Setup to test RSVD

If \[MS-RSVD\] is not implemented, this section can be ignored.

Create two virtual hard disk files named **rsvd.vhdx** and **convert.vhdx**, as well as a virtual hard disk set file named **rsvd.vhds**, put them in a share of SUT.

For Windows platform,

1.  About how to create a virtual hard disk file, please refer to section [7.3.19](#_Create_the_virtual).

2.  About how to create a virtual hard disk set file, please refer to section [7.3.20](#_Create_Asymmetric_share).

3.  Put **rsvd.vhdx**, **convert.vhdx** and **rsvd.vhds** in the share **\\\\scaleoutfs\\SMBClustered**

#### Setup to test SQOS

If \[MS-SQOS\] is not implemented, this section can be ignored.

1.  Create a virtual hard disk file named **sqos.vhdx** and put it in a share of SUT.

2.  Create a new policy (with MinimumIoRate: 100 and MaximumIoRate: 200)

Save the policy id to the ptfconfig item: SqosPolicyId

For Windows platform,

1.  About how to create a virtual hard disk file, please refer to section [7.3.19](#_Create_the_virtual).

2.  Put **sqos.vhdx** in the share [**\\\\scaleoutfs\\SMBClustered**](file:///\\scaleoutfs\SMBClustered)

3.  About how to create the new policy, please refer to section [7.3.23](#_Create_an_SQOS)

#### Setup to test Authorization

1.  Create a share named **AzShare**.

| NTFS Permission  | Allow Everyone      |
|------------------|---------------------|
| Share Permission | Allow Domain Admins |

1.  Create a share named **AzFolder**.

| NTFS Permission  | Allow Domain Admins |
|------------------|---------------------|
| Share Permission | Allow Everyone      |

1.  Create a share named **AzFile**.

| NTFS Permission  | Allow Domain Admins |
|------------------|---------------------|
| Share Permission | Allow Everyone      |

1.  Create a share named **AzCBAC**.

| NTFS Permission  | Allow Everyone |
|------------------|----------------|
| Share Permission | Allow Everyone |

If **SRVS** (Server Service Remote Protocol) is not supported, please create 6 shares: **AzShare01**... **AzShare06.** Share permissions should be set according to the following table:

| AzShare01 | O:SYG:SYD:(A;;0x1fffff;;;\[SID of AzUser01\])(A;;FA;;;BA)   |
|-----------|-------------------------------------------------------------|
| AzShare02 | O:SYG:SYD:(A;;0x1fffff;;; \[SID of AzGroup01\])(A;;FA;;;BA) |
| AzShare03 | O:SYG:SYD:(D;;0x1fffff;;; \[SID of AzUser01\])(A;;FA;;;BA)  |
| AzShare04 | O:SYG:SYD:(D;;0x1fffff;;; \[SID of AzGroup01\])(A;;FA;;;BA) |
| AzShare05 | O:SYG:SYD:(A;;FA;;;BA)                                      |
| AzShare06 | O:SYG:SYD:(D;;;;; \[SID of AzUser01\])(A;;FA;;;BA)          |

Steps to create the above shares are similar to creating share **SMBBasic**.

For Windows platform on how to create **SMBBasic** and configure permissions, please refer to section [7.3.1](#_Create_SMBBasic_share), [7.3.2](#_Set_NTFS_Permissions) and [7.3.3](#_Set_Share_Permissions).

1.  <span id="_Toc420507634" class="anchor"><span id="_Toc420573791" class="anchor"></span></span> Computer Setup for Windows platform
    ----------------------------------------------------------------------------------------------------------------------------------

2.  ### <span id="_Create_SMBBasic_share" class="anchor"><span id="_Toc425336643" class="anchor"></span></span>Create share SMBBasic

Create a folder **C:\\SMBBasic**.

Share it with name **SMBBasic** and read/write access to **node01\\administrator** (and **contoso\\administrator** if Domain Environment).

![](./media/image7.png)

1.  ### <span id="_Toc395706985" class="anchor"><span id="_Add_a_share" class="anchor"><span id="_Create_share_SameWithSMBBasic" class="anchor"><span id="_Set_NTFS_Permissions" class="anchor"><span id="_Toc425336644" class="anchor"></span></span></span></span></span>![](./media/image8.png)Set NTFS Permissions

2.  ### <span id="_Set_Share_Permissions" class="anchor"><span id="_Toc425336645" class="anchor"></span></span>Set Share Permissions

![](./media/image9.png)

### <span id="_Create_share_SameWithSMBBasic_1" class="anchor"><span id="_Toc425336646" class="anchor"></span></span>Create share SameWithSMBBasic 

Add another share name **SameWithSMBBasic** to **C:\\SMBBasic** folder

![](./media/image10.png)

### <span id="_Add_symbolic_link" class="anchor"><span id="_Toc425336647" class="anchor"></span></span>Create symbolic link

Start console window as administrator privilege:

Click **Start**-&gt;Type **cmd**-&gt;Right click **Command Prompt**-&gt;select **Run as Administrator**

![](./media/image11.png)

Run the following commands in console window:

1.  cd c:\\SMBBasic

2.  mklink /D Symboliclink c:\\

3.  mkdir Sub

4.  cd Sub

5.  mklink /D Symboliclink2 c:\\SMBBasic

### <span id="_Create_SMBEncrypted_share" class="anchor"><span id="_Create_share_with" class="anchor"><span id="_Toc425336648" class="anchor"></span></span></span>Create share with Encrypt data enabled

1.  Create a folder **C:\\SMBEncrypted** on node01.

2.  Share with read/write access to **node01\\administrator** (and **contoso\\administrator** if Domain Environment).

3.  Enable share folder with **Encrypt data**.

    Navigate to **Server Manager** &gt; **File and Storage Services** &gt; **Share**&gt;

    **SMBEncrypted** &gt; **Properties** &gt; **Settings** &gt; **Encrypt data**

    ![](./media/image12.png)

### <span id="_Enable_FORCE_LEVELII_OPLOCK_on_1" class="anchor"><span id="_Toc425336649" class="anchor"></span></span>Enable FORCE\_LEVELII\_OPLOCK on share

Please modify and run below two commands from console window:

> Cd C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\Bin\\
>
> ShareUtil.exe &lt;computer name of the share&gt; &lt;share name&gt; &lt;flag &gt; true

Examples:

1.  To set SHI1005\_FLAGS\_FORCE\_LEVELII\_OPLOCK flag on **\\\\node01\\ShareForceLevel2**, run the following command:

    *ShareUtil.exe node01 ShareForceLevel2 SHI1005\_FLAGS\_FORCE\_LEVELII\_OPLOCK true*

2.  To set SHI1005\_FLAGS\_FORCE\_LEVELII\_OPLOCK flag on **\\\\ScaleOutFS\\ SMBClusteredForceLevel2**, run the following command:

    *ShareUtil.exe ScaleOutFS SMBClusteredForceLevel2 SHI1005\_FLAGS\_FORCE\_LEVELII\_OPLOCK true*

### <span id="_Create_share_on" class="anchor"><span id="_Toc425336650" class="anchor"></span></span>Create share on volume supporting integrity

<span id="_Create_SMBReFSShare_for" class="anchor"></span>Execute “diskmgmt.msc” from console window, create **ReFS** format volume.

![](./media/image13.png)Right-click unallocated disk to create new simple volume. Or right-click on **Disk 0** and select “**Shrink volume**” to split a free space (5G) at first.

![](./media/image14.png)Format new created volume as ReFS fie system.

Create a share in the new volume with share name: **SMBReFSShare** and share it with read/write access with **node01\\administrator** and **contoso\\administrator** (If Domain Environment).

![](./media/image15.png)

1.  ### <span id="_Set_up_a" class="anchor"><span id="_Toc425336651" class="anchor"></span></span>Set up a Windows-based DC

2.  #### Install DNS Service and Active Directory Domain Services with a specific domain

    Install Active Directory domain services, and create domain (like contoso.com), set domain Administrator’s password to “**Password01!**”

3.  #### Turn off windows firewall from Control Panel

Please refer to [7.3.11](#_Connect_to_the)

#### Install Remote Access role and configure Routing and Remote Access

From Server Manager click Add Roles and Features and add Remote Access to Server Roles.

-   Note

This step is for routing of two different networks which are used in multiple channel cases

![](./media/image16.png)

Select **Routing** from **Role Services**

![](./media/image17.png)

Submit Confirmation

![](./media/image18.png)

1.  #### Configure and start Routing service

    From **Server Manager** click **Tools,** select **Routing and Remote Access**.

    ![](./media/image19.png)

    Right-click DC01, select **Configure and Enable Routing and Remote Access**.

    ![](./media/image20.png)

    ![](./media/image21.png)

    ![](./media/image22.png)

    And then start the service.

2.  #### Create Non-Admin user account 

Run the following commands in console window as administrator privilege:

dsadd user "CN=nonadmin,CN=Users,DC=contoso,DC=com"  -pwd Password01! -desc contoso -disabled no -mustchpwd no -pwdneverexpires yes

#### Enable Guest user account 

> Launch **Active Directory Users and Computers** form **Server Manager**.

Right Click **Guest**, Click **Enable Account** in the popup menu.

![](./media/image23.png)

Right Click **Guest**, Click Reset **Password…** and set the password to “**Password01!**”

![](./media/image24.png)

#### Create a Domain group

Run the following commands in console window as administrator:

dsadd group "CN=AzGroup01,CN=Users,DC=contoso,DC=com" 

#### Create a Domain account that belongs to a Domain group

Run the following commands in console window as administrator:

dsadd user "CN=AzUser01,CN=Users,DC=contoso,DC=com"  -pwd Password01! -desc contoso -memberof "CN=AzGroup01,CN=Users,DC=contoso,DC=com" -disabled no -mustchpwd no -pwdneverexpires yes

1.  #### Configure Claimed Based Access Control (CBAC)

2.  ##### Create User Claim

Navigate to **Server Manager** &gt; **Tools** &gt; **Active Directory Administrative Center**

![](./media/image25.png)

Then **Dynamic Access Control** &gt; **Claim Types**

![](./media/image26.png)

In the right panel, select **New** &gt; **Claim Type**

![](./media/image27.png)

Input **department** in the filter, and then input **Department** as display name, and then add value **IT** and **Payroll** as suggested values.

![](./media/image28.png)

Similarly, add another claim type: **CountryCode**, “Suggested Values” set to **156, 840** and **392**.

![](./media/image29.png)

##### Create Central Access Rules

Navigate to **Server Manager** &gt; **Tools** &gt;**Active Directory Administrative Center** &gt; **Dynamic Access Control** &gt; **Central Access Rules**.

In the right panel, select **New** &gt; **Central Access Rule**

![](./media/image30.png)

Enter name: **CountryCodeEquals156Rule**

![](./media/image31.png)

In the **Permission** tab, click **Edit**

![](./media/image32.png)

Click **Add**

![](./media/image33.png)

Click **Select a principal**

![](./media/image34.png)

Input **Authenticated Users**, then click **Check Names** and **OK**

![](./media/image35.png)

Check **Full Control** for **Basic permissions**.

![](./media/image36.png)Click **Add a condition**, and select **CountryCode** **Equals** **156** and click **OK**

Back to **Create Central Access Rule** page, and click **OK.**

![](./media/image37.png)

Repeat above procedure to create the rest 9 rules

![](./media/image38.png)

Conditions of the 9 rules:

**CountryCodeAnyOf156Or840Rule**: User.CountryCode Any of {156,840}

**CountryCodeEquals156AndITDepartmentRule**: (User.CountryCode Equals 156) And (User.Department Equals “IT”)

**CountryCodeEquals156OrITDepartmentRule**: (User.CountryCode Equals 156) Or (User.Department Equals “IT”)

**CountryCodeGreaterThan392Rule**: User.CountryCode Greater than 392

**CountryCodeGreaterThanOrEquals392Rule**: User.CountryCode Greater than or equal to 392

**CountryCodeLessThan392Rule**: User.CountryCode Less than 392

**CountryCodeLessThanOrEquals392Rule**: User.CountryCode Less than or equal to 392

**CountryCodeNotAnyOf156Or840Rule**: User.CountryCode Not any of {156, 840}

**CountryCodeNotEquals156Rule**: User.CountryCode Not equals 156

##### Create Central Access Policies

Navigate to **Server Manager** &gt; **Tools** &gt; **Active Directory Administrative Center** &gt; **Dynamic Access Control** &gt; **Create Central Access Policies**

In the right panel, select **New** &gt; **Central Access Policy**

![](./media/image39.png)

In the **Create Central Access Policy** page, input **CountryCodeEquals156Policy** in Name, and click **Add** to select rule **CountryCodeEquals156Rule**, then click **OK**.

![](./media/image40.png)

![](./media/image41.png)

Repeat the above procedure and add the rest 9 policies:

![](./media/image42.png)

##### Create Users

Navigate to **Server Manager** &gt; **Tools** &gt; **ADSI Edit**

![](./media/image43.png)

In the right panel, right click **ADSI Edit**, and select **Connect to**

![](./media/image44.png)

Click **OK**

![](./media/image45.png)

Expand **Default naming context** &gt; **DC=contoso,DC=com**, right click **CN=Users**, select **New** &gt; **Object**

![](./media/image46.png)

Select **user** and click **Next** ![](./media/image47.png)

Input **ITadmin01** and then **Next**

![](./media/image48.png)

Input **ITadmin01** and then **Next**

![](./media/image49.png)

Press **Finish**

![](./media/image50.png)

Right click the created user CN=ITadmin01, and select Properties

![](./media/image51.png)

Select **CountryCode**, and input **156** to it, then click **OK**

![](./media/image52.png)

Select **department** and enter **IT**

![](./media/image53.png)

Repeat the above procedure to add the 4 users as follows:

**ITmember01,** department: IT, countrycode: 392

**Payrollmember01,** department: Payroll, countrycode: 156

**Payrollmember02,** department: Payroll, countrycode: 840

**Payrollmember03,** department: Payroll, countrycode: 392

##### Apply Central Access Policies

Open **Run** &gt; **gpme.msc**, select **Default Domain Policy** and click **OK**

![](./media/image54.png)

In the right panel, navigate to **Computer Configuration &gt; Policies &gt; Windows Settings &gt; Security Settings &gt; File System &gt; Central Access Policy**

![](./media/image55.png)

Right click **Central Access Policy** and select **Manage Central Access Policies…**

![](./media/image56.png)

Select all the 10 rules added in section 7.3.7.9.3, click **Add**, and then **OK**

![](./media/image57.png)

First, update group policy in DC01, then in Node01 by executing the following command with administrator privilege in PowerShell or CMD:

gpupdate /force

1.  ### <span id="_Setup_Distributed_File" class="anchor"><span id="_Toc425336652" class="anchor"></span></span>Set up Distributed File System (DFS)

2.  #### Install Roles and Features on node01

In **Server Roles** &gt; **File And Storage Services**, select **DFS Namespaces.**

![](./media/image58.png)

Click **Add Features** when prompted to install **DFS Management** **Tools**.

![](./media/image59.png)

Click **Install** in **Confirmation** page to complete the wizard.

![](./media/image60.png)

#### Create share FileShare 

If there is node02 in the environment, create a folder **C:\\FileShare** on node02 and share it with read/write access with **node02\\administrator** and **contoso\\administrator** (If Domain Environment).

Otherwise, create a folder **C:\\FileShare** on node01 and share it with read/write access with **node01\\administrator** and **contoso\\administrator** (If Domain Environment).

1.  #### Create DFS name spaces

2.  ##### Create Stand-alone namespace: SMBDfs

<!-- -->

1.  Launch **DFS Management** from **Server Manager** on node01

    ![](./media/image61.png)

2.  Right-click **Namespace** and select **New Namespace…**.

    ![](./media/image62.png)

3.  Specify the server that will host the namespace as **node01**.

    ![](./media/image63.png)

4.  Specify the name of namespace **SMBDfs** and click **Edit Settings…** to set access permission to “**Administrators have full access; other users have read-only permissions**”.

    ![](./media/image64.png)

    ![](./media/image65.png)

5.  Select Stand-alone namespace.

    ![](./media/image66.png)

    Then click **Create** to complete the wizard.

    ![](./media/image67.png)

##### Create Stand-alone namespace: Standalone

Repeat steps 1 to 5 in section [7.3.10.3.1](#create-stand-alone-namespace-smbdfs), and create another Stand-alone namespace: **Standalone**.

##### Create Domain-based namespace: **DomainBased**

Repeat steps 1 to 4 in section [7.3.10.3.1](#create-stand-alone-namespace-smbdfs), and then choose to create Domain-based namespace: **DomainBased**

![](./media/image68.png)

Then click **Create** to complete the wizard.

![](./media/image69.png)

1.  #### Create DFS link for the namespaces

2.  ##### Add folder SMBDfsLink to namespace SMBDfs

Right-click the newly created namespace **\\\\NODE01\\SMBDfs** and click **New Folder…**

![](./media/image70.png)

Specify the folder name **SMBDfsLink** and click **Add…** to add target folder **\\\\NODE01\\FileShare**.

![](./media/image71.png)

![](./media/image72.png)

![](./media/image73.png)

##### Add two folders to namespace Standalone

Right-click the newly created namespace **\\\\NODE01\\Standalone** and click **New Folder**

Add two folders, one is **DFSLink**, target is **\\\\node02\\FileShare**.

If there is no node02 in the environment, set the target to **\\\\node01\\FileShare**.

![](./media/image74.png)

![](./media/image75.png)

The other is **Interlink**, target is **\\\\node01\\SMBDfs\\SMBDfsLink**

![](./media/image76.png)

![](./media/image77.png)

Two folders under **Standalone** namespace in total.

![](./media/image78.png)

##### Add two folders to DomainBased namespace

Right-click the namespace **\\\\contoso.com\\DomainBased** and click **New Folder.**

Add two folders.

One folder is **DFSLink**, target is **\\\\node02\\FileShare**.

If no node02 in environment, set the target to **\\\\node01\\FileShare**.

![](./media/image79.png)

![](./media/image80.png)

Press OK and add another folder **Interlink**, target is **\\\\node01\\SMBDfs\\SMBDfsLink**

![](./media/image81.png)

![](./media/image82.png)

Two folders under **DomainBased** namespace in total.

![](./media/image83.png)

### <span id="_Setup_SAN_Storage_1" class="anchor"><span id="_Toc425336653" class="anchor"></span></span>Set up SAN Storage Server

This section provides information about how to setup a SAN storage server for use with this test suite.

![](./media/image3.png)Note

This server may not join the Domain provided by the DC.

1.  #### Configure IP addresses and set the computer name to “Storage01”.

2.  #### Turn off windows firewall from Control Panel.

3.  #### Install iSCSI Target.

    From **Server Manager** click **Add Roles and Features** and add **File Services** from **Server Roles**.

    ![](./media/image84.png)

    Click **Next** with default settings until **Server Roles** page. Expand tree **File And Storage Services** and select **File Server** and **iSCSI Target Server** to install.

    ![](./media/image85.png)

    Confirm to add required features for iSCSI Target Server.

    ![](./media/image86.png)

    Click **Next** with default settings until last **Confirmation** page and click **Install**.

    ![](./media/image87.png)

4.  #### Create virtual disks for failover cluster nodes.

After installation of the above step, navigate to **Server Manager** &gt; **File and Storage Services** &gt; **iSCSI Virtual Disks** to launch the New Virtual Disk wizard to create a virtual disk.

![](./media/image88.png)

Either select volume or type a custom path for the new virtual disk location.

![](./media/image89.png)

Specify the names and sizes of the virtual disks. The following table gives recommended virtual disk number and size used in this test suite.

| **Disk Name** | **Size** | **Purpose**                                                                    |
|---------------|----------|--------------------------------------------------------------------------------|
| Quorumdisk    | 1GB      | Quorum disk for a two-node cluster                                             |
| FSDisk01      | 10GB     | Storage used by file server for general use on failover cluster                |
| FSDisk02      | 10GB     | Storage used by file server for scale-out application data on failover cluster |

![](./media/image90.png)

![](./media/image91.png)

Create a new iSCSI target for failover cluster node access.

![](./media/image92.png)

Specify the name of the iSCSI target name.

![](./media/image93.png)

Specify the initiators of failover cluster nodes which need to access the virtual disk and target.

![](./media/image94.png)

When **Add…** is clicked there’re multiple ways for you to give the target access to a specific machine. You could select the IQNs of failover cluster nodes from the list if **Select from the initiator cache on the target server** option is available. Or add the values in specific type of the failover cluster nodes if the **Enter a value for the selected type** option is selected and a type is specified. Here, take this example with **IP Address** type.

![](./media/image95.png)

![](./media/image96.png)

Click **Next** with default settings and click **Create** in **Confirmation** page to complete the wizard.

![](./media/image97.png)

After the 1<sup>st</sup> virtual disk is created, right click from **iSCSI VIRTUAL DISKS** blank space to launch new “**New iSCSI Virtual Disk…**” wizard.

![](./media/image98.png)

Repeat the above steps to create more virtual disks as shared storages for the failover cluster use. Please note that you must select the existing iSCSI target which was just created in the above steps.

![](./media/image99.png)

After iSCSI target is created, you could find iSCSI target name as below when select any virtual disk.

![](./media/image100.png)

1.  ### <span id="_Setup_cluster_nodes" class="anchor"><span id="_Setup_Cluster" class="anchor"><span id="_Toc425336654" class="anchor"></span></span></span>Set up Cluster 

2.  #### Connect to the iSCSI disks provided by the SAN Storage Server from two nodes

From **Server Manager** of each failover cluster node, click **iSCSI Initiator** and select **Yes** to start **Microsoft iSCSI service** when prompted for its first use.

![](./media/image101.png)

![](./media/image102.png)

Add the IP address of the SAN storage server when discover target portal in **Discovery** tab.

![](./media/image103.png)

Navigate to **Targets** tab click **Refresh** and **Connect**

![](./media/image104.png)

On either of the failover cluster nodes, open **Run** &gt; **DISKMGMT.MSC** to launch disk manager, 3 disks can be found if you follow the recommended disk settings. Online and initiate them as GPT or MBR disk, and then create simple volumes with NTFS format on them. Do not forget to format these three disks.

![](./media/image105.png)

Right click **Disk 1**, the left panel, **Online** this disk and then choose **Initialize Disk**

![](./media/image106.png)

Select **MBR**, and click **OK**

![](./media/image107.png)

Then right click **Disk 1** again, the right panel

Choose **New Simple Volume**

![](./media/image108.png)

Press **Next &gt;**

![](./media/image109.png)

**Next &gt;**

![](./media/image110.png)

**Next &gt;**

![](./media/image111.png)

**Next &gt;**

![](./media/image112.png)

Press **Finish**.

![](./media/image113.png)

Repeat the same steps to format the other two disks.

#### Install Roles and Features on each failover cluster node

In **Server Roles** &gt; **File And Storage Services**, select **File Server**, **File Server Resource Manager,** **File Server** **VSS Agent Service. **

![](./media/image114.png)

**In Features,** select **Failover Clustering**.

![](./media/image115.png)

Click **Add Features** when prompt to install **Failover Clustering Tools**.

![](./media/image116.png)

Click **Install** in **Confirmation** page to complete the wizard.

1.  #### Create failover cluster in either node

    Logon on either of the failover cluster nodes using a domain account which has permission to create computer object to Active Directory.

    **Server Manager** &gt; **Tools** &gt; **Failover Cluster Manager** to open management console.

    Create cluster using “**Create Cluster…**” wizard.

![](./media/image3.png)Note

Prior to create the cluster, the shared storage needs to be created and added to the failover cluster nodes. For more information about this, please check section [7.3.11](#_Setup_SAN_Storage_1)

![](./media/image117.png)

Input the computer name of failover cluster nodes.

![](./media/image118.png)

Optionally select if you want do cluster configuration validation tests. For simplicity, we just select “No” to skip such validation tests here.

![](./media/image119.png)

Specify the cluster name **Cluster01**, and also the IP address of the cluster (e.g. **192.168.1.100**, and **192.168.2.100**) if the cluster nodes are using static IP addresses in a subnet. You could ignore specifying the IP address in the subnet which is used for cluster internal traffic.

![](./media/image120.png)

![](./media/image121.png)

![](./media/image122.png)

1.  #### Create file server on failover cluster

    In **Failover Cluster Manager** from either failover cluster node, right click on **Roles**, select **Configure Role…**

    ![](./media/image123.png)

    ![](./media/image124.png)

    Select either **File Server for general use** or **File Server for scale-out application data**. Here we select the first one.

    ![](./media/image125.png)

    Specify the access point name (such as “**GeneralFS**”) and IP address if the failover cluster nodes use static IP addresses in a subnet (such as “**192.168.1.200**”, “**192.168.2.200**”).

    ![](./media/image126.png)

    Select a disk for the file server consumption.

    ![](./media/image127.png)

    Click “**Next**” in following pages to complete the wizard.

    ![](./media/image128.png)

    Repeat above steps when select **File Server for scale-out application data** to create file server for scale-out application data except just specifying client access point name like **ScaleOutFS** as follows.

    ![](./media/image129.png)

2.  #### Create file share for cluster

    Select **Roles** from **Failover Cluster Manager**, to create a file share on file server for general use, right click one of the file server items with **Type** “**File Server**” (**GeneralFS**) and select **Add Shared Folder**

    ![](./media/image130.png)

    Select “**SMB Share – Quick**”

    ![](./media/image131.png)

    ![](./media/image132.png)

    Specify the share name, such as “**SMBClustered**”.

    ![](./media/image133.png)

    Check “**Enable continuous availability**” and “**Allow caching of share**” checkboxes.

    ![](./media/image134.png)

    Give **Full Control** to the account that will access the file share.

    ![](./media/image135.png)

    ![](./media/image136.png)

    ![](./media/image137.png)

    ![](./media/image138.png)

    ![](./media/image139.png)

    ![](./media/image140.png)

    To create a file share on the file server for scale-out application data, first add an existing free cluster disk (**Assigned To** is **Available Storage**) as **Cluster Shared Volumes** like below.

    ![](./media/image141.png)

3.  #### Create file share with Oplock Force Level 2 enabled for cluster 

Repeat the steps in section [7.3.12.5](#create-file-share-for-cluster) except for setting the share location on the scale-out file server role on **Select the Server and path for this share** page and setting the share name to **SMBClusteredForceLevel2** on **Specify share name** page.

Enable **FORCE\_LEVELII\_OPLOCK** on this share. Please refer to section [7.3.7](#_Enable_FORCE_LEVELII_OPLOCK_on_1).

#### Create file share with Encrypt data enabled for cluster

To create **SMBClusteredEncrypted** share on failover cluster (both **GeneralFS** and **ScaleOutFS**), repeat the steps in section [7.3.12.5](#create-file-share-for-cluster) except select **Encrypt data access** option in **Other Settings** page.

### <span id="_Toc362725923" class="anchor"><span id="_Toc362725924" class="anchor"><span id="_Connect_to_the" class="anchor"><span id="_Turn_off_Firewall" class="anchor"><span id="_Toc425336655" class="anchor"><span id="_Toc361668096" class="anchor"></span></span></span></span></span></span>Turn off Firewall

Run the following command in console window:

netsh advfirewall set allprofiles state off

### <span id="_Create_a_Non-Admin" class="anchor"><span id="_Create_Admin_User" class="anchor"><span id="_Toc425336656" class="anchor"></span></span></span>Create Local Admin User Account

Do not need to create a new one. Just use **Administrator**.

If the OS of SUT is Windows client SKU, you need to enable build-in administrator by running the following command:

net.exe user administrator /active:yes

### <span id="_Create_a_Non-Admin_1" class="anchor"><span id="_Toc425336657" class="anchor"></span></span>Create a Local Non-Admin User Account

Run the following commands in console window as administrator privilege:

net.exe user nonadmin Password01! /add

### <span id="_Enable_the_Local" class="anchor"><span id="_Toc425336658" class="anchor"></span></span>Enable the Local Guest Account

Run the following commands in an elevated console window (run as administrator):

net.exe user Guest /active:yes

If the OS of SUT is Windows client SKU, you need to remove Guest from the user right Deny log on as follows:

1.  Open the Group Policy Management Editor. To do so, click Start, then in the Start Search box, type gpedit.msc, and then press ENTER.

2.  If the User Account Control dialog box appears, confirm that the action it displays is what you want, and then click Continue.

3.  In the navigation pane, open Computer Configuration, Windows Settings, Security Settings, Local Policies, and User Rights Assignment.

4.  In the details pane, double-click "Deny log on locally" or "Deny access to this computer from the network".

5.  Select the user or group account you want to remove, and then click Remove.

6.  Click OK to save your changes to the GPO.

7.  Users that are currently logged on must log off and back on to be affected by the changed GPO setting.

### <span id="_Reset_Password_to" class="anchor"><span id="_Toc425336659" class="anchor"></span></span>Reset Password to Local Users

Run the following commands in console window as administrator privilege:

Net.exe user administrator Password01!

### Create Asymmetric share

Execute the following command in a console window on both node01 and node02:

REG ADD HKLM\\System\\CurrentControlSet\\Services\\LanmanServer\\Parameters /v AsymmetryMode /t REG\_DWORD /d 2 /f

### <span id="_Create_the_virtual" class="anchor"><span id="_Toc425336661" class="anchor"></span></span>Create the virtual hard disk file

There are two ways to create the virtual hard disk file: PowerShell and manually.

Use “rsvd.vhdx” as an example of the virtual hard disk file name.

1.  Run PowerShell as administrator privilege, and execute the following command:

"create vdisk file=C:\\rsvd.vhdx maximum=1024 type=expandable" | diskpart.exe

1.  Open Hyper-V manager, in the right panel, click **New** -&gt; **Hard Disk**

Choose **VHDX** for **Disk Format**

![](./media/image142.png)

Choose **Dynamically expanding** for **Disk Type**

![](./media/image143.png)

![](./media/image144.png)Input the name of the virtual hard disk file in **Name** field

Input **1** for **Size** field. And click **Finish** to create the virtual disk file.

### ![](./media/image145.png)<span id="_Create_Asymmetric_share" class="anchor"><span id="_Toc425336662" class="anchor"></span></span>Create the virtual hard disk set file

There are two ways to create the virtual hard disk set file: PowerShell and manually.

Use “rsvd.vhds” as an example of the virtual hard disk set file name.

1.  Run PowerShell as administrator privilege, and execute the following command:

> New-VHD ”c:\\rsvd.vhds” -SizeBytes 1GB -Fixed -LogicalSectorSizeBytes 512

1.  Open Hyper-V manager, in the right panel, click **New** -&gt; **Hard Disk**

Choose **VHD Set** for **Disk Format**

![](./media/image146.png)

![](./media/image147.png)Choose **Fixed size** for **Disk Type**

> Input the name of the virtual hard disk set file in **Name** field

![](./media/image148.png)

Input **1** for **Size** field. And click **Finish** to create the virtual disk file.

![](./media/image149.png)

### <span id="_Modify_the_Signing" class="anchor"><span id="_Toc425336663" class="anchor"></span></span>Modify the Signing configuration

Execute the following command in a console window on node01 to enable SigningRequired:

cmd /C reg ADD "HKLM\\System\\ControlSet001\\Services\\LanmanServer\\Parameters" /f /v requiresecuritysignature /t REG\_DWORD /d 1

Execute the following command in a console window on node01 to disable SigningRequired:

cmd /C reg DELETE "HKLM\\System\\ControlSet001\\Services\\LanmanServer\\Parameters" /f /v requiresecuritysignature

### <span id="_Manually_Setup_Computer" class="anchor"><span id="_Manually_Set_up" class="anchor"><span id="_Toc425336664" class="anchor"></span></span></span>Manually Set up Computer Password

Run the following command in a console window:

ksetup /SetComputerPassword &lt;password&gt;

to set the password for the computer’s domain account. And restart the computer.

### <span id="_Create_an_SQOS" class="anchor"><span id="_Toc425336665" class="anchor"></span></span>Create an SQOS policy 

Run the following PowerShell command to create a MultiInstance sqos policy in the scale out file server:

$policy = New-StorageQosPolicy -Name sqostest -PolicyType MultiInstance -MinimumIops 100 -MaximumIops 200

Get the policy id:

$policy.PolicyId

<span id="_Configure_Claimed_Based" class="anchor"><span id="_Toc420507657" class="anchor"><span id="_Toc420573814" class="anchor"><span id="_Toc420507658" class="anchor"><span id="_Toc420573815" class="anchor"><span id="_Toc420507660" class="anchor"><span id="_Toc420573817" class="anchor"><span id="_Toc420507661" class="anchor"><span id="_Toc420573818" class="anchor"><span id="_Toc420507662" class="anchor"><span id="_Toc420573819" class="anchor"><span id="_Toc420507663" class="anchor"><span id="_Toc420573820" class="anchor"><span id="_Toc420507664" class="anchor"><span id="_Toc420573821" class="anchor"><span id="_Toc420507665" class="anchor"><span id="_Toc420573822" class="anchor"><span id="_Toc420507666" class="anchor"><span id="_Toc420573823" class="anchor"><span id="_Toc420507667" class="anchor"><span id="_Toc420573824" class="anchor"><span id="_Toc420507668" class="anchor"><span id="_Toc420573825" class="anchor"><span id="_Toc420507669" class="anchor"><span id="_Toc420573826" class="anchor"><span id="_Toc420507670" class="anchor"><span id="_Toc420573827" class="anchor"><span id="_Toc420507671" class="anchor"><span id="_Toc420573828" class="anchor"><span id="_Toc420507672" class="anchor"><span id="_Toc420573829" class="anchor"><span id="_Toc420507673" class="anchor"><span id="_Toc420573830" class="anchor"><span id="_Toc420507674" class="anchor"><span id="_Toc420573831" class="anchor"><span id="_Toc420507675" class="anchor"><span id="_Toc420573832" class="anchor"><span id="_Toc420507676" class="anchor"><span id="_Toc420573833" class="anchor"><span id="_Toc420507677" class="anchor"><span id="_Toc420573834" class="anchor"><span id="_Toc420507678" class="anchor"><span id="_Toc420573835" class="anchor"><span id="_Toc420507679" class="anchor"><span id="_Toc420573836" class="anchor"><span id="_Toc420507680" class="anchor"><span id="_Toc420573837" class="anchor"><span id="_Toc420507681" class="anchor"><span id="_Toc420573838" class="anchor"><span id="_Toc420507682" class="anchor"><span id="_Toc420573839" class="anchor"><span id="_Toc420507683" class="anchor"><span id="_Toc420573840" class="anchor"><span id="_Toc420507684" class="anchor"><span id="_Toc420573841" class="anchor"><span id="_Toc420507685" class="anchor"><span id="_Toc420573842" class="anchor"><span id="_Toc420507686" class="anchor"><span id="_Toc420573843" class="anchor"><span id="_Toc420507687" class="anchor"><span id="_Toc420573844" class="anchor"><span id="_Toc420507688" class="anchor"><span id="_Toc420573845" class="anchor"><span id="_Toc420507689" class="anchor"><span id="_Toc420573846" class="anchor"><span id="_Toc420507690" class="anchor"><span id="_Toc420573847" class="anchor"><span id="_Toc420507691" class="anchor"><span id="_Toc420573848" class="anchor"><span id="_Toc420507692" class="anchor"><span id="_Toc420573849" class="anchor"><span id="_Toc420507693" class="anchor"><span id="_Toc420573850" class="anchor"><span id="_Toc420507694" class="anchor"><span id="_Toc420573851" class="anchor"><span id="_Toc420507695" class="anchor"><span id="_Toc420573852" class="anchor"><span id="_Toc420507696" class="anchor"><span id="_Toc420573853" class="anchor"><span id="_Toc420507697" class="anchor"><span id="_Toc420573854" class="anchor"><span id="_Toc420507698" class="anchor"><span id="_Toc420573855" class="anchor"><span id="_Toc420507699" class="anchor"><span id="_Toc420573856" class="anchor"><span id="_Toc420507700" class="anchor"><span id="_Toc420573857" class="anchor"><span id="_Toc420507701" class="anchor"><span id="_Toc420573858" class="anchor"><span id="_Toc420507702" class="anchor"><span id="_Toc420573859" class="anchor"><span id="_Toc420507703" class="anchor"><span id="_Toc420573860" class="anchor"><span id="_Toc420507704" class="anchor"><span id="_Toc420573861" class="anchor"><span id="_Toc420507705" class="anchor"><span id="_Toc420573862" class="anchor"><span id="_Toc420507706" class="anchor"><span id="_Toc420573863" class="anchor"><span id="_Toc420507707" class="anchor"><span id="_Toc420573864" class="anchor"><span id="_Toc420507708" class="anchor"><span id="_Toc420573865" class="anchor"><span id="_Toc420507709" class="anchor"><span id="_Toc420573866" class="anchor"><span id="_Toc420507710" class="anchor"><span id="_Toc420573867" class="anchor"><span id="_Toc420507711" class="anchor"><span id="_Toc420573868" class="anchor"><span id="_Toc420507712" class="anchor"><span id="_Toc420573869" class="anchor"><span id="_Toc420507713" class="anchor"><span id="_Toc420573870" class="anchor"><span id="_Toc420507715" class="anchor"><span id="_Toc420573872" class="anchor"><span id="_Toc420507716" class="anchor"><span id="_Toc420573873" class="anchor"><span id="_Toc420507717" class="anchor"><span id="_Toc420573874" class="anchor"><span id="_Toc420507718" class="anchor"><span id="_Toc420573875" class="anchor"><span id="_Toc420507719" class="anchor"><span id="_Toc420573876" class="anchor"><span id="_Toc420507720" class="anchor"><span id="_Toc420573877" class="anchor"><span id="_Toc420507721" class="anchor"><span id="_Toc420573878" class="anchor"><span id="_Toc420507722" class="anchor"><span id="_Toc420573879" class="anchor"><span id="_Toc420507723" class="anchor"><span id="_Toc420573880" class="anchor"><span id="_Toc420507724" class="anchor"><span id="_Toc420573881" class="anchor"><span id="_Toc420507725" class="anchor"><span id="_Toc420573882" class="anchor"><span id="_Toc420507726" class="anchor"><span id="_Toc420573883" class="anchor"><span id="_Toc420507727" class="anchor"><span id="_Toc420573884" class="anchor"><span id="_Toc420507728" class="anchor"><span id="_Toc420573885" class="anchor"><span id="_Toc420507729" class="anchor"><span id="_Toc420573886" class="anchor"><span id="_Toc420507730" class="anchor"><span id="_Toc420573887" class="anchor"><span id="_Toc420507731" class="anchor"><span id="_Toc420573888" class="anchor"><span id="_Toc420507732" class="anchor"><span id="_Toc420573889" class="anchor"><span id="_Toc420507733" class="anchor"><span id="_Toc420573890" class="anchor"><span id="_Toc420507734" class="anchor"><span id="_Toc420573891" class="anchor"><span id="_Toc420507735" class="anchor"><span id="_Toc420573892" class="anchor"><span id="_Toc420507736" class="anchor"><span id="_Toc420573893" class="anchor"><span id="_Toc420507737" class="anchor"><span id="_Toc420573894" class="anchor"><span id="_Toc420507738" class="anchor"><span id="_Toc420573895" class="anchor"><span id="_Toc420507739" class="anchor"><span id="_Toc420573896" class="anchor"><span id="_Toc420507740" class="anchor"><span id="_Toc420573897" class="anchor"><span id="_Toc420507741" class="anchor"><span id="_Toc420573898" class="anchor"><span id="_Toc420507742" class="anchor"><span id="_Toc420573899" class="anchor"><span id="_Toc420507743" class="anchor"><span id="_Toc420573900" class="anchor"><span id="_Toc420507744" class="anchor"><span id="_Toc420573901" class="anchor"><span id="_Toc420507745" class="anchor"><span id="_Toc420573902" class="anchor"><span id="_Toc420507746" class="anchor"><span id="_Toc420573903" class="anchor"><span id="_Toc420507749" class="anchor"><span id="_Toc420573906" class="anchor"><span id="_Toc420507750" class="anchor"><span id="_Toc420573907" class="anchor"><span id="_Toc420507751" class="anchor"><span id="_Toc420573908" class="anchor"><span id="_Toc420507752" class="anchor"><span id="_Toc420573909" class="anchor"><span id="_Toc420507753" class="anchor"><span id="_Toc420573910" class="anchor"><span id="_Toc420507754" class="anchor"><span id="_Toc420573911" class="anchor"><span id="_Toc420507755" class="anchor"><span id="_Toc420573912" class="anchor"><span id="_Toc420507756" class="anchor"><span id="_Toc420573913" class="anchor"><span id="_Toc420507757" class="anchor"><span id="_Toc420573914" class="anchor"><span id="_Toc420507758" class="anchor"><span id="_Toc420573915" class="anchor"><span id="_Toc420507759" class="anchor"><span id="_Toc420573916" class="anchor"><span id="_Toc420507760" class="anchor"><span id="_Toc420573917" class="anchor"><span id="_Toc420507761" class="anchor"><span id="_Toc420573918" class="anchor"><span id="_Toc420507762" class="anchor"><span id="_Toc420573919" class="anchor"><span id="_Toc420507763" class="anchor"><span id="_Toc420573920" class="anchor"><span id="_Toc420507764" class="anchor"><span id="_Toc420573921" class="anchor"><span id="_Toc420507765" class="anchor"><span id="_Toc420573922" class="anchor"><span id="_Toc420507766" class="anchor"><span id="_Toc420573923" class="anchor"><span id="_Toc420507767" class="anchor"><span id="_Toc420573924" class="anchor"><span id="_Toc420507768" class="anchor"><span id="_Toc420573925" class="anchor"><span id="_Toc420507769" class="anchor"><span id="_Toc420573926" class="anchor"><span id="_Toc420507770" class="anchor"><span id="_Toc420573927" class="anchor"><span id="_Toc420507771" class="anchor"><span id="_Toc420573928" class="anchor"><span id="_Toc420507772" class="anchor"><span id="_Toc420573929" class="anchor"><span id="_Toc420507773" class="anchor"><span id="_Toc420573930" class="anchor"><span id="_Toc420507774" class="anchor"><span id="_Toc420573931" class="anchor"><span id="_Toc420507775" class="anchor"><span id="_Toc420573932" class="anchor"><span id="_Toc420507776" class="anchor"><span id="_Toc420573933" class="anchor"><span id="_Toc420507777" class="anchor"><span id="_Toc420573934" class="anchor"><span id="_Toc420507778" class="anchor"><span id="_Toc420573935" class="anchor"><span id="_Toc420507779" class="anchor"><span id="_Toc420573936" class="anchor"><span id="_Toc420507780" class="anchor"><span id="_Toc420573937" class="anchor"><span id="_Toc420507781" class="anchor"><span id="_Toc420573938" class="anchor"><span id="_Toc420507782" class="anchor"><span id="_Toc420573939" class="anchor"><span id="_Toc420507783" class="anchor"><span id="_Toc420573940" class="anchor"><span id="_Toc420507784" class="anchor"><span id="_Toc420573941" class="anchor"><span id="_Toc420507785" class="anchor"><span id="_Toc420573942" class="anchor"><span id="_Toc420507786" class="anchor"><span id="_Toc420573943" class="anchor"><span id="_Toc420507787" class="anchor"><span id="_Toc420573944" class="anchor"><span id="_Toc420507788" class="anchor"><span id="_Toc420573945" class="anchor"><span id="_Toc420507789" class="anchor"><span id="_Toc420573946" class="anchor"><span id="_Toc420507790" class="anchor"><span id="_Toc420573947" class="anchor"><span id="_Toc420507791" class="anchor"><span id="_Toc420573948" class="anchor"><span id="_Toc420507792" class="anchor"><span id="_Toc420573949" class="anchor"><span id="_Toc420507793" class="anchor"><span id="_Toc420573950" class="anchor"><span id="_Toc420507794" class="anchor"><span id="_Toc420573951" class="anchor"><span id="_Toc420507795" class="anchor"><span id="_Toc420573952" class="anchor"><span id="_Toc420507796" class="anchor"><span id="_Toc420573953" class="anchor"><span id="_Toc420507797" class="anchor"><span id="_Toc420573954" class="anchor"><span id="_Toc420507799" class="anchor"><span id="_Toc420573956" class="anchor"><span id="_Toc420507800" class="anchor"><span id="_Toc420573957" class="anchor"><span id="_Toc420507801" class="anchor"><span id="_Toc420573958" class="anchor"><span id="_Toc420507802" class="anchor"><span id="_Toc420573959" class="anchor"><span id="_Toc420507803" class="anchor"><span id="_Toc420573960" class="anchor"><span id="_Toc420507804" class="anchor"><span id="_Toc420573961" class="anchor"><span id="_Toc420507805" class="anchor"><span id="_Toc420573962" class="anchor"><span id="_Toc420507806" class="anchor"><span id="_Toc420573963" class="anchor"><span id="_Toc420507807" class="anchor"><span id="_Toc420573964" class="anchor"><span id="_Toc420507808" class="anchor"><span id="_Toc420573965" class="anchor"><span id="_Toc420507809" class="anchor"><span id="_Toc420573966" class="anchor"><span id="_Toc420507810" class="anchor"><span id="_Toc420573967" class="anchor"><span id="_Toc420507811" class="anchor"><span id="_Toc420573968" class="anchor"><span id="_Toc420507812" class="anchor"><span id="_Toc420573969" class="anchor"><span id="_Toc420507813" class="anchor"><span id="_Toc420573970" class="anchor"><span id="_Toc420507814" class="anchor"><span id="_Toc420573971" class="anchor"><span id="_Toc420507815" class="anchor"><span id="_Toc420573972" class="anchor"><span id="_Toc420507817" class="anchor"><span id="_Toc420573974" class="anchor"><span id="_Toc420507818" class="anchor"><span id="_Toc420573975" class="anchor"><span id="_Toc420507819" class="anchor"><span id="_Toc420573976" class="anchor"><span id="_Toc420507820" class="anchor"><span id="_Toc420573977" class="anchor"><span id="_Toc420507821" class="anchor"><span id="_Toc420573978" class="anchor"><span id="_Toc420507822" class="anchor"><span id="_Toc420573979" class="anchor"><span id="_Toc420507823" class="anchor"><span id="_Toc420573980" class="anchor"><span id="_Toc420507824" class="anchor"><span id="_Toc420573981" class="anchor"><span id="_Toc420507825" class="anchor"><span id="_Toc420573982" class="anchor"><span id="_Toc420507826" class="anchor"><span id="_Toc420573983" class="anchor"><span id="_Toc420507827" class="anchor"><span id="_Toc420573984" class="anchor"><span id="_Toc420507828" class="anchor"><span id="_Toc420573985" class="anchor"><span id="_Toc420507829" class="anchor"><span id="_Toc420573986" class="anchor"><span id="_Toc420507830" class="anchor"><span id="_Toc420573987" class="anchor"><span id="_Toc420507831" class="anchor"><span id="_Toc420573988" class="anchor"><span id="_Toc420507832" class="anchor"><span id="_Toc420573989" class="anchor"><span id="_Toc420507833" class="anchor"><span id="_Toc420573990" class="anchor"><span id="_Toc420507834" class="anchor"><span id="_Toc420573991" class="anchor"><span id="_Toc420507835" class="anchor"><span id="_Toc420573992" class="anchor"><span id="_Toc420507836" class="anchor"><span id="_Toc420573993" class="anchor"><span id="_Toc420507837" class="anchor"><span id="_Toc420573994" class="anchor"><span id="_Toc420507838" class="anchor"><span id="_Toc420573995" class="anchor"><span id="_Toc420507839" class="anchor"><span id="_Toc420573996" class="anchor"><span id="_Toc420507840" class="anchor"><span id="_Toc420573997" class="anchor"><span id="_Toc420507841" class="anchor"><span id="_Toc420573998" class="anchor"><span id="_Toc420507842" class="anchor"><span id="_Toc420573999" class="anchor"><span id="_Toc420507843" class="anchor"><span id="_Toc420574000" class="anchor"><span id="_Toc420507844" class="anchor"><span id="_Toc420574001" class="anchor"><span id="_Toc420507845" class="anchor"><span id="_Toc420574002" class="anchor"><span id="_Toc420507846" class="anchor"><span id="_Toc420574003" class="anchor"><span id="_Toc420507847" class="anchor"><span id="_Toc420574004" class="anchor"><span id="_Toc420507848" class="anchor"><span id="_Toc420574005" class="anchor"><span id="_Toc420507849" class="anchor"><span id="_Toc420574006" class="anchor"><span id="_Toc420507850" class="anchor"><span id="_Toc420574007" class="anchor"><span id="_Toc420507851" class="anchor"><span id="_Toc420574008" class="anchor"><span id="_Toc420507853" class="anchor"><span id="_Toc420574010" class="anchor"><span id="_Toc420507854" class="anchor"><span id="_Toc420574011" class="anchor"><span id="_Toc420507856" class="anchor"><span id="_Toc420574013" class="anchor"><span id="_Toc420507859" class="anchor"><span id="_Toc420574016" class="anchor"><span id="_Toc420507860" class="anchor"><span id="_Toc420574017" class="anchor"><span id="_Toc420507861" class="anchor"><span id="_Toc420574018" class="anchor"><span id="_Toc420507863" class="anchor"><span id="_Toc420574020" class="anchor"><span id="_Toc420507864" class="anchor"><span id="_Toc420574021" class="anchor"><span id="_Toc420507867" class="anchor"><span id="_Toc420574024" class="anchor"><span id="_Toc420507868" class="anchor"><span id="_Toc420574025" class="anchor"><span id="_Toc420507869" class="anchor"><span id="_Toc420574026" class="anchor"><span id="_Toc420507870" class="anchor"><span id="_Toc420574027" class="anchor"><span id="_Toc420507871" class="anchor"><span id="_Toc420574028" class="anchor"><span id="_Toc420507872" class="anchor"><span id="_Toc420574029" class="anchor"><span id="_Toc420507873" class="anchor"><span id="_Toc420574030" class="anchor"><span id="_Toc420507875" class="anchor"><span id="_Toc420574032" class="anchor"><span id="_Toc420507876" class="anchor"><span id="_Toc420574033" class="anchor"><span id="_Toc420507877" class="anchor"><span id="_Toc420574034" class="anchor"><span id="_Toc420507878" class="anchor"><span id="_Toc420574035" class="anchor"><span id="_Toc420507879" class="anchor"><span id="_Toc420574036" class="anchor"><span id="_Toc420507880" class="anchor"><span id="_Toc420574037" class="anchor"><span id="_Toc420507881" class="anchor"><span id="_Toc420574038" class="anchor"><span id="_Toc420507882" class="anchor"><span id="_Toc420574039" class="anchor"><span id="_Toc420507883" class="anchor"><span id="_Toc420574040" class="anchor"><span id="_Toc420507884" class="anchor"><span id="_Toc420574041" class="anchor"><span id="_Toc420507885" class="anchor"><span id="_Toc420574042" class="anchor"><span id="_Toc420507886" class="anchor"><span id="_Toc420574043" class="anchor"><span id="_Toc420507887" class="anchor"><span id="_Toc420574044" class="anchor"><span id="_Toc420507888" class="anchor"><span id="_Toc420574045" class="anchor"><span id="_Toc420507889" class="anchor"><span id="_Toc420574046" class="anchor"><span id="_Toc420507890" class="anchor"><span id="_Toc420574047" class="anchor"><span id="_Toc420507891" class="anchor"><span id="_Toc420574048" class="anchor"><span id="_Toc420507892" class="anchor"><span id="_Toc420574049" class="anchor"><span id="_Toc420507893" class="anchor"><span id="_Toc420574050" class="anchor"><span id="_Toc420507894" class="anchor"><span id="_Toc420574051" class="anchor"><span id="_Toc420507895" class="anchor"><span id="_Toc420574052" class="anchor"><span id="_Toc420507896" class="anchor"><span id="_Toc420574053" class="anchor"><span id="_Toc420507897" class="anchor"><span id="_Toc420574054" class="anchor"><span id="_Toc420507898" class="anchor"><span id="_Toc420574055" class="anchor"><span id="_Toc420507899" class="anchor"><span id="_Toc420574056" class="anchor"><span id="_Toc420507900" class="anchor"><span id="_Toc420574057" class="anchor"><span id="_Toc420507901" class="anchor"><span id="_Toc420574058" class="anchor"><span id="_Toc420507902" class="anchor"><span id="_Toc420574059" class="anchor"><span id="_Toc420507904" class="anchor"><span id="_Toc420574061" class="anchor"><span id="_Toc420507905" class="anchor"><span id="_Toc420574062" class="anchor"><span id="_Toc420507906" class="anchor"><span id="_Toc420574063" class="anchor"><span id="_Toc420507907" class="anchor"><span id="_Toc420574064" class="anchor"><span id="_Toc420507908" class="anchor"><span id="_Toc420574065" class="anchor"><span id="_Toc420507909" class="anchor"><span id="_Toc420574066" class="anchor"><span id="_Toc420507910" class="anchor"><span id="_Toc420574067" class="anchor"><span id="_Toc420507911" class="anchor"><span id="_Toc420574068" class="anchor"><span id="_Toc420507912" class="anchor"><span id="_Toc420574069" class="anchor"><span id="_Toc420507913" class="anchor"><span id="_Toc420574070" class="anchor"><span id="_Toc420507914" class="anchor"><span id="_Toc420574071" class="anchor"><span id="_Toc420507915" class="anchor"><span id="_Toc420574072" class="anchor"><span id="_Toc420507916" class="anchor"><span id="_Toc420574073" class="anchor"><span id="_Toc420507917" class="anchor"><span id="_Toc420574074" class="anchor"><span id="_Toc420507918" class="anchor"><span id="_Toc420574075" class="anchor"><span id="_Toc420507919" class="anchor"><span id="_Toc420574076" class="anchor"><span id="_Toc420507920" class="anchor"><span id="_Toc420574077" class="anchor"><span id="_Toc420507921" class="anchor"><span id="_Toc420574078" class="anchor"><span id="_Toc420507922" class="anchor"><span id="_Toc420574079" class="anchor"><span id="_Toc420507923" class="anchor"><span id="_Toc420574080" class="anchor"><span id="_Toc420507924" class="anchor"><span id="_Toc420574081" class="anchor"><span id="_Toc420507925" class="anchor"><span id="_Toc420574082" class="anchor"><span id="_Toc420507926" class="anchor"><span id="_Toc420574083" class="anchor"><span id="_Toc420507927" class="anchor"><span id="_Toc420574084" class="anchor"><span id="_Toc420507928" class="anchor"><span id="_Toc420574085" class="anchor"><span id="_Toc420507929" class="anchor"><span id="_Toc420574086" class="anchor"><span id="_Toc420507930" class="anchor"><span id="_Toc420574087" class="anchor"><span id="_Toc420507931" class="anchor"><span id="_Toc420574088" class="anchor"><span id="_Toc420507932" class="anchor"><span id="_Toc420574089" class="anchor"><span id="_Toc420507934" class="anchor"><span id="_Toc420574091" class="anchor"><span id="_Toc420507935" class="anchor"><span id="_Toc420574092" class="anchor"><span id="_Toc420507936" class="anchor"><span id="_Toc420574093" class="anchor"><span id="_Toc420507937" class="anchor"><span id="_Toc420574094" class="anchor"><span id="_Toc420507938" class="anchor"><span id="_Toc420574095" class="anchor"><span id="_Toc420507939" class="anchor"><span id="_Toc420574096" class="anchor"><span id="_Toc420507940" class="anchor"><span id="_Toc420574097" class="anchor"><span id="_Toc420507941" class="anchor"><span id="_Toc420574098" class="anchor"><span id="_Toc420507942" class="anchor"><span id="_Toc420574099" class="anchor"><span id="_Toc420507943" class="anchor"><span id="_Toc420574100" class="anchor"><span id="_Toc420507944" class="anchor"><span id="_Toc420574101" class="anchor"><span id="_Toc420507945" class="anchor"><span id="_Toc420574102" class="anchor"><span id="_Toc420507946" class="anchor"><span id="_Toc420574103" class="anchor"><span id="_Toc420507947" class="anchor"><span id="_Toc420574104" class="anchor"><span id="_Toc420507948" class="anchor"><span id="_Toc420574105" class="anchor"><span id="_Toc420507949" class="anchor"><span id="_Toc420574106" class="anchor"><span id="_Toc420507950" class="anchor"><span id="_Toc420574107" class="anchor"><span id="_Toc420507951" class="anchor"><span id="_Toc420574108" class="anchor"><span id="_Toc420507952" class="anchor"><span id="_Toc420574109" class="anchor"><span id="_Toc420507953" class="anchor"><span id="_Toc420574110" class="anchor"><span id="_Toc420507954" class="anchor"><span id="_Toc420574111" class="anchor"><span id="_Toc420507955" class="anchor"><span id="_Toc420574112" class="anchor"><span id="_Toc420507956" class="anchor"><span id="_Toc420574113" class="anchor"><span id="_Toc420507957" class="anchor"><span id="_Toc420574114" class="anchor"><span id="_Toc420507958" class="anchor"><span id="_Toc420574115" class="anchor"><span id="_Toc420507959" class="anchor"><span id="_Toc420574116" class="anchor"><span id="_Toc420507960" class="anchor"><span id="_Toc420574117" class="anchor"><span id="_Toc420507961" class="anchor"><span id="_Toc420574118" class="anchor"><span id="_Toc420507962" class="anchor"><span id="_Toc420574119" class="anchor"><span id="_Toc420507963" class="anchor"><span id="_Toc420574120" class="anchor"><span id="_Toc420507964" class="anchor"><span id="_Toc420574121" class="anchor"><span id="_Toc420507965" class="anchor"><span id="_Toc420574122" class="anchor"><span id="_Toc420507966" class="anchor"><span id="_Toc420574123" class="anchor"><span id="_Toc420507967" class="anchor"><span id="_Toc420574124" class="anchor"><span id="_Toc420507968" class="anchor"><span id="_Toc420574125" class="anchor"><span id="_Toc420507969" class="anchor"><span id="_Toc420574126" class="anchor"><span id="_Toc420507970" class="anchor"><span id="_Toc420574127" class="anchor"><span id="_Toc420507971" class="anchor"><span id="_Toc420574128" class="anchor"><span id="_Toc420507972" class="anchor"><span id="_Toc420574129" class="anchor"><span id="_Toc420507973" class="anchor"><span id="_Toc420574130" class="anchor"><span id="_Toc420507974" class="anchor"><span id="_Toc420574131" class="anchor"><span id="_Toc420507975" class="anchor"><span id="_Toc420574132" class="anchor"><span id="_Toc420507976" class="anchor"><span id="_Toc420574133" class="anchor"><span id="_Toc420507977" class="anchor"><span id="_Toc420574134" class="anchor"><span id="_Toc420507978" class="anchor"><span id="_Toc420574135" class="anchor"><span id="_Toc420507979" class="anchor"><span id="_Toc420574136" class="anchor"><span id="_Toc420507980" class="anchor"><span id="_Toc420574137" class="anchor"><span id="_Toc420507981" class="anchor"><span id="_Toc420574138" class="anchor"><span id="_Toc420507982" class="anchor"><span id="_Toc420574139" class="anchor"><span id="_Toc420507983" class="anchor"><span id="_Toc420574140" class="anchor"><span id="_Toc420507984" class="anchor"><span id="_Toc420574141" class="anchor"><span id="_Toc420507985" class="anchor"><span id="_Toc420574142" class="anchor"><span id="_Toc420507986" class="anchor"><span id="_Toc420574143" class="anchor"><span id="_Toc420507987" class="anchor"><span id="_Toc420574144" class="anchor"><span id="_Toc420507988" class="anchor"><span id="_Toc420574145" class="anchor"><span id="_Toc420507989" class="anchor"><span id="_Toc420574146" class="anchor"><span id="_Toc420507990" class="anchor"><span id="_Toc420574147" class="anchor"><span id="_Toc420507991" class="anchor"><span id="_Toc420574148" class="anchor"><span id="_Toc420507992" class="anchor"><span id="_Toc420574149" class="anchor"><span id="_Toc420507993" class="anchor"><span id="_Toc420574150" class="anchor"><span id="_Toc420507994" class="anchor"><span id="_Toc420574151" class="anchor"><span id="_Toc420507995" class="anchor"><span id="_Toc420574152" class="anchor"><span id="_Toc420507996" class="anchor"><span id="_Toc420574153" class="anchor"><span id="_Toc420507997" class="anchor"><span id="_Toc420574154" class="anchor"><span id="_Toc420507998" class="anchor"><span id="_Toc420574155" class="anchor"><span id="_Toc420507999" class="anchor"><span id="_Toc420574156" class="anchor"><span id="_Toc420508000" class="anchor"><span id="_Toc420574157" class="anchor"><span id="_Toc420508001" class="anchor"><span id="_Toc420574158" class="anchor"><span id="_Toc420508002" class="anchor"><span id="_Toc420574159" class="anchor"><span id="_Toc420508003" class="anchor"><span id="_Toc420574160" class="anchor"><span id="_Toc420508005" class="anchor"><span id="_Toc420574162" class="anchor"><span id="_Toc420508006" class="anchor"><span id="_Toc420574163" class="anchor"><span id="_Toc420508007" class="anchor"><span id="_Toc420574164" class="anchor"><span id="_Toc420508008" class="anchor"><span id="_Toc420574165" class="anchor"><span id="_Toc420508009" class="anchor"><span id="_Toc420574166" class="anchor"><span id="_Toc420508010" class="anchor"><span id="_Toc420574167" class="anchor"><span id="_Toc420508011" class="anchor"><span id="_Toc420574168" class="anchor"><span id="_Toc420508012" class="anchor"><span id="_Toc420574169" class="anchor"><span id="_Toc420508013" class="anchor"><span id="_Toc420574170" class="anchor"><span id="_Toc420508015" class="anchor"><span id="_Toc420574172" class="anchor"><span id="_Toc420508016" class="anchor"><span id="_Toc420574173" class="anchor"><span id="_Toc420508017" class="anchor"><span id="_Toc420574174" class="anchor"><span id="_Toc420508018" class="anchor"><span id="_Toc420574175" class="anchor"><span id="_Toc420508019" class="anchor"><span id="_Toc420574176" class="anchor"><span id="_Toc420508020" class="anchor"><span id="_Toc420574177" class="anchor"><span id="_Toc420508021" class="anchor"><span id="_Toc420574178" class="anchor"><span id="_Toc420508022" class="anchor"><span id="_Toc420574179" class="anchor"><span id="_Toc420508023" class="anchor"><span id="_Toc420574180" class="anchor"><span id="_Toc420508024" class="anchor"><span id="_Toc420574181" class="anchor"><span id="_Toc420508025" class="anchor"><span id="_Toc420574182" class="anchor"><span id="_Toc420508026" class="anchor"><span id="_Toc420574183" class="anchor"><span id="_Toc420508027" class="anchor"><span id="_Toc420574184" class="anchor"><span id="_Toc420508028" class="anchor"><span id="_Toc420574185" class="anchor"><span id="_Toc420508029" class="anchor"><span id="_Toc420574186" class="anchor"><span id="_Toc420508031" class="anchor"><span id="_Toc420574188" class="anchor"><span id="_Toc420508032" class="anchor"><span id="_Toc420574189" class="anchor"><span id="_Toc420508033" class="anchor"><span id="_Toc420574190" class="anchor"><span id="_Toc420508034" class="anchor"><span id="_Toc420574191" class="anchor"><span id="_Toc420508035" class="anchor"><span id="_Toc420574192" class="anchor"><span id="_Toc420508036" class="anchor"><span id="_Toc420574193" class="anchor"><span id="_Toc420508042" class="anchor"><span id="_Toc420574199" class="anchor"><span id="_Toc420508043" class="anchor"><span id="_Toc420574200" class="anchor"><span id="_Toc420508044" class="anchor"><span id="_Toc420574201" class="anchor"><span id="_Toc362208491" class="anchor"><span id="_Toc362208807" class="anchor"><span id="_Toc362260326" class="anchor"><span id="_Toc362260641" class="anchor"><span id="_Toc362725927" class="anchor"><span id="_Toc362726313" class="anchor"><span id="_Toc362208492" class="anchor"><span id="_Toc362208808" class="anchor"><span id="_Toc362260327" class="anchor"><span id="_Toc362260642" class="anchor"><span id="_Toc362725928" class="anchor"><span id="_Toc362726314" class="anchor"><span id="_Toc362208493" class="anchor"><span id="_Toc362208809" class="anchor"><span id="_Toc362260328" class="anchor"><span id="_Toc362260643" class="anchor"><span id="_Toc362725929" class="anchor"><span id="_Toc362726315" class="anchor"><span id="_Toc362208494" class="anchor"><span id="_Toc362208810" class="anchor"><span id="_Toc362260329" class="anchor"><span id="_Toc362260644" class="anchor"><span id="_Toc362725930" class="anchor"><span id="_Toc362726316" class="anchor"><span id="_Toc362208495" class="anchor"><span id="_Toc362208811" class="anchor"><span id="_Toc362260330" class="anchor"><span id="_Toc362260645" class="anchor"><span id="_Toc362725931" class="anchor"><span id="_Toc362726317" class="anchor"><span id="_Toc362208496" class="anchor"><span id="_Toc362208812" class="anchor"><span id="_Toc362260331" class="anchor"><span id="_Toc362260646" class="anchor"><span id="_Toc362725932" class="anchor"><span id="_Toc362726318" class="anchor"><span id="_Toc362208497" class="anchor"><span id="_Toc362208813" class="anchor"><span id="_Toc362260332" class="anchor"><span id="_Toc362260647" class="anchor"><span id="_Toc362725933" class="anchor"><span id="_Toc362726319" class="anchor"><span id="_Toc362208498" class="anchor"><span id="_Toc362208814" class="anchor"><span id="_Toc362260333" class="anchor"><span id="_Toc362260648" class="anchor"><span id="_Toc362725934" class="anchor"><span id="_Toc362726320" class="anchor"><span id="_Toc362208499" class="anchor"><span id="_Toc362208815" class="anchor"><span id="_Toc362260334" class="anchor"><span id="_Toc362260649" class="anchor"><span id="_Toc362725935" class="anchor"><span id="_Toc362726321" class="anchor"><span id="_Toc362208500" class="anchor"><span id="_Toc362208816" class="anchor"><span id="_Toc362260335" class="anchor"><span id="_Toc362260650" class="anchor"><span id="_Toc362725936" class="anchor"><span id="_Toc362726322" class="anchor"><span id="_Toc362208501" class="anchor"><span id="_Toc362208817" class="anchor"><span id="_Toc362260336" class="anchor"><span id="_Toc362260651" class="anchor"><span id="_Toc362725937" class="anchor"><span id="_Toc362726323" class="anchor"><span id="_Toc362208502" class="anchor"><span id="_Toc362208818" class="anchor"><span id="_Toc362260337" class="anchor"><span id="_Toc362260652" class="anchor"><span id="_Toc362725938" class="anchor"><span id="_Toc362726324" class="anchor"><span id="_Toc362208503" class="anchor"><span id="_Toc362208819" class="anchor"><span id="_Toc362260338" class="anchor"><span id="_Toc362260653" class="anchor"><span id="_Toc362725939" class="anchor"><span id="_Toc362726325" class="anchor"><span id="_Toc362208504" class="anchor"><span id="_Toc362208820" class="anchor"><span id="_Toc362260339" class="anchor"><span id="_Toc362260654" class="anchor"><span id="_Toc362725940" class="anchor"><span id="_Toc362726326" class="anchor"><span id="_Toc362208505" class="anchor"><span id="_Toc362208821" class="anchor"><span id="_Toc362260340" class="anchor"><span id="_Toc362260655" class="anchor"><span id="_Toc362725941" class="anchor"><span id="_Toc362726327" class="anchor"><span id="_Toc362208506" class="anchor"><span id="_Toc362208822" class="anchor"><span id="_Toc362260341" class="anchor"><span id="_Toc362260656" class="anchor"><span id="_Toc362725942" class="anchor"><span id="_Toc362726328" class="anchor"><span id="_Toc362208507" class="anchor"><span id="_Toc362208823" class="anchor"><span id="_Toc362260342" class="anchor"><span id="_Toc362260657" class="anchor"><span id="_Toc362725943" class="anchor"><span id="_Toc362726329" class="anchor"><span id="_Toc362208508" class="anchor"><span id="_Toc362208824" class="anchor"><span id="_Toc362260343" class="anchor"><span id="_Toc362260658" class="anchor"><span id="_Toc362725944" class="anchor"><span id="_Toc362726330" class="anchor"><span id="_Toc362208509" class="anchor"><span id="_Toc362208825" class="anchor"><span id="_Toc362260344" class="anchor"><span id="_Toc362260659" class="anchor"><span id="_Toc362725945" class="anchor"><span id="_Toc362726331" class="anchor"><span id="_Toc362208510" class="anchor"><span id="_Toc362208826" class="anchor"><span id="_Toc362260345" class="anchor"><span id="_Toc362260660" class="anchor"><span id="_Toc362725946" class="anchor"><span id="_Toc362726332" class="anchor"><span id="_Toc362208511" class="anchor"><span id="_Toc362208827" class="anchor"><span id="_Toc362260346" class="anchor"><span id="_Toc362260661" class="anchor"><span id="_Toc362725947" class="anchor"><span id="_Toc362726333" class="anchor"><span id="_Toc362208512" class="anchor"><span id="_Toc362208828" class="anchor"><span id="_Toc362260347" class="anchor"><span id="_Toc362260662" class="anchor"><span id="_Toc362725948" class="anchor"><span id="_Toc362726334" class="anchor"><span id="_Toc362208513" class="anchor"><span id="_Toc362208829" class="anchor"><span id="_Toc362260348" class="anchor"><span id="_Toc362260663" class="anchor"><span id="_Toc362725949" class="anchor"><span id="_Toc362726335" class="anchor"><span id="_Toc362208514" class="anchor"><span id="_Toc362208830" class="anchor"><span id="_Toc362260349" class="anchor"><span id="_Toc362260664" class="anchor"><span id="_Toc362725950" class="anchor"><span id="_Toc362726336" class="anchor"><span id="_Toc362208515" class="anchor"><span id="_Toc362208831" class="anchor"><span id="_Toc362260350" class="anchor"><span id="_Toc362260665" class="anchor"><span id="_Toc362725951" class="anchor"><span id="_Toc362726337" class="anchor"><span id="_Toc362208516" class="anchor"><span id="_Toc362208832" class="anchor"><span id="_Toc362260351" class="anchor"><span id="_Toc362260666" class="anchor"><span id="_Toc362725952" class="anchor"><span id="_Toc362726338" class="anchor"><span id="_Toc362208517" class="anchor"><span id="_Toc362208833" class="anchor"><span id="_Toc362260352" class="anchor"><span id="_Toc362260667" class="anchor"><span id="_Toc362725953" class="anchor"><span id="_Toc362726339" class="anchor"><span id="_Toc362208518" class="anchor"><span id="_Toc362208834" class="anchor"><span id="_Toc362260353" class="anchor"><span id="_Toc362260668" class="anchor"><span id="_Toc362725954" class="anchor"><span id="_Toc362726340" class="anchor"><span id="_Toc362208519" class="anchor"><span id="_Toc362208835" class="anchor"><span id="_Toc362260354" class="anchor"><span id="_Toc362260669" class="anchor"><span id="_Toc362725955" class="anchor"><span id="_Toc362726341" class="anchor"><span id="_Toc362208520" class="anchor"><span id="_Toc362208836" class="anchor"><span id="_Toc362260355" class="anchor"><span id="_Toc362260670" class="anchor"><span id="_Toc362725956" class="anchor"><span id="_Toc362726342" class="anchor"><span id="_Toc362208521" class="anchor"><span id="_Toc362208837" class="anchor"><span id="_Toc362260356" class="anchor"><span id="_Toc362260671" class="anchor"><span id="_Toc362725957" class="anchor"><span id="_Toc362726343" class="anchor"><span id="_Toc362208522" class="anchor"><span id="_Toc362208838" class="anchor"><span id="_Toc362260357" class="anchor"><span id="_Toc362260672" class="anchor"><span id="_Toc362725958" class="anchor"><span id="_Toc362726344" class="anchor"><span id="_Toc362208523" class="anchor"><span id="_Toc362208839" class="anchor"><span id="_Toc362260358" class="anchor"><span id="_Toc362260673" class="anchor"><span id="_Toc362725959" class="anchor"><span id="_Toc362726345" class="anchor"><span id="_Toc362208524" class="anchor"><span id="_Toc362208840" class="anchor"><span id="_Toc362260359" class="anchor"><span id="_Toc362260674" class="anchor"><span id="_Toc362725960" class="anchor"><span id="_Toc362726346" class="anchor"><span id="_Toc362208525" class="anchor"><span id="_Toc362208841" class="anchor"><span id="_Toc362260360" class="anchor"><span id="_Toc362260675" class="anchor"><span id="_Toc362725961" class="anchor"><span id="_Toc362726347" class="anchor"><span id="_Toc362208526" class="anchor"><span id="_Toc362208842" class="anchor"><span id="_Toc362260361" class="anchor"><span id="_Toc362260676" class="anchor"><span id="_Toc362725962" class="anchor"><span id="_Toc362726348" class="anchor"><span id="_Toc362208527" class="anchor"><span id="_Toc362208843" class="anchor"><span id="_Toc362260362" class="anchor"><span id="_Toc362260677" class="anchor"><span id="_Toc362725963" class="anchor"><span id="_Toc362726349" class="anchor"><span id="_Toc362208528" class="anchor"><span id="_Toc362208844" class="anchor"><span id="_Toc362260363" class="anchor"><span id="_Toc362260678" class="anchor"><span id="_Toc362725964" class="anchor"><span id="_Toc362726350" class="anchor"><span id="_Toc362208529" class="anchor"><span id="_Toc362208845" class="anchor"><span id="_Toc362260364" class="anchor"><span id="_Toc362260679" class="anchor"><span id="_Toc362725965" class="anchor"><span id="_Toc362726351" class="anchor"><span id="_Toc362208530" class="anchor"><span id="_Toc362208846" class="anchor"><span id="_Toc362260365" class="anchor"><span id="_Toc362260680" class="anchor"><span id="_Toc362725966" class="anchor"><span id="_Toc362726352" class="anchor"><span id="_Toc362208531" class="anchor"><span id="_Toc362208847" class="anchor"><span id="_Toc362260366" class="anchor"><span id="_Toc362260681" class="anchor"><span id="_Toc362725967" class="anchor"><span id="_Toc362726353" class="anchor"><span id="_Toc362208532" class="anchor"><span id="_Toc362208848" class="anchor"><span id="_Toc362260367" class="anchor"><span id="_Toc362260682" class="anchor"><span id="_Toc362725968" class="anchor"><span id="_Toc362726354" class="anchor"><span id="_Toc362208533" class="anchor"><span id="_Toc362208849" class="anchor"><span id="_Toc362260368" class="anchor"><span id="_Toc362260683" class="anchor"><span id="_Toc362725969" class="anchor"><span id="_Toc362726355" class="anchor"><span id="_Toc362208534" class="anchor"><span id="_Toc362208850" class="anchor"><span id="_Toc362260369" class="anchor"><span id="_Toc362260684" class="anchor"><span id="_Toc362725970" class="anchor"><span id="_Toc362726356" class="anchor"><span id="_Toc362208535" class="anchor"><span id="_Toc362208851" class="anchor"><span id="_Toc362260370" class="anchor"><span id="_Toc362260685" class="anchor"><span id="_Toc362725971" class="anchor"><span id="_Toc362726357" class="anchor"><span id="_Toc362208536" class="anchor"><span id="_Toc362208852" class="anchor"><span id="_Toc362260371" class="anchor"><span id="_Toc362260686" class="anchor"><span id="_Toc362725972" class="anchor"><span id="_Toc362726358" class="anchor"><span id="_Toc362208537" class="anchor"><span id="_Toc362208853" class="anchor"><span id="_Toc362260372" class="anchor"><span id="_Toc362260687" class="anchor"><span id="_Toc362725973" class="anchor"><span id="_Toc362726359" class="anchor"><span id="_Toc362208538" class="anchor"><span id="_Toc362208854" class="anchor"><span id="_Toc362260373" class="anchor"><span id="_Toc362260688" class="anchor"><span id="_Toc362725974" class="anchor"><span id="_Toc362726360" class="anchor"><span id="_Toc362208539" class="anchor"><span id="_Toc362208855" class="anchor"><span id="_Toc362260374" class="anchor"><span id="_Toc362260689" class="anchor"><span id="_Toc362725975" class="anchor"><span id="_Toc362726361" class="anchor"><span id="_Toc362208540" class="anchor"><span id="_Toc362208856" class="anchor"><span id="_Toc362260375" class="anchor"><span id="_Toc362260690" class="anchor"><span id="_Toc362725976" class="anchor"><span id="_Toc362726362" class="anchor"><span id="_Toc362208541" class="anchor"><span id="_Toc362208857" class="anchor"><span id="_Toc362260376" class="anchor"><span id="_Toc362260691" class="anchor"><span id="_Toc362725977" class="anchor"><span id="_Toc362726363" class="anchor"><span id="_Toc362208542" class="anchor"><span id="_Toc362208858" class="anchor"><span id="_Toc362260377" class="anchor"><span id="_Toc362260692" class="anchor"><span id="_Toc362725978" class="anchor"><span id="_Toc362726364" class="anchor"><span id="_Toc362208543" class="anchor"><span id="_Toc362208859" class="anchor"><span id="_Toc362260378" class="anchor"><span id="_Toc362260693" class="anchor"><span id="_Toc362725979" class="anchor"><span id="_Toc362726365" class="anchor"><span id="_Toc362208544" class="anchor"><span id="_Toc362208860" class="anchor"><span id="_Toc362260379" class="anchor"><span id="_Toc362260694" class="anchor"><span id="_Toc362725980" class="anchor"><span id="_Toc362726366" class="anchor"><span id="_Toc362208545" class="anchor"><span id="_Toc362208861" class="anchor"><span id="_Toc362260380" class="anchor"><span id="_Toc362260695" class="anchor"><span id="_Toc362725981" class="anchor"><span id="_Toc362726367" class="anchor"><span id="_Toc362208546" class="anchor"><span id="_Toc362208862" class="anchor"><span id="_Toc362260381" class="anchor"><span id="_Toc362260696" class="anchor"><span id="_Toc362725982" class="anchor"><span id="_Toc362726368" class="anchor"><span id="_Toc362208547" class="anchor"><span id="_Toc362208863" class="anchor"><span id="_Toc362260382" class="anchor"><span id="_Toc362260697" class="anchor"><span id="_Toc362725983" class="anchor"><span id="_Toc362726369" class="anchor"><span id="_Toc362208548" class="anchor"><span id="_Toc362208864" class="anchor"><span id="_Toc362260383" class="anchor"><span id="_Toc362260698" class="anchor"><span id="_Toc362725984" class="anchor"><span id="_Toc362726370" class="anchor"><span id="_Toc362208549" class="anchor"><span id="_Toc362208865" class="anchor"><span id="_Toc362260384" class="anchor"><span id="_Toc362260699" class="anchor"><span id="_Toc362725985" class="anchor"><span id="_Toc362726371" class="anchor"><span id="_Toc362208550" class="anchor"><span id="_Toc362208866" class="anchor"><span id="_Toc362260385" class="anchor"><span id="_Toc362260700" class="anchor"><span id="_Toc362725986" class="anchor"><span id="_Toc362726372" class="anchor"><span id="_Toc362208551" class="anchor"><span id="_Toc362208867" class="anchor"><span id="_Toc362260386" class="anchor"><span id="_Toc362260701" class="anchor"><span id="_Toc362725987" class="anchor"><span id="_Toc362726373" class="anchor"><span id="_Toc362208552" class="anchor"><span id="_Toc362208868" class="anchor"><span id="_Toc362260387" class="anchor"><span id="_Toc362260702" class="anchor"><span id="_Toc362725988" class="anchor"><span id="_Toc362726374" class="anchor"><span id="_Toc362208553" class="anchor"><span id="_Toc362208869" class="anchor"><span id="_Toc362260388" class="anchor"><span id="_Toc362260703" class="anchor"><span id="_Toc362725989" class="anchor"><span id="_Toc362726375" class="anchor"><span id="_Toc362208554" class="anchor"><span id="_Toc362208870" class="anchor"><span id="_Toc362260389" class="anchor"><span id="_Toc362260704" class="anchor"><span id="_Toc362725990" class="anchor"><span id="_Toc362726376" class="anchor"><span id="_Toc362208555" class="anchor"><span id="_Toc362208871" class="anchor"><span id="_Toc362260390" class="anchor"><span id="_Toc362260705" class="anchor"><span id="_Toc362725991" class="anchor"><span id="_Toc362726377" class="anchor"><span id="_Toc362208556" class="anchor"><span id="_Toc362208872" class="anchor"><span id="_Toc362260391" class="anchor"><span id="_Toc362260706" class="anchor"><span id="_Toc362725992" class="anchor"><span id="_Toc362726378" class="anchor"><span id="_Toc362208557" class="anchor"><span id="_Toc362208873" class="anchor"><span id="_Toc362260392" class="anchor"><span id="_Toc362260707" class="anchor"><span id="_Toc362725993" class="anchor"><span id="_Toc362726379" class="anchor"><span id="_Toc362208558" class="anchor"><span id="_Toc362208874" class="anchor"><span id="_Toc362260393" class="anchor"><span id="_Toc362260708" class="anchor"><span id="_Toc362725994" class="anchor"><span id="_Toc362726380" class="anchor"><span id="_Toc362208559" class="anchor"><span id="_Toc362208875" class="anchor"><span id="_Toc362260394" class="anchor"><span id="_Toc362260709" class="anchor"><span id="_Toc362725995" class="anchor"><span id="_Toc362726381" class="anchor"><span id="_Toc362208560" class="anchor"><span id="_Toc362208876" class="anchor"><span id="_Toc362260395" class="anchor"><span id="_Toc362260710" class="anchor"><span id="_Toc362725996" class="anchor"><span id="_Toc362726382" class="anchor"><span id="_Toc362208561" class="anchor"><span id="_Toc362208877" class="anchor"><span id="_Toc362260396" class="anchor"><span id="_Toc362260711" class="anchor"><span id="_Toc362725997" class="anchor"><span id="_Toc362726383" class="anchor"><span id="_Toc362208562" class="anchor"><span id="_Toc362208878" class="anchor"><span id="_Toc362260397" class="anchor"><span id="_Toc362260712" class="anchor"><span id="_Toc362725998" class="anchor"><span id="_Toc362726384" class="anchor"><span id="_Toc362208563" class="anchor"><span id="_Toc362208879" class="anchor"><span id="_Toc362260398" class="anchor"><span id="_Toc362260713" class="anchor"><span id="_Toc362725999" class="anchor"><span id="_Toc362726385" class="anchor"><span id="_Toc362208564" class="anchor"><span id="_Toc362208880" class="anchor"><span id="_Toc362260399" class="anchor"><span id="_Toc362260714" class="anchor"><span id="_Toc362726000" class="anchor"><span id="_Toc362726386" class="anchor"><span id="_Toc362208565" class="anchor"><span id="_Toc362208881" class="anchor"><span id="_Toc362260400" class="anchor"><span id="_Toc362260715" class="anchor"><span id="_Toc362726001" class="anchor"><span id="_Toc362726387" class="anchor"><span id="_Toc362208566" class="anchor"><span id="_Toc362208882" class="anchor"><span id="_Toc362260401" class="anchor"><span id="_Toc362260716" class="anchor"><span id="_Toc362726002" class="anchor"><span id="_Toc362726388" class="anchor"><span id="_Toc362208567" class="anchor"><span id="_Toc362208883" class="anchor"><span id="_Toc362260402" class="anchor"><span id="_Toc362260717" class="anchor"><span id="_Toc362726003" class="anchor"><span id="_Toc362726389" class="anchor"><span id="_Toc362208568" class="anchor"><span id="_Toc362208884" class="anchor"><span id="_Toc362260403" class="anchor"><span id="_Toc362260718" class="anchor"><span id="_Toc362726004" class="anchor"><span id="_Toc362726390" class="anchor"><span id="_Toc362208569" class="anchor"><span id="_Toc362208885" class="anchor"><span id="_Toc362260404" class="anchor"><span id="_Toc362260719" class="anchor"><span id="_Toc362726005" class="anchor"><span id="_Toc362726391" class="anchor"><span id="_Toc362208570" class="anchor"><span id="_Toc362208886" class="anchor"><span id="_Toc362260405" class="anchor"><span id="_Toc362260720" class="anchor"><span id="_Toc362726006" class="anchor"><span id="_Toc362726392" class="anchor"><span id="_Toc362208571" class="anchor"><span id="_Toc362208887" class="anchor"><span id="_Toc362260406" class="anchor"><span id="_Toc362260721" class="anchor"><span id="_Toc362726007" class="anchor"><span id="_Toc362726393" class="anchor"><span id="_Toc362208572" class="anchor"><span id="_Toc362208888" class="anchor"><span id="_Toc362260407" class="anchor"><span id="_Toc362260722" class="anchor"><span id="_Toc362726008" class="anchor"><span id="_Toc362726394" class="anchor"><span id="_Toc362208573" class="anchor"><span id="_Toc362208889" class="anchor"><span id="_Toc362260408" class="anchor"><span id="_Toc362260723" class="anchor"><span id="_Toc362726009" class="anchor"><span id="_Toc362726395" class="anchor"><span id="_Toc362208574" class="anchor"><span id="_Toc362208890" class="anchor"><span id="_Toc362260409" class="anchor"><span id="_Toc362260724" class="anchor"><span id="_Toc362726010" class="anchor"><span id="_Toc362726396" class="anchor"><span id="_Toc362208575" class="anchor"><span id="_Toc362208891" class="anchor"><span id="_Toc362260410" class="anchor"><span id="_Toc362260725" class="anchor"><span id="_Toc362726011" class="anchor"><span id="_Toc362726397" class="anchor"><span id="_Toc362208576" class="anchor"><span id="_Toc362208892" class="anchor"><span id="_Toc362260411" class="anchor"><span id="_Toc362260726" class="anchor"><span id="_Toc362726012" class="anchor"><span id="_Toc362726398" class="anchor"><span id="_Toc362208577" class="anchor"><span id="_Toc362208893" class="anchor"><span id="_Toc362260412" class="anchor"><span id="_Toc362260727" class="anchor"><span id="_Toc362726013" class="anchor"><span id="_Toc362726399" class="anchor"><span id="_Toc362208578" class="anchor"><span id="_Toc362208894" class="anchor"><span id="_Toc362260413" class="anchor"><span id="_Toc362260728" class="anchor"><span id="_Toc362726014" class="anchor"><span id="_Toc362726400" class="anchor"><span id="_Toc362208579" class="anchor"><span id="_Toc362208895" class="anchor"><span id="_Toc362260414" class="anchor"><span id="_Toc362260729" class="anchor"><span id="_Toc362726015" class="anchor"><span id="_Toc362726401" class="anchor"><span id="_Toc362208580" class="anchor"><span id="_Toc362208896" class="anchor"><span id="_Toc362260415" class="anchor"><span id="_Toc362260730" class="anchor"><span id="_Toc362726016" class="anchor"><span id="_Toc362726402" class="anchor"><span id="_Toc362208581" class="anchor"><span id="_Toc362208897" class="anchor"><span id="_Toc362260416" class="anchor"><span id="_Toc362260731" class="anchor"><span id="_Toc362726017" class="anchor"><span id="_Toc362726403" class="anchor"><span id="_Toc362208582" class="anchor"><span id="_Toc362208898" class="anchor"><span id="_Toc362260417" class="anchor"><span id="_Toc362260732" class="anchor"><span id="_Toc362726018" class="anchor"><span id="_Toc362726404" class="anchor"><span id="_Toc362208583" class="anchor"><span id="_Toc362208899" class="anchor"><span id="_Toc362260418" class="anchor"><span id="_Toc362260733" class="anchor"><span id="_Toc362726019" class="anchor"><span id="_Toc362726405" class="anchor"><span id="_Toc362208584" class="anchor"><span id="_Toc362208900" class="anchor"><span id="_Toc362260419" class="anchor"><span id="_Toc362260734" class="anchor"><span id="_Toc362726020" class="anchor"><span id="_Toc362726406" class="anchor"><span id="_Toc362208585" class="anchor"><span id="_Toc362208901" class="anchor"><span id="_Toc362260420" class="anchor"><span id="_Toc362260735" class="anchor"><span id="_Toc362726021" class="anchor"><span id="_Toc362726407" class="anchor"><span id="_Toc362208586" class="anchor"><span id="_Toc362208902" class="anchor"><span id="_Toc362260421" class="anchor"><span id="_Toc362260736" class="anchor"><span id="_Toc362726022" class="anchor"><span id="_Toc362726408" class="anchor"><span id="_Toc362208587" class="anchor"><span id="_Toc362208903" class="anchor"><span id="_Toc362260422" class="anchor"><span id="_Toc362260737" class="anchor"><span id="_Toc362726023" class="anchor"><span id="_Toc362726409" class="anchor"><span id="_Toc362208588" class="anchor"><span id="_Toc362208904" class="anchor"><span id="_Toc362260423" class="anchor"><span id="_Toc362260738" class="anchor"><span id="_Toc362726024" class="anchor"><span id="_Toc362726410" class="anchor"><span id="_Toc362208589" class="anchor"><span id="_Toc362208905" class="anchor"><span id="_Toc362260424" class="anchor"><span id="_Toc362260739" class="anchor"><span id="_Toc362726025" class="anchor"><span id="_Toc362726411" class="anchor"><span id="_Toc362208590" class="anchor"><span id="_Toc362208906" class="anchor"><span id="_Toc362260425" class="anchor"><span id="_Toc362260740" class="anchor"><span id="_Toc362726026" class="anchor"><span id="_Toc362726412" class="anchor"><span id="_Toc361668102" class="anchor"><span id="_Toc362208591" class="anchor"><span id="_Toc362208907" class="anchor"><span id="_Toc362260426" class="anchor"><span id="_Toc362260741" class="anchor"><span id="_Toc362726027" class="anchor"><span id="_Toc362726413" class="anchor"><span id="_Toc361668103" class="anchor"><span id="_Toc362208592" class="anchor"><span id="_Toc362208908" class="anchor"><span id="_Toc362260427" class="anchor"><span id="_Toc362260742" class="anchor"><span id="_Toc362726028" class="anchor"><span id="_Toc362726414" class="anchor"><span id="_Toc361668104" class="anchor"><span id="_Toc362208593" class="anchor"><span id="_Toc362208909" class="anchor"><span id="_Toc362260428" class="anchor"><span id="_Toc362260743" class="anchor"><span id="_Toc362726029" class="anchor"><span id="_Toc362726415" class="anchor"><span id="_Toc361668105" class="anchor"><span id="_Toc362208594" class="anchor"><span id="_Toc362208910" class="anchor"><span id="_Toc362260429" class="anchor"><span id="_Toc362260744" class="anchor"><span id="_Toc362726030" class="anchor"><span id="_Toc362726416" class="anchor"><span id="_Toc362208595" class="anchor"><span id="_Toc362208911" class="anchor"><span id="_Toc362260430" class="anchor"><span id="_Toc362260745" class="anchor"><span id="_Toc362726031" class="anchor"><span id="_Toc362726417" class="anchor"><span id="_Toc362208596" class="anchor"><span id="_Toc362208912" class="anchor"><span id="_Toc362260431" class="anchor"><span id="_Toc362260746" class="anchor"><span id="_Toc362726032" class="anchor"><span id="_Toc362726418" class="anchor"><span id="_Toc362208597" class="anchor"><span id="_Toc362208913" class="anchor"><span id="_Toc362260432" class="anchor"><span id="_Toc362260747" class="anchor"><span id="_Toc362726033" class="anchor"><span id="_Toc362726419" class="anchor"><span id="_Toc362208598" class="anchor"><span id="_Toc362208914" class="anchor"><span id="_Toc362260433" class="anchor"><span id="_Toc362260748" class="anchor"><span id="_Toc362726034" class="anchor"><span id="_Toc362726420" class="anchor"><span id="_Toc362208599" class="anchor"><span id="_Toc362208915" class="anchor"><span id="_Toc362260434" class="anchor"><span id="_Toc362260749" class="anchor"><span id="_Toc362726035" class="anchor"><span id="_Toc362726421" class="anchor"><span id="_Toc362208600" class="anchor"><span id="_Toc362208916" class="anchor"><span id="_Toc362260435" class="anchor"><span id="_Toc362260750" class="anchor"><span id="_Toc362726036" class="anchor"><span id="_Toc362726422" class="anchor"><span id="_Toc362208601" class="anchor"><span id="_Toc362208917" class="anchor"><span id="_Toc362260436" class="anchor"><span id="_Toc362260751" class="anchor"><span id="_Toc362726037" class="anchor"><span id="_Toc362726423" class="anchor"><span id="_Toc362208602" class="anchor"><span id="_Toc362208918" class="anchor"><span id="_Toc362260437" class="anchor"><span id="_Toc362260752" class="anchor"><span id="_Toc362726038" class="anchor"><span id="_Toc362726424" class="anchor"><span id="_Toc362208603" class="anchor"><span id="_Toc362208919" class="anchor"><span id="_Toc362260438" class="anchor"><span id="_Toc362260753" class="anchor"><span id="_Toc362726039" class="anchor"><span id="_Toc362726425" class="anchor"><span id="_Toc362208604" class="anchor"><span id="_Toc362208920" class="anchor"><span id="_Toc362260439" class="anchor"><span id="_Toc362260754" class="anchor"><span id="_Toc362726040" class="anchor"><span id="_Toc362726426" class="anchor"><span id="_Toc362208605" class="anchor"><span id="_Toc362208921" class="anchor"><span id="_Toc362260440" class="anchor"><span id="_Toc362260755" class="anchor"><span id="_Toc362726041" class="anchor"><span id="_Toc362726427" class="anchor"><span id="_Toc362208606" class="anchor"><span id="_Toc362208922" class="anchor"><span id="_Toc362260441" class="anchor"><span id="_Toc362260756" class="anchor"><span id="_Toc362726042" class="anchor"><span id="_Toc362726428" class="anchor"><span id="_Toc362208607" class="anchor"><span id="_Toc362208923" class="anchor"><span id="_Toc362260442" class="anchor"><span id="_Toc362260757" class="anchor"><span id="_Toc362726043" class="anchor"><span id="_Toc362726429" class="anchor"><span id="_Toc362208608" class="anchor"><span id="_Toc362208924" class="anchor"><span id="_Toc362260443" class="anchor"><span id="_Toc362260758" class="anchor"><span id="_Toc362726044" class="anchor"><span id="_Toc362726430" class="anchor"><span id="_Toc362208609" class="anchor"><span id="_Toc362208925" class="anchor"><span id="_Toc362260444" class="anchor"><span id="_Toc362260759" class="anchor"><span id="_Toc362726045" class="anchor"><span id="_Toc362726431" class="anchor"><span id="_Toc362208610" class="anchor"><span id="_Toc362208926" class="anchor"><span id="_Toc362260445" class="anchor"><span id="_Toc362260760" class="anchor"><span id="_Toc362726046" class="anchor"><span id="_Toc362726432" class="anchor"><span id="_Toc362208611" class="anchor"><span id="_Toc362208927" class="anchor"><span id="_Toc362260446" class="anchor"><span id="_Toc362260761" class="anchor"><span id="_Toc362726047" class="anchor"><span id="_Toc362726433" class="anchor"><span id="_Toc362208612" class="anchor"><span id="_Toc362208928" class="anchor"><span id="_Toc362260447" class="anchor"><span id="_Toc362260762" class="anchor"><span id="_Toc362726048" class="anchor"><span id="_Toc362726434" class="anchor"><span id="_Toc362208613" class="anchor"><span id="_Toc362208929" class="anchor"><span id="_Toc362260448" class="anchor"><span id="_Toc362260763" class="anchor"><span id="_Toc362726049" class="anchor"><span id="_Toc362726435" class="anchor"><span id="_Toc362208614" class="anchor"><span id="_Toc362208930" class="anchor"><span id="_Toc362260449" class="anchor"><span id="_Toc362260764" class="anchor"><span id="_Toc362726050" class="anchor"><span id="_Toc362726436" class="anchor"><span id="_Toc362208615" class="anchor"><span id="_Toc362208931" class="anchor"><span id="_Toc362260450" class="anchor"><span id="_Toc362260765" class="anchor"><span id="_Toc362726051" class="anchor"><span id="_Toc362726437" class="anchor"><span id="_Toc362208616" class="anchor"><span id="_Toc362208932" class="anchor"><span id="_Toc362260451" class="anchor"><span id="_Toc362260766" class="anchor"><span id="_Toc362726052" class="anchor"><span id="_Toc362726438" class="anchor"><span id="_Toc362208617" class="anchor"><span id="_Toc362208933" class="anchor"><span id="_Toc362260452" class="anchor"><span id="_Toc362260767" class="anchor"><span id="_Toc362726053" class="anchor"><span id="_Toc362726439" class="anchor"><span id="_Toc362208618" class="anchor"><span id="_Toc362208934" class="anchor"><span id="_Toc362260453" class="anchor"><span id="_Toc362260768" class="anchor"><span id="_Toc362726054" class="anchor"><span id="_Toc362726440" class="anchor"><span id="_Toc362208619" class="anchor"><span id="_Toc362208935" class="anchor"><span id="_Toc362260454" class="anchor"><span id="_Toc362260769" class="anchor"><span id="_Toc362726055" class="anchor"><span id="_Toc362726441" class="anchor"><span id="_Toc362208620" class="anchor"><span id="_Toc362208936" class="anchor"><span id="_Toc362260455" class="anchor"><span id="_Toc362260770" class="anchor"><span id="_Toc362726056" class="anchor"><span id="_Toc362726442" class="anchor"><span id="_Toc362208621" class="anchor"><span id="_Toc362208937" class="anchor"><span id="_Toc362260456" class="anchor"><span id="_Toc362260771" class="anchor"><span id="_Toc362726057" class="anchor"><span id="_Toc362726443" class="anchor"><span id="_Toc362208622" class="anchor"><span id="_Toc362208938" class="anchor"><span id="_Toc362260457" class="anchor"><span id="_Toc362260772" class="anchor"><span id="_Toc362726058" class="anchor"><span id="_Toc362726444" class="anchor"><span id="_Toc362208623" class="anchor"><span id="_Toc362208939" class="anchor"><span id="_Toc362260458" class="anchor"><span id="_Toc362260773" class="anchor"><span id="_Toc362726059" class="anchor"><span id="_Toc362726445" class="anchor"><span id="_Toc362208624" class="anchor"><span id="_Toc362208940" class="anchor"><span id="_Toc362260459" class="anchor"><span id="_Toc362260774" class="anchor"><span id="_Toc362726060" class="anchor"><span id="_Toc362726446" class="anchor"><span id="_Toc362208625" class="anchor"><span id="_Toc362208941" class="anchor"><span id="_Toc362260460" class="anchor"><span id="_Toc362260775" class="anchor"><span id="_Toc362726061" class="anchor"><span id="_Toc362726447" class="anchor"><span id="_Toc362208626" class="anchor"><span id="_Toc362208942" class="anchor"><span id="_Toc362260461" class="anchor"><span id="_Toc362260776" class="anchor"><span id="_Toc362726062" class="anchor"><span id="_Toc362726448" class="anchor"><span id="_Toc362208627" class="anchor"><span id="_Toc362208943" class="anchor"><span id="_Toc362260462" class="anchor"><span id="_Toc362260777" class="anchor"><span id="_Toc362726063" class="anchor"><span id="_Toc362726449" class="anchor"><span id="_Toc362208628" class="anchor"><span id="_Toc362208944" class="anchor"><span id="_Toc362260463" class="anchor"><span id="_Toc362260778" class="anchor"><span id="_Toc362726064" class="anchor"><span id="_Toc362726450" class="anchor"><span id="_Toc362208629" class="anchor"><span id="_Toc362208945" class="anchor"><span id="_Toc362260464" class="anchor"><span id="_Toc362260779" class="anchor"><span id="_Toc362726065" class="anchor"><span id="_Toc362726451" class="anchor"><span id="_Toc362208630" class="anchor"><span id="_Toc362208946" class="anchor"><span id="_Toc362260465" class="anchor"><span id="_Toc362260780" class="anchor"><span id="_Toc362726066" class="anchor"><span id="_Toc362726452" class="anchor"><span id="_Toc362208631" class="anchor"><span id="_Toc362208947" class="anchor"><span id="_Toc362260466" class="anchor"><span id="_Toc362260781" class="anchor"><span id="_Toc362726067" class="anchor"><span id="_Toc362726453" class="anchor"><span id="_Toc362208632" class="anchor"><span id="_Toc362208948" class="anchor"><span id="_Toc362260467" class="anchor"><span id="_Toc362260782" class="anchor"><span id="_Toc362726068" class="anchor"><span id="_Toc362726454" class="anchor"><span id="_Toc362208633" class="anchor"><span id="_Toc362208949" class="anchor"><span id="_Toc362260468" class="anchor"><span id="_Toc362260783" class="anchor"><span id="_Toc362726069" class="anchor"><span id="_Toc362726455" class="anchor"><span id="_Toc362208634" class="anchor"><span id="_Toc362208950" class="anchor"><span id="_Toc362260469" class="anchor"><span id="_Toc362260784" class="anchor"><span id="_Toc362726070" class="anchor"><span id="_Toc362726456" class="anchor"><span id="_Toc362208635" class="anchor"><span id="_Toc362208951" class="anchor"><span id="_Toc362260470" class="anchor"><span id="_Toc362260785" class="anchor"><span id="_Toc362726071" class="anchor"><span id="_Toc362726457" class="anchor"><span id="_Toc362208636" class="anchor"><span id="_Toc362208952" class="anchor"><span id="_Toc362260471" class="anchor"><span id="_Toc362260786" class="anchor"><span id="_Toc362726072" class="anchor"><span id="_Toc362726458" class="anchor"><span id="_Toc362208637" class="anchor"><span id="_Toc362208953" class="anchor"><span id="_Toc362260472" class="anchor"><span id="_Toc362260787" class="anchor"><span id="_Toc362726073" class="anchor"><span id="_Toc362726459" class="anchor"><span id="_Toc362208638" class="anchor"><span id="_Toc362208954" class="anchor"><span id="_Toc362260473" class="anchor"><span id="_Toc362260788" class="anchor"><span id="_Toc362726074" class="anchor"><span id="_Toc362726460" class="anchor"><span id="_Toc362208639" class="anchor"><span id="_Toc362208955" class="anchor"><span id="_Toc362260474" class="anchor"><span id="_Toc362260789" class="anchor"><span id="_Toc362726075" class="anchor"><span id="_Toc362726461" class="anchor"><span id="_Toc362208640" class="anchor"><span id="_Toc362208956" class="anchor"><span id="_Toc362260475" class="anchor"><span id="_Toc362260790" class="anchor"><span id="_Toc362726076" class="anchor"><span id="_Toc362726462" class="anchor"><span id="_Toc362208641" class="anchor"><span id="_Toc362208957" class="anchor"><span id="_Toc362260476" class="anchor"><span id="_Toc362260791" class="anchor"><span id="_Toc362726077" class="anchor"><span id="_Toc362726463" class="anchor"><span id="_Toc362208642" class="anchor"><span id="_Toc362208958" class="anchor"><span id="_Toc362260477" class="anchor"><span id="_Toc362260792" class="anchor"><span id="_Toc362726078" class="anchor"><span id="_Toc362726464" class="anchor"><span id="_Toc362208643" class="anchor"><span id="_Toc362208959" class="anchor"><span id="_Toc362260478" class="anchor"><span id="_Toc362260793" class="anchor"><span id="_Toc362726079" class="anchor"><span id="_Toc362726465" class="anchor"><span id="_Toc362208644" class="anchor"><span id="_Toc362208960" class="anchor"><span id="_Toc362260479" class="anchor"><span id="_Toc362260794" class="anchor"><span id="_Toc362726080" class="anchor"><span id="_Toc362726466" class="anchor"><span id="_Toc362208645" class="anchor"><span id="_Toc362208961" class="anchor"><span id="_Toc362260480" class="anchor"><span id="_Toc362260795" class="anchor"><span id="_Toc362726081" class="anchor"><span id="_Toc362726467" class="anchor"><span id="_Toc362208646" class="anchor"><span id="_Toc362208962" class="anchor"><span id="_Toc362260481" class="anchor"><span id="_Toc362260796" class="anchor"><span id="_Toc362726082" class="anchor"><span id="_Toc362726468" class="anchor"><span id="_Toc362208647" class="anchor"><span id="_Toc362208963" class="anchor"><span id="_Toc362260482" class="anchor"><span id="_Toc362260797" class="anchor"><span id="_Toc362726083" class="anchor"><span id="_Toc362726469" class="anchor"><span id="_Toc362208648" class="anchor"><span id="_Toc362208964" class="anchor"><span id="_Toc362260483" class="anchor"><span id="_Toc362260798" class="anchor"><span id="_Toc362726084" class="anchor"><span id="_Toc362726470" class="anchor"><span id="_Toc362208649" class="anchor"><span id="_Toc362208965" class="anchor"><span id="_Toc362260484" class="anchor"><span id="_Toc362260799" class="anchor"><span id="_Toc362726085" class="anchor"><span id="_Toc362726471" class="anchor"><span id="_Toc362208650" class="anchor"><span id="_Toc362208966" class="anchor"><span id="_Toc362260485" class="anchor"><span id="_Toc362260800" class="anchor"><span id="_Toc362726086" class="anchor"><span id="_Toc362726472" class="anchor"><span id="_Toc362208651" class="anchor"><span id="_Toc362208967" class="anchor"><span id="_Toc362260486" class="anchor"><span id="_Toc362260801" class="anchor"><span id="_Toc362726087" class="anchor"><span id="_Toc362726473" class="anchor"><span id="_Toc362208652" class="anchor"><span id="_Toc362208968" class="anchor"><span id="_Toc362260487" class="anchor"><span id="_Toc362260802" class="anchor"><span id="_Toc362726088" class="anchor"><span id="_Toc362726474" class="anchor"><span id="_Toc362208653" class="anchor"><span id="_Toc362208969" class="anchor"><span id="_Toc362260488" class="anchor"><span id="_Toc362260803" class="anchor"><span id="_Toc362726089" class="anchor"><span id="_Toc362726475" class="anchor"><span id="_Toc362208654" class="anchor"><span id="_Toc362208970" class="anchor"><span id="_Toc362260489" class="anchor"><span id="_Toc362260804" class="anchor"><span id="_Toc362726090" class="anchor"><span id="_Toc362726476" class="anchor"><span id="_Toc362208655" class="anchor"><span id="_Toc362208971" class="anchor"><span id="_Toc362260490" class="anchor"><span id="_Toc362260805" class="anchor"><span id="_Toc362726091" class="anchor"><span id="_Toc362726477" class="anchor"><span id="_Toc362208656" class="anchor"><span id="_Toc362208972" class="anchor"><span id="_Toc362260491" class="anchor"><span id="_Toc362260806" class="anchor"><span id="_Toc362726092" class="anchor"><span id="_Toc362726478" class="anchor"><span id="_Toc362208657" class="anchor"><span id="_Toc362208973" class="anchor"><span id="_Toc362260492" class="anchor"><span id="_Toc362260807" class="anchor"><span id="_Toc362726093" class="anchor"><span id="_Toc362726479" class="anchor"><span id="_Toc362208658" class="anchor"><span id="_Toc362208974" class="anchor"><span id="_Toc362260493" class="anchor"><span id="_Toc362260808" class="anchor"><span id="_Toc362726094" class="anchor"><span id="_Toc362726480" class="anchor"><span id="_Toc362208659" class="anchor"><span id="_Toc362208975" class="anchor"><span id="_Toc362260494" class="anchor"><span id="_Toc362260809" class="anchor"><span id="_Toc362726095" class="anchor"><span id="_Toc362726481" class="anchor"><span id="_Toc362208660" class="anchor"><span id="_Toc362208976" class="anchor"><span id="_Toc362260495" class="anchor"><span id="_Toc362260810" class="anchor"><span id="_Toc362726096" class="anchor"><span id="_Toc362726482" class="anchor"><span id="_Toc362208661" class="anchor"><span id="_Toc362208977" class="anchor"><span id="_Toc362260496" class="anchor"><span id="_Toc362260811" class="anchor"><span id="_Toc362726097" class="anchor"><span id="_Toc362726483" class="anchor"><span id="_Toc362208662" class="anchor"><span id="_Toc362208978" class="anchor"><span id="_Toc362260497" class="anchor"><span id="_Toc362260812" class="anchor"><span id="_Toc362726098" class="anchor"><span id="_Toc362726484" class="anchor"><span id="_Toc362208663" class="anchor"><span id="_Toc362208979" class="anchor"><span id="_Toc362260498" class="anchor"><span id="_Toc362260813" class="anchor"><span id="_Toc362726099" class="anchor"><span id="_Toc362726485" class="anchor"><span id="_Toc362208664" class="anchor"><span id="_Toc362208980" class="anchor"><span id="_Toc362260499" class="anchor"><span id="_Toc362260814" class="anchor"><span id="_Toc362726100" class="anchor"><span id="_Toc362726486" class="anchor"><span id="_Toc362208665" class="anchor"><span id="_Toc362208981" class="anchor"><span id="_Toc362260500" class="anchor"><span id="_Toc362260815" class="anchor"><span id="_Toc362726101" class="anchor"><span id="_Toc362726487" class="anchor"><span id="_Toc362208666" class="anchor"><span id="_Toc362208982" class="anchor"><span id="_Toc362260501" class="anchor"><span id="_Toc362260816" class="anchor"><span id="_Toc362726102" class="anchor"><span id="_Toc362726488" class="anchor"><span id="_Toc361668110" class="anchor"><span id="_Toc362208667" class="anchor"><span id="_Toc362208983" class="anchor"><span id="_Toc362260502" class="anchor"><span id="_Toc362260817" class="anchor"><span id="_Toc362726103" class="anchor"><span id="_Toc362726489" class="anchor"><span id="_Toc362208668" class="anchor"><span id="_Toc362208984" class="anchor"><span id="_Toc362260503" class="anchor"><span id="_Toc362260818" class="anchor"><span id="_Toc362726104" class="anchor"><span id="_Toc362726490" class="anchor"><span id="_Toc362208669" class="anchor"><span id="_Toc362208985" class="anchor"><span id="_Toc362260504" class="anchor"><span id="_Toc362260819" class="anchor"><span id="_Toc362726105" class="anchor"><span id="_Toc362726491" class="anchor"><span id="_Toc362208670" class="anchor"><span id="_Toc362208986" class="anchor"><span id="_Toc362260505" class="anchor"><span id="_Toc362260820" class="anchor"><span id="_Toc362726106" class="anchor"><span id="_Toc362726492" class="anchor"><span id="_Toc362208671" class="anchor"><span id="_Toc362208987" class="anchor"><span id="_Toc362260506" class="anchor"><span id="_Toc362260821" class="anchor"><span id="_Toc362726107" class="anchor"><span id="_Toc362726493" class="anchor"><span id="_Toc362208672" class="anchor"><span id="_Toc362208988" class="anchor"><span id="_Toc362260507" class="anchor"><span id="_Toc362260822" class="anchor"><span id="_Toc362726108" class="anchor"><span id="_Toc362726494" class="anchor"><span id="_Toc362208673" class="anchor"><span id="_Toc362208989" class="anchor"><span id="_Toc362260508" class="anchor"><span id="_Toc362260823" class="anchor"><span id="_Toc362726109" class="anchor"><span id="_Toc362726495" class="anchor"><span id="_Toc362208674" class="anchor"><span id="_Toc362208990" class="anchor"><span id="_Toc362260509" class="anchor"><span id="_Toc362260824" class="anchor"><span id="_Toc362726110" class="anchor"><span id="_Toc362726496" class="anchor"><span id="_Toc362208675" class="anchor"><span id="_Toc362208991" class="anchor"><span id="_Toc362260510" class="anchor"><span id="_Toc362260825" class="anchor"><span id="_Toc362726111" class="anchor"><span id="_Toc362726497" class="anchor"><span id="_Installed_Files_and" class="anchor"><span id="_Setup_SAN_storage" class="anchor"><span id="_Set_Up_SAN" class="anchor"><span id="_Installed_Files_and_1" class="anchor"><span id="_Toc361668112" class="anchor"><span id="_Toc362208676" class="anchor"><span id="_Toc362208992" class="anchor"><span id="_Toc362260511" class="anchor"><span id="_Toc362260826" class="anchor"><span id="_Toc362726112" class="anchor"><span id="_Toc362726498" class="anchor"><span id="_Toc361668113" class="anchor"><span id="_Toc362208677" class="anchor"><span id="_Toc362208993" class="anchor"><span id="_Toc362260512" class="anchor"><span id="_Toc362260827" class="anchor"><span id="_Toc362726113" class="anchor"><span id="_Toc362726499" class="anchor"><span id="_Toc361668114" class="anchor"><span id="_Toc362208678" class="anchor"><span id="_Toc362208994" class="anchor"><span id="_Toc362260513" class="anchor"><span id="_Toc362260828" class="anchor"><span id="_Toc362726114" class="anchor"><span id="_Toc362726500" class="anchor"><span id="_Toc361668115" class="anchor"><span id="_Toc362208679" class="anchor"><span id="_Toc362208995" class="anchor"><span id="_Toc362260514" class="anchor"><span id="_Toc362260829" class="anchor"><span id="_Toc362726115" class="anchor"><span id="_Toc362726501" class="anchor"><span id="_Toc361668116" class="anchor"><span id="_Toc362208680" class="anchor"><span id="_Toc362208996" class="anchor"><span id="_Toc362260515" class="anchor"><span id="_Toc362260830" class="anchor"><span id="_Toc362726116" class="anchor"><span id="_Toc362726502" class="anchor"><span id="_Toc361668117" class="anchor"><span id="_Toc362208681" class="anchor"><span id="_Toc362208997" class="anchor"><span id="_Toc362260516" class="anchor"><span id="_Toc362260831" class="anchor"><span id="_Toc362726117" class="anchor"><span id="_Toc362726503" class="anchor"><span id="_Toc361668118" class="anchor"><span id="_Toc362208682" class="anchor"><span id="_Toc362208998" class="anchor"><span id="_Toc362260517" class="anchor"><span id="_Toc362260832" class="anchor"><span id="_Toc362726118" class="anchor"><span id="_Toc362726504" class="anchor"><span id="_Toc361668119" class="anchor"><span id="_Toc362208683" class="anchor"><span id="_Toc362208999" class="anchor"><span id="_Toc362260518" class="anchor"><span id="_Toc362260833" class="anchor"><span id="_Toc362726119" class="anchor"><span id="_Toc362726505" class="anchor"><span id="_Toc361668120" class="anchor"><span id="_Toc362208684" class="anchor"><span id="_Toc362209000" class="anchor"><span id="_Toc362260519" class="anchor"><span id="_Toc362260834" class="anchor"><span id="_Toc362726120" class="anchor"><span id="_Toc362726506" class="anchor"><span id="_Toc361668121" class="anchor"><span id="_Toc362208685" class="anchor"><span id="_Toc362209001" class="anchor"><span id="_Toc362260520" class="anchor"><span id="_Toc362260835" class="anchor"><span id="_Toc362726121" class="anchor"><span id="_Toc362726507" class="anchor"><span id="_Toc361668122" class="anchor"><span id="_Toc362208686" class="anchor"><span id="_Toc362209002" class="anchor"><span id="_Toc362260521" class="anchor"><span id="_Toc362260836" class="anchor"><span id="_Toc362726122" class="anchor"><span id="_Toc362726508" class="anchor"><span id="_Toc361668123" class="anchor"><span id="_Toc362208687" class="anchor"><span id="_Toc362209003" class="anchor"><span id="_Toc362260522" class="anchor"><span id="_Toc362260837" class="anchor"><span id="_Toc362726123" class="anchor"><span id="_Toc362726509" class="anchor"><span id="_Toc361668124" class="anchor"><span id="_Toc362208688" class="anchor"><span id="_Toc362209004" class="anchor"><span id="_Toc362260523" class="anchor"><span id="_Toc362260838" class="anchor"><span id="_Toc362726124" class="anchor"><span id="_Toc362726510" class="anchor"><span id="_Toc361668125" class="anchor"><span id="_Toc362208689" class="anchor"><span id="_Toc362209005" class="anchor"><span id="_Toc362260524" class="anchor"><span id="_Toc362260839" class="anchor"><span id="_Toc362726125" class="anchor"><span id="_Toc362726511" class="anchor"><span id="_Toc362208690" class="anchor"><span id="_Toc362209006" class="anchor"><span id="_Toc362260525" class="anchor"><span id="_Toc362260840" class="anchor"><span id="_Toc362726126" class="anchor"><span id="_Toc362726512" class="anchor"><span id="_Toc362208691" class="anchor"><span id="_Toc362209007" class="anchor"><span id="_Toc362260526" class="anchor"><span id="_Toc362260841" class="anchor"><span id="_Toc362726127" class="anchor"><span id="_Toc362726513" class="anchor"><span id="_Toc362208692" class="anchor"><span id="_Toc362209008" class="anchor"><span id="_Toc362260527" class="anchor"><span id="_Toc362260842" class="anchor"><span id="_Toc362726128" class="anchor"><span id="_Toc362726514" class="anchor"><span id="_Toc362208693" class="anchor"><span id="_Toc362209009" class="anchor"><span id="_Toc362260528" class="anchor"><span id="_Toc362260843" class="anchor"><span id="_Toc362726129" class="anchor"><span id="_Toc362726515" class="anchor"><span id="_Toc362208701" class="anchor"><span id="_Toc362209017" class="anchor"><span id="_Toc362260536" class="anchor"><span id="_Toc362260851" class="anchor"><span id="_Toc362726137" class="anchor"><span id="_Toc362726523" class="anchor"><span id="_Toc362208702" class="anchor"><span id="_Toc362209018" class="anchor"><span id="_Toc362260537" class="anchor"><span id="_Toc362260852" class="anchor"><span id="_Toc362726138" class="anchor"><span id="_Toc362726524" class="anchor"><span id="_Toc362208703" class="anchor"><span id="_Toc362209019" class="anchor"><span id="_Toc362260538" class="anchor"><span id="_Toc362260853" class="anchor"><span id="_Toc362726139" class="anchor"><span id="_Toc362726525" class="anchor"><span id="_Toc362208704" class="anchor"><span id="_Toc362209020" class="anchor"><span id="_Toc362260539" class="anchor"><span id="_Toc362260854" class="anchor"><span id="_Toc362726140" class="anchor"><span id="_Toc362726526" class="anchor"><span id="_Toc362208705" class="anchor"><span id="_Toc362209021" class="anchor"><span id="_Toc362260540" class="anchor"><span id="_Toc362260855" class="anchor"><span id="_Toc362726141" class="anchor"><span id="_Toc362726527" class="anchor"><span id="_Toc362208706" class="anchor"><span id="_Toc362209022" class="anchor"><span id="_Toc362260541" class="anchor"><span id="_Toc362260856" class="anchor"><span id="_Toc362726142" class="anchor"><span id="_Toc362726528" class="anchor"><span id="_Toc362208707" class="anchor"><span id="_Toc362209023" class="anchor"><span id="_Toc362260542" class="anchor"><span id="_Toc362260857" class="anchor"><span id="_Toc362726143" class="anchor"><span id="_Toc362726529" class="anchor"><span id="_Toc362208708" class="anchor"><span id="_Toc362209024" class="anchor"><span id="_Toc362260543" class="anchor"><span id="_Toc362260858" class="anchor"><span id="_Toc362726144" class="anchor"><span id="_Toc362726530" class="anchor"><span id="_Toc283040548" class="anchor"><span id="_Toc283114621" class="anchor"><span id="_Toc361668129" class="anchor"><span id="_Toc425336666" class="anchor"></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span>Installed Files and Folders
===================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================

The installation process adds the following folders and files to the driver computer at C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\.

![](./media/image3.png)Note

This path may vary based on your installation location.

The &lt;*version\#&gt;* placeholder indicates the installed build of the test suite.

| **File or Folder** | **Description**                                                                                                                                                                                |
|--------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Batch              | Command files you can use to run individual test cases or all test cases.                                                                                                                      |
| Bin                |                                                                                                                                                                                                |
| Docs               | **\[MS-SMB2\].pdf**                                                                                                                                                                            
                                                                                                                                                                                                                      
                      **\[MS-FSRVP\].pdf**                                                                                                                                                                            
                                                                                                                                                                                                                      
                      **\[MS-FSA\].pdf**                                                                                                                                                                              
                                                                                                                                                                                                                      
                      **\[MS-SWN\].pdf**                                                                                                                                                                              
                                                                                                                                                                                                                      
                      **\[MS-DFSC\].pdf**                                                                                                                                                                             
                                                                                                                                                                                                                      
                      **\[MS-RSVD\].pdf**                                                                                                                                                                             
                                                                                                                                                                                                                      
                      **\[MS-SQOS-Preview\].pdf**                                                                                                                                                                     
                                                                                                                                                                                                                      
                      – The version of the technical document that this test suite is based on.                                                                                                                       
                                                                                                                                                                                                                      
                      **FileSharing\_ServerUserGuide.docx** – A user guide that explains how to install and configure the driver computer, the SUT, SAN storage server and the DC, as well as how to run test cases.  
                                                                                                                                                                                                                      
                      **FileSharing\_ServerTestDesignSpecification.docx** – An overview document containing conceptual information about File Sharing environment and test scenario design.                           
                                                                                                                                                                                                                      
                      **MS-SQOS\_ServerTestDesignSpecification.docx -** An overview document containing conceptual information about SQOS test scenario design.                                                       
                                                                                                                                                                                                                      
                      **MS-Auth\_ServerTestDesignSpecification.docx** – An overview document containing conceptual information about the authentication and authorization scenario in File Sharing family protocols.  
                                                                                                                                                                                                                      
                      **MS-FSA\_ServerTestDesignSpecification.docx -** An overview document containing conceptual information about FSA test scenario design.                                                         
                                                                                                                                                                                                                      
                      **ReleaseNotes.txt** – An overview of recent releases.                                                                                                                                          |
| Scripts            | Scripts that are used to set up and configure the driver computer and the SUT, and the DC.                                                                                                     |
| Source             | Test suite source code.                                                                                                                                                                        |
| EULA.rtf           | The End User License Agreement.                                                                                                                                                                |

Additionally the installation process also adds following folders and files to the driver computer at C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\Infrastructure\\src\\, which contains the protocol SDK source code of projects that test suite will referenced.

| **File or Folder** | **Description** |
|--------------------|-----------------|
| ProtoSDK           |                 |

<span id="_Network_Configuration_1" class="anchor"><span id="_Toc425336667" class="anchor"><span id="_Toc361668130" class="anchor"></span></span></span>Configure and Run Test Cases 
=====================================================================================================================================================================================

This section explains how to configure the test suite and how to run test cases.

<span id="_Network_Configuration_2" class="anchor"><span id="_Verify_Connectivity_to" class="anchor"><span id="_Verify_Connectivity_from" class="anchor"><span id="_Configuring_the_Test_1" class="anchor"><span id="_PTF_Configuration_details" class="anchor"><span id="_Configuring_the_Test" class="anchor"><span id="_Toc425336668" class="anchor"><span id="_Toc278822246" class="anchor"><span id="_Toc281907710" class="anchor"><span id="_Toc361668131" class="anchor"><span id="_Toc266454777" class="anchor"></span></span></span></span></span></span></span></span></span></span></span>Configure and Run Test Cases Using Protocol Test Manager
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Protocol Test Manager is a UI tool to help you to configure and run test cases.

You can follow the following steps to use Protocol Test Manager to configure the test suite:

1.  Launch the ProtocolTestManager.msi and install the Protocol Test Manager.

2.  **Select Test Suite:** Open the Protocol Test Manager from the shortcut on the desktop. Click **Configure Wizard** to configure the selected File Sharing test suite.

    ![](./media/image150.png)

3.  After the first click, you can see the **Test Suite Introduction**, which provides some general information about File Sharing test suite:

    ![](./media/image151.png)

4.  Click **Deployment Guide** to see how to do the configuration in Domain or Workgroup Environment

    ![](./media/image152.png)

5.  Click **Next**, “Protocol Test Manager” supports three ways to configure the test suite. Auto-Detection, Manual Configuration and load a saved profile.

    ![](./media/image153.png)

<!-- -->

1.  **Configure the test suite with Run Auto-Detection**:

-   Click **Run Auto-Detection** and navigate to **Auto-Detection** page.

-   For each field in **Auto-Detection** page, you can use the value loaded from ptfconfig file, or change it by assigning an appointed value, and then click **Detect**. If SUT is in WORKGROUP, set **Domain Name** field to the same value as the **Target SUT** field.

    ![](./media/image154.png)

-   When the detection is finished, click **Next** to check the **Detection Result**, such as whether the SUT supports the specified capability or ioctl code. You can check the result by clicking each item.

    ![](./media/image155.png)

-   **Filter Test Cases**: The checked cases are selected automatically, which means that the features tested in these cases are supported by SUT. If the detection result is not supported, the feature is italic. You can also edit the selection on demand.

    ![](./media/image156.png)

1.  **Configure the test suite manually**

-   Click **Do Manual Configuration** and jump to **Filter Test Cases**.

-   **Filter Test Cases**: Select the test cases you want to run.

    ![](./media/image157.png)

1.  **Load Profile**: The Profile includes the information of previously selected test cases and the configuration.

    -   Click **Load Profile**, select an existing profile and jump to **Filter Test Cases**.

        ![](./media/image158.png)

    <!-- -->

    -   **Filter Test Cases**: The checked cases are selected in the profile. You can also edit the selection if needed.

        ![](./media/image159.png)

<!-- -->

1.  **Configure Test Cases**: In this page, you can check the property values which are configured by detection result and user modification.

    ![](./media/image160.png)

2.  **Configure Adapter**: In this page, you can change the SUT control adapter type.

    ![](./media/image161.png)

3.  **Run Selected Test Cases**: Run all the test cases by clicking **Run All**; Select test cases and run them by clicking **Run Selected Test**. If a test case is executed, you can click this test case and the test log of the case is shown in the **Test logs** windows.

    ![](./media/image162.png)

4.  After the test cases execution completed, you can click the hyperlink at the upper-right corner to open the Result folder of the test suite.

    ![](./media/image163.png)

5.  **Export Results**: You can export the test cases from the Export/Import menu, you can also **Save Profile** from this menu.

    ![](./media/image164.png)

![](./media/image3.png)Note

1.  You can drag the seperator between the case list and the log to adjust the width of the window.

2.  You can right-click on the test cases to uncheck all the test cases.

> ![](./media/image165.png)

1.  Protocol Test Manager has a command line interface (ptmcli.exe) which can be used for automation test run.

The command line interface ptmcli.exe is located in the bin folder of the Protocol Test Framework installation folder. To use PTM, you need to provide a profile using -p option.

You can use “ptmcli.exe -h” to show more information.

Configure Test Suite Manually
-----------------------------

This test suite is installed with default configuration settings. You may need to change these settings if you use a customized test environment or if to customize your test runs.

You can configure the test suite for various purposes including, for example, to:

-   Define the settings of the test environment, including computer names and IP addresses.

-   Define the folders and formats used for output from test runs.

To change configuration settings, edit the \*.deployment.ptfconfig files including

-   CommonTestSuite.deployment.ptfconfig

-   FileSharing\_ServerFailoverTestSuite.deployment.ptfconfig

-   MS-DFSC\_ServerTestSuite.deployment.ptfconfig

-   MS-FSRVP\_ServerTestSuite.deployment.ptfconfig

-   MS-SMB2\_ServerTestSuite.deployment.ptfconfig

-   MS-SMB2Model\_ServerTestSuite.deployment.ptfconfig

-   MS-RSVD\_ServerTestSuite.deployment.ptfconfig

-   MS-SQOS\_ServerTestSuite.deployment.ptfconfig

-   Auth\_ServerTestSuite.deployment.ptfconfig

You can find the files in

-   C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\Source\\Server\\TestCode\\Common\\TestSuite\\

-   C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\Source\\Server\\TestCode\\DFSC\\TestSuite

-   C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\ Source\\Server\\TestCode\\FSRVP\\TestSuite

-   C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\ Source\\Server\\TestCode\\ServerFailover\\TestSuite

-   C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\ Source\\Server\\TestCode\\SMB2\\TestSuite

-   C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\ Source\\Server\\TestCode\\SMB2Model\\TestSuite

-   C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\ Source\\Server\\TestCode\\RSVD\\TestSuite

-   C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\ Source\\Server\\TestCode\\SQOS\\TestSuite

-   C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\ Source\\Server\\TestCode\\Auth\\TestSuite

**Note:**

You can also find ptfconfig files at “**C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\Bin**” after test suite installed, which will be used when running batch scripts. However, the batch scripts will fetch the ptfconfig files from “**C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\&lt;version\#&gt;\\ Source\\Server\\TestCode\\&lt;*project*&gt;\\TestSuite**” and override the original config files in Bin folder. So please update the ptfconfig files from this project folder instead of the Bin folder.

### <span id="_Required_Configuration_Settings" class="anchor"><span id="_Toc361668132" class="anchor"><span id="_Toc425336670" class="anchor"></span></span></span>Brief introduction to Configuration Settings

1.  **Common Settings**

> Common settings in “CommonTestSuite.deployment.ptfconfig” are shared by all the sub test suites.

1.  **Settings for file server failover test suite**

    Settings in “FileSharing\_ServerFailoverTestSuite.deployment.ptfconfig” are for the File server failover test suite.

2.  **Settings for DFSC server test suite**

    Settings in “MS-DFSC\_ServerTestSuite.deployment.ptfconfig” are for the DFSC server test suite.

3.  **Settings for FSRVP server test suite**

    Settings in “MS-FSRVP\_ServerTestSuite.deployment.ptfconfig” are for the FSRVP server test suite.

4.  **Settings for SMB2 server test suite**

    Settings in “MS-SMB2\_ServerTestSuite.deployment.ptfconfig” are for the SMB2 server test suite.

5.  **Settings for SMB2 server model test suite**

    Settings in “MS-SMB2Model\_ServerTestSuite.deployment.ptfconfig” are for the SMB2 server model test suite.

6.  **Settings for RSVD server test suite**

    Settings in “MS-RSVD\_ServerTestSuite.deployment.ptfconfig” are for the RSVD server test suite.

7.  **Settings for SQOS server test suite**

    Settings in “MS-SQOS\_ServerTestSuite.deployment.ptfconfig” are for the SQOS server test suite.

8.  **Settings for Auth server test suite**

    Settings in “Auth\_ServerTestSuite.deployment.ptfconfig” are for the Auth test suite.

Please refer to the corresponding \*.deployment.ptfconfig files for configuration details.

<span id="_Enable_FORCE_LEVELII_OPLOCK_on" class="anchor"><span id="_Toc362208713" class="anchor"><span id="_Toc362209029" class="anchor"><span id="_Toc362260548" class="anchor"><span id="_Toc362260863" class="anchor"><span id="_Toc362726149" class="anchor"><span id="_Toc362726535" class="anchor"><span id="_Toc362208714" class="anchor"><span id="_Toc362209030" class="anchor"><span id="_Toc362260549" class="anchor"><span id="_Toc362260864" class="anchor"><span id="_Toc362726150" class="anchor"><span id="_Toc362726536" class="anchor"><span id="_Toc362208715" class="anchor"><span id="_Toc362209031" class="anchor"><span id="_Toc362260550" class="anchor"><span id="_Toc362260865" class="anchor"><span id="_Toc362726151" class="anchor"><span id="_Toc362726537" class="anchor"><span id="_Toc362208716" class="anchor"><span id="_Toc362209032" class="anchor"><span id="_Toc362260551" class="anchor"><span id="_Toc362260866" class="anchor"><span id="_Toc362726152" class="anchor"><span id="_Toc362726538" class="anchor"><span id="_Toc362208717" class="anchor"><span id="_Toc362209033" class="anchor"><span id="_Toc362260552" class="anchor"><span id="_Toc362260867" class="anchor"><span id="_Toc362726153" class="anchor"><span id="_Toc362726539" class="anchor"><span id="_Toc362208718" class="anchor"><span id="_Toc362209034" class="anchor"><span id="_Toc362260553" class="anchor"><span id="_Toc362260868" class="anchor"><span id="_Toc362726154" class="anchor"><span id="_Toc362726540" class="anchor"><span id="_Toc362208719" class="anchor"><span id="_Toc362209035" class="anchor"><span id="_Toc362260554" class="anchor"><span id="_Toc362260869" class="anchor"><span id="_Toc362726155" class="anchor"><span id="_Toc362726541" class="anchor"><span id="_Toc362208720" class="anchor"><span id="_Toc362209036" class="anchor"><span id="_Toc362260555" class="anchor"><span id="_Toc362260870" class="anchor"><span id="_Toc362726156" class="anchor"><span id="_Toc362726542" class="anchor"><span id="_Get_the_Value" class="anchor"><span id="_Get_the_DsaGuid" class="anchor"><span id="_Toc420331833" class="anchor"><span id="_Toc425336671" class="anchor"></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span>Run Test Cases by Batch Script
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

This test suite includes command files that you can use to complete some basic test cases. Each test case verifies the protocol implementation based on a given scenario.

You can find and run these test cases in the following directory:

C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\Batch

You can run these command files at the command prompt, or by selecting and clicking one or more of the files from the directory.

### <span id="_Run_Scenario0_Test" class="anchor"><span id="_Run_Smoke_Test" class="anchor"><span id="_Run_the_Smoke" class="anchor"><span id="_Run_the_BVT" class="anchor"><span id="_Toc278822248" class="anchor"><span id="_Toc281907712" class="anchor"><span id="_Toc361668134" class="anchor"></span></span></span></span></span></span></span> Run the BVT Test

This test suite includes a set of basic tests called "BVT". Together these test cases perform a basic functionality test to evaluate the implementation on SUT machine. Use the following steps to run all BVT test cases.

To run the BVT test in a Domain environment

-   From the desktop of the driver computer, double-click the **Run FileSharing Server-Domain\_BVTTestCases** shortcut. This shortcut is created during the installation process.

Alternatively, go to C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\Batch, and double-click the **Domain\_RunBVTTestCases.cmd** file.

To run the BVT test in a Workgroup environment

-   From the desktop of the driver computer, double-click the **Run FileSharing Server-Workgroup\_BVTTestCases** shortcut. This shortcut is created during the installation process.

Alternatively, go to C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\Batch, and double-click the **Workgroup\_RunBVTTestCases.cmd** file.

### <span id="_Run_All_Test" class="anchor"><span id="_Toc278822249" class="anchor"><span id="_Toc281907713" class="anchor"><span id="_Toc361668135" class="anchor"></span></span></span></span> Run All Test Cases

Use the following steps to run all test cases.

To run all test cases in a Domain environment

-   From the desktop of the driver computer, double-click the **Run FileSharing Server-Domain\_AllTestCases** shortcut. This shortcut is created during the installation process.

Alternatively, go to C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\Batch, and double-click the **Domain\_RunAllTestCases.cmd** file.

To run all test cases in a Workgroup environment

-   From the desktop of the driver computer, double-click the **Run FileSharing Server-Workgroup\_AllTestCases** shortcut. This shortcut is created during the installation process.

Alternatively, go to C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\Batch, and double-click the **Workgroup\_RunAllTestCases.cmd** file.

### <span id="_Run_Specified_Test" class="anchor"><span id="_Analyzing_Test_Results" class="anchor"><span id="_Check_Test_Results" class="anchor"><span id="_Toc361668136" class="anchor"><span id="_Toc278822251" class="anchor"><span id="_Toc281907715" class="anchor"><span id="_Toc271803749" class="anchor"></span></span></span></span></span></span></span> Check Test Results

Test suite generates test result files in different paths based on the way how test case is executed.

For running test case with batch: C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version*\#&gt;\\Batch\\TestResults

For running test case with Visual Studio: C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version*\#&gt;\\Source\\Server\\TestCode\\TestResults

For running test case with PTM: C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version*\#&gt;\\TestResults

For further information about logging in the Protocol Test Framework (PTF), please see the *PTF User Guide* in the PTF installation directory

<span id="_Debugging_Test_Cases" class="anchor"><span id="_Toc361668139" class="anchor"><span id="_Toc425336675" class="anchor"></span></span></span>Debugging Test Cases
=========================================================================================================================================================================

You can use the Visual Studio solution (.sln) file included with this test suite to debug additional test cases that you create for your protocol implementation.

![](./media/image3.png)Note

While using Microsoft® Visual Studio® 2012 or above to run test cases, test suite may throw exception with message of “Cannot get test site”. To solve this issue, please select the test settings file under test settings menu.

![](./media/image166.png)

To debug a test case

1.  On the driver computer, use Microsoft® Visual Studio® to open the following solution file: C:\\MicrosoftProtocolTests\\FileSharing\\Server-Endpoint\\*&lt;version\#&gt;*\\Source\\Server\\TestCode\\FileSharing\_Server.sln

2.  In Visual Studio, in the Solution Explorer window, right-click the **Solution ‘FileSharing\_Server’**, and select **Build Solution**.

3.  When you build the test project, the tests appear in **Test Explorer**. If Test Explorer is not visible, choose **Test** on the Visual Studio menu, choose **Windows**, and then choose **Test Explorer**.

4.  Select your test cases from Test Explorer and run or debug them. <span id="_Troubleshooting_1" class="anchor"><span id="_Troubleshooting" class="anchor"><span id="_Using_Protocol_Test" class="anchor"></span></span></span>
