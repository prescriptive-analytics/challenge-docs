Quick Start with Simple Rules
***************

Setting 1: With pre-defined strategies
++++++++++++++++++++++++++++++++++++++++

In this setting, we provide eight strategy templates, the competitor would need to choose the optimal strategy though specifying the templates and parameters.

Strategy Templates
++++++++++++++++++

Here, we provide an example intervention strategy, with different actions on different groups of people. 

We can define the following six groups of people based on their levels of health risks: 

- G1: Infected (critical)
- G2: Infected (discovered)
- G3: Acquaintance contact of G1
- G4: Stranger contact of G1
- G5: Acquaintance contact of G2
- G6: Stranger contact of G2

We can choose to take the following three actions to them:

- A0: Free
- A1: Confine
- A2: Quarantine (todo)
- A3: Isolate
- A4: Hospitalized

Based on these definitions, we have provided 8 strategies to intervene the actions of certain groups of people:

+--------------------------+---------+------------+---------------+------------+----------------+
|                          | A0:Free | A1:Confine | A2:Quarantine | A3:Isolate | A4:Hospitalize |
+--------------------------+---------+------------+---------------+------------+----------------+
| G1: Critital             |         |            |               |            | S1             |
+--------------------------+---------+------------+---------------+------------+----------------+
| G2: Discovered           |         |            |               |            | S4             |
+--------------------------+---------+------------+---------------+------------+----------------+
| G3: Aquantaince Contacts |         | S3         |               | S2         |                |
| of Critical              |         |            |               |            |                |
+--------------------------+---------+------------+---------------+------------+----------------+
| G4: Simple Contacts of   |         | S8         |               | S7         |                |
| Critital                 |         |            |               |            |                |
+--------------------------+---------+------------+---------------+------------+----------------+
| G5: Aquantaince Contacts |         | S6         |               | S5         |                |
| of Discovered            |         |            |               |            |                |
+--------------------------+---------+------------+---------------+------------+----------------+
| G6:Simple Contacts       |         |            |               |            |                |
| of Discovered            |         |            |               |            |                |
+--------------------------+---------+------------+---------------+------------+----------------+

We use an 8-bit binary string to represent the policy. Each bit represents whether the corresponding strategy is selected. For instance, “11000000” represents that S1 and S2 are selected. Note that, when multiple strategies are applied to the same group of people, the more strict one will take effect. For instance, if S2 and S5 are selected, only S2 will take effect.

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
| --help                                | Show this help message                                       |
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
        

