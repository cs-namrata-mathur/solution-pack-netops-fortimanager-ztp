| [Home](../README.md) |
|----------------------|

# Contents

The **NetOps - FortiManager ZTP** solution pack provides the resources required to automate Zero Touch Provisioning (ZTP) workflows for FortiManager-managed devices.

## Roles

|**Role**|**Description**|
| :- | :- |
| FortiManager-Playbook-Appliance | Grants the playbook appliance full access to records so it can update fields managed by FortiManager.  |
| FortiManager-Admin | Allows administrators to manage FortiManager resources while preventing direct modification of fields synchronized from FortiManager. |
  Full App Permissions | Existing FortiSOAR role used to grant access to newly installed modules within the solution pack. |
  
## System Views

|**System View**|**Description**|
| :- | :- |
| FortiManager | A menu option that provides access to the FortiManager modules, including Managers, Devices, and ZTP Profiles. |

## Module Schemas

|**Module Schema**|**Description**|
| :- | :- |
| Managers | Stores connection and other information for one or more FortiManager instances. |
| Devices | Stores device records synchronized from the FortiManager DeviceDB table. |
| Metafield Templates | Stores template files used to populate device metadata in device records. |
| Scripts | Stores reusable scripts, including Markdown reports, DeviceDB scripts, Remote CLI scripts, PolicyDB scripts, and TCL scripts executed through the FortiManager API.|
| ZTP Profiles | Defines automation workflows that apply templates and scripts when devices are discovered or run on demand. |
| Announcements | Stores announcements displayed in dashboards and other solution components. |
| ZTP Quick Links | Stores quick links that provide access to dashboards and FortiManager modules. |

## Widgets

|**Widget**|**Description**|
| :- | :- |
| Playbook Buttons | Displays playbook buttons on a record's detail page, allowing users to start automation directly from the record. |

## Playbook Collections


|**Playbook Collection Name**|**Description**|
| :- | :- |
| 10 - SP - NetOps-FortiManager ZTP - Microservices | Contains playbooks that communicate with the FortiManager API. |
| 10 - SP - NetOps-FortiManager ZTP - Buttons | Contains playbooks that contain buttons that users can use to invoke sections of the framework on various records. |
| 10 - SP - NetOps-FortiManager ZTP - Record Handlers | Contains playbooks that contain automation to manage record lifecycle events within the solution. |
| 10 - SP - NetOps-FortiManager ZTP - Synchronizing | Contains playbooks that synchronizes data between FortiSOAR and FortiManager. |
| 10 - SP - NetOps-FortiManager ZTP - Triggers | Contains event-driven playbooks that respond to record changes and support the ZTP workflow. |
| 10 - SP - NetOps-FortiManager ZTP - Custom Playbooks | Contains playbooks that can be customized and used as metadata sources when building automation workflows. |

>**Important:** Clone the provided playbooks before making customizations. This prevents custom changes from being overwritten when the solution pack is upgraded.


## Connectors

|**Connector**|**Description**|
| :- | :- |
| Fortinet FortiManager JSON-RPC  | Connector used for communication with FortiManager using API calls. Information on the FortiManager API is  documented in [FNDN FortiAPI for FortiManager](https://fndn.fortinet.net/index.php?/fortiapi/5-fortimanager/). | 
| Code Snippet  | Connector used for executing custom code (for example, Python functions) as part of playbook workflows. |


## Record Sets

|**Record Set**|**Description**|
| :- | :- |
|  Attachments  | Provides sample spreadsheets used by the example metadata lookup playbooks. | 
|  Metafield Templates  |  Metafield template Records by ZTP profiles to demonstrate feature examples.  |
|  Scripts  |  Records to run various scripts on devices in the feature examples. |
|  ZTP Profiles  |  ZTP Profile records that can be assigned automatically or manually to devices for testing and validation in FortiManager by matching the name `FG0#` for each feature test. |
|  ZTP Quick Links  |  Used in ZTP Profiles records to create quick links to Dashboards and FortiManager Modules.  |

## Picklists

- AnnouncementType
- AnnouncementCriticality
- AttachmentsType
- EnableDisable
- Script Type
- ZTP Assignment Module
- ZTP Assignment Search Field
- ZTP Assignment Search Type
- ZTP Phase
- ZTP Profile Mode


## Module Views

- Devices - Form, Detail List
- Scripts - Form, Detail List
- Managers - Form, Detail List
- Announcements - Form, Detail List
- ZTP Profiles - Form, Detail List
- ZTP Quick Links - Form, Detail List
- Metafields Templates - Form, Detail List

## Playbook Blocks

- Dynamic Manager and Device List

## Global Variables

- Server_fqhn


## Feature Tests (ZTP Profiles)

The solution pack includes sample ZTP profiles that demonstrate common Zero Touch Provisioning (ZTP) workflows and automation scenarios. You can assign these profiles automatically by creating devices that match the profile's `Assignment Search` criteria, or manually assign a profile to one or more devices for testing.

| Feature Test ID | Profile Name | Description | 
| --------------- | ------- | ----------- |
| FT001 | Prompt User for Metadata via FortiSOAR | Prompts the user to enter a value for the `loopback0_ip` metafield by using a manual input task before the workflow continues. |
| FT002 | Prompt User for Metadata and Monitor FG config before proceeding | Prompts the user to populate the `loopback0_ip` metafield. Before completing the profile, the workflow verifies that the `lo0_exists` metafield is set to `yes`. It polls the FortiManager API every `20` seconds until the `lo0` interface is detected. |
| FT003 | Metadata Source via Custom Playbook | Demonstrates how custom playbooks populate the `ip_table` and `ip_table_lookup` metafields.  |
| FT004 | Monitor Metafield from FG config before proceeding | Performs the same workflow as **FT001**, but polls the device every `60` seconds before continuing. |
| FT005 | Assign Device to Multiple Device Groups | Assigns the device to the `FT005a`, `FT005b`, and `FT005c` device groups. The workflow creates the groups in FortiManager if they do not already exist.  |
| FT006 | Assign Device to Groups from Device Metafield | Creates the `platform` and `device_groups` metafields. The `device_groups` value is generated dynamically based on the device model and device series. |
| FT007 | Retrieve Device Config | Retrieves the configuration from a live device during a ZTP phase. Running this profile against a device model generates the expected ZTP phase error. |
| FT008 | Lookup Metadata in Spreadsheet from Hostname | Demonstrates how to retrieve metadata from CSV and XLSX files stored in FortiSOAR Attachments. The workflow reads each file in chronological order and appends matching metadata to the device record. |
| FT009 | Clone of **FT008** and is never assigned due to order priority. | Uses the same `Assignment Search` expression as **FT008** but is never assigned because FT008 has higher assignment priority. This profile demonstrates how `assignment order` affects profile selection. |
| FT010 | Install Site VLANs using DeviceDB Config | Creates the `site_id`, `site_subnet`, `vlan_cidr`, and `vlan_count` metafields. The workflow derives the `site_subnet` by allocating a `/24` from `10.48.0.0/12` address space. |
| FT011 | Install Site VLANs using DeviceDB Config and Run Interface Report | Performs the same workflow as **FT010** then generates the Markdown interface report used in **FT013**. |
| FT012 | Install Site VLANs using DeviceDB Config and set ZTP Profile Next to Run a Report | Performs the same workflow as **FT010** then sets the `ZTP Profile FT013 - Run Device Interface Report` as the next `ZTP Profile` to demonstrate profile chaining.  |
| FT013 | Run Device Interface Report | Retrieves device interface information from the FortiManager API using a `Markdown Report` script template that is appended to the device's `Report Markdown` field. |
| FT014 | Automatic Assignment - Advanced Search for FG0113 | Demonstrates automatic profile assignment by using an advanced regular expression to match device name, platform, and serial number. The regex used us `^(?=.*\bdevicename:FG0+113\b)(?=.*\bplatform:Forti\S+\b)(?=.*\bsn:FG0+113\b).*` to find devices based on `device name`, `platform`, and `serial`.  |
| FG015 | Automatic Assignment - Jinja Search for FG0114-FG0118 <=6 Ports | Uses the hostname and a FortiManager API query to count physical interfaces with the Jinja json_query filter. Assigns the profile when the device has six or fewer physical ports. The query checks the `hostname` for `FG0114-FG0118`. Then use an API call to FMG to check `/global/system/interface` and a Jinja `json_query` function to count `physical` ports on a device and assign if it matches the range. |
| FG016 | Automatic Assignment - Jinja Search for FG0114-FG0118 <=12 Ports | Performs the same workflow as **FT015**, but matches devices with up to 12 physical ports. | 
| FG017 | Automatic Assignment - Jinja Search for FG0114-FG0118 <=20 Ports | Performs the same workflow as **FT015**, but matches devices with up to 20 physical ports. | 
| FG018 | Automatic Assignment - Jinja Search for FG0114-FG0118 >20 Ports |  Performs the same workflow as **FT015**, but it serves as the fallback profile for devices with more than 20 physical ports when no previous profile matches. | 
| FG019 |  Toggle Device Hostname and FortiManager Device Name using a Custom Script | Demonstrates how to use a custom script and playbook to make FortiManager API calls and update the corresponding FortiSOAR device record. | 
| FG020 |  Creates CLI Templates and a Provisioning Template Group | Authorizes the device, derives the `site_id` from the hostname, creates CLI and Jinja templates for loopback interfaces (`lo1` and `lo2`), creates the provisioning template group if it does not exist, and installs the generated configuration on the device. | 
| FG021 |  Creates a Policy Package & Install | Creates a policy package named **FT021** if it does not already exist, then installs the policy package on the target device. After the installation completes, the workflow automatically starts **FT022**. | 
| FG022 |  Updates a Policy Package and Install with Updates | Updates the policy package created by **FT021** with any required changes, then reinstalls the updated policy package on the target device. | 

| [Installation](./setup.md#installation) | [Configuration](./setup.md#configuration) | [Usage](./usage.md) |
|-----------------------------------------|-------------------------------------------|---------------------|