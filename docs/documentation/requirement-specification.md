# Requirement Specification Language

We developed a small domain specific language to specify requirements in the metadata:

- **hardware** – physical and computational resources, such as sensors, hardware, memory, GPU, and disk capacity.
- **software** – software components and runtime capabilities, including installed tools, APIs, services, network connectivity, ports, and filesystem access.

All requirements in the same list are conjunctive, meaning that every requirement must be satisfied.

## Language Grammar

Every requirement expression is constructed using one of two structures:

- Subject + Operator
- Subject + Operator + Value

For example, below are valid expressions:

### Subject and Value

Subject and Value are namespaces. For example, `hardware.compute.cpu` or `software.docker.runtime`. 
Currently, there is no strict restrictions on creating namespaces, but it is generally recommended to follow the semantic model structure specified in [RODEOS](https://github.com/rox-architecture/RODEOS).

For instance, the below heirarchy can be considered.
```
hardware
├── compute
│   ├── cpu
│   ├── memory
│   ├── gpu
├── robot
├── end_effector
├── sensor
│   ├── camera
│   ├── lidar
│   ├── radar
│   ├── imu
│   ├── force_torque
│   ├── proximity
│   └── encoder
└── interface
    ├── usb
    ├── ethernet
    ├── serial
    ├── i2c
    └── gpio

software
├── os
├── runtime
├── framework
├── middleware
├── api
├── service
├── network
├── filesystem
├── package
└── permission
```

### Operator

Operator is the bridge between Subject and Value. 
Depending on the operator, Value can be not required.
Currently, we identify the following operators:

| Expression         | Description                                                                   |
| ------------------ | ----------------------------------------------------------------------------- |
| `x required`       | The specified resource, API, service, or interface must be available for use. |
| `x = value`        | The specified property must have the exact value.                             |
| `x >= value`       | The specified property must meet the minimum value.                           |
| `x <= value`       | The specified property must not exceed the maximum value.                     |
| `x in {a, b, ...}` | The specified property must match one of the listed values.                   |

Notice that only the `required` operator requires no Value.

### Examples

```text
kubernetes.api required
memory >= 8 GB
cpu.architecture in {amd64, arm64}
```

```text
hardware.sensor.camera.type = depth
hardware.sensor.camera.frame_rate >= 30 Hz
```

```text
software.runtime.python >= 3.12
```

```text
hardware.robot required
hardware.robot.model = UR5e
```