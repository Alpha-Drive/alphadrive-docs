# Getting Started

Last Updated: February 20th, 2019

## What is Alpha Drive?

Alpha Drive provides hosted cloud simulators and a dockerized agent
environment. Your agent script is all you need to bring to get
started.

The current system provides instances of CARLA 0.9.3 in the cloud.

When you use the *alpha* command on your local workstation, it will
launch a simulator instance in the cloud and then launch your agent on
your local machine inside a docker container.

The docker container (named *alphadrive/cli*) has everything your
python-based agent script needs to run including the simulator client
library, python 3.5 and 2.7.

## Install the CLI

Note: The CLI is currently supported only on MacOS and Ubuntu 16.04 or
newer. Please let us know if you need other platforms.

## Prerequisites

Prior to installing the *alpha* command. You will need the following:

* A local docker installation (https://www.docker.com/get-started)
* cURL (https://curl.haxx.se)

## Install the alpha drive command

To install:

    bash <(curl -o - https://raw.githubusercontent.com/Alpha-Drive/alpha-command/master/install.sh)

This will download a shell script and place it in the folder of your choosing.

Once you have that installed, you can login via the cli:

    alpha login

This will pop open a browser window and take you through the login process. When complete, your authorization token will reside in *$HOME/.alphadrive/auth*.

Now you are ready to setup Alpha Drive integration with an agent.

## Test Drive with Alpha Drive Samples

We have a sample repository with a slightly modified version of a CARLA sample script.

```
git clone git@github.com:Alpha-Drive/alphadrive-samples.git
cd alphadrive-samples
alpha login
alpha drive
```

After you run this, you will find a folder *_out* under the
alphadrive-samples folder that contains the output from the sample
agent.

## Setting up Alpha Drive with Your Agents

Alpha Drive can have different profiles for the same agent code base. The *alpha setup* command can help you create the initial *alphadrive.yml*.

```
alpha setup
```

The YML file that gets created for linux or mac might look like this:

```
description: Alpha Drive Agent Configuration
profiles:
  default:
    simulator: carla
    launch_command: python tutorial.py --host=$SIM_HOST --port=$SIM_PORT
  bench:
    simulator: carla
    benchmark: true
    fps: 20
    map: /Game/Carla/Maps/Town05
    launch_command: python driving_benchmark.py --host=$SIM_HOST --port=$SIM_PORT
```

In the second example, you can see the `benchmark`, `fps`, and `map` parameters. These will result
in the corresponding options being set when the simulator starts.

## Running a Profile

Once you've completed the setup process, you can go for a drive.

    alpha drive <profile>

### Default profile

To run the default profile:

    alpha drive

As specified above, the default profile will launch a cloud simulator
and then run your agent and the VNC viewer so you can watch the
results.

## CARLA and Alpha Drive Notes

### Host and Port of the Simulator

Many of the sample CARLA scripts assume connection to a simuator on
the local workstation. Your script must have some way to specify the
host and port. When we execute your agent, we define *SIM_HOST* and
*SIM_PORT* in the environment. You can either use these as command
line arguments as shown in the sample YML above or you can have your code
use *os.environ* to access those values.

E.g., 

```
client = carla.Client(os.getenv('SIM_HOST'), int(os.getenv('SIM_PORT')))
```

*It is crucial that the SIM_PORT be provided to the client constructor
as an integer.*

See *spawn_npc.py* for an example of using *argparse* to get those values off
the command line.

### Saving Output

When you execute the *alpha drive* command, the current directory will
be mounted inside the docker container and your script can write files
to it. Use relative paths when you save images and other data to disk.

The *tutorial.py* example in the samples repository shows how this can be done:

```
camera.listen(lambda image: image.save_to_disk('_out/%06d.png' % image.frame_number, cc))
```

### Visualizations (Optional)

For visualizations, you will need a local X11 installation. Note that this is 
a feature of the service that will be enhanced with a more streamlined experience soon.

#### DISPLAY Environment Variable

If your environment where you execute the alpha command has DISPLAY
set, then the alpha script will do its best to ensure that programs
inside the docker container can display windows locally. 

*Note: We currently only support connecting to the X11 server on your local machine.*

#### X11 Server on MacOS

XQuartz, but default, does not allow network connections to the X11 server. To enable network
connections, you can run this command in terminal:

```defaults write org.macosforge.xquartz.X11 nolisten_tcp -int 0```

After changing the default as above, you need to quit and restart the XQuartz application.

*Warning: There are definite security risks involved in allowing network
connections to X11 on any system. Future iterations of the service will not
require X11.*
