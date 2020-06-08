Try it yourself
*********

We provide a `Starter-Kit <https://github.com/prescriptive-analytics/starter-kit>`_ as guidelines for setting up the simulator and submit results. Here, we go through them in detail, and introduce the basic background about the simulator, template strategies and internvening APIs supported the simulator.


Installation Guide
==================

Our simulator can run in multiple platforms. Here, we provide two kinds of installation.


Install in your environment
---------------------------

1. Check that you have python 3 installed. Other version of python might work, however, we only tested on python with version >= 3.6.


2. Preparation

    2.1 Install boost library and make sure you have libboost path added to your system path. We sugguest to use `boost 1.58.0 <https://sourceforge.net/projects/boost/files/boost-binaries/1.58.0/>`_.

    - Mac OS: `brew install boost`

    - Linux:

        - Installation with `sudo apt-get install libboost-dev`
        - Locate libboost_system.so with `locate libboost_system.so`
        - Add the last step paths to your system path with `export PATH={your paths}:$PATH`

    - Windows: Please refer to `install-boost-build <https://www.boost.org/doc/libs/1_73_0/more/getting_started/windows.html#install-boost-build>`_


    2.2 Specify python environment 

    Make sure you have compatible python environment that marches with the simulator. Currently, we support the following versions of python:

    - Mac OS: python 3.6, 3.7

    - Linux: python 3.6


3. Clone Starter-Kit from GitHub.

.. code-block:: shell
    
    git clone https://github.com/prescriptive-analytics/starter-kit.git

    
4. Go to Simualtor project's root directory and run the following to test the installation

.. code-block:: python
    
    import simulator
    eng = simulator.Engine


.. note::
    You might need to rename the ``.so`` file that corresponds with your system and python to ``simulator.so``.


Use a docker
------------

1. Install docker with the `docker instruction <https://www.docker.com/products/docker-desktop>`_

2. Pull the docker image. Please pull from docker hub, with the following command in your terminal.

.. code-block:: shell

    docker pull episim2020/simulator:latest

4. Clone Starter-Kit from GitHub.

.. code-block:: shell
    
    git clone https://github.com/prescriptive-analytics/starter-kit.git


3. Create a docker container and map starter-kit into the containter by typing the following command in your terminal.

.. code-block:: shell

   docker run -it -v path/to/starter-kit/in/your/local/computer:path/to/starter-kit/in/docker/containter episim2020/simulator

4. You should have entered the container. Please navigate to the starter-kit folder in docker containter, then you can run the following command to start an experiment.

.. code-block:: shell

   python example.py


.. note::

    1. For other uses of docker, please refer to `docker run <https://docs.docker.com/engine/reference/run/>`_.

    2. Please pay attention to the security of your files, since docker container will be granted the access to change your files in the folders that you have mapped into the container. Please use carefully at your own risk.

    3. The dockerfile to build this image is also attached here. You can build your own image for personalized use. For this approach, please download the specified `anaconda <https://www.anaconda.com/products/individual>`_.  version. You need to put it in the same folder as the docker file. (Remember to change the file name in the dockerfile if you are using a different version.) Then, you can run the following command to build an image.

    .. code-block:: shell 

        docker build -t simulator -f simulator.Dockerfile 
        

    4. Docker container will be destroyed after you exit. If you wish to install your own package, we recommend you to build your own image based on our image. Please refer to `this link <https://docs.docker.com/engine/reference/commandline/build/>`_


Run Simulation
==============


Initiate engine
---------------


.. code-block:: python
    
    import simulator
    eng = simulator.Engine(thread_num=1, write_mode="append", specified_run_name="test")
    eng.reset() # reset() should be called right after the create of engine

- ``thread_num``: number of threads.
- ``specified_run_name``: results saving folder name.
- ``write_mode``: mode of saving simulation results, ``write`` will overwrite results from different rounds of simulation in the same ``specified_run_name`` folder, ``append`` will append the results from current simulation round to existing result files.


Simulate one step
-----------------


To simulate one step, simply call ``eng.next_step()``. All other data access/control APIs should be called after ``next_step()``.

.. code-block:: python

    eng.next_step()




Sample codes
------------

Here we provide a sample code for running our simulator, which can be found in the starter kit - `example.py <https://github.com/prescriptive-analytics/starter-kit/blob/master/example.py>`_. 

.. code-block:: python

    import simulator
    import os
    import json

    period = 840

    engine = simulator.Engine(specified_run_name="test")

    engine.reset() # reset should be called right after the create of engine

    for i in range(period):
        engine.next_step() # all data access/control APIs should be called after next_step()
        print(engine.get_current_time())
        print(engine.get_individual_visited_history(1))
        print(engine.get_individual_infection_state(1))
        print(engine.get_individual_visited_history(1))
        print(engine.get_area_infected_cnt(1))

        engine.set_individual_confine_days({1: 5}) # {individualID: day}
        engine.set_individual_quarantine_days({2: 5}) # {individualID: day}
        engine.set_individual_isolate_days({3: 5}) # {individualID: day}
        engine.set_individual_to_treat({4: True}) # {individualID: day}

    del engine



Results
=======

During simulation, the simulator will generate the submission file ``sub_xxx.txt`` and log files.  ``xxx`` corresponds with your ``specified_run_name`` when initiating the engine ``simulator.Engine(specified_run_name="xxx")``.


Submission
-----


Before submission, make sure:
 
- You are running the simulation for 840 time steps (60 simulation days in simulator). 

- You are required to set the engine write mode to "append" with ``simulator.Engine(write_mode="append")``, and run 10 times of your subsequent codes. 

- Please upload the ``sub_xxx.txt`` to the website.


Here we provide a sample code of simulation that matches with submission requirements, which can be found `here <https://github.com/prescriptive-analytics/starter-kit/blob/master/submit.py>`_.



Logs
--------------------

We also provide simulaiton logs to competetors.


1. The city-wide daily log file ``cnt_xxx.txt``.

2. The area level daily log file ``hex_cnt_xxx.txt``.

3. The city-wide daily r file ``r0_xxx.txt``.


Their Formats are as follows:

1. 'cnt_xxx.txt':

+----+--------------------+-----------+--------------+---------------------------------------------------------+
| #  | Name               | Data Tpye | Example Data | Description                                             |
+====+====================+===========+==============+=========================================================+
| 0  | day                | int       | 0            | Current day in simulation                               |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 1  | hospitalizeNum     | int       | 0            | # of hospitalized people                                |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 2  | isolateNum         | int       | 0            | # of isolated people                                    |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 3  | quarantineNum      | int       | 0            | # of quarantined people                                 |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 4  | confineNumfree_num | int       | 0            | # of confined people                                    |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 5  | free               | int       | 201          | # of people without intervention                        |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 6  | CurrentHealthy     | int       | 199          | # of people that are not infected                       |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 7  | CurrentInfected    | int       | 2            | # of infected cases                                     |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 7  | CurrentEffective   | int       | 2            | # of infected cases without any intervention            |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 9  | CurrentSusceptible | int       | 199          | # of susceptible people                                 |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 10 | CurrentIncubation  | int       | 2            | # of pre-symptomatic cases                              |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 11 | CurrentDiscovered  | int       | 0            | # of symptomatic cases                                  |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 12 | CurrentCritical    | int       | 0            | # of critical cases                                     |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 13 | CurrentRecovered   | int       | 0            | # of recovered cases                                    |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 14 | AccDiscovered      | int       | 0            | Accumulated # of symptomatic cases                      |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 15 | AccCritical        | int       | 0            | Accumulated # of critical cases                         |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 16 | AccAcquaintance    | int       | 0            | Accumulated # of infected through stranger contacts     |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 17 | AccStranger        | int       | 0            | Accumulated # of infected through acquaintance contacts |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 18 | measurement        | int       | 2            | an example measurement                                  |
+----+--------------------+-----------+--------------+---------------------------------------------------------+


2. `hex_cnt_xxx.txt`: Area-level replay data file.

+----+--------------------+-----------+--------------+----------------------------------+
| #  | header             | Data Tpye | Example Data | Description                      |
+====+====================+===========+==============+==================================+
| 0  | day                | int       | 0            | Current day in simulation        |
+----+--------------------+-----------+--------------+----------------------------------+
| 1  | area_id            | int       | 0            | area id                          |
+----+--------------------+-----------+--------------+----------------------------------+
| 2  | lat                | double    | 114.05019    | latitude                         |
+----+--------------------+-----------+--------------+----------------------------------+
| 3  | lng                | double    | 30.445043    | langitude                        |
+----+--------------------+-----------+--------------+----------------------------------+
| 4  | CurrentSusceptible | int       | 26           | # of susceptible cases           |
+----+--------------------+-----------+--------------+----------------------------------+
| 5  | CurrentIncubation  | int       | 0            | # of pre-symptomatic cases       |
+----+--------------------+-----------+--------------+----------------------------------+
| 6  | CurrentDiscovered  | int       | 0            | # of discovered cases            |
+----+--------------------+-----------+--------------+----------------------------------+
| 7  | CurrentCritical    | int       | 0            | # of critical cases              |
+----+--------------------+-----------+--------------+----------------------------------+
| 8  | CurrentRecovered   | int       | 0            | # of recovered cases             |
+----+--------------------+-----------+--------------+----------------------------------+
| 9  | CurrentInfected    | int       | 0            | # of infected cases              |
+----+--------------------+-----------+--------------+----------------------------------+
| 10 | free               | int       | 26           | # of people without intervention |
+----+--------------------+-----------+--------------+----------------------------------+

3.  "r0_xxx.txt": daily R-value (effective reproduction number).

+----+--------------------+-----------+--------------+----------------------------------+
| #  | header             | Data Tpye | Example Data | Description                      |
+====+====================+===========+==============+==================================+
| 0  | day                | int       | 0            | Current day in simulation        |
+----+--------------------+-----------+--------------+----------------------------------+
| 1  | r                  | double    | 0.889        | R value                          |
+----+--------------------+-----------+--------------+----------------------------------+


.. note:: 
    The calculation of R is based on: 

    - Fred Brauer. (2010, July). Epidemic Models I: Reproduction Numbers and Final Size Relations. Summer 2010 Thematic Program on the Mathematics of Drug Resistance in Infectious Diseases, Toronto, Canada.

