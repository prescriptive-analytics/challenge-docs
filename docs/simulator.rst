Simulator
*********
The simulator simulates individual mobility in a city of :math:`R` Regions with :math:`M` people. Each region consists of several POIs, where each POI can serve three functions: working, living and commercial purposes. Each person is associated with one living POI and one working POI (randomly sampled from :math:`K_{work}` nearest POIs of the living POI).

Mobility of a person
++++++++++++++++++++
A person has different modes of mobility during workdays and weekends:

On working days, a person will go to work at a certain time :math:`T_{start} \sim Uniform(t_{s1}, t_{s2})`, and stay there for :math:`T_{work} \sim Uniform(t_{w1}, t_{w2})` hours. After work, they may visit a nearby commercial POI (randomly sampled from :math:`K_{com}` nearest POIs of the commercial POI)  with a probability of P_{com} and stay there for :math:`T_{mall} \sim Uniform (t_{c1}, t_{c2})` hours. Then, they will return home.

On weekends, people may visit a commercial POI within the whole city with a probability :math:`P’_{com}` and stay there for :math:`T'_{mall} \sim Uniform (t’_{c1}, t’_{c2})` hours. After that, they will return home.

Health status (HS) of a person
++++++++++++++++++++++++++++++
Each person can have several health stages: 
* :math:`1. Healthy \rightarrow 2. Infected\ (undiscovered) \rightarrow 3. Infected\ (discovered) \rightarrow 4. Infected (critical) \rightarrow 5. Immune`

From HS 1 to HS 2, people can get infected via contact with infected people, with different probabilities from two different types of contacts.

* :math:`Inf_{simple}` from simple contact: All people working in the same region or living in the same region are regarded as simple contacts. People will also be regarded as simple contacts if they appear in the same commercial POI at the same time.
* :math:`Inf_{close}` from close contact: A group of people with size :math:`K_L \sim Uniform(I_{c1}, I_{c2})` in a person’s living region is considered as his/her closed contacts. And a group of people with size :math:`K_W \sim Uniform(I_{c1}, I_{c2})` in a person’s working region is also considered as his/her closed contacts.

From HS 2 to HS 3, there is a fixed incubation period of :math:`INC` days.

From HS 3 to HS 4, there is a development time period :math:`DEV \sim Normal(d_1, d_2)`.

From HS 4 to HS 5, there is a fixed treatment time period of :math:`IMM` days.

Controlling action of a person
++++++++++++++++++++++++++++++
We can provide different actions to each individual:


* Free: The person can move normally.
* In quarantine: The person is quarantined, with access from his/her close contacts at the living POI.
* In isolation: The person is isolated, without access from anyone.
* In hospital: The person is under treatment in the hospital. A person can only be in the hospital after Health Stage 2.
