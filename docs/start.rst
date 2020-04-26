.. _start:

Simple Strategy with Templates
***************

We provide eight strategy templates, the competitor would need to choose the optimal strategy though specifying the templates and parameters.


Strategy Templates
++++++++++++++++++

Here, we provide an example intervention strategy, with different actions on different groups of people. 

We can define the following six groups of people based on their levels of health risks: 

- G1: Infected (critical)
- G2: Infected (symptomatic)
- G3: Acquaintance contact of G1
- G4: Stranger contact of G1
- G5: Acquaintance contact of G2
- G6: Stranger contact of G2

We can choose to take the following three actions to them:

- A0: Free
- A1: Confine
- A2: Quarantine
- A3: Isolate
- A4: Hospitalized

Based on these definitions, we have provided 8 strategies to intervene the actions of certain groups of people:

+--------------------------------------------+--------------------------------------------------------+-----------------+
| Health Status                              | Action                                                 | Position of Bit |
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
++++++++++++++++++++++

We provide the following mobility parameters to intervene:

* The day to start intervention policies
* The number of days to isolate people
* The number of days to confine people in their residential community
* The number of  days to trace back when looking for the stranger contacts of the discovered case
* The thresholding number of discovered cases in one POI. If one POI has discovered cases more than this threshold, all the people visited the POI are considered as stranger contacts additionally.

Get started with your own specification
#######################################
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
        

