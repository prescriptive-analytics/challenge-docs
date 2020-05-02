Try it yourself
*********

We provide a [Starter-Kit](https://github.com/prescriptive-analytics/starter-kit) as guidelines for setting up the simulator and submit results. Here, we go through them in detail, and introduce the basic background about the simulator, template strategies and internvening APIs supported the simulator.


Installation Guide
==================

If you want to get nightly version of Simulator or running on native system, you can build Simulator from source. Currently, we only support building on Unix systems. This guide is based on Ubuntu 16.04.

Simualtor has little dependencies, so building from source is not scary.

1. Check that you have python 3 installed. Other version of python might work, however, we only tested on python with version >= 3.5.


2. Preparation

Mac OS: `brew install boost`

Linux: `sudo apt-get install libboost-dev`

Windows: Please refer to [install-boost-build](https://www.boost.org/doc/libs/1_73_0/more/getting_started/windows.html#install-boost-build)


3. Clone Starter-Kit from GitHub.

.. code-block:: shell
    
    git clone https://github.com/prescriptive-analytics/starter-kit.git

    
4. Go to Simualtor project's root directory and run the following to test the installation

.. code-block:: python
    
    import simulator
    eng = simulator.Engine



Mobility Intervention
=====================


Template strategy
-----------------

Here, we provide an example intervention strategy, with different actions on different groups of people. 

We can define the following six groups of people based on their levels of health risks: 

- G1: Infected (critical)
- G2: Infected (symptomatic)
- G3: Acquaintance contact of G1
- G4: Stranger contact of G1
- G5: Acquaintance contact of G2

Based on these definitions, we have provided 8 strategies to intervene the actions of certain groups of people:

+--------------------------------------------+--------------------------------------------------------+-----------------+
| Health Status                              | Code for Action and meaning                            | Position of Bit |
+============================================+========================================================+=================+
| Critical                                   | 0: No intervene; 1:Hospitalize                         | 0               |
+--------------------------------------------+--------------------------------------------------------+-----------------+
| Symptomatic                                | 0: No intervene; 1: Isolate; 2: Hospitalize            | 1               |
+--------------------------------------------+--------------------------------------------------------+-----------------+
| Acquaintance contacts of critical cases    | 0: No intervene; 1: Confine; 2. Quatantine; 3: Isolate | 2               |
+--------------------------------------------+--------------------------------------------------------+-----------------+
| Stranger contacts of critical cases        | 0: No intervene; 1: Confine; 2. Quatantine; 3: Isolate | 3               |
+--------------------------------------------+--------------------------------------------------------+-----------------+
| Acquaintance contacts of symptomatic cases | 0: No intervene; 1: Confine; 2. Quatantine; 3: Isolate | 4               |
+--------------------------------------------+--------------------------------------------------------+-----------------+

We use an 5-bit binary string to represent the policy. Each bit represents whether the corresponding strategy is selected. For instance, “21000” represents that "To hospitalize the critical cases and isolate the symptomatic cases". Note that, when multiple strategies are applied to the same person, the more strict one will take effect.

Intervening Parameters
^^^^^^^^^^^^^^^^^^^^^^

We provide the following mobility parameters to intervene:

* The day to start intervention policies
* The number of days to isolate people
* The number of days to confine people in their residential community
* The number of  days to trace back when looking for the stranger contacts of the discovered case
* The thresholding number of discovered cases in one POI. If one POI has discovered cases more than this threshold, all the people visited the POI are considered as stranger contacts additionally.

Get started with your own specification
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
You can conduct the experiment by running the following commands in the config file of the simulator, with options listed here to select different strategies and parameters.

+---------------------------------------+--------------------------------------------------------------+
|                                       | Show this help message                                       |
+=======================================+==============================================================+
| --totalRounds                         | How many times to run the whole experiment                   |
+---------------------------------------+--------------------------------------------------------------+
| --strategy                            | A binary string to specify the different policies to conduct |
+---------------------------------------+--------------------------------------------------------------+
| --startIntervene                      | Specify the day to start intervention                        |
+---------------------------------------+--------------------------------------------------------------+
| --daysToTrack                         | # days to trace back when looking for the contacts           |
|                                       |  of the confirmed case                                       |
+---------------------------------------+--------------------------------------------------------------+
| --daysToTreat                         | # days to quarantine                                         |
+---------------------------------------+--------------------------------------------------------------+
| --daysToIsolate                       | # days to force people isolate at home                       |
+---------------------------------------+--------------------------------------------------------------+
| --daysToQuarantine                    | # days to force people stay at home                          |
+---------------------------------------+--------------------------------------------------------------+
| --daysToConfine                       | # days to force people stay within community                 |
+---------------------------------------+--------------------------------------------------------------+
| --regionInfectedThresForSimpleContact | If one region has confirmed cases more than this threshold,  |
|                                       | policies on this region will take effect (e.g., quarantine)  |
+---------------------------------------+--------------------------------------------------------------+
 

Flexible strategy 
-----------------

We provide flexible internvention strategies on individuals. You can intervene the mobility of an individual by setting his/her mobility with the following actions:

- Confined: An individual is confined in the neighborhood that he/she lives in, with access from his/her acquaintance contacts and stranger contacts in the residential region.
- Quarantine: The person is quarantined at home with other individuals sharing the same residential POI. 
- Isolation: The person is isolated, even from the individuals living in the same residential POI.
- Hospitalized: The person is under treatment in the hospital. A person can only be in the hospital after Stage 2.

Their corresponding control API are:



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
    eng = simulator.Engine(config_path, thread_num=1)


- ``config_path``: path for config file.
- ``thread_num``: number of threads.

Arguments In Config File
^^^^^^^^^^^^^^^^^^^^^^^^
- ``help``: Show this help message


- ``strategy``: A binary string to specify the different strategy templates to conduct. The meanings of the binary string can be found here: :ref:`start`.
- ``startIntervene``: Specify the day to start intervention 
- ``daysToTrack``: # days to trace back when looking for the contacts of the confirmed case
- ``daysToTreat``: # days to treat at hospotal before immume
- ``daysToIsolate``: # days to force people isolate at home
- ``daysToQuarantine``: # days to force people stay at home
- ``daysToConfine``: # days to force people stay within community
- ``regionInfectedThresForStrangerContact: If one region has confirmed cases more than this threshold, policies on this region will take effect (e.g., quarantine)

- ``seed``: Experiment seed,
- ``dir``: File directory for reading configs and writing logs, default "./examples"
- ``predefinedStrategy``: whether to use pre-defined strategy templates, default "true"
- ``saveReplay``: whether to save logs for replay simulation, default "true"
- ``results_dir``: File directory name under ``dir`` for saving measurement results, default "results"
- ``save_replay_dir``: File directory name under ``dir`` for saving logs, default "".

- ``POI_file``: POI file to read, default "w_small.txt" with 33 POIs,
- ``population``: Population, default 200,
- ``total_step``: Total simulation steps, default 840,
- ``location_file``: Location definition file for visualization, default "w_small_visual.json"

        
Sample Config File
^^^^^^^^^^^^^^^^^^^

.. note::
    Runnable sample config files can be found in ``examples`` folder.

.. code-block:: json

{

    "regionsFile": "sample1_regions.csv",
    "dailyStatsFile": "sample1_daily_stats.csv",
    "regionStatsFile": "sample1_region_stats.csv"

    "population": 10000,
    "total_step": 840,
}



Simulate one step
-----------------


To simulate one step, simply call ``eng.next_step()``

.. code-block:: python

    eng.next_step()




Sample Codes
------------

Here we provide a sample code for running our simulator, which can be found [here]https://github.com/prescriptive-analytics/starter-kit/blob/master/example.py).

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

During simulation, the simulator will generate the submission file ``sub_xxx.txt`` and visualization files.


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