# Quick Start

In this quick start we'll get a CARLA instance up and running in the cloud streaming to a local agent that can manually drive through the simulator using your keyboard controls. 

Once we've successfully manually driven the car, we'll move on to testing a smarter (but not too smart, we leave that to you) self driving agent. 

Finally, we'll wrap the quick start by showing you how to optimize your time by running the agent headless and in parallel. 

## Let's Get Going
1. [Install the Alpha Drive CLI](/docs/cli/install)

    ```
    bash <(curl -o - https://raw.githubusercontent.com/Alpha-Drive/alpha-command/master/install.sh)
    ```
    
2. [Authenticate the CLI](/docs/cli/authenticate)

    ```
    alpha login
    ```

3. [Enable visual diplay](/docs/display/visual) 
    Our current visualization relies on X11 and is available on Linux out of the box. 
    
    Mac is not officially supported, but if you need help, contact us and we can walk you through it. Windows does not support X11. 
    
    Our next release will have visualizations back in the browser (as our previous release did), but with added support for visuals from any/multiple sensors.


4. [Initialize the workspace](/docs/cli/init)

    ```
    alpha init demo
    ```

    This demo includes a couple of pre-written agents. 

    ```
    cd demo
    ls
    ```

    You will find a manual_control.py that will allow you to drive the car in simulation and an example_agent.py in the directory that is our attempt at a self driving agent. It really just sets up some agents in the world and returns back the data. We know your agents will be better. 

    You'll notice that there is another file in the directory called alphadrive.yml. You can read more about it in the [config](/docs/cli/config) section, but let's take a peak under the hood:

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

    You'll notice that there are two profiles - manual for our own driving abilities and the default which is an agent driving. The purpose of this file is to provide configuration for the input to the command line interface. 

5. Take a test drive:

    So now let's take one of those profiles out for a spin. Manual in this case doesn't refer to stick shift, but we will be using our handy keyboard controls to drive the direction of the car. Give it a go.

    ``` 
    alpha drive manual
    ```

    To explain, your manual agent is running locally on you box and connecting to a streaming CARLA instance up in the cloud. It's like magic - but don't be fooled - it was a lot of work to make it happen. 

    ** Of note: the performance is much better on a GPU enabled machine and can get upwards of 60fps

    Now, to put the peddle to the metal. How does an agent perform without you at the wheel? We mentioned that we've included a sample agent example_agent.py. 

    ``` 
    alpha drive
    ```

    You'll notice that there wasn't a display enabled here. We are able to run the agent in headless mode which means that it can run much faster. 
    


6. See the results

    When running a local agent with the cloud simulator the output from the simulator is stored locally. See for yourself. 

    ```
    ls
    ```

    You should see a \_out folder.  
    
    ```
    cd _out
    ```
    If you navigate to that folder you will see a surprise inside. DATA. You can configure what data is needed and it will be outputted locally.
    Viewing the example_agent.py file you can see an example:

    ```
    camera.listen(lambda image: image.save_to_disk('_out/%06d.png' % image.frame_number, cc))
    ```

7. Running in Parallel

    Being able to run multiple instances of the CARLA simulator in the cloud will greatly improve your productivity. This comes enabled by default. You can kick off as many 'alpha drive (profile)' commands as you like (just be careful as you will be charged for time of each server running). 
    
## Troubleshooting

Questions, comments, concerns or copy edits? Email us at [support@alphadrive.ai](mailto:support@alphadrive.ai) 
