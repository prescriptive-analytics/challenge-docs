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
- ``strategy``: A binary string to specify the different policies to conduct
- ``startGL``: Specify the day to start quarantine  
- ``daysToTrack``: # days to trace back when looking for the contacts of the confirmed case
- ``daysToQuarantine``: # days to quarantine
- ``daysToForceAtHome``: # days to force people stay at home
- ``regionInfectedThresForSimpleContact: If one region has confirmed cases more than this threshold, policies on this region will take effect (e.g., quarantine)
        
Sample Config File
^^^^^^^^^^^^^^^^^^^

.. note::
    Runnable sample config files can be found in ``examples`` folder.

.. code-block:: json

    {
        "strategy": "10001000",
        "daysToTrack": 1,
        "startGL": 1,
        "daysToTrack": 1,
        "regionInfectedThresForSimpleContact": 3,
        "daysToTreat": 1e9,
        "daysToQuarantine": 1,
        "daysToForceAtHome": 1,
        "seed": 1,
        "dir": "./examples",
        "saveReplay": false
    }

Simulation
----------

To simulate one step, simply call ``eng.next_step()``

.. code-block:: python

    eng.next_step()

Data Access API
---------------

``get_man_infection_state(XXXX)``:

- Explanation
- Input format
- Output format


``get_region_visited_history(XXXX)``:

- Explanation
- Input format
- Output format

``get_man_visited_history(XXXX)``:

- Explanation
- Input format
- Output format

``get_region_contained_man()``:

- Explanation
- Input format
- Output format

``get_region_infected_cnt(XXXX)``:

- Explanation
- Input format
- Output format

``get_life_count()``:

- Return the number of people not in hospital.

``get_hospital_count()``:

- Return the number of hospitalized people.

``get_gl_count()``:

- Return the number of GL people.

``get_infect_count()``:

- Return the number of infected people.

``get_quanrantine_count()``:

- Return the number of non-free people.

``get_simple_count()``

- Return the number of simple contacts.

``get_close_count()``

- Return the number of close contacts.


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
-----------

``set_man_at_home_days(XXX)``: 

- Explanation
- Input format
- Output format

``set_man_is_GL_days(XXX)``:

- Explanation
- Input format
- Output format

``set_man_treat_days(XXX)``:

- Explanation
- Input format
- Output format

``reset(seed=False)``: 

- Reset the simulation
- Reset random seed if ``seed`` is set to ``True``


``set_random_seed(seed)``:

- Set seed of random generator to ``seed``
- Input format: int


Other API
---------

``TBD``