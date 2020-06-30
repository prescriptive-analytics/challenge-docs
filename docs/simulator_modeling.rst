Simulator
#########

Simulator Modeling
==================

The simulator simulates individual mobility in a city of :math:`R` areas with :math:`M` people. Each area belongs to one of the three categories: working, residential, and commercial. An individual is associated with two fixed POIs: one for residential, and one for working. 

+----------------------+---+------------------+--------+
|     Default parameters in the simulator              |
+----------------------+---+------------------+--------+
| :math:`\mathcal{A}`  | 11|           M      | 10000  |
+----------------------+---+------------------+--------+

.. note::
    - In the competition, we have five simulated scenarios, whose parameters will be specified in the `next section <https://hzw77-demo.readthedocs.io/en/round2/scenario.html>`_.


Human Mobility Model
++++++++++++++++++++
Our simulator simulates the human mobility from 8 A.M. to 10 P.M. with one simulation step corresponding with one hour in the real world. An individual has different modes of mobility during weekdays and weekends. 

On weekdays, an individual will start from residential area to working area at a certain time :math:`T^d_{start} \sim U(t^d_{s1}, t^d_{s2})`, and stay there for :math:`T_{work} \sim U(t_{w1}, t_{w2})` hours. After work, they may visit a nearby commercial area (randomly sampled from :math:`K_{com}` nearest areas of the working area)  with a probability of :math:`P^d_{com}` and stay there for :math:`T^d_{com} \sim U (t^d_{c1}, t^d_{c2})` hours. Then, they will return to residential area.

On weekends, people may visit a random commercial area at a certain time :math:`T^e_{start} \sim U(t^e_{s1}, t^e_{s2})` with a probability :math:`P^e_{com}` and stay there for :math:`T^e_{com} \sim U (t^e_{c1}, t^e_{c2})` hours. After that, they will return to residential area.

+------------------+---+------------------+---+-------------------+-----+-------------------+-----+
| :math:`t^d_{s1}` | 1 | :math:`t^d_{s2}` | 3 | :math:`t_{w1}`    |  7  | :math:`t_{w2}`    | 10  |
+------------------+---+------------------+---+-------------------+-----+-------------------+-----+
| :math:`t^d_{c1}` | 1 | :math:`t^d_{c2}` | 2 | :math:`t^e_{s1}`  |  1  | :math:`t^e_{s2}`  |  5  |
+------------------+---+------------------+---+-------------------+-----+-------------------+-----+
| :math:`t^e_{c1}` | 1 | :math:`t^e_{c2}` | 2 | :math:`P^d_{com}` | 0.1 | :math:`P^e_{com}` | 0.3 |
+------------------+---+------------------+---+-------------------+-----+-------------------+-----+
| :math:`K`        | 1 |                  |   |                   |     |                   |     |
+------------------+---+------------------+---+-------------------+-----+-------------------+-----+

Disease Transmission Model
++++++++++++++++++++++++++
The disease can transmit from an infected individual through two kinds of contacts:

- Acquaintance contacts: An individual has a fixed small group of acquaintance contacts with size :math:`K_l \sim U(l_{c1}, l_{c2})` in his/her residential area, and a fixed group of acquaintance contacts with size :math:`K_w \sim U(w_{c1}, w_{c2})` in his/her working area. Note that not all the individuals in the same residential/working area are the acquaintance contactsof the individual. At each timestamp, there is a probability :math:`P_c` for an individual to get infected from an infected acquaintance contact.

- Stranger contacts: An individual could be in contact with strangers visiting the same area at the same time. At each timestamp, there is probability :math:`P_s` for an individual to get infected from an infected stranger contact. 

+-----------------+---------+-----------------+---+-----------------+--------+-----------------+----+
| :math:`l_{c_1}` | 1       | :math:`l_{c_2}` | 6 | :math:`w_{c_1}` | 5      | :math:`w_{c_2}` | 15 |
+-----------------+---------+-----------------+---+-----------------+--------+-----------------+----+
| :math:`P_c`     | 2.5e-2  |                 |   | :math:`P_s`     | 5e-3   |                 |    |
+-----------------+---------+-----------------+---+-----------------+--------+-----------------+----+

.. note::
    The parameters are calibrated to align with the :math:`R_0` of COVID-19 (2 ~ 2.5) provided by World Health Organization:

    - World Health Organization. (2020, May 8). Report of the WHO-China Joint Mission on Coronavirus Disease 2019 (COVID-19). Retrieved May 8, 2020, from https://www.who.int/docs/default-source/coronaviruse/who-china-joint-mission-on-covid-19-final-report.pdf

Health Status of an Individual
+++++++++++++++++++++++++
An individual’s health status follows the stages below:

+---------------------+------------------------------+----------+------------+----------+--------+
| Health status       | Description                  | Infected | Contagious | Symptoms | Immune |
+---------------------+------------------------------+----------+------------+----------+--------+
| 1. Susceptible      | liable to be infected        |          |            |          |        |
+---------------------+------------------------------+----------+------------+----------+--------+
| 2. Pre-symptomatic  | before the onset of symptoms | ✔        | ✔          |          |        |
+---------------------+------------------------------+----------+------------+----------+--------+
| 3. Symptomatic      | showing symptoms             | ✔        | ✔          | ✔        |        |
|                     |                              |          |            |          |        |
|                     | compatible with infection    |          |            |          |        |
+---------------------+------------------------------+----------+------------+----------+--------+
| 4. Critical         | symptomatic with severe      | ✔        | ✔          | ✔        |        |
|                     |                              |          |            |          |        |
|                     | acute respiratory illness    |          |            |          |        |
+---------------------+------------------------------+----------+------------+----------+--------+
| 5. Recovered        | recovered and resistant      |          |            |          | ✔      |
+---------------------+------------------------------+----------+------------+----------+--------+


- Stage 1. ``Susceptible``: Liable to be infected

- Stage 2.  ``Pre-symptomatic`` infected: Infected and undiscovered
    * From Stage 1 to Stage 2, people can get infected via contact with infected people, with different probabilities from their contacts.

- Stage 3. ``Symptomatic`` infected:  Infected and showing signs and symptoms
    * From Stage 2 to Stage 3, there is an incubation period of :math:`INC \sim U(inc_1, inc_2)` days.

- Stage 4. Symptomatic infected with ``critical`` health condition
    * From Stage 3 to Stage 4, there is a development time period :math:`d \sim \mathcal{N}(\mu, \phi)`.

- Stage 5. ``Recovered``: recovered and resistant
    * Each infected individual can recover through self-recovery or hospitalization. He/she will recover after being hospitalized consecutively for :math:`TREAT` days, and become immune to the disease. He/she can also self-recover after :math:`RECOVER \sim U(rec_1, recc_2)` days.

+---------------+---+---------------+---+-------------+---+-------------+---+---------------+-----+
| :math:`inc_1` | 1 | :math:`inc_2` | 5 | :math:`\mu` | 2 |:math:`\phi` | 3 | :math:`TREAT` | 15  |
+---------------+---+---------------+---+-------------+---+-------------+---+---------------+-----+
| :math:`rec_1` |15 | :math:`rec_2` | 30|             |   |             |   |               |     |
+---------------+---+---------------+---+-------------+---+-------------+---+---------------+-----+



.. note::
    Terms are  in align with recent variations of the Susceptible-Infected-Resistant (SIR) compartment models in the context of Epidemic modeling and WHO guidelines:

    1. Ferretti, L., Wymant, C., Kendall, M., Zhao, L., Nurtay, A., Abeler-Dörner, L., ... & Fraser, C. (2020). Quantifying SARS-CoV-2 transmission suggests epidemic control with digital contact tracing. Science.
    2. World Health Organization. (2020, April 24). Situation reports. Retrieved April 24, 2020, from https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports/


Mobility Intervention Actions
=============================
We can provide 5 levels of mobility intervention to each individual:


- Level 0 - No intervene: The individual can move normaly.
- Level 1 - Confine: An individual is confined in the neighborhood that he/she lives in, in contact with his/her acquaintance contacts and stranger contacts in the residential area.
- Level 2 - Quarantine: The individual is quarantined at home, in contact with acquaintance contacts sharing the same residential area. 
- Level 3 - Isolate: The individual is isolated, even from the acquaintance contacts living in the same residential area.
- Level 4 - Hospitalize: The individual is under treatment in the hospital. 


+------------------+-----------------------------------------------------------------------+
|                  |                            In Contact with?                           |
+------------------+-----------------------------+-----------------------------------------+
| Intervention     | acquaintance contacts       | stranger contacts                       |
+------------------+-----------------------------+-----------------------------------------+
| #0: No Intervene | ✔ (Residential and working) | ✔ (Residential, working and commercial) |
+------------------+-----------------------------+-----------------------------------------+
| #1: Confine      | ✔ (Residential only)        | ✔ (Residential only)                    |
+------------------+-----------------------------+-----------------------------------------+
| #2: Quarantine   | ✔ (Residential only)        | ✘                                       |
+------------------+-----------------------------+-----------------------------------------+
| #3: Isolate      | ✘                           | ✘                                       |
+------------------+-----------------------------+-----------------------------------------+
| #4: Hopitalize   | ✘                           | ✘                                       |
+------------------+-----------------------------+-----------------------------------------+

.. note::
    - When an individual is intended with multiple interventions , only the highest level of intervention will be applied.
    - Intervention actions are only effective when being set at the start of one day.



Evaluation Metrics
==================

We first define two basic metrics:

- :math:`I`: the total number of infected people on day :math:`T`.
- :math:`Q`: the weighted sum of :math:`N_v`, where if an individual is under intervention :math:`v` for one day, it will put towards adding 1 towards :math:`N_v` (:math:`v\in\{hospitalized, isolated, quarantined, confined\}`):

    - :math:`Q = \lambda_h * N_{hospitalized} + \lambda_i * N_{isolated} + \lambda_q * N_{quarantined} + \lambda_c * N_{confined}`


Based on these two basic metrics, we calculate the following score for this competition.

.. math::

    Score = exp\{\frac{I}{\theta_I}\}+Q_w*exp\{\frac{Q}{\theta_Q}\}.

Our goal is to minimize the score, evaluated on the 60th day of simulation.

+--------------------+------+-------------------+--------+-------------------+--------+-------------------+--------+
| :math:`\theta_I`   | 500  | :math:`\theta_Q`  | 10000  | :math:`Q_w`       |  1.0   |  :math:`T`        |   60   |
+--------------------+------+-------------------+--------+-------------------+--------+-------------------+--------+
| :math:`\lambda_h`  | 1.0  | :math:`\lambda_i` |  0.5   | :math:`\lambda_q` |  0.3   | :math:`\lambda_c` |  0.2   |
+--------------------+------+-------------------+--------+-------------------+--------+-------------------+--------+
