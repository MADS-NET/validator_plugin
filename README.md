[![Build and Release](https://github.com/MADS-NET/validator_plugin/actions/workflows/release.yml/badge.svg)](https://github.com/MADS-NET/validator_plugin/actions/workflows/release.yml) ![mads package](https://img.shields.io/badge/mads_package-available-blue)

# validator plugin for MADS

This is a Sink plugin for [MADS](https://github.com/MADS-NET/MADS). 

This plugins validates the JSON messages received from the subscribed topics against the JSON schemas defined in `schemas.json`. If a message does not conform to the schema, an error is logged.

*Required MADS version: 2.1.0.*


## Supported platforms

Currently, the supported platforms are:

* **Linux** 
* **MacOS**
* **Windows**

## Package install

with MADS v2.1.1 or later, install with:

```bash
mads package --install validator.plugin
```


## Install from binaries

Linux and MacOS:

```bash
cmake -Bbuild -DCMAKE_INSTALL_PREFIX="$(mads -p)"
cmake --build build -j4
sudo cmake --install build
```

Windows:

```powershell
cmake -Bbuild -DCMAKE_INSTALL_PREFIX="$(mads -p)"
cmake --build build --config Release
cmake --install build --config Release
```


## INI settings

The plugin supports the following settings in the INI file:

```ini
[validator]
schemas = "schemas.json" # path to the JSON schemas file (default="schemas.json")
sub_topic = [""] # subscribe to these topics for validation (default=[])
```

All settings are optional; if omitted, the default values are used.

**NOTE**: the plugin never deals with messages on the `agent_event` topic to avoid potential infinite loop, as the plugin itself may generate `agent_event` messages when validation fails.


## JSON schemas

The plugin expects a JSON file containing the schemas for validating the messages. The file should be in the following format:

```json
{
  "topic_name": {
    "$id": "https://example.com/schema_name.json",
    "$schema": "http://json-schema.org/draft/2020-12/schema",
    "properties": {
      // schema properties
    },
    "required": [
      // required properties
    ]
  },
  // more schemas for other topics...
}
```

There is a useful schema editor online: [JSON Schema Editor](https://json.ophir.dev). Simply select `Infer from JSON`, then possibly correct the resulting schema, and finally copy the schema into the `schemas.json` file, under a key that matches the topic name you want to validate.



## Executable demo

Runs some simple tests to demonstrate the plugin functionality. 

---
