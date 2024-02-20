Compiled by Sabari Kumar, 20240220 for the CHEM578B class @ CSU.

Below are detailed instructions for getting conda set up/installing OpenMM.

Use the following steps to install conda. Note that these specific steps are for the version of the conda installer specified here; if you use a different version, you may get different prompts.

1. Download the conda installer using `wget https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh`
2. Run `bash https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh` to install conda. Follow the prompts, as detailed below. 
    a. Press Enter to scroll through the license agreement. Pressing q will take you to the bottom.
    b. Type yes and press Enter to accept the license terms.
    c. Press Enter to confirm the default /home/YOURUSER/anaconda3 install location.
    d. Update your shell profile to automatically initialize conda by typing yes, then press Enter to confirm (note that this is not the default option!)
3. Log out of your current session and log back in to initialize conda.
4. You should see `(base)` before your username in the bash prompt - if not, run `conda init` and repeat step 3

Once you have access to a machine with a GPU, follow these steps to install a GPU-accelerated version of OpenMM:

1. Create a new conda environment with Python 3.9 using `conda create -n YOURENVNAME python=3.9` Feel free to change `YOURENVNAME` to whatever you want. Please use a descriptive environment name. In my experience, Python 3.9 has the broadest compatibility with computational chemistry/machine learning software packages.
2. Activate your new environment: `conda activate YOURENVNAME` You should see that the `(base)` before your login prompt has changed to `(YOURENVNAME)`
3. Install jupyter lab in your conda environment: `conda install -c conda-forge jupyterlab`
4. Find the CUDA toolkit version installed using: `nvcc --version` Don't use `nvidia-smi` to do this - `nvidia-smi` displays the highest supported CUDA toolkit version, not the one that is currently installed.
5. Install openmm: `conda install -c conda-forge openmm cudatoolkit=YOURCUDATOOLKITVERSION`. Despite what the OpenMM website says, sometimes the conda installer will fail to automatically determine the proper CUDA toolkit version, leading to complex package incompatibility issues. You should manually specify the `cudatoolkitversion`. If no GPU is found, the CPU version will be installed.
    a.Test your OpenMM install using `python -m openmm.testInstallation` NOTE: There is an open bug that may cause this command to fail with particular nvidia drivers - this is not an issue with the CUDA installation (as the error message suggests) but with the OpenCL implementation and can be safely ignored (unless for some reason you want to use the OpenCL implementation). See https://github.com/openmm/openmm/issues/3329 for further details. This can be fixed by upgrading nvidia drivers from 495.29.05 to a more current version.
6. Install openmm-setup: `conda install -c conda-forge openmm pdbfixer flask`

Finally, to run jupyter on your remote server from your local laptop, use the following instructions:

1. Start a terminal session, log in to your remote computer, activate your conda environment, and start a jupyter server on a specific port number using `jupyter lab --port YOURPORTNUMBER` In general, pick a port > 8000; the default is 8888, and jupyter will automatically switch to another port if the one you specify is already in use. Jupyter will print some log messages, along with a link to the running server. Make note of the port number specified in this link, and copy the link
2. In another terminal session, run `ssh -L YOURPORTNUMBER:localhost:YOURPORTNUMBER  YOURACCCOUNT@YOURREMOTEMACHINE` This will forward `YOURPORTNUMBER` from the remote machine to your local computer.
3. In your preferred web browser, and paste the copied link. You should now be able to access the jupyter server running on the remote machine.

