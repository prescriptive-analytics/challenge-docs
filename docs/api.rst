APIs
****

Simulation Initialization API
=====================

``Engine(thread_num=1, write_mode="append", specified_run_name="test")``:
- Args: 

	- ``thread_num``: number of threads.
	- ``specified_run_name``: results saving folder name.
	- ``write_mode``: mode of saving simulation results, ``write`` will overwrite results from different rounds of simulation in the same ``specified_run_name`` folder, ``append`` will append the results from current simulation round to existing result files.

- Return: an initialized engine without ``reset()``, should call ``reset()`` after this function.



Simulation Config API
=====================

``reset(seed=False)``: 

- Reset the simulation, this should be called after we creat an engine with ``Engine()``
- Reset random seed if ``seed`` is set to ``True``


``set_random_seed(seed)``:

- Set seed of random generator to ``seed``
- Input format: int

``next_step()``:
- Simulate one step, a simulation step indicates one hour in the real world.


.. note::
	1. ``reset()`` should be called every time we creat an engine with ``Engine()``
	2. All the data acess/ intervention APIs should be called after ``next_step()``
	3. All the IDs for men and areas start from 0.


Data Access API
===============


``get_individual_infection_state(individualID)``:

- Args: individualID - id for individual
- Return: infection status of this individual, ``1: susceptible or pre-symptomatic``, ``3: symptomatic``, ``4: critical``, ``5: recovered``.


``get_individual_intervention_state(individualID)``:

- Args: individualID - id for individual
- Return: instervention status of this individual, ``0: no such individual id``,``1: without intervention``, ``2: confined``, ``3: quarantined``, ``4: isolated``, ``5: hospitalized``.


``get_area_visited_history(areaID)``:

- Args: areaID - id for the area
- Return: a 2D list of the visited history of one area in past 5 days. Each of the inner 1D list represents the history for one hour. The order of the list is chronological, with the earlist time appearing the first in the list. Specifically, ``areaID=-1`` stands for hospital.
[[individualID1, individualID2, individualID3, ...], [individualID7, individualID8,]]


``get_individual_visited_history(individualID)``:

- Args: individualID
- Return: a 1D list of the id of the areas that he/she has visited in past 5 days. The order of the list is chronological, with the earlist time appearing the first in the list.
[areaID1, areaID2, ...].


``get_individual_visited_history_with_p_infection(individualID)``:

- Args: individualID
- Return: a 2D list of the probabilities of geting infected (from acquantaince contacts and stranger contacts) in the areas that he/she has visited in past 5 days. The order of the list is chronological, with the earlist time appearing the first in the list. This should be corresponding with  ``get_individual_visited_history``.
[[p_acq1, p_stranger1], [p_acq2, p_stranger2], ...]


``get_area_contained_individual()``:

- Return: a dictionary with all area id as the keys, and the list of individualID who live in this area as the value 

``get_area_infected_cnt(areaID)``:

- Args: areaID
- Return: an int representing the number of infected people in this area


``get_life_count()``:

- Return the total number of people not in hospital of the whole environment.

``get_infect_count()``:

- Return the number of infected people in the whole environment.


``get_hospitalize_count()``:

- Return the number of hospitalized people in the whole environment.

``get_isolate_count()``:

- Return the number of isolated people in the whole environment.

``get_quarantine_count()``:

- Return the number of quanrantined people in the whole environment.

``get_confine_count()``:

- Return the number of confined people in the whole environment.


``get_stranger_count()``

- Return the number of stranger contacts.

``get_acquaintance_count()``

- Return the number of acquaintance contacts.


``get_current_time()``:

- Get simulation time (in hour)
- Return a ``int``, starting from 0

``get_current_hour()``:

- Get simulation time (in hour of day)
- Return a ``int``, ranging from 0 to 13

``get_current_day()``:

- Get simulation time (in day)
- Return a ``int``, starting from 0



Intervention API
===========

Intervention APIs are only effective when being called at the start of one day.

``set_individual_isolate_days(days_to_isolate)``: 

- Args: days_to_isolate 
- a dictionary with individualID as key and days for each person to be isolated as value.

``set_individual_quarantine_days(days_to_quarantine)``:

- Args: days_to_quarantine 
- a dictionary with individualID as key and days for each person to be quarantined as value.

``set_individual_confine_days(days_to_confine)``:

- Args: days_to_confine - a dictionary with individualID as key and days for each person to be confined as value.

``set_individual_to_treat(if_treat)``

- Args: if_treat 
- a dictionary with individualID as key and whether he/she is sent to be treated as value.



Other API
=========

``TBD``
