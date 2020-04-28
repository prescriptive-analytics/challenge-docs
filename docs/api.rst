APIs
****

Simulation Config API
=====================

``reset(seed=False)``: 

- Reset the simulation
- Reset random seed if ``seed`` is set to ``True``


``set_random_seed(seed)``:

- Set seed of random generator to ``seed``
- Input format: int

``next_step()``:
- Simulate one step, a simulation step indicates one hour in the real world.


Data Access API
===============

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



Intervention API
===========

``set_man_isolate_days(days_to_isolate)``: 

- Args: days_to_isolate - a dictionary with manID as key and days for each person to be isolated as value.

``set_man_quarantine_days(days_to_quarantine)``:

- Args: days_to_quarantine - a dictionary with manID as key and days for each person to be quarantined as value.

``set_man_confine_days(days_to_confine)``:

- Args: days_to_confine - a dictionary with manID as key and days for each person to be confined as value.

``set_man_to_treat(if_treat)``
- Args: if_treat - a dictionary with manID as key and whether he/she is sent to be treated as value.



Other API
=========

``TBD``
