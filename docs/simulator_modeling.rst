Simulator
#########

Simulator Modeling
==================

The epidemic spreading in this simulator is modeled according to what is known about the COVID-19. The assumptions about the epidemic spreading and mobility implemented in the simulator are based on the published research as well as interactions with the epidemiologists. We plan to update the simulator as more and more about epidemic will be known.

The simulator simulates individual mobility in a city of :math:`R` POIs with :math:`M` people. Each POI belongs to one of the three categories: working, residential, and commercial. An individual is associated with two fixed POIs: one for residential, and one for working. 

+------------------+---+------------------+--------+
|     Parameter specification in this challenge    |
+------------------+---+------------------+--------+
| R                | 11|           M      | 10000  |
+------------------+---+------------------+--------+


Human Mobility Model
++++++++++++++++++++
A person has different modes of mobility during workdays and weekends.

On working days, a person will start from home to working POI at a certain time :math:`T^d_{start} \sim Uniform(t^d_{s1}, t^d_{s2})`, and stay there for :math:`T_{work} \sim Uniform(t_{w1}, t_{w2})` hours. After work, they may visit a nearby commercial POI (randomly sampled from :math:`K_{com}` nearest POIs of the working POI)  with a probability of :math:`P^d_{com}` and stay there for :math:`T^d_{com} \sim Uniform (t^d_{c1}, t^d_{c2})` hours. Then, they will return to residential POI.

On weekends, people may visit a random commercial POI within the whole city at a certain time :math:`T^e_{start} \sim Uniform(t^e_{s1}, t^e_{s2})` with a probability :math:`P^e_{com}` and stay there for :math:`T^e_{com} \sim Uniform (t^e_{c1}, t^e_{c2})` hours. After that, they will return residential POI.

+------------------+---+------------------+---+-------------------+-----+-------------------+-----+
| :math:`t^d_{s1}` | 1 | :math:`t^d_{s2}` | 2 | :math:`t_{w1}`    |  7  | :math:`t_{w2}`    | 10  |
+------------------+---+------------------+---+-------------------+-----+-------------------+-----+
| :math:`t^d_{c1}` | 1 | :math:`t^d_{c2}` | 2 | :math:`t^e_{s1}`  |  1  | :math:`t^e_{s2}`  |  5  |
+------------------+---+------------------+---+-------------------+-----+-------------------+-----+
| :math:`t^e_{c1}` | 1 | :math:`t^e_{c2}` | 2 | :math:`P^d_{com}` | 0.1 | :math:`P^e_{com}` | 0.3 |
+------------------+---+------------------+---+-------------------+-----+-------------------+-----+

Disease Transmission Model
++++++++++++++++++++++++++
The disease can transmit from an infected person through two kinds of contacts:

- Acquaintance contacts: An individual has a fixed group of acquaintance contacts with size :math:`K_l \sim Uniform(l_{c1}, l_{c2})` in his/her residential POI, and a fixed group of acquaintance contacts with size :math:`K_w \sim Uniform(w_{c1}, w_{c2})` in his/her working POI. At each timestamp, there is a probability :math:`P_c` for an individual to get infected from an infected acquaintance contact.

- Stranger contacts: An individual could be in contact with strangers visiting the same type of POI in the same region at the same time. At each timestamp, there is probability :math:`P_s` for a person to get infected from an infected stranger contact. 

+-----------------+---------+-----------------+---+-----------------+--------+-----------------+----+
| :math:`l_{c_1}` | 1       | :math:`l_{c_2}` | 6 | :math:`w_{c_1}` | 5      | :math:`w_{c_2}` | 15 |
+-----------------+---------+-----------------+---+-----------------+--------+-----------------+----+
| :math:`P_c`     | 0.00033 |                 |   | :math:`P_s`     | 0.00005|     --------    |    |
+-----------------+---------+-----------------+---+-----------------+--------+-----------------+----+

Health status of a person
+++++++++++++++++++++++++
An individual’s health status follows the stages below:

+-----------------------------+------------------------------+----------+------------+----------+--------+
| Health status               | Description                  | Infected | Contagious | Symptoms | Immune |
+=============================+==============================+==========+============+==========+========+
| 1. Susceptible case            | liable to be infected        | -        | -          | -        | -      |
+-----------------------------+------------------------------+----------+------------+----------+--------+
| 2. Pre-symptomatic             | before the onset of symptoms | ✔        | ✔          | -        | -      |
| infected case               |                              |          |            |          |        |
+-----------------------------+------------------------------+----------+------------+----------+--------+
| 3. Symptomatic infected case   | showing signs and symptoms   | ✔        | ✔          | ✔        | -      |
|                             | compatible with infection    |          |            |          |        |
+-----------------------------+------------------------------+----------+------------+----------+--------+
| 4. Symptomatic infected cases  | symptomatic with severe      | ✔        | ✔          | ✔        | -      |
| with critical condition     | acute respiratory illness    |          |            |          |        |
+-----------------------------+------------------------------+----------+------------+----------+--------+
| 5. Recovered                   | recovered and resistant      | -        | -          | -        | ✔      |
+-----------------------------+------------------------------+----------+------------+----------+--------+



- Stage 1. ``Susceptible``: Liable to be infected

- Stage 2.  ``Pre-symptomatic`` infected: Infected and undiscovered
    * From Stage 1 to Stage 2, people can get infected via contact with infected people, with different probabilities from their contacts.

- Stage 3. ``Symptomatic`` infected:  Infected and showing signs and symptoms
    * From Stage 2 to Stage 3, there is a fixed incubation period of :math:`INC` days.

- Stage 4. Symptomatic infected with ``critical`` health condition
    * From Stage 3 to Stage 4, there is a development time period :math:`d \sim Normal(d_1, d_2)`.

- Stage 5. ``Recovered``: recovered and resistant
    * From Stage 4 to Stage 5, there is a fixed hospitalized time period of :math:`TREAT` days after he/she is sent to the hospital.

+-------------+---+-------------+---+-------------+---+---------------+-----+
| :math:`INC` | 3 | :math:`d_1` | 2 | :math:`d_2` | 3 | :math:`TREAT` | inf |
+-------------+---+-------------+---+-------------+---+---------------+-----+



.. note::
	Terms are  in align with recent variations of the Susceptible-Infected-Resistant (SIR) compartment models in the context of Epidemic modeling and WHO guidelines:

	1. Ferretti, L., Wymant, C., Kendall, M., Zhao, L., Nurtay, A., Abeler-Dörner, L., ... & Fraser, C. (2020). Quantifying SARS-CoV-2 transmission suggests epidemic control with digital contact tracing. Science.
	2. World Health Organization. (2020, April 24). Situation reports. Retrieved April 24, 2020, from https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports/


Mobility Intervention Actions
=============================
We can provide 5 levels of mobility intervention to each individual:


- Level 0 - No intervene: The individual can move normally
- Level 1 - Confine: An individual is confined in the neighborhood that he/she lives in, with access from his/her acquaintance contacts and stranger contacts in the residential region.
- Level 2 - Quarantine: The person is quarantined at home, with access from acquaintance contacts sharing the same residential POI. 
- Level 3 - Isolate: The person is isolated, without access from the acquaintance contacts living in the same residential POI.
- Level 4 - Hospitalize: The person is under treatment in the hospital. 

.. note::
    When an individual is intended with multiple interventions , only the highest level of intervention will be applied.




Evaluation Metrics
==================

We first define two basic metrics:

- :math:`I`: the total number of infected people from day 1 to day :math:`T`.
- :math:`Q`: a weighted sum of the number of people confined at community, quarantined at home, isolated, and hospitalized from day 1 to day :math:`T`. We have a weighted sum as:

    - :math:`Q = \lambda_h * inHospitalNum + \lambda_i * isolateNum + \lambda_q * quarantineNum + \lambda_c * confineNum`

Based on these two basic metrics, we calculate the following score for this competition.

.. math::

	Score = exp\{\frac{I}{\theta_I}\}+Q_w*exp\{\frac{Q}{\theta_Q}\}.

Our goal is to minimize the score, evaluated on the 60th day of simulation.

+-------+------+-----+--------+-----+--------+-----+--------+
| θ_I   | 1000 | θ_Q | 100000 | Q_w |  1.0   |  T  |   60   |
+-------+------+-----+--------+-----+--------+-----+--------+
