| [Home](../README.md) |
|----------------------|

# Contents

The **NetOps - FortiManager ZTP** solution pack contains the following resources.

## Connectors

|**Name**|**Description**|
| :- | :- |
| Fortinet FortiManager JSON-RPC  | Connector used to do generic API calls to FortiManager using API calls documented in [FNDN FortiAPI for FortiManager](https://fndn.fortinet.net/index.php?/fortiapi/5-fortimanager/). |

## Module Schema

|**Name**|**Description**|
| :- | :- |
| Managers | Stores FortiManager information for one or more FortiManagers |
| Devices | Synchronized from the FortiManager's Device DB table. |
| Metafield Templates | Template files to populate Device Metadata on Device records. |
| Scripts | Records designed to do a variety of scripts. Markdown Reports per device, DeviceDB, Remote CLI, PolicyDB, and/or TCL Scripts on devices via the FortiManager API. |
| ZTP Profiles | Defined solutions to apply templates and scripts to devices on demand or when discovered. |
   
## Roles

|**Name**|**Description**|
| :- | :- |
| FortiManager-Playbook-Appliance | The playbook appliance needs to have full access to records to update fields that are owned by FortiManager.  |
| FortiManager-Admin | Users should not directly edit fields that are owned by FortiManager. |
  Full App Permissions | Existing FortiSOAR Role to merge newly created modules.

## Record Set

|**Name**|**Description**|
| :- | :- |
|  Announcements  |  Used in Dashboards for Quick Links.  |
|  ZTP Profiles  |  ZTP Profile Records designed to assign to testing Models in FortiManager matching the name `FG0#` for each feature test and can often be auto-assigned. |
|  Metafield Templates  |  Metafield Template Records are used in ZTP Profiles to handle various Feature Examples.  |
|  Scripts  |  Records to run various scripts on devices in the Feature Examples. |
|  Attachments  | Example spreadsheets used in custom playbooks with metadata lookup examples are included in this pack. |

## Playbook Collection

|**Playbook Collection Name**|**Description**|
| :- | :- |
| 10 - SP - NetOps-FortiManager ZTP - Microservices | Focuses on FortiManager API calls. |
| 10 - SP - NetOps-FortiManager ZTP - Buttons | Buttons that users can use to invoke sections of the framework on carious records. |
| 10 - SP - NetOps-FortiManager ZTP - Record Handlers | Automation around records within the framework. |
| 10 - SP - NetOps-FortiManager ZTP - Synchronizing | Synchronize data from FortiSOAR and FortiManager. |
| 10 - SP - NetOps-FortiManager ZTP - Triggers | Actions triggered by record changes within the system and used heavily in the ZTP Flow process. |
| 10 - SP - NetOps-FortiManager ZTP - Custom Playbooks | Playbooks that can be created by users as metadata sources and used in Template files to build custom solutions within the integration. |

>**Warning:** We recommend that you clone these playbooks before customizing to avoid loss of information while upgrading the solution pack.

## Feature Tests (ZTP Profiles)

This solution pack provide various examples of ZTP Profiles that can be used to test some common features needed in building an automation solution. You can use the automatic assigning of these profiles by creating devices matching the names already put in the ZTP Profile `Assignment Search` or you can manually assign a profile to one or more devices to perform the feature test. 

| Feature Test ID | Profile Name | Description | 
| --------------- | ------- | ----------- |
| FT001 | Prompt User for Metadata via FortiSOAR | The metafield `loopback0_ip` is empty and FortiSOAR Users are prompted to complete through a system manual input task. |
| FT002 | Prompt User for Metadata and Monitor FG config before proceeding | The metafield `loopback0_ip` is empty and the metafield `lo0_exists` needs to be set to `yes` before the profile will complete. The `lo0` interface is checked using an API call to FMG to check `/global/system/interface` for the device on a `20 second` loop loop until seen. |
| FT003 | Metadata Source via Custom Playbook | Use two custom playbooks to populate the metafields `ip_table` and `ip_table_lookup`.  |
| FT004 | Monitor Metafield from FG config before proceeding | Performs the same steps as **FT001** but with a `60 second` loop. |
| FT005 | Assign Device to Multiple Device Groups | Simply assign a device to the device groups `FT005a`,`FT005b`,`FT005c`. The device groups are created in FMG if they do not exist. |
| FT006 | Assign Device to Groups from Device Metafield | Create the metafield `platform` and `device_groups`. The `device_groups` field dynamically contains one group based on the device model and one on the device series. |
| FT007 | Retrieve Device Config | Use a ZTP Phase to retrieve the config from a live device. This will cause an expected ZTP Phase error if run on a device model. |
| FT008 | Lookup Metadata in Spreadsheet from Hostname | Includes sample spreadsheets in the Attachment section of FortiSOAR. Use a custom playbook to search for `CSV` and `XLSX` files in the FortiSOAR Attachment records that are tagged as type `metadata`. Reach each file, in order of oldest to newest, and find and append metadata to the device record as found. |
| FT009 | Clone of **FT008** and is never assigned due to order priority. | To show `order priority` working this profile has the same `Assignment Search` regex as FT008 but will not match on the device names due to order. |
| FT010 | Install Site VLANs using DeviceDB Config | Create the metafield `site_id`, `site_subnet`, `vlan_cidr`, and `vlan_count`. The `site_subnet` is derived from taking a `/24` from `10.48.0.0/12` using the `site_subnet`. |
| FT011 | Install Site VLANs using DeviceDB Config and Run Interface Report | Performs the same steps as **FT010** but then runs the same `Markdown Report` linked in **FT013**. |
| FT012 | Install Site VLANs using DeviceDB Config and set ZTP Profile Next to Run a Report | Performs the same steps as **FT010** but then sets the `ZTP Profile Next` to the ZTP Profile `FT013 - Run Device Interface Report` to chain a second profile with **FT013**.  |
| FT013 | Run Device Interface Report | Call the API of FMG for the device interfaces using a `Markdown Report` script template and append the device `Report Markdown` field for reporting. |
| FT014 | Automatic Assignment - Advanced Search for FG0113 | Using Advanced search we leverage the regex `^(?=.*\bdevicename:FG0+113\b)(?=.*\bplatform:Forti\S+\b)(?=.*\bsn:FG0+113\b).*` to find devices based on `device name`, `platform`, and `serial`.  |
| FG015 | Automatic Assignment - Jinja Search for FG0114-FG0118 <=6 Ports | Check the `hostname` for `FG0114-FG0118`. Then use an API call to FMG to check `/global/system/interface` and a Jinja `json_query` function to count `physical` ports on a device and assign if it matches the range. |
| FG016 | Automatic Assignment - Jinja Search for FG0114-FG0118 <=12 Ports | Same as **FG015** but with a different group of port count. | 
| FG017 | Automatic Assignment - Jinja Search for FG0114-FG0118 <=20 Ports | Same as **FG015** but with a different group of port count. | 
| FG018 | Automatic Assignment - Jinja Search for FG0114-FG0118 >20 Ports |  Same as **FG015** but a catch all if no other profile with a port count matched. | 
| FG019 |  Toggle Device Hostname and FortiManager Device Name using a Custom Script | By leveraging a custom script we can run custom playbooks to make arbitrary API calls and us the output of the custom script to modify the FortiSOAR device record. | 

| [Installation](./setup.md#installation) | [Configuration](./setup.md#configuration) | [Usage](./usage.md) |
|-----------------------------------------|-------------------------------------------|---------------------|