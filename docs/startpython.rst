.. _startpython:

Quick Start with Python Simulator
===========

Installation
------------

If you have not installed simulator yet, :ref:`install` is a simple guide for installation.


Create Engine
-------------

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

        "strategy": "100010001",
        "daysToTrack": 5,
        "startIntervene": 1,
        "regionInfectedThresForStrangerContact": 1,
        "daysToTreat": 1e9,
        "daysToIsolate": 1,
        "daysToQuarantine": 1,
        "daysToConfine": 1,

        "seed": 1,
        "dir": "./examples",
        "predefinedStrategy": true,
        "saveReplay": true,
        "results_dir": "results",
        "save_replay_dir": "",


        "POI_file": "w_small.txt",
        "population": 200,
        "location_file": "w_small_visual.json"
      }

Simulation
----------

To simulate one step, simply call ``eng.next_step()``

.. code-block:: python

    eng.next_step()


APIs
----

Simulation Config API
^^^^^^^^^^^^^^^^^^^^^

``reset(seed=False)``: 

- Reset the simulation
- Reset random seed if ``seed`` is set to ``True``


``set_random_seed(seed)``:

- Set seed of random generator to ``seed``
- Input format: int

``next_step()``:
- Simulate one step, a simulation step indicates one hour in the real world.


Data Access API
^^^^^^^^^^^^^^^

``get_man_infection_state(manID)``:

- Args: manID - id for man
- Return: infection status of this man



``get_region_visited_history(regionID)``:

- Args: regionID - id for region
- Return: a 2D list of the visited history of one region. Each of the inner 1D list represents the history for one hour. [[manID1, manID2, manID3, ...], [manID7, manID8,]]


``get_man_visited_history(manID)``:

- Args: manID
- Return: a 1D list of the id of the regions that he/she has visited. 
[regionID1, regionID2, ...]


``get_region_contained_man()``:

- Return: a dictionary with region id as the key, and the list of manID who live in this region as the value 

``get_region_infected_cnt(regionID)``:

- Args: regionID
- Return: an int representing the number of infected people in this region


``get_life_count()``:

- Return the number of people not in hospital.

``get_infect_count()``:

- Return the number of infected people.


``get_hospitalize_count()``:

- Return the number of hospitalized people.

``get_isolate_count()``:

- Return the number of isolated people.

``get_quarantine_count()``:

- Return the number of quanrantined people.

``get_confine_count()``:

- Return the number of confined people.


``get_stranger_count()``

- Return the number of stranger contacts.

``get_acquaintance_count()``

- Return the number of acquaintance contacts.


``get_current_time()``:

- Get simulation time (in hour)
- Return a ``int``

``get_current_hour()``:

- Get simulation time (in hour of day)
- Return a ``int``

``get_current_day()``:

- Get simulation time (in day)
- Return a ``int``



Control API
^^^^^^^^^^^

``set_man_isolate_days(days_to_isolate)``: 

- Args: days_to_isolate - a dictionary with manID as key and days for each person to be isolated as value.

``set_man_quarantine_days(days_to_quarantine)``:

- Args: days_to_quarantine - a dictionary with manID as key and days for each person to be quarantined as value.

``set_man_confine_days(days_to_confine)``:

- Args: days_to_confine - a dictionary with manID as key and days for each person to be confined as value.

``set_man_to_treat(if_treat)``
- Args: if_treat - a dictionary with manID as key and whether he/she is sent to be treated as value.



Other API
^^^^^^^^^

``TBD``



Running Example
---------------

Here we provide a sample code for running our simulator, which can be found [here](https://github.com/gjzheng93/COVID/blob/wrapping/tests/python/test_api.py).

.. code-block:: python

    import simulator
    import os

    # os.chdir(os.path.join("..", ".."))
    # print(os.getcwd())

    config_file = os.path.join("examples", "config.json")
    period = 100

    engine = simulator.Engine(config_file=config_file)

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
        engine.set_man_hospitalize_days({1: 5}) # {manID: day}


    del engine

