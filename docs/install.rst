.. _install:

Installation Guide for Python
==================


Build From Source
-----------------

If you want to get nightly version of Simulator or running on native system, you can build Simulator from source. Currently, we only support building on Unix systems. This guide is based on Ubuntu 16.04.

Simualtor has little dependencies, so building from source is not scary.

1. Check that you have python 3 installed. Other version of python might work, however, we only tested on python with version >= 3.5.


2. Install cpp dependencies

.. code-block:: shell
    
    sudo apt update && sudo apt install -y build-essential cmake

3. Clone Simualtor project from github.

.. code-block:: shell
    
    git clone https://github.com/gjzheng93/COVID.git
    git checkout wrapping
    
4. Go to Simualtor project's root directory and run

.. code-block:: shell
    
    pip install .

5. Wait for installation to complete and CityFlow should be successfully installed.

.. code-block:: python
    
    import simulator
    eng = simulator.Engine

For Windows Users
------------------

For Windows users, it is recommended to run CityFlow under Windows Subsystem for Linux (WSL).