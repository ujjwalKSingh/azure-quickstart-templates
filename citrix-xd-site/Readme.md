# Citrix XD Site Deployment Sample

This template demonstrates the creation of a self-contained XD deployment in Azure, deploying the following resources:

* XenDesktop Desktop Delivery Controller
* SQL Server
* Citrix License Server
* Citrix Storefront Server
* Citrix Netscaler
* Citrix Virtual Desktop Agents
* Domain Controller

After deployment, the components are fully configured and a simple XenDesktop site is created in a number of steps:

1. A new user-specified domain is created and the machines are automatically joined to it.
2. The SQL server database is configured and a site is created on the XenDesktop Delivery Controller.
3. The VDAs are provisioned and joined to the new site.
4. A Storefront deployment is created, providing access to published Apps & Desktops.
5. Netscaler is configured to provide external user access.

# Pre-Requisites

1) These templates make use of sysprepped images which must have the above XenDesktop components fully installed. There are currently three required images which must reside in the 'images' container of the storage account specified by 'vhdStorage':

| Image   | Description    |
|:--- |:---|
xd-arthur.vhd | Full XenDesktop installation
xd-arthur-rdsh.vhd | Server VDA installation
xd-arthur-vdi.vhd | Server VDI installation

2) An Azure KeyVault with a certificate deployed as a secret. It is referenced in the parameters 'vaultName,' 'vaultResourceGroup,' and 'certificateUrl.' The given certificate is installed on all windows machines and bound to their WinRM listeners. For more information about KeyVaults in Azure, see:

* <a href="http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx">KeyVault step-by-step</a>
* <a href="https://github.com/Azure/azure-quickstart-templates/tree/master/101-create-key-vault">Deploying a KeyVault with Azure Resource Manager</a>
* <a href="https://msdn.microsoft.com/en-us/library/ff699202.aspx">Generating a test certificate</a>

Click the button below to deploy

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Falexstoddard%2Fazure-quickstart-templates%2Fmaster%2Fcitrix-xd-site%2Fcitrix-xd.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Template parameters:

| Name   | Description    |
|:--- |:---|
| vhdStorage | Name of the storage account which will contain the deployed machines (must already exist and house vhds described above). |
| vhdStorageGroup | Existing resource group where 'vhdStorage' exists. |
| publicDnsName | Unique public DNS prefix for the deployment. The fqdn will look something like '<dnsname>.westus.cloudapp.azure.com'. Up to 62 chars, digits or dashes, lowercase, should start with a letter: must conform to '^[a-z][a-z0-9-]{1,61}[a-z0-9]$'. |
| windowsOSVersion | Windows OS version for the DC, allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter. |
| vmSize | The size of the virtual machines (default: Standard_A2) |
| domainName | Name of the new domain to be created (e.g. 'contoso.com'.) |
| adminUsername | Domain and Citrix admin username. |
| adminPassword | Domain and Citrix admin password. |
| siteName | Name of the XenDesktop site to be created (e.g. 'AzureSite'.) |
| portRdp | The RDP point you will use to interact with the DDC. |
| portDebug | The port you will use for active debugging of the DDC. |
| portPowershell | The port you will use for controlling the DDC through powershell. |
| vaultName | Name of the KeyVault containing your certificate secret. |
| vaultResourceGroup | Name of the resource group containing KeyVault 'vaultName.' |
| certificateUrl | Url of the certificate to be installed on the Windows machines. |
