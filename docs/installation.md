# Installation

The AWS EC2 Connector installation consists of multiple steps that are required to complete the installation successfully. The connector requires the installation of a Windows Agent.

## Supported Software Levels
The following software levels are required to implement the JDEdwards Connector.

- OpCon Release 19.0 or higher.
- OpCon Rest API.
- Uses an embedded Open-JDK (Version 11).

## Installation
The installation process consists of the following steps:

- AWS EC2 Connector Installation.
- Adding AWS EC2 Connector job subtype to Enterprise Manager.
- AWS EC2 Connector Configuration.
 
### AWS EC2 Connector Installation
The AWS EC2 connector can be installed on any Windows Server as long as there is an OpCon MSLSAM Agent installed on the Windows Server. 

Copy the supplied install file AWSEC2Connector-win.zip and extract it into the installation directory.

After the extraction, the root installation directory contains the connector executable (awsec2.exe), the encryption software (EncryptValue.exe), the Connector.config file and two directories, java and emplugins. 
The java directory contains the java software required to execute the connector (OpenJDK 11) and the emplugins directory contains the job sub-type plugin for Enterprise Manager.

### Create AWSEC2Path Global Property
Create a global property **AWSEC2Path** that contains the full path of the installation directory.

### Create Special Properties
The Amazon EC2 connector uses three global properties to hold the values of images, regions and the associated sizes.

Name            | Description
--------------- | -----------
**AWS_IMAGES**  | Create the global property and add the values contained in Appendix A using a comma to separate them. The doubles quotes surrounding the values must be retained. These values will then be visible in the drop-down list (see List of Virtual Machines). Image names can be added to the drop-down list by editing the property. The format of item must be ***description : image***.
**AWS_SIZES**   | Create the global property and add the values contained in Appendix B using a comma to separate them. The doubles quotes surrounding the values must be retained. These values will then be visible in the drop-down list (see List of Virtual Machine Sizes).
**AWS_REGIONS** | Create the global property and add the values contained in Appendix C using a comma to separate them. The doubles quotes surrounding the values must be retained. These values will then be visible in the drop-down list (see List of Regions).

### Job Subtype Installation
Copy the Enterprise Manager plug-in from the ***installation_dir***\\emplugins directory to the dropins directory of the Enterprise Manager installation. 

If the dropins directory does not exist, create the dropins directory off the root directory. 

Restart Enterprise Manager and a new Windows job subtype called AWS EC2 will be visible.

If not restart Enterprise Manager using 'Run as Administrator'. After this Enterprise Manager can be used normally.

## AWSEC2 Connector Configuration
The configuration of the AWSEC2 Connector requires setting the required values in the Connector.config file. 

All puser and assword values placed in the configuration file must be encrypted using the EncryptValue.exe utility provided with the connector. When executing the tool, a single argument -v is used to provide the value that must be encrypted.

### EncryptValue Utility
The EncryptValue utility uses standard 64 bit encryption.

Supports a -v argument and displays the encrypted value

On Windows, example on how to encrypt the value "abcdefg":

```
EncryptValue.exe -v "abcdefg"

```

### Connector.config configuration
Configure the Connector.config file in the installation directory setting the required information.
The Connector.config contains the following values

Property Name                 | Value
----------------------------- | -----------
**[CONNECTOR]**               | header
**CONNECTOR_NAME**            | The name of the connector. This value should not be changed.
**MAX_WAIT_TIME_FOR_STARTUP** | The maximum time in minutes to wait for startup completion before terminating the requested action and returning an error condition. Default value is 10.
**DEBUG**                     | If the Connector supports a debug mode, it can be used to set the connector into DEBUG mode. Value either ON or OFF (default OFF).
**[USER]**                    | header : An identifier that is used to determine which security credentials should be used for the request. When a request is processed, the User ID value of the job definition is matched to a header entry to load the required credentials.
**CONNECTOR_USER_ACCESS_KEY** | The user access key of the user that has access to the AWS EC2 environment. The key must be encrypted using the EncryptValue tool.
**CONNECTOR_USER_SECRET_KEY** | The matching user secret key. The key must be encrypted using the EncryptValue tool. 
**[OPCON API INFORMATION]**   | header
**ADDRESS**                   | The address of the OpCon-API RESTFul server that the connector will communicate with. Includes the port number 9000 for non-tls, 9010 for tls. Value : ***address***:***port number*** 
**USING_TLS**                 | Indicates if the server is supporting TLS. Value : True or False (default True). 
**TOKEN**                     | An application token that provides the necessary authentication to use the OpCon API. 

Example configuration file. 

```
[CONNECTOR]
CONNECTOR_NAME=AWS EC2 Connector
MAX_WAIT_TIME_FOR_STARTUP=10
DEBUG=ON

[USER1]
CONNECTOR_USER_ACCESS_KEY=5155744a5156524d4e6c525a54556c55574646545355314a5431493d
CONNECTOR_USER_SECRET_KEY=5433553353325a314b79396c526d4572576d39595a584a77576e5273595852764b

[OPCON API INFORMATION]
ADDRESS=BVHTEST02:9010
USING_TLS=True
TOKEN=ec0fe84e-cabe-4b26-841e-2d958d3af253

```
## List of Virtual Machine 
"Microsoft Windows Server 2016 Base : ami-0f4c7e570f044b46f","Amazon Linux AMI 2018.03.0 : ami-0080e4c5bc078760e","Red Hat Enterprise Linux 7.6 (HVM) : ami-011b3ccf1bd6db744"

## List of Virtual Machine Sizes 
"a1.medium","a1.large","a1.xlarge","a1.2xlarge","a1.4xlarge","m4.large","m4.xlarge","m4.2xlarge","m4.4xlarge","m4.10xlarge","m4.16xlarge","m5.large","m5.xlarge","m5.2xlarge","m5.4xlarge","m5.12xlarge","m5.24xlarge","m5a.large","m5a.xlarge","m5a.2xlarge","m5a.4xlarge","m5a.12xlarge","m5a.24xlarge","m5d.large","m5d.xlarge","m5d.2xlarge","m5d.4xlarge","m5d.12xlarge","m5d.24xlarge","t2.nano","t2.micro","t2.small","t2.medium","t2.large","t2.xlarge","t2.2xlarge","t3.nano","t3.micro","t3.small","t3.medium","t3.large","t3.xlarge","t3.2xlarge"

## List of Regions
"AP_NORTHEAST_1","AP_NORTHEAST_2","AP_SOUTH_1","AP_SOUTHEAST_1","AP_SOUTHEAST_2","CA_CENTRAL_1","CN_NORTH_1","CN_NORTHWEST_1","EU_CENTRAL_1","EU_NORTH_1","EU_WEST_1","EU_WEST_2","EU_WEST_3","SA_EAST_1","US_EAST_1","US_GOV_EAST_1","US_WEST_1","US_WEST_2"

---
