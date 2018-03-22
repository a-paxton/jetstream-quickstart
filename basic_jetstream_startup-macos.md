# Getting started on Jetstream (for macOS)

## Initializing Jetstream
  1. Head to Jetstream: https://use.jetstream-cloud.org
  1. Log into Jetstream using your XSEDE user credentials.
  1. From the "Projects" tab, create a new project by clicking the "Create new project" button.
      * Use a project name that will be unique among your project.
      * Include a few words as a project description.
      * Click "Create".
  1. Select the desired project from the "Projects" tab.
  1. Create a new instance for the project.
      * Under the "Resources" tab for the project, click "New" and select "Instance" from the resulting drop-down menu.
      * Search for `Ubuntu 14.04.3 Development GUI` and click to select.
      * If desired, rename the instance to something more informative.
      * Add bash setup script to the instance:
          * Click "Advanced Options".
          * Select "Create a new script".
          * Into the "Script URL" box, paste `https://raw.githubusercontent.com/aculich/jetstream-setup/master/jetstream-setup.sh`
              * This includes setup for terminal multiplexer, latest version of Docker, and other helpful information
          * Give it a useful name.
          * Click "Save and add script".
          * Click "Continue to launch".
      * Check to make sure that the correct allocation source, provider, and instance size are specified.
      * Click "Launch instance".
  1. Create a new volume for the project.
      * Under the "Resources" tab for the project, click "New" and select "Volume" from the resulting drop-down menu.
      * Check to make sure that the provider is **identical** to the provider you specified for the instance.
      * Give it a useful name.
      * Update the volume size.
      * Click "Create Volume".
  1. Wait for instance to be ready.
      * Jetstream will send an email notification to you when your instance is ready to use.
  1. Attach volume to instance.
      * Click name of new volume.
      * Click "Attach".
      * Select name of instance.
      * Click "Attach".

## Preparing Jetstream
  1. If paused/stopped, re-start the Jetstream instance.
      * Log into Jetstream using your XSEDE user credentials.
      * Select desired project under the "Projects" tab.
      * Select the desired instance by clicking on its name.
      * Click "Resume".
      * Wait status to change to "Active".
  1. Add instance IP address to `~/.ssh/config` file on local computer.
      * **Note**: The IP address may change if you have resumed the instance after having paused it. Be sure to update it in your `config` file as needed.
      * Example addition to your `config` file:
              
               
              Host your_instance_nickname
                 Hostname 123.456.789.000
                 User your_xsede_username
              
          * For "Host", create a nickname for the instance. (*Tip*: Be informative.)
          * For "Hostname", use your instance's IP address.
          * For "User", use your XSEDE username.
  1. From local terminal window, securely connect to the instance: `ssh your_instance_nickname`
      * If it requests a password, add key to VM:
          * On local terminal: `cat ~/.ssh/id_rsa.pub | pbcopy`
          * In Web Shell: `cat >> ~/.ssh/authorized_keys` then Control+D
      * If you'd like to add another key (i.e., not your own computer's) to your VM:
          * In remote terminal or Web Shell: `curl https://github.com/GITHUB_USERNAME.keys >> ~/.ssh/authorized_keys`
  1. Execute Dockerfile in remote terminal window: `./run-container`
      * **Note**: Re-run this step even if you're simply resuming a stopped/paused instance.
  1. Move data from local computer to volume.
      * In remote terminal window, create a new folder for your data.
          * Move into your volume: `cd /vol_b/`
              * **Note**: Volumes are named using consecutive letters (starting from `b`) fresh and on the fly after resuming from a stopped/paused state. Keep this in mind if you are attaching and then detaching multiple *different* volumes from your instance without pausing/stopping it each time.
          * Create new directory for your data: `mkdir data`
      * In local terminal window, copy entire data folder from laptop to server in local terminal window: `rsync -avP ~/local/path/to/data/folder/ your_instance_nickname:/vol_b/data/`
          * **Note**: The trailing slashes are *vital* for moving entire folders.

## Running Jupyter and RStudio on Jetstream
  1. Go to remote Jupyter in local browser.
      * Go to hostname IP.
      * Copy Jupyter Notebook token from remote terminal window into login on local browser.
  1. Click "New" to start a new computational environment:
      * Start a new Jupyter notebook by selecting the appropriate environment for your notebook (e.g., Python 2.7, R) from drop-down menu.
      * Start an RStudio instance by selecting "RStudio" from drop-down menu.
  1. Proceed as normal!
      * **Note**: It's helpful to double-check your working directory to be sure you're storing/reading data using the *volume* rather than the *instance*. Again, this will likely be `vol_b` (see note on volume directories in previous section).
