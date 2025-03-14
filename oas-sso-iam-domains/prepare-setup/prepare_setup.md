# Prepare Setup

## Introduction
This lab will show you how to download the Oracle Resource Manager (ORM) stacks zip file needed to set up the resources to run this workshop in further labs.

*Estimated Lab Time:* 15 minutes

### Objectives
1. Download ORM stack for deploying the **OAS Application, App Gateway Compute Instance and OCI IAM Identity Domains**
2. Download ORM stack for configuring the **OAS Application, App Gateway Compute Instance and OCI IAM Identity Domains**
3. Configure an existing Virtual Cloud Network (VCN)
4. Get Connection String from Virtual Machine DB system.

### Prerequisites
This lab assumes you have:
1. An Oracle Cloud account with at least **PAY GO** subscription
2. A Compartment apart from *root*
3. An existing *VCN* and an *Internet Gateway* attached to it.
4. A *Public* subnet
5. A *SSH key* pair. A single SSH Key pair to be used for both the servers (Make sure you have the .key and .pem formats of the private key available)
6. An Oracle Cloud Database - VM DB System. The database must be deployed in the same region as Oracle Analytics Server and accessible from the VCN where you plan to deploy Oracle Analytics Server. The database edition must be Oracle Database Enterprise Edition (EE) version 12.1, 12.2, 18+, or 19+. The database must be a pluggable database (PDB) on an Oracle Cloud virtual machine DB system.

**Note** - *This Live lab considers all resources along with VM DB in same VCN. If you use different VCNs for OAS and VMDB then please make sure you have setup correct connectivity between them.*

## Task 1: Download Oracle Resource Manager (ORM) stack zip file to Deploy 
1.  Click on the link below to download the Resource Manager zip file you need to build your environment:

    - [Stack1-Deploy.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/n/id3kvohtwgjy/b/LIveLab/o/OAS-SSO-IAM-Domains%2FStack1_Deploy.zip)
	
	- [Stack2-Configure.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/n/id3kvohtwgjy/b/LIveLab/o/OAS-SSO-IAM-Domains%2FStack2_Configure.zip)

2.  Save in your *downloads* folder.

We strongly recommend using this stack in a self-contained/dedicated VCN with your instance. Proceed to the next task to update your existing VCN with the required Ingress rules.

## Task 2: Adding security rules to an existing VCN

This workshop requires a certain number of ports to be available, a requirement that can be met by using the default ORM stack execution that creates a dedicated VCN. In order to use an existing VCN/subnet, the following rules should be added to the security list.

| Type           | Source Port    | Source CIDR | Destination Port | Protocol | Description                           |
| :-----------   |   :--------:   |  :--------: |    :----------:  | :----:   | :------------------------------------ |
| Ingress        | All            | 0.0.0.0/0   | 22               | TCP      | SSH                                   |
| Ingress        | All            | 0.0.0.0/0   | 9500             | TCP      | For OAS Weblogic Admin Server         |
| Ingress        | All            | 0.0.0.0/0   | 9501             | TCP      | For OAS Application                   |
| Ingress        | All            | 0.0.0.0/0   | 9502             | TCP      | For OAS Application                   |
| Ingress        | All            | 0.0.0.0/0   | 4443             | TCP      | For App Gateway Access (SSL)     	  |
| Ingress        | All            | 0.0.0.0/0   | 1521             | TCP      | For VM DB System Database             |
{: title="List of Required Network Security Rules"}

<!-- **Notes**: This next table is for reference and should be adapted for the workshop. If optional rules are needed as shown in the example below, then uncomment it and add those optional rules. The first entry is just for illustration and may not fit your workshop -->

<!--
| Type           | Source Port    | Source CIDR | Destination Port | Protocol | Description                           |
| :-----------   |   :--------:   |  :--------: |    :----------:  | :----:   | :------------------------------------ |
| Ingress        | All            | 0.0.0.0/0   | 443               | TCP     | e.g. Remote access for web app        |
{: title="List of Optional Network Security Rules"}
-->

1.  Go to *Networking >> Virtual Cloud Networks*
2.  Choose your network
3.  Under Resources, select Security Lists
4.  Click on Default Security Lists under the Create Security List button
5.  Click Add Ingress Rule button
6.  Enter the following:  
    - Source Type: CIDR
    - Source CIDR: 0.0.0.0/0
    - IP Protocol: TCP
    - Source Port Range: All (Keep Default)
    - Destination Port Range: *Select from the above table*
    - Description: *Select the corresponding description from the above table*
7.  Click the Add Ingress Rules button
8. Repeat steps [5-7] until a rule is created for each port listed in the table

## Task 3: Get Connection String from Virtual Machine DB system.

As the part of prerequisites, you should have the VM DB system on OCI and use the same DB system to get the connection string.

You must use the database connection string format:
    ```
    <hostname or IP address>:<port>:<PDB_name>.<DB_domain>
    ```

*For example: oasdb.subnetname.oasvcn.oraclevcn.com:1512:OASDB_pdb1.subnetname.oasvcn.oraclevcn.com*

## Appendix
If you wish to change the version then you can make the below parameter changes in the **terraform.tfvars** file of **stack1-Deploy.zip**.

**Note**: Make sure you zip the folder again before deploying it in upcoming steps.


| OAS Version           | mp\_OAS\_listing\_resource\_version    | 				mp\_OAS\_listing\_image\_resource\_id 								                         |
| :-----------------:   |   :----------------------------:   |  :----------------------------------------------------------------------------: |
|OAS 2023 0301			    |	  2023\_0301						             |	ocid1.image.oc1..aaaaaaaauvdpokewuntrs7ewrnspng7tl4ayofiajbiwdhjgaowfukzjcg4q  |
|OAS 2022 (6.4) 0601	  |  	2022\_(6.4)\_0601					         |	ocid1.image.oc1..aaaaaaaalbzf7wtfpdac6fs4w2pzqqvqmiuex4kaxjhcei6v6e56ewt64jgq  |
|OAS 2022 (6.4) 0501	  |	  2022\_(6.4)\_0501					         |	ocid1.image.oc1..aaaaaaaale4pe5fvrdqvzq7swfgpb3tggnit5lgfmjybxflbit7ypushdgla  |
|OAS 2023 0201			    |	  2023\_0201						             |	ocid1.image.oc1..aaaaaaaadurqktr6zuz2fezl762s32gftipjodhprgrnxduwx75epvn4dbpa  |
|OAS 2023 0101			    |	  2023\_0101						             |	ocid1.image.oc1..aaaaaaaazeuqqbs7no5duaqgdwmv4432scjkku7wg3kuat56obfssan2jqia  |
|OAS 2022(6.4) 0101		  |	  2022\_(6.4)\_0101                  |	ocid1.image.oc1..aaaaaaaayk4odypeg54gdkm6yfzadjosswbjtdyiv3rv35nzzo6pin6j5mga  |

**Note**: This live lab considers the version **OAS 2023 0101** as default version.

You may now **proceed to the next lab.**

## Acknowledgements
* **Author** - Sagar Takkar, Chetan Soni
* **Lead By** - Deepthi Shetty 
* **Last Updated By/Date** - Sagar Takkar November 2023
