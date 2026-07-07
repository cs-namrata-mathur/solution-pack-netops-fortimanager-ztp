| [Home](../../README.md) / [Usage](../usage.md) |
|------------------------------------------------|

# Jinja Rendering with Metafield Sources

Metafield sources extend the data available to Jinja templates by retrieving additional information at render time. This allows templates to generate dynamic output using data from FortiManager, FortiSOAR playbooks, or other supported sources.

You can use metafield sources with the following record types:

 - ZTP Profile Assignment (Search Type = `Jinja`)
 - Metafield Templates
 - Script Templates

When a template is rendered, the framework automatically provides the following Jinja variables: 
 - `record`— The current record being processed.
 - `devmeta`— Device metadata associated with the current record.

You can inspect the available variables while rendering script templates or metafield templates by looking for the `get_jinja_variables` step or `jinja_vars` variable within the **Render Jinja** playbook execution logs. 

You can use these variables directly in playbooks by opening any playbook and in the Playbook Designer click `Tools > Jinja Editor` and pasting the Jinja variables into the `JSON` section of your `Jinja Template` to test and validate Jinja templates during development.

```
{
  "jinja_vars": {
    "record": {
      "id": 258,
      "sn": "FG00001",
      "adom": "root",
      "vdom": "root",
      "devname": "FG00001",
      "firmware": "7.2.0-build1514",
      "hostname": null,
      "platform": "FortiGate-80F-POE",
      "meta_fields": {
        "Address": "",
        "Contact Email": "admin@test.com",
        "Company/Organization": "",
        "Contact Phone Number": ""
      },
      "connector_config": "FMG1-1"
    },
    "devmeta": {
      "site_id": 100,
      "site_subnet": "192.168.10.0/24"
    }
  }
}
```

A metafield source retrieves additional data and appends it to the Jinja variable set under the configured `keyname`. The resulting object (with additional custom data) becomes available as a new attribute that can be referenced within the Jinja template. This mechanism allows templates to combine record data with dynamically retrieved information from external sources.
 
```
{
  "jinja_vars": {
    "record": {},
    "devmeta": {},
    "metafield_source_keyname": {}
  }
}
```

## Metafield Source Types

A metafield source defines how additional data is retrieved before a Jinja template is rendered. The framework supports the following source types:

 - `fmg` 
 - `playbook`

A metafield source can define one or more data sources and combine both API (fmg) and playbook source types. Each source type uses a different schema because the required parameters depend on how the Jinja variables are retrieved. The following sections describe each supported source type and its schema.

### Metafield Source Type: `fmg`

#### Schema (fmg)

The `fmg` source type retrieves data directly from the FortiManager API by performing a `JSON-RPC GET` request against the endpoint specified in the `location` field. Use this source type to retrieve FortiManager objects such as device tables, interfaces, policies, or other configuration data that should be available during template rendering.

```
[
  {
    "source_type": "fmg",
    "source": "<fmg connector config_name>",
    "keyname": "<attribute name for data>",
    "location": "<api url>"
  }
]
```

#### Example (fmg)

The following example retrieves the `interface` table for the device associated with the current record where we are rendering the Jinja. The returned data is added to the Jinja variables and can be referenced within the template.

```
[
  {
    "source_type": "fmg",
    "source": "{{record.connector_config}}",
    "keyname": "interfaces",
    "location": "/pm/config/device/{{record.devname}}/global/system/interface"
  }
]
```

### Metadata Source Type: `playbook`

#### Schema (playbook)

The `playbook` source type executes a FortiSOAR playbook and uses its output as Jinja variables. Custom playbooks can implement any logic required to build Jinja variables by retrieving data from supported integrations, transforming it as needed, and returning the results for use during template rendering.

```
[
  {
    "source_type": "playbook",
    "keyname": "<attribute name for data>",
    "playbook_id": "<custom playbook id found in url of playbook>",
    "payload": "<input data for playbook>"
  }
]
```

#### Example (playbook)

The following example is used by the ZTP Flow feature examples. The referenced playbooks perform simple processing and return sample data to demonstrate how custom playbooks can extend the Jinja rendering framework.

```
[
  {
    "source_type": "playbook",
    "keyname": "ip_table",
    "playbook_id": "9e6cff1f-0fe3-4c85-bf25-5cb409cea778",
    "argument": ""
  },
  {
    "source_type": "playbook",
    "keyname": "ip_table_lookup",
    "playbook_id": "1c6e570a-605e-46ac-9125-32f2af1877e7",
    "argument": "{{record.devname}}"
  }
]
```
