# Quick Start

Let's get our first self driving car agent up and running locally and talking to a cloud simulator. 

In this quick start we'll first be getting a CARLA instance up and running in the cloud with a local agent that can manually drive through the simulator. 

Once we've successfully manually driven the car, we'll move on to testing a smarter self driving agent running in the cloud. 

Finally, we'll wrap the quick start by should you how to optimize your time by running the agent in a headless. 

## Let's Get Going
1. [Install the Alpha Drive CLI](/docs/cli/install)
2. [Authenticate the CLI](/docs/cli/authenticate)

    ```
    alpha login
    ```

3. [Enable visual diplay](/docs/display/visual) (Optional)


4. [Initialize the workspace](/docs/cli/init)

    ```
    alpha init demo
    ```

    This demo includes a couple of pre-written agents. 

    ```
    cd demo
    ls
    ```

    You will find a manual_control.py that will allow you to drive the car in simulation and an example_agent.py in the directory that is our (lame) attempt at a self driving agent. We hope yours will be much better.

    You'll notice that there is another file in the directory called alphadrive.yml. You can read more about it in the [init] (/docs/cli/init) section, but let's take a peak under the hood:

    ```
    description: AlphaDrive Agent Configuration
    global: {}
    profiles:
      default:
        simulator: carla
        launch_command: python -u example_agent.py
        map: /Game/Carla/Maps/Town05
      manual:
        simulator: carla
        map: /Game/Carla/Maps/Town05
        launch_command: python -u ../manual_control.py --host=$SIM_HOST --port=$SIM_PORT --res=640x480
     ```

    You'll notice that there are two profiles - manual for our own driving abilities and the default which is an agent driving. The purpose of this file is to provide configuration so that we can automate as much as possible and make getting up and going super easy. 

5. Take a test drive:

    So now let's take one of those profiles out for a spin. So manual in this case doesn't refer to stick shift, but we will be using our handy keyboard controls to drive the direction of the car. Give it a go.

    ``` 
    alpha drive manual
    ```

    To explain, your manual agent is running locally on you box and connecting to a CARLA instance up in the cloud. It's like magic - but don't be fooled - it was a lot of work to make that magic happen. 

    ** Of note: while this works on OSX and Linux, but the performance is much better on a GPU enabled machine (i.e. linux, which can achieve 60fps on the client side)

    Now, to put the peddle to the metal. How does an agent perform without you at the wheeel?

    ``` 
    alpha drive
    ```

    You'll notice that there wasn't a display enabled here. We are able to run the agent in headless mode which means that it can run much faster. 

6. See the results

    When running a local agent with the cloud simulator the output from the simulator is stored locally. See for yourself. 

    ```
    ls
    ```

    You should see a \_out folder. If you navigate to that folder you will see a surprise inside. DATA. You can configure what data is needed and it will be outputted locally. 

    Viewing the example_agent.py file you can see an example:

    ```
    camera.listen(lambda image: image.save_to_disk('_out/%06d.png' % image.frame_number, cc))
    ```

7. Running in Parallel

    We aren't talking about parallel parking just yet, we're talking about being able to run multiple instances of the CARLA simulator in the cloud. This comes enabled by default. You can kick off as many 'alpha drive (profile)' commands as you like (just be careful as you will be charged for time of each server running). [More about running parallel simulations] (/docs/cloud/cloudai)

    Our professional level provides more tools for developers to optimize their parallel development runs. [More about Cloud AI and Cloud Simulation] (/docs/cloud/cloudai)

Questions, comments, concerns or copy edits? Email us at [support@alphadrive.ai](mailto:support@alphadrive.ai) 
