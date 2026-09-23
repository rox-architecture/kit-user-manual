# Graph Specification

## KIT Graph 

KIT is represented as a directed acyclic graph (DAG). Thus, no loop is allowed.

A graph is simply `KIT = {workflow_name, nodes, edges}`.

- workflow_name: name of the KIT
- nodes: list of nodes
- edges: list of edges

```json
"Graph": {
    "workflow_name": String,
    "nodes" : [],
    "edges" : []
}
```

### Node Definition

A node `v = {id, label, parameters, position}`.

- id: node ID used internally for KIT execution.
- label: name of the node. For **dataspace nodes, the label must match the asset name.**
- parameters: data from asset metadata needed to execute the node (See pydantic schema in [KIT metadata](kit-metadata.md))
- position: [OPTIONAL] node position required only for visualisation in GUI 

Examples:

```json
"nodes": [
  {
    "id": "container-1789053957702",
    "data": {
      "label": "pybullet_sim_empty_env.tar",
      "params": {
        "type": "dlr.container",
        "provider_bpn": "BPNLM67H9AVUVPTD",
        "provider_url": "https://vision-x-api.base-x-ecosystem.org/connectors/dlr-rox-conn/cp/protocol",
        "asset_id": "e73daa3d-0e25-4e7f-90e9-81dc227fa11d",
        "representation": "docker_archive",
        "platforms": [
          "linux/amd64"
        ],
        "image_name": "robotarm-simulator",
        "image_tag": "poc",
        "registry_addr": null
      }
    },
    "position": {
      "x": -92.88419043360128,
      "y": -174.35878850328044
    }
  },
  {
    "id": "save_to_file-1789053985743",
    "data": {
      "label": "Save as a File",
      "params": {
        "type": "save_to_file",
        "file_path": "robot/robot.zip"
      }
    },
    "position": {
      "x": -325.1338378258013,
      "y": 13.332207460753708
    }
  },
```

### Edge Definition

An edge `e = {source, target, source_port, target_port}`

- source: node ID of the source node
- target: node ID of the target node
- sourceHandle: name of the port on the source
- targetHandle: name of the port on the target

Important rules:

- Every node has input `dep` port and output `dep` port for dependency indication only. `dep` ports does not pass any data.
- For now, every node can have either no input data port or one input data port with the fixed port name `input-0`.
- For now, every node can have either no output data port or one output data port with the fixed port name `output_0`.

Example:

```json
"edges": [
    {
      "source": "save_to_file-1789053985743",
      "target": "unzipper-1789053988358",
      "sourceHandle": "dep",
      "targetHandle": "dep"
    },
    {
      "source": "container-1789053957702",
      "target": "bash_command-1789055057553",
      "sourceHandle": "dep",
      "targetHandle": "dep"
    },
    {
      "source": "unzipper-1789053988358",
      "target": "bash_command-1789055057553",
      "sourceHandle": "dep",
      "targetHandle": "dep"
    },
    {
      "source": "static_file-1790178290598",
      "target": "save_to_file-1789053985743",
      "sourceHandle": "output_0",
      "targetHandle": "input_0"
    }
  ]
```