Creating an AbanteCart and Wordpress (woocommerce) based e-commerce Virtual Machine on Azure
=======================================================================================

The ability to create a virtual machine on demand, whether a standard image or from one you supply, can be very useful. This approach, commonly known as Infrastructure as a Service (IaaS), is what Azure Virtual Machines provides.

To create a VM, you specify which VHD to use and the VM's size. You then pay for the time that the VM is running. You pay by the minute and only while it's running, though there is a minimal storage fee for keeping the VHD available. Azure offers a gallery of stock VHDs (called "images") that contain a bootable operating system to start from. These include Microsoft and partner options, such as Windows Server and Linux, SQL Server, Oracle and many more. You're free to create VHDs and images and then upload them yourself. You can even upload VHDs that contain only data and then access them from your running VMs.

This quite general approach to cloud computing can be used to address many different issues:

* **Dev/Test** - You might use them to create an inexpensive development and test platform that you can shut down when you've finished using it. You might also create and run applications that use whatever languages and libraries you like. Those applications can use any of the data management options that Azure provides, and you can also choose to use SQL Server or another DBMS running on one or more virtual machines.
* **Move Applications to Azure (Lift-and-shift)** - "Lift-and-shift" refers to moving your application much like you'd use a forklift to move a large object. You "lift" the VHD from your local datacenter, and "shift" it to Azure and run it there. You will typically have to do some work to remove dependencies on other systems. If there are too many, you may choose option 3 instead.
* **Extend your Datacenter** - Use Azure VMs as an extension of your on-premises datacenter, running SharePoint or other applications. To support this, it's possible to create Windows domains in the cloud by running Active Directory in Azure VMs. You can use Azure Virtual Network to tie together your local And Azure networks.

In this lab, you will learn how to create virtual machines using different options provided by Azure. You will also add data disks, access them and install VM extensions.

This lab includes the following tasks:

* [**Creating a Virtual Machine with AbanteCart Installed using Azure Portal**](#creating-a-abantecart-vm-using-portal) 

* [**Creating a Virtual Machine with Wordpress (woocommerce) Installed using Azure Portal**](#creating-a-wordpress-and-woocommerce-vm-using-portal)
	
    In this task you will create a Windows virtual machine using an existing image from the Azure Management Portal.


There are various other ways to create the Virual Machine using Powershell, Azure Command Line Interface, or ARM templates.
This lab will just demonstrate the usage of the Azure Portal to create an VM preinstalled with AbanteCart.

<a name="creating-a-abantecart-vm-using-portal"></a>
## Creating a Virtual Machine with AbanteCart using Azure Portal ##

In this task you will create a Virtual Machine in Azure Portal.
If you do not already have an Azure Account, sign up for a [Trial Account](https://azure.microsoft.com/en-us/free/).

1. Sign in to the [Azure Management Portal](https://portal.azure.com/).

1. On the Left Side bar, click **+ NEW** and then click on **See all**.

	![Creating VM - Click New and Everything](images/click-new-and-everything.png?raw=true)

	_Creating a VM_ 

1. Type in **AbanteCart Bitnami**, Hit enter and then click the **Abante Server** tile.

	![Creating VM - Click Virtual Machines and Windows Server](images/creating-compute-vm-abantecartserver.png?raw=true)

	_Creating a VM - Search for AbanteCart then select the AbanteCart tile_

1. In the **AbanteCart** blade, Select **'Resource Manager'** from dropdown **select a deployment model**, and then click **Create**.

	![Creating VM Confirm image](images/creating-vm-abantecart-confirm-image.png?raw=true)

	_Creating a VM - Click Create to confirm the use of this image_

1. On the **Create Virtual Machine** blade that opens, enter: 

	* **Name**: virtual machine name (e.g. testvmforavantecart)
	* **User Name**: administrator user for the virtual machine (e.g. azureuser). You cannot use root for security reasons.
	* **Password**: Choose "Password" for ease of setup. In production environment, it is recommended to use SSH Keys.
	* **Password**: unique password for the administrator account (e.g. pass@word123)
	* **Subscription**: Select if you have multiple subscriptions
	* **Resource**: New or Existing (e.g. create-vm). We recommend create 'New' for ease of cleaning up later.
	* **Location**: select the location for the virtual machine. (e.g. Southeast Asia)
	
	![Creating a VM - basic configuration](images/create-vm-resource-basic-config.png?raw=true)

	_Creating a VM - Basic Configuration_
	
	* **Size**: select the size of virtual machine needed. (Select **View All** for checking all sizes and details)
	
	![Creating a VM - choose size](images/create-vm-resource-choose-size.png?raw=true)

	_Creating a VM - Types of Sizes_
	
	* **Disk Type**: select the disk size. (e.g. Standard/Premium(SSD))
	* **storage account**: storage account details(if existing select the storage account at specified location or create new)
	* **virtual network**: virtual network for the virtual machine to create
	* **Subnet**: subnets under one Virtual network
	* **Public IP address**: public IP address

	![Creating a VM - Settings Optional features](images/create-vm-resource-settings-config.png?raw=true)

	_Creating a VM - Settings_
	
	* **Summary**: virtual machine summary details before you click on create.

	![Creating a VM - VM Summary](images/create-vm-summary.png?raw=true)

	* **Summary**: AbanteCart VM purchase details before you click on create.

	![Creating a VM - VM Summary](images/create-vm-summary-buy.png?raw=true)

	_Creating a VM - Summary_

	> **Note:** Premium storage, available for DS-series virtual machines in certain regions. For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](http://azure.microsoft.com/en-us/documentation/articles/storage-premium-storage-preview-portal/).


1. Click **Purchase**.

1. The VM will start being created. You can monitor the creation progress on the **Notifications**. As this can take a few minutes, this task ends here. 

	![Creating a VM - Monitor progress on the Notifications](images/creating-vm-monitor-progress-on-the-notifi.png?raw=true)

	_Creating a VM - Monitor progress in the Notifications Hub_

	> **Note:** After the VM is created, the Virtual Machine blade will open. A pin for the VM (e.g. azureVM) is also added to the **Startboard**. You can use it to access the VM.  
	>
	>![Creating a VM - A pin in the startboard exists after creation](images/creating-vm-pin-in-startboard-after-creation.png?raw=true)
	>
	>_Creating a VM - A pin was created in the Startboard_

	>Once the virtual machine has been created you can attach new or existing data disks to the Virtual Machine. See [About Virtual Machine Disks in Azure](https://msdn.microsoft.com/library/azure/dn790303.aspx) for more information. 
	>
	>![Creating a VM - A pin in the startboard exists after creation](images/create-vm-details-created.png?raw=true)
	>
	>_Virtual Machine details after Creation_


## Setting up and Configuring AbanteCart##

1. Copy your **Public IP address/DNS name** and paste it into your browser. You will be prompted to your AbanteCart startpage.

    ![Creating VM Confirm image](images/configuring-and-managing-abantecart.png?raw=true)
    _Starting page of your abantecart_

    > **Note:** Click on the bottom right hand corner to configure your instance.
    > You can find your login ID and Password by following the [guide on bitnami]( https://docs.bitnami.com/azure/faq/#find_credentials)


<a name="creating-a-wordpress-and-woocommerce-vm-using-portal"></a>
## Creating a Virtual Machine with Wordpress and WooCommerce using Azure Portal ##

	## Creating a Virtual Machine with Wordpress and WooCommerce using Azure Portal ##

In this task you will create a Virtual Machine in Azure Portal.
If you do not already have an Azure Account, sign up for a [Trial Account](https://azure.microsoft.com/en-us/free/).

1. Sign in to the [Azure Management Portal](https://portal.azure.com/).

1. On the Left Side bar, click **+ NEW** and then click on **See all**.

	![Creating VM - Click New and Everything](images/click-new-and-everything.png?raw=true)

	_Creating a VM_ 

1. Type in **Wordpress Bitnami**, Hit enter and then click the **Wordpress** tile.

	![Creating VM - Click Virtual Machines and Windows Server](images/creating-compute-vm-wordpress.png?raw=true)

	_Creating a VM - Search for Wordpress then select the Wordpress tile_

1. In the **Wordpress** blade, **'Resource Manager'** from dropdown **select a deployment model** is choosen by default. Just click **Create**.

	![Creating VM Confirm image](images/creating-vm-wordpress-confirm-image.png?raw=true)

	_Creating a VM - Click Create to confirm the use of this image_

1. On the **Create Virtual Machine** blade that opens, enter: 

	* **Name**: virtual machine name (e.g. testvmforwordpress)
	* **User Name**: administrator user for the virtual machine (e.g. azureuser). You cannot use root for security reasons.
	* **Password**: Choose "Password" for ease of setup. In production environment, it is recommended to use SSH Keys.
	* **Password**: unique password for the administrator account (e.g. pass@word123)
	* **Subscription**: Select if you have multiple subscriptions
	* **Resource**: New or Existing (e.g. create-vm). We recommend create 'Existing', choosing the same Resource group created previously for ease of cleaning up later.
	* **Location**: select the location for the virtual machine. (e.g. Southeast Asia)
	
	![Creating a VM - basic configuration](images/create-vm-resource-basic-config-wordpress.png?raw=true)

	_Creating a VM - Basic Configuration_
	
	* **Size**: select the size of virtual machine needed. (Select **View All** for checking all sizes and details)
	
	![Creating a VM - choose size](images/create-vm-resource-choose-size.png?raw=true)

	_Creating a VM - Types of Sizes_
	
	* **Disk Type**: select the disk size. (e.g. Standard/Premium(SSD))
	* **storage account**: storage account details(if existing select the storage account at specified location or create new)
	* **virtual network**: virtual network for the virtual machine to create
	* **Subnet**: subnets under one Virtual network
	* **Public IP address**: public IP address

	![Creating a VM - Settings Optional features](images/create-vm-resource-settings-config-wordress.png?raw=true)

	_Creating a VM - Settings_
	
	* **Summary**: virtual machine summary details before you click on create.

	![Creating a VM - VM Summary](images/create-vm-summary-wordpress.png?raw=true)

	* **Summary**: Wordpress VM purchase details before you click on create.

	![Creating a VM - VM Summary](images/create-vm-summary-wordpress-buy.png?raw=true)

	_Creating a VM - Summary_

	> **Note:** Premium storage, available for DS-series virtual machines in certain regions. For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](http://azure.microsoft.com/en-us/documentation/articles/storage-premium-storage-preview-portal/).


1. Click **Purchase**.

1. The VM will start being created. You can monitor the creation progress on the **Notifications**. As this can take a few minutes, this task ends here. 

	![Creating a VM - Monitor progress on the Notifications](images/creating-vm-monitor-progress-on-the-notifi.png?raw=true)

	_Creating a VM - Monitor progress in the Notifications Hub_

	> **Note:** After the VM is created, the Virtual Machine blade will open. A pin for the VM (e.g. azureVM) is also added to the **Startboard**. You can use it to access the VM.  
	>
	>![Creating a VM - A pin in the startboard exists after creation](images/creating-vm-pin-in-startboard-after-creation.png?raw=true)
	>
	>_Creating a VM - A pin was created in the Startboard_

	>Once the virtual machine has been created you can attach new or existing data disks to the Virtual Machine. See [About Virtual Machine Disks in Azure](https://msdn.microsoft.com/library/azure/dn790303.aspx) for more information. 
	>
	>![Creating a VM - A pin in the startboard exists after creation](images/create-vm-details-created.png?raw=true)
	>
	>_Virtual Machine details after Creation_


## Setting up and Configuring Wordpress with WooCommerce##

1. Copy your **Public IP address/DNS name** and paste it into your browser. You will be prompted to your Wordpress startpage.

    ![Creating VM Confirm image](images/configuring-and-managing-wordpress.png?raw=true)
    _Starting page of your abantecart_

    > **Note:** Click on the bottom right hand corner to configure your instance.
    > You can find your login ID and Password by following the [guide on bitnami]( https://docs.bitnami.com/azure/faq/#find_credentials)

<a name="cleanup"></a>
##Appendix - Cleanup

In this task you will learn how to delete the virtual machines created in the previous sections, along with the related data disks created. 

### Remove VM using Azure Portal

1. Scroll to the bottom on the left pane and Click **Browse >**. Then either search in the search box at the top or scroll down and find **Virtual machines (Classic)**.

	![Clicking Browse in the left pane and search in the box](images/clicking-browse-virtualmachine.png?raw=true)

	_Clicking Browse in the left Menu_

1. A page listing all Virtual Machines will be displayed. 

	![Virtual Machines view in portal](images/virtual-machines-view-in-portal.png?raw=true)

	_Viewing all virtual machines created_

1. Click the **...** menu to find the virtual machine to delete and in the context menu that opens, click **Delete**.

	![Deleting a virtual machine](images/deleting-a-virtual-machine.png?raw=true)

	_Deleting a virtual machine_

1. In the **Confirmation** blade that opens, type the virtual machine name, select all other items to delete like disks and domain names, and click the **Delete** button.

	![Confirming the deletion of virtual machine](images/confirming-the-deletion-of-virtual-machine.png?raw=true)

	_Confirming the deletion of the virtual machine_

The virtual machine as well as other items selected in the previous step will be deleted. You can monitor the progress of this operation from the **Notifications** Hub.

Once complete, the **Virtual Machines** list will refresh and the virtual machine recently deleted will no longer appear. Follow the same instructions to delete all other virtual machines created in this lab.

##Summary

By completing this lab you have learned how to create virtual machines using several different methods: the Azure Portal interface, the Cross-Platform Command Line Tools, PowerShell and Automation Runbook. Additionally, you have seen how to attach an empty datadisk to the virtual machine, how to generate a Remote Desktop Protocol file to connect to the machine, and how to install extensions.
