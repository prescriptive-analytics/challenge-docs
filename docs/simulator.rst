Simulator
*********
The simulator simulates individual mobility in a city of :math:`R` Regions with :math:`M` people. Each region consists of several POIs, where each POI can contains three types of POIs: working, residential, and commercial. An individual is associated with two fixed POIs: one for residential, and one for working. 


Human Mobility Model
++++++++++++++++++++
A person has different modes of mobility during workdays and weekends.

On working days, a person will start from home to working POI at a certain time :math:`T^d_{start} \sim Uniform(t^d_{s1}, t^d_{s2})`, and stay there for :math:`T_{work} \sim Uniform(t_{w1}, t_{w2})` hours. After work, they may visit a nearby commercial POI (randomly sampled from :math:`K_{com}` nearest POIs of the working POI)  with a probability of :math:`P^d_{com}` and stay there for :math:`T^d_{com} \sim Uniform (t^d_{c1}, t^d_{c2})` hours. Then, they will return to residential POI.

On weekends, people may visit a random commercial POI within the whole city at a certain time :math:`T^e_{start} \sim Uniform(t^e_{s1}, t^e_{s2})` with a probability :math:`P^e_{com}` and stay there for :math:`T^e_{com} \sim Uniform (t^e_{c1}, t^e_{c2})` hours. After that, they will return residential POI.


Disease Transmission Model
++++++++++++++++++++++++++
The disease can transmit from an infected person through two kinds of contacts:
- Acquaintance contacts: An individual has a fixed group of acquaintance contacts with size :math:`K_l \sim Uniform(l_{c1}, l_{c2})` in his/her residential POI, and a fixed group of acquaintance contacts with size :math:`K_w \sim Uniform(w_{c1}, w_{c2})` in his/her working POI. At each timestamp, there is a probability :math:`P_c` for an individual to get infected from an infected acquaintance contact.
- Stranger contacts: An individual could be in contact with :math:`K` strangers visiting the same type of POI in the same region at the same time. At each timestamp, there is probability :math:`P_s` for a person to get infected from an infected stranger contact. 


Health status of a person
+++++++++++++++++++++++++
An individual’s health status follows the stages below:

- Stage 1. Susceptible 

- Stage 2. Infected and undiscovered
    * From Stage 1 to Stage 2, people can get infected via contact with infected people, with different probabilities from their contacts.

- Stage 3. Infected and discovered 
    * From Stage 2 to Stage 3, there is a fixed incubation period of :math:`INC` days.

- Stage 4. Infected and critical
    * From Stage 3 to Stage 4, there is a development time period :math:`d \sim Normal(d_1, d_2)`.

- Stage 5. Recovered 
    * From Stage 4 to Stage 5, there is a fixed hospitalized time period of :math:`TREAT` days after he/she is sent to the hospital.


Mobility Intervention Actions
++++++++++++++++++++++++++++++
We can provide different actions to each individual:


- Free: The person can move normally.
- Confined: An individual is confined in the neighborhood that he/she lives in, with access from his/her acquaintance contacts and stranger contacts in the residential region.
- Quarantine: The person is quarantined at home with other individuals sharing the same residential POI. 
- Isolation: The person is isolated, even from the individuals living in the same residential POI.
- Hospitalized: The person is under treatment in the hospital. A person can only be in the hospital after Stage 2.
