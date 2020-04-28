Try it yourself
*********


Installation Guide
==================

If you want to get nightly version of Simulator or running on native system, you can build Simulator from source. Currently, we only support building on Unix systems. This guide is based on Ubuntu 16.04.

Simualtor has little dependencies, so building from source is not scary.

1. Check that you have python 3 installed. Other version of python might work, however, we only tested on python with version >= 3.5.


2. Install cpp dependencies

.. code-block:: shell
    
    sudo apt update && sudo apt install -y build-essential cmake

3. Clone Simualtor project from github.

.. code-block:: shell
    
    git clone https://github.com/wingsweihua/COVID.git
    git checkout wrapping
    
4. Go to Simualtor project's root directory and run

.. code-block:: shell
    
    pip install .

5. Wait for installation to complete and CityFlow should be successfully installed.

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

- Args: days_to_isolate - a dictionary with manID as key and days for each person to be isolated as value.

``set_man_quarantine_days(days_to_quarantine)``:

- Args: days_to_quarantine - a dictionary with manID as key and days for each person to be quarantined as value.

``set_man_confine_days(days_to_confine)``:

- Args: days_to_confine - a dictionary with manID as key and days for each person to be confined as value.

``set_man_to_treat(if_treat)``
- Args: if_treat - a dictionary with manID as key and whether he/she is sent to be treated as value.






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
- ``location_file``: Location definition file for visualization, default "w_small_visual.json"

        
Sample Config File
^^^^^^^^^^^^^^^^^^^

.. note::
    Runnable sample config files can be found in ``examples`` folder.

.. code-block:: json

      {
        "strategy": "12323",
        "daysToTrack": 5,
        "startIntervene": 1,
        "regionInfectedThresForStrangerContact": 1,
        "daysToTreat": 1e9,
        "daysToIsolate": 15,
        "daysToQuarantine": 15,
        "daysToConfine": 15,

        "seed": 1,
        "dir": "./examples",
        "predefinedStrategy": true,
        "saveReplay": false,
        "results_dir": "results",
        "save_replay_dir": "",


        "POI_file": "w_small.txt",
        "population": 200,
        "location_file": "w_small_visual.json"
      }



Simulate one step
-----------------


To simulate one step, simply call ``eng.next_step()``

.. code-block:: python

    eng.next_step()




Sample Codes
------------

Here we provide a sample code for running our simulator, which can be found [here](https://github.com/gjzheng93/COVID/blob/wrapping/tests/python/test_api.py).

.. code-block:: python

    import simulator
    import os

    # os.chdir(os.path.join("..", ".."))
    # print(os.getcwd())

    config_file = os.path.join("examples", "config.json")
    period = 100

    engine = simulator.Engine(config_file=config_file, thread_num = 1)

    print("here")

    engine.reset()
    for i in range(period):
        engine.next_step()
        print("engine.getCurrentTime()", engine.get_current_time())
        print("engine.getRegionVisitedHis(1)", engine.get_man_visited_history(1))
        print("engine.getManInfectionState(1)", engine.get_man_infection_state(1))
        print("engine.getManVisitedHis(1)", engine.get_man_visited_history(1))
        print("engine.getRegionInfectedCnt(1)", engine.get_region_infected_cnt(1))

        engine.set_man_confine_days({1: 5}) # {manID: day}
        engine.set_man_isolate_days({1: 5}) # {manID: day}
        engine.set_man_to_treat({1: True}) # {manID: bool}


    del engine



Results
=======

Score
-----



Visualization/Replay
--------------------

We provide offline visualization after each simulation.


Start
^^^^^

1. enter the ``frontend`` folder and open ``index.html`` in your browser.

2. choose the replay log file (as defined by ``City Visualization File`` field in the config file, **not 'relayLogFile'**) and wait for it to be loaded. When it has finished loading, there will be a message shown in the info box.

3. choose the replay file (as defined by ``replayLogFile`` field in the config file) .

4. choose the chart data file (optional, see section *Chart* below).

5. press ``Start`` button to start the replay.

Control
^^^^^^^

- Use the mouse to navigate. Dragging and mouse wheel zooming are supported.

- Move the slider in Control Box to adjust the replay speed. You can also press ``1`` on keyboard to slow down or ``2`` to speed up.

- Press ``Pause`` button in Control Box to pause/resume. You can also double-click on the map to pause and resume.

- Press ``[`` or ``]`` on keyboard to take a step backward or forward.

- To restart the replay, just press ``Start`` button again.

- The ``debug`` option enables displaying the ID of vehicles, roads and intersections during a mouse hover. **This will cause a slower replaying**, so we suggest using it only for debugging purposes.

Chart
^^^^^

The player supports showing the change of different metrics in a chart simultaneously with the replay process.

To provide required data, a log file in a format as shown below is needed:

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
| 8  | CurrentSusceptible | int       | 199          | # of susceptible people                                 |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 9  | CurrentIncubation  | int       | 2            | # of pre-symptomatic cases                              |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 10 | CurrentDiscovered  | int       | 0            | # of symptomatic cases                                  |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 11 | CurrentCritical    | int       | 0            | # of critical cases                                     |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 12 | CurrentRecovered   | int       | 0            | # of recovered cases                                    |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 13 | AccDiscovered      | int       | 0            | Accumulated # of symptomatic cases                      |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 14 | AccCritical        | int       | 0            | Accumulated # of critical cases                         |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 15 | AccAcquaintance    | int       | 0            | Accumulated # of infected through stranger contacts     |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 16 | AccStranger        | int       | 0            | Accumulated # of infected through acquaintance contacts |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 17 | measurement        | int       | 2            | an example measurement                                  |
+----+--------------------+-----------+--------------+---------------------------------------------------------+


Each row stands for one day and each column stands for a specific metric.

In one row, numbers are separated by one spaces and comma.

The numbers in one column will be shown as points connected by one line in the chart.

.. note::
  Make sure that each row is corresponding with the right time step.

Notes
^^^^^

- To get the example replay files, run ``download_replay.py`` under ``frontend`` folder.

- If you create a new Engine object with same ``replayLogFile``, it will clear the old replay file first

- Using ``eng.reset()`` won't clear old replays, it will append newly generated replay to the end of ``replayLogFile``

- You can change ``replayLogFile`` during runtime using ``set_replay_file``, see :ref:`set-replay-file`

