| [Home](../README.md) |
|----------------------|

# Usage

The `NetOps - FortiManager ZTP` solution pack has a variety of uses. This solution pack provides various examples of ZTP Profiles that can be used to test some common features needed in building an automation solution. You can use the automatic assigning of these profiles by creating devices matching the names already put in the ZTP Profile `Assignment Search` or you can manually assign a profile to one or more devices to perform the feature test. 

![](./res/usage/ztp_profile_summary_list.png)

# Module Summary

## [Managers](./usage/managers.md)>
Manager records define the FortiManagers, and the current firmware and status, used in your solution. Creating a Manager record will automatically create the FortiManager JSON-RPC connector configuration for API access. 

## [Devices](./usage/devices.md)>
Devices are synchronized from the FortiManager and stored in FortiSOAR. Automation can then occur on one or more devices based on device properties and workflow needs. Once created, they are not removed from FortiSOAR to preserve the last known status of the Device record even if the device has been removed, or moved, from your FortiManager. 

## [Metafield Templates](./usage/metafield_templates.md)>
Managing metadata is crucial to the success of any network deployment and provisioning. Metafield Templates can be used to prompt required users to respond to unknown, but required, fields before deployment. Templates can be used to integrate external systems and retrieve key data requirements before deployment to create dynamic metadata based on a wide range of advanced and complex requirements through [Metafield Sources](./usage/jinja_rendering_with_metafield_sources.md). Metadata inside FortiSOAR can be exports to FortiManger to be used by already existing solutions with minimal effort.

## [Script Templates](./usage/script_templates.md)>
Scripts in FortiSOAR can be used for creating customized FortiManager CLI, Device DB, Policy DB, and/or TCL Scripts per device. Scripts in FortiSOAR can also maintain and create Provisioning CLI Templates in FortiManager such as when new ADOMs are created and need to be setup with Templates. Device reporting scripts can also be used to create a custom dashboard with user defined content. Scripts can also be enhanced by gathering external data using [Metafield Sources](./usage/jinja_rendering_with_metafield_sources.md).

## [ZTP Profiles](./usage/ztp_profiles.md)>
The ZTP Profiles module describes how to handle the provisioning of each device maintained by the respective FortiManager. As a device shows up in the FortiManager Device DB, unauthorized or modeled, when ZTP Profiles are assigned to those devices the defined device templates and provisioning steps are applied and reported per device. ZTP Profiles can be assigned on demand or automatically assigned when devices are created in FortiManger regardless of how the devices were created. Automatic assignment of ZTP Profiles can leverage [Metafield Sources](./usage/jinja_rendering_with_metafield_sources.md) to create complex auto assignment rules.


## Setup FMG for Feature Tests

To create random test devices in FMG leverage the Manager automation `Device Model - Create Randomly` and when prompted for `Range of Devices` enter `101-118`. 
![](./res/usage/ztpflowtest-create-random-models.png) 

Once the devices show in the system the ZTP Profile will automatically be assigned and ZTP Phases will kick off. 
![](./res/usage/ztpflowtest-random-models-assigned-profiles.png)

The Dashboard `ZTP Profile and Phase for Devices` then shows what stage the device is in for ZTP phases. 
![](./res/usage/ztpflowtest-random-models-phases.png)

## ZTP Profile Details

### User Input via FortiSOAR

The Profiles **FT001** and **FT002** both request FortiSOAR users for metafield input. 

![](./res/usage/ztpflowtest-user-input-tasks.png)
![](./res/usage/ztpflowtest-FT001-prompt-user.png)

### Monitoring for FG Config via FMG API

The Profiles **FT002** and **FT004** are monitoring for the existence of the `lo0` interface. 
![](./res/usage/ztpflowtest-monitoring-phases.png)

The search for `lo0` is done using this Jinja Template that searches the results of `/pm/config/device/{{record.devname}}/global/system/interface`:
```
{%- set lo0 = interfaces|json_query( '[?name == `lo0`].name' ) -%}
{%- set lo0_test = "no" -%}
{%- if lo0|length > 0  -%}
  {%- set lo0_test = "yes" -%}
{%- endif -%}
{%- set md = {} -%}
{%- set _do = md.update({"lo0_exists":lo0_test}) -%}
{{md}}
```

This can be created anyway you like, the commands are simply:
```
config system interface
  edit "lo0"
    set type loopback
    set vdom "root"
    set allowaccess ping
    set description "Create lo0 for monitor test"
  next
end
```

You can use FortiSOAR to run scripts arbitrarily. In FortiSOAR and `FortiManager -> Scripts` you can open the record `Create Loopback - lo0 - DeviceDB` and run the `Render Script with a Device` automation. This will provide you with the CLI you need to create the Loopback and you can run this via the FMG API. If you want to see the script created in FMG then check the `Skip Delete Script` option and go check FortiManager. 

![](./res/usage/ztpflowtest-script-render-create-lo0.png)

Once this is done the ZTP Profile should complete monitoring. 
![](./res/usage/ztpflowtest-monitoring-phases-complete.png)