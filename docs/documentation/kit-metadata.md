# KIT Metadata

In a dataspace, everything is represented as an asset: raw data, configuration files, container images, service endpoints, continuous data streams, and so on.
However, having no type creates ambiguity regarding how they should be accessed, consumed, and deployed.
To address this ambiguity, we identify a few operational types based on how KIT needs to access and handle them from the dataspace.

## Operational Types

1. **Static File**: A finite data that can be directly transferred (e.g., CSV, JSON, images)
2. **Container**: A software package distributed as a container image (e.g., Docker/Archive/OCI image)
3. **File Service**: A service endpoint that returns a finite file upon invocation (e.g., image conversion, report generation)
4. **Streaming Service**: A service endpoint that provides a continuous stream of data (e.g., camera feed)

It is important that each dataspace asset you publish must indicate exactly one operational type in the metadata.
For example:

```json
{
    ...
    "name": "my-asset",
    "operational_type": "container",
    ...
}
```

Depending on the operational type, KIT expects you to provide small additional information:

## Static File

| Field                   | Type          | Required | Description                                                                 |
| ----------------------- | ------------- | :------: | --------------------------------------------------------------------------- |
| `operational_type`      | `enum`        |     ✓    | Fixed value: `static_file`.                                                 |
| `semantic_model`        | `JSON object` |     ✓    | JSON object describing the semantic meaning and structure of the asset.     |
| `contact_email`         | `string`      |     ✓    | Email address of the contact person responsible for maintaining the asset.  |
| `file_format`           | `string`      |     ✓    | File format (e.g., `csv`, `json`, `jpg`, `mp4`).                            |
| `file_size`             | `integer`     |          | Size of the file in bytes.                                                  |
| `checksum`              | `string`      |          | SHA-256 checksum used to verify file integrity.                             |
| `encoding`              | `string`      |          | Character encoding for text-based files (e.g., `UTF-8`).                    |
| `hardware_requirements` | `JSON object` |          | Minimum or recommended hardware resources required for execution.           |
| `software_requirements` | `JSON object` |          | Required software, runtime, drivers, or platform dependencies.              |

In particular, the KIT framework backend defines the following Pydantic schema for the static file operation:

```python
class ParamSpec(BaseModel):
    provider_bpn: str
    provider_url: HttpUrl
    asset_id: str
```

## Container

| Field                   | Type          | Required | Description                                                                                      |
| ----------------------- | ------------- | :------: | ------------------------------------------------------------------------------------------------ |
| `operational_type`      | `enum`        |     ✓    | Fixed value: `container`.                                                                        |
| `semantic_model`        | `JSON object` |     ✓    | JSON object describing the semantic meaning and capabilities of the asset.                       |
| `contact_email`         | `string`      |     ✓    | Email address of the contact person responsible for maintaining the asset.                       |
| `distribution_type`     | `enum`        |     ✓    | Distribution method of the container asset: `oci_registry`, `image_archive`, or `dockerfile`.    |
| `image_name`            | `string`      |     ✓    | Name of the container image (e.g., `object-detector`).                                           |
| `image_tag`             | `string`      |     ✓    | Tag identifying a specific image version (e.g., `1.2.0`, `latest`).                              |
| `platforms`             | `set<enum>`   |     ✓    | Supported target platforms: `linux/amd64`, `linux/arm64`, `windows/amd64`, `windows/arm64`.      |
| `hardware_requirements` | `JSON object` |          | Minimum or recommended hardware resources required for execution.                                |
| `software_requirements` | `JSON object` |          | Required software, runtime, drivers, or platform dependencies.                                   |

In particular, the KIT framework backend defines the following Pydantic schema for the container operation:

```python
Platform = Literal[
    "linux/amd64",
    "linux/arm64",
    "windows/amd64",
    "windows/arm64",]

class ParamSpec(BaseModel):
    provider_bpn: str
    provider_url: HttpUrl
    asset_id: str

    representation: Literal[
        "dockerfile", 
        "source_archive", 
        "docker_archive"]
    platforms: set[Platform] = Field(min_length=1)

    image_name: str
    image_tag: str
    registry_addr: str | None = None
```


## File Service

| Field                   | Type          | Required | Description                                                                                      |
| ----------------------- | ------------- | :------: | ------------------------------------------------------------------------------------------------ |
| `operational_type`      | `enum`        |    ✓     | Fixed value: `file_service`.                                                                     |
| `semantic_model`        | `JSON object` |    ✓     | RODEOS semantic model describing the semantic meaning and capabilities of the service.           |
| `contact_email`         | `string`      |    ✓     | Email address of the contact person responsible for maintaining the asset.                       |
| `file_format`           | `string`      |    ✓     | Format of the file returned by the service (e.g., `csv`, `json`, `jpg`, `mp4`).                  |
| `request_method`        | `enum`        |    ✓     | HTTP method used to invoke the service (e.g., `GET`, `POST`).                                    |
| `subpath`               | `string`      |    ✓     | Relative API path appended to the service endpoint (e.g., `/convert`, `/reports/generate`).      |
| `api_documentation_url` | `URI`         |          | URL of the API documentation, such as a Swagger UI page or OpenAPI document.                     |

In particular, the KIT framework backend defines the following Pydantic schema for the service operation:

## Streaming Service

Currently WIP

## Semantic Model

The `semantic_model` is the RoX semantic model based on [https://github.com/rox-architecture/RODEOS](https://github.com/rox-architecture/RODEOS).

## Hardware / Software Requirements

To specify `hardware_requirements` and `software_requirements` in the metadata, we developed a simple domain-specific language described [here](./requirement-specification.md).