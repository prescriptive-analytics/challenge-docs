Try it yourself
*********

We provide a `Starter-Kit<https://github.com/prescriptive-analytics/starter-kit>` as guidelines for setting up the simulator and submit results. Here, we go through them in detail, and introduce the basic background about the simulator, template strategies and internvening APIs supported the simulator.


Installation Guide
==================

1. Check that you have python 3 installed. Other version of python might work, however, we only tested on python with version >= 3.5.


2. Preparation

* 2.1 Install boost library.

Mac OS: `brew install boost`

Linux: `sudo apt-get install libboost-dev`

Windows: Please refer to `install-boost-build<https://www.boost.org/doc/libs/1_73_0/more/getting_started/windows.html#install-boost-build>`

* 2.2 Specify system and python environment 

Make sure you have compatible python environment that marches with the simulator. Currently, we support the following versions of python:

Mac OS: python 3.6, 3.7

Linux: python 3.6.8



3. Clone Starter-Kit from GitHub.

.. code-block:: shell
    
    git clone https://github.com/prescriptive-analytics/starter-kit.git

    
4. Go to Simualtor project's root directory and run the following to test the installation

.. code-block:: python
    
    import simulator
    eng = simulator.Engine



Mobility Intervention
=====================


Flexible strategy 
-----------------

We provide flexible internvention strategies on individuals. You can intervene the mobility of an individual by setting his/her mobility with the following actions:

- No intervention: The individual can move normally.
- Confined: An individual is confined in the neighborhood that he/she lives in, with access from his/her acquaintance contacts and stranger contacts in the residential region.
- Quarantine: The person is quarantined at home, with access from acquaintance contacts sharing the same residential POI. 
- Isolation: The person is isolated, without access from the acquaintance contacts living in the same residential POI.
- Hospitalized: The person is under treatment in the hospital. A person can only be in the hospital after Stage 2.

Their corresponding intervention APIs are:



``set_man_isolate_days(days_to_isolate)``: 

- Args: days_to_isolate 
- a dictionary with manID as key and days for each person to be isolated as value.

``set_man_quarantine_days(days_to_quarantine)``:

- Args: days_to_quarantine 
- a dictionary with manID as key and days for each person to be quarantined as value.

``set_man_confine_days(days_to_confine)``:

- Args: days_to_confine - a dictionary with manID as key and days for each person to be confined as value.

``set_man_to_treat(if_treat)``

- Args: if_treat 
- a dictionary with manID as key and whether he/she is sent to be treated as value.






Run Simulation
==============


Initiate engine
---------------


.. code-block:: python
    
    import simulator
    eng = simulator.Engine(thread_num=1, write_mode="append", specified_run_name="exmample")

- ``thread_num``: number of threads.
- ``specified_run_name``: results saving folder name.
- ``write_mode``: mode of saving simulation results, ``write`` will overwrite results from different rounds of simulation in the same ``specified_run_name`` folder, ``append`` will append the results from current simulation round to existing result files.


Simulate one step
-----------------


To simulate one step, simply call ``eng.next_step()``

.. code-block:: python

    eng.next_step()




Sample Codes
------------

Here we provide a sample code for running our simulator, which can be found in the starter kit - `example.py<https://github.com/prescriptive-analytics/starter-kit/blob/master/example.py>`.

.. code-block:: python

    import simulator
    import os
    import json

    period = 840

    engine = simulator.Engine()

    engine.reset()
    for i in range(period):
        engine.next_step()
        print(engine.get_current_time())
        print(engine.get_man_visited_history(1))
        print(engine.get_man_infection_state(1))
        print(engine.get_man_visited_history(1))
        print(engine.get_region_infected_cnt(1))

        engine.set_man_confine_days({1: 5}) # {manID: day}
        engine.set_man_quarantine_days({2: 5}) # {manID: day}
        engine.set_man_isolate_days({3: 5}) # {manID: day}
        engine.set_man_to_treat({4: True}) # {manID: day}

    del engine



Results
=======

During simulation, the simulator will generate the submission file ``sub_xxx.txt`` and log files.


Submission
-----


Before submission, make sure:
 
- You are running the simulation for 840 time steps (60 simulation days in simulator). 

- Run 10 times of the experiments and set the engine write mode to "append" with ``simulator.Engine(write_mode="append")``. 

- Please upload the ``sub_xxx.txt`` to the website.


Here we provide a sample code of simulation that matches with submission requirements, which can be found [here](https://github.com/prescriptive-analytics/starter-kit/blob/master/submission.py).



Logs
--------------------

We also provide simulaiton logs to competetors.


1. The city-wide daily log file ``cnt_xxx.txt``.

2. The POI level daily log file ``hex_cnt_xxx.txt``.


Their Formats are as follows:

1. 'cnt_xxx':

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


2. `hex_cnt_xxxx.csv`: Region-level replay data file.

+----+--------------------+-----------+--------------+----------------------------------+
| #  | header             | Data Tpye | Example Data | Description                      |
+====+====================+===========+==============+==================================+
| 0  | day                | int       | 0            | Current day in simulation        |
+----+--------------------+-----------+--------------+----------------------------------+
| 1  | poi_id             | int       | 0            | POI id                           |
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