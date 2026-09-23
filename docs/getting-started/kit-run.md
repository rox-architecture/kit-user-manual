# Getting Started - 3. Running a KIT

In this guide, we will import and run a simple KIT using GUI: *customisable robot arm 3D simulation*.

We will use the following tool(s):

- [KIT framework (Github repository)](https://github.com/rox-architecture/kit-framework-deployment)

## Preliminary Requirement

- KIT framework is installed (completed the [Installation guide](../installation.md))

## Video Guide

A video guide for the instructions in the following sections.

<div style="position:relative;padding-bottom:56.25%;height:0;overflow:hidden;">
  <iframe
    src="https://www.youtube.com/embed/ZyVOqnt4QhE"
    style="position:absolute;top:0;left:0;width:100%;height:100%;"
    frameborder="0"
    allowfullscreen>
  </iframe>
</div>

## Written Guide

Download the KIT stored at [https://github.com/rox-architecture/kit-collections/blob/main/examples/customisable-robot-arm-simulation-kit-v1.2.3.json](https://github.com/rox-architecture/kit-collections/blob/main/examples/customisable-robot-arm-simulation-kit-v1.2.3.json)

Or you can pull the entire repository, and find the file in the `/examples` directory.
```
git clone https://github.com/rox-architecture/kit-collections.git
``` 

### Step 1. Import KIT

Access GUI at [localhost:8088](http://localhost:8088/).

Click "Import" button and select the KIT file you downloaded.

### Step 2. Initial checks

We first check that the backend terminal is visible.

Click the grey button (with monitor icon) on the top menu.

Use the drop-down menu to switch the terminal to `Worker 1`.

If you don't see any terminal outputs, then your docker socket in the [installation](./../installation.md) was not set correct.

Then, we will check if we can see the Artifact directory by clicking the yellow button (folder icon) on the top menu.

The default path should be `/artifacts` if you did not reconfigured.

If both terminal and explorer work fine, move to the next step.

### Step 3. Negotiation

Before to run the KIT, the backend expects all the dataspace assets to be negotiated already.

Thus, open `Add Node` menu and find two dataspace assets in the KIT:
- ur5e_simulation_model.zip
- pybullet_sim_empty_env.tar

If you did not negotiate these assets yet, this asset appears without the hand-shaking icon.

Alternatively, you can click `Consolidated Requirements` to see the list of assets requiring negotiation.

If you don't see these assets in the Add Node menu, it means that you are not allowed to see them.

Then, you need to ask the asset provider to create a contract policy for you.

Before to go to the next step, make sure that the two assets are negotiated.

### Step 4. Trigger execution

Open the termianl `Worker 1`. 

While keeping the terminal opened, click the green (play icon) button on the top menu to run the KIT.

A textbox will pop-up, and press OK.

Observe the terminal and wait until the execution completes. 

It may take few minutes depending on your internet speed because the pybulle simulation container image needs to be pulled.

Since the Bash Command node has the `docker run` command, the simulation will be triggered automatically after fetching dataspace assets.

Since the Bash Command node is our last node in the KIT, you want to see the message like to know that the execution is completed.

```
Task cee.execute_node[3685fa51-5531-4325-997a-905ae04a395f] succeeded in 31.990098208014388s: {'execution_id': '9222661a-10b5-446a-80fd-60420210c1dc', 'node_id': 'bash_command-1789055057553', 'outputs': {}}
```

### Step 4. Check the generated outputs

This KIT is designed to produce a simulation video at the end of the simulation.

Click the yellow button to open the Artifacts directory. 

You can see `simulation.mp4` is generated. Watch the simulation result (currently random control policy).

Also, the `robot` directory contains the UR5e robot arm URDF and Mesh data.

### Step 5. Check Execution History

Open the Execution History in the top menu. It shows the workflow ID (KIT's backend ID) and its execution reference ID, along with the state either (FINISHED or FAILED).

### Step 6. Customise with another robot arm

Now, we want to swap the robot arm model with another manufacturer's robot, for instance ABB's IRB1200 robot.

Delete the UR5e node by clicking the trash bin icon under the node.

Open `Add Node` menu and search for `irb1200` in the search bar and negotiate the `irb1200_simulation_model.zip` asset.

Load the asset into the graph and connect to `Save as a File` node like before.

That's it! Click the play button to run the KIT, watch the simulation.mp4 in the artifacts directory. 

This time, you see ABB robot in the simulation.

In the explorer (yellow button), you can also click `Clear All` to remove all files.













