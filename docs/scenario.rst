Competition Scenarios and Leaderboard Ranking
*********************************************

Competition Scenarios
=====================

In the official competition, we provide five scenarios: 

1. Default
----------
In this scenario, the default parameters are specified in this section. Unless specified, the parameters in the following scenarios are the same as the default scenario.


2. Higher infection rates
-------------------------
In this scenario, we aim to simulate a disease with higher infection rates. Specifically, infection rate from acquaintance contact :math:`P_c=0.0375`, infection rate from stranger contact :math:`P_s=0.0075`. Their detailed description can be found `here <https://hzw77-demo.readthedocs.io/en/round2/simulator_modeling.html#disease-transmission-model>`_.


3. Larger initial infected population
-------------------------------------
In this scenario, there is a large infected population at the beginning of the simulation. Specifically, there are 300 individuals at the first day of simulation.


4. Larger range of start-working time
----------------------------------------
In this scenario, each individual will have a larger range of time to go to work. Specifically, the start-working time :math:`T^d_{start} \sim U(t^d_{s1}, t^d_{s2})` with :math:`t^d_{s1}=1` and :math:`t^d_{s2}=5`. Their detailed description can be found `here <https://hzw77-demo.readthedocs.io/en/round2/simulator_modeling.html#human-mobility-model>`_.


5. More areas in the city
-------------------------
In this scenario, there will be 98 areas in the city, :math:`\mathcal{A}=98`.



Leaderboard Ranking
===================

The final ranking is based on the sum of rankings over all 5 scenarios.

