Quick Start with C++ simulator
***************
Here, we provide an example control strategy, with different actions on different groups of people.

We can define the following six groups of people based on their levels of health risks:



* G1: Infected (critical)
* G2: Infected (discovered)
* G3: Close contact of G1
* G4: Simple contact of G1
* G5: Close contact of G2
* G6: Simple contact of G2

We can choose to take the following three actions to them:



* A0: Free
* A1: In quarantine
* A2: In isolation
* A3: In hospital

Simple Control
##############
Based on these definitions, we have provided 8 strategies to control the actions of certain groups of people:



* S1: G1 A1
* S2: G3 A2
* S3: G3 A3
* S4: G2 A1
* S5: G5 A2
* S6: G5 A3
* S7: G4 A2
* S8: G4 A3

We use an 8-bit binary string to represent the policy. Each bit represents whether the corresponding strategy is selected. For instance, “11000000” represents that S1 and S2 are selected. Note that, when multiple strategies are applied to the same group of people, the more strict one will take effect. For instance, if S2 and S5 are selected, only S2 will take effect.

Get started with your own specification
#######################################
You can conduct the experiment by running the following commands, with options listed here to select different strategies and parameters.
Usage:
    --help		 Show this help message
    --totalRounds	 How many times to run the whole experiment
    --strategy	 A binary string to specify the different policies to conduct
    --startGL	 Specify the day to start quarantine	
    --daysToTrack	 # days to trace back when looking for the contacts of the confirmed case
    --daysToQuarantine	 # days to quarantine
    --daysToForceAtHome	 # days to force people stay at home
    --regionInfectedThresForSimpleContact	 If one region has confirmed cases more than this threshold, policies on this region will take effect (e.g., quarantine)
        

