# Getting Started - 1. Asset Creation

This guide walks you through the asset creation process using an example.

We will use the following tool(s):

- [RObotic Data EcOsystem Semantic Model (RODEOS)](https://github.com/rox-architecture/rox-rodeos-dlr-dataspace-integration) : RODEOS is a RoX semantic model generator based on a LLM. Given a technical documentation, it returns JSON data with already fill-in values. RODEOS can run locally, but we will use an online service at [here](https://dataspace-dev-wallet.base-x-ecosystem.org/rodeos)
- [DLR dataspace Web UI](https://vision-x-auth.base-x-ecosystem.org/realms/user/protocol/openid-connect/auth?client_id=react-app&redirect_uri=https%3A%2F%2Fvision-x-dataspace.base-x-ecosystem.org%2F&state=7714b649-6a69-4c00-9b51-8523496809ac&response_mode=fragment&response_type=code&scope=openid&nonce=139f9ee7-4b2e-4145-8aec-ac2ca678d406&code_challenge=_HpiXceLujyQjO66YFbjS58VTD5JqmAM4ALWglR_FI4&code_challenge_method=S256) : We will use it to register a dataspace asset manually.

Overall, the JSON data from RODEOS becomes part of the asset metadata. Asset registering can also be done programmatically using dataspace API, but it is not covered here.

## Preliminary Requirements

- The asset you want to register in the dataspace must already have an accessible URL.
- Identify *what is my asset type?*:
    - `static_file`: a single file (or a zip file for multiple)
    - `container`
        - "dockerfile": a Dockerfile 
        - "source_archive": an archive that contains a Dockerfile (e.g., Github repo zip file)
        - "docker_archive": a docker image archive made by `docker save`
        - Currently, OCI Registry or OCI archive types are not supported
    - `file_service`: a service end-point that returns a file or static data
    - `streaming_service`: a service end-point that streams data
    - `workflow` = a KIT

## Video Guide

A quick and simple guide via a recorded video. Written instructions (same as the video) are also provided in the following sections.

<div style="position:relative;padding-bottom:56.25%;height:0;overflow:hidden;">
  <iframe
    src="https://www.youtube.com/embed/1p2-ZD1TwLQ"
    style="position:absolute;top:0;left:0;width:100%;height:100%;"
    frameborder="0"
    allowfullscreen>
  </iframe>
</div>

## Written Guide

### Step 1. Semantic model 

First, have your asset available via URL.
In this example, we will use an open-source dataset [SortScrew](https://github.com/ATATC/SortScrews/tree/main) to create an asset.

Go to the online RODEOS service at [https://dataspace-dev-wallet.base-x-ecosystem.org/rodeos](https://dataspace-dev-wallet.base-x-ecosystem.org/rodeos).

Copy and paste the text description of your asset to have the semantic model automatically generated.

Check the generated values and enter any missing values.

### Step 2. Asset Registration

Next, login to the [DLR dataspace Web UI](https://vision-x-auth.base-x-ecosystem.org/realms/user/protocol/openid-connect/auth?client_id=react-app&redirect_uri=https%3A%2F%2Fvision-x-dataspace.base-x-ecosystem.org%2F&state=7714b649-6a69-4c00-9b51-8523496809ac&response_mode=fragment&response_type=code&scope=openid&nonce=139f9ee7-4b2e-4145-8aec-ac2ca678d406&code_challenge=_HpiXceLujyQjO66YFbjS58VTD5JqmAM4ALWglR_FI4&code_challenge_method=S256).

Click "My Assets" menu on the sidebar. 

We need the access URL of the asset. In this case, we go back to the SortScrews repository and copy the download link.

In the dataspace UI "My Assets" page, paste this URL in the top text area and click "Add Asset" button.

In the pop-up modal menu, click the "include search engine metadata ... ". 

In the below, click "import JSON" and paste your semantic model into the appeared box, and click Apply.

Enter the compulsory fields like filename and description, then select `group-rox-only` policy at the bottom.

Finally, create an asset







