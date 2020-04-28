Simulator Modeling
******************
The infection spread in this simulator is modeled according to what is known about COVID-19. The assumptions about the COVID-19 spread and mobility implemented in the simulator are based on the published research as well as interactions with the epidemiologists. We plan to update the simulator as more and more about COVID-19 will be known.

The simulator simulates individual mobility in a city of :math:`R` Regions with :math:`M` people. Each region consists of several POIs, where each POI can contains three types of POIs: working, residential, and commercial. An individual is associated with two fixed POIs: one for residential, and one for working. 


Human Mobility Model
++++++++++++++++++++
A person has different modes of mobility during workdays and weekends.

On working days, a person will start from home to working POI at a certain time :math:`T^d_{start} \sim Uniform(t^d_{s1}, t^d_{s2})`, and stay there for :math:`T_{work} \sim Uniform(t_{w1}, t_{w2})` hours. After work, they may visit a nearby commercial POI (randomly sampled from :math:`K_{com}` nearest POIs of the working POI)  with a probability of :math:`P^d_{com}` and stay there for :math:`T^d_{com} \sim Uniform (t^d_{c1}, t^d_{c2})` hours. Then, they will return to residential POI.

On weekends, people may visit a random commercial POI within the whole city at a certain time :math:`T^e_{start} \sim Uniform(t^e_{s1}, t^e_{s2})` with a probability :math:`P^e_{com}` and stay there for :math:`T^e_{com} \sim Uniform (t^e_{c1}, t^e_{c2})` hours. After that, they will return residential POI.

+----------+---+----------+---+-----------+-----+-----------+-----+
| t^d_{s1} | 1 | t^d_{s2} | 2 | t_{w1}    |  7  | t_{w2}    | 10  |
+----------+---+----------+---+-----------+-----+-----------+-----+
| t^d_{c1} | 1 | t^d_{c2} | 2 | t^e_{s1}  |  1  | t^e_{s2}  |  5  |
+----------+---+----------+---+-----------+-----+-----------+-----+
| t^e_{c1} | 1 | t^e_{c2} | 2 | P^d_{com} | 0.1 | P^e_{com} | 0.3 |
+----------+---+----------+---+-----------+-----+-----------+-----+

Disease Transmission Model
++++++++++++++++++++++++++
The disease can transmit from an infected person through two kinds of contacts:

- Acquaintance contacts: An individual has a fixed group of acquaintance contacts with size :math:`K_l \sim Uniform(l_{c1}, l_{c2})` in his/her residential POI, and a fixed group of acquaintance contacts with size :math:`K_w \sim Uniform(w_{c1}, w_{c2})` in his/her working POI. At each timestamp, there is a probability :math:`P_c` for an individual to get infected from an infected acquaintance contact.

- Stranger contacts: An individual could be in contact with strangers visiting the same type of POI in the same region at the same time. At each timestamp, there is probability :math:`P_s` for a person to get infected from an infected stranger contact. 

+---------+---------+---------+---+---------+--------+---------+----+
| l_{c_1} | 1       | l_{c_2} | 6 | w_{c_1} | 5      | w_{c_2} | 15 |
+---------+---------+---------+---+---------+--------+---------+----+
| P_c     | 0.00033 |         |   | P_s     | 0.00005|         |    |
+---------+---------+---------+---+---------+--------+---------+----+

Health status of a person
+++++++++++++++++++++++++
An individual’s health status follows the stages below:

+-----------------------------+------------------------------+----------+------------+----------+--------+
| Health status               | Description                  | Infected | Contagious | Symptoms | Immune |
+=============================+==============================+==========+============+==========+========+
| Susceptible case            | liable to be infected        | -        | -          | -        | -      |
+-----------------------------+------------------------------+----------+------------+----------+--------+
| Pre-symptomatic             | before the onset of symptoms | ✔        | ✔          | -        | -      |
| infected case               |                              |          |            |          |        |
+-----------------------------+------------------------------+----------+------------+----------+--------+
| Symptomatic infected case   | showing signs and symptoms   | ✔        | ✔          | ✔        | -      |
|                             | compatible with infection    |          |            |          |        |
+-----------------------------+------------------------------+----------+------------+----------+--------+
| Symptomatic infected cases  | symptomatic with severe      | ✔        | ✔          | ✔        | -      |
| with critical condition     | acute respiratory illness    |          |            |          |        |
+-----------------------------+------------------------------+----------+------------+----------+--------+
| Recovered                   | recovered and resistant      | -        | -          | -        | ✔      |
+-----------------------------+------------------------------+----------+------------+----------+--------+


- Stage 1. **Susceptible**: Liable to be infected

- Stage 2. **Pre-symptomatic** infected: Infected and undiscovered
    * From Stage 1 to Stage 2, people can get infected via contact with infected people, with different probabilities from their contacts.

- Stage 3. **Symptomatic** infected:  Infected and showing signs and symptoms
    * From Stage 2 to Stage 3, there is a fixed incubation period of :math:`INC` days.

- Stage 4. Symptomatic infected with **critical** condition
    * From Stage 3 to Stage 4, there is a development time period :math:`d \sim Normal(d_1, d_2)`.

- Stage 5. **Recovered**: recovered and resistant
    * From Stage 4 to Stage 5, there is a fixed hospitalized time period of :math:`TREAT` days after he/she is sent to the hospital.

+-----+---+-----+---+-----+---+-------+-----+
| INC | 3 | d_1 | 2 | d_2 | 3 | TREAT | inf |
+-----+---+-----+---+-----+---+-------+-----+



.. note::
	Terms are  in align with recent variations of the Susceptible-Infected-Resistant (SIR) compartment models in the context of COVID-19 modeling and WHO guidelines:
	[1] Ferretti, L., Wymant, C., Kendall, M., Zhao, L., Nurtay, A., Abeler-Dörner, L., ... & Fraser, C. (2020). Quantifying SARS-CoV-2 transmission suggests epidemic control with digital contact tracing. Science.
	[2] World Health Organization. (2020, April 24). Situation reports. Retrieved April 24, 2020, from https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports/

Mobility Intervention Actions
++++++++++++++++++++++++++++++
We can provide different actions to each individual:


- Free: The person can move normally.
- Confined: An individual is confined in the neighborhood that he/she lives in, with access from his/her acquaintance contacts and stranger contacts in the residential region.
- Quarantine: The person is quarantined at home with other individuals sharing the same residential POI. 
- Isolation: The person is isolated, even from the individuals living in the same residential POI.
- Hospitalized: The person is under treatment in the hospital. A person can only be in the hospital after Stage 2.
