Simulator
=========

Simulation Process
---------

The simulator covers a city of 100 regions, with 5000 people. Each region can serve three functions: working, living and commercial purposes. Each person is associated with one working region and one living region. 

On working days, people will go to work at a certain time (e.g., 8 am), and stay there for several hours (randomly integer between 7 and 10). After work, they may go shopping in the nearby region with a probability P_{sw}. Then, they will return home.

On weekends, people may go shopping with a probability P_{se}, and randomly choose a commercial region from the whole city. After that, they will return home.

Each person can have several stages (we do not consider the treatment process now).
Healthy → 2. Infected (undiscovered) → 3. Infected (discovered) → 4. Infected (critical) 

From stage 1 to stage 2, people can get infected via contact with infected people. There are two different types of contact.
Simple contact: All people working in the same region or living in the same region are regarded as simple contacts. People will also be regarded as simple contacts if they appear in the same commercial region at the same time.
Close contact: Each person has a close contact group in his/her living region (size of which described by a distribution D_l) and another close contact group in his/her working region (size of which described by a distribution D_w).
From stage 2 to stage 3, there is a fixed incubation period of two days.
The temporal length (# of days) from stage 3 to stage 4 is drawn from a distribution D_c.

We will provide policies to put people in hospital, quarantine people, or force people to stay at home.

Control policies
-----------------

There are in total six groups of people with different levels of risks. 
- G1: Infected (critical)
- G2: Infected (discovered)
- G3: Close contact of G1
- G4: Simple contact of G1
- G5: Close contact of G2
- G6: Simple contact of G2


We can take the following three actions to them:
- A1: Treat (put in hospital)
- A2: Quarantine
- A3: Force to stay at home

Based on these definitions, we have provided 8 different strategies (we will add more later):
- S1: G1 A1
- S2: G3 A2
- S3: G3 A3
- S4: G2 A1
- S5: G5 A2
- S6: G5 A3
- S7: G4 A2
- S8: G4 A3

We use an 8-bit binary string to represent the policy. Each bit represents whether the corresponding strategy is selected. For instance, “11000000” represents that S1 and S2 are selected. 

Note that, when multiple strategies are applied to the same group of people, the more strict one will take effect. For instance, if S2 and S5 are selected, only S2 will take effect.



Running the simulator
---------------------

You can conduct the experiment by running the following commands, with options listed here to select different strategies and parameters.
./fast.out

Usage: <option(s)>
	--help		 Show this help message
	--totalRounds	 How many times to run the whole experiment
	--strategy	 A binary string to specify the different policies to conduct
	--startGL	 Specify the day to start quarantine	
    --daysToTrack	 Number of days to trace back when looking for the contacts of the confirmed case
	--daysToQuarantine	 Number of days to quarantine
	--daysToForceAtHome	 Number of days to force people stay at home
	--regionInfectedThresForSimpleContact	 If one region has confirmed cases more than this threshold, policies on this region will take effect (e.g., quarantine)
