Mobility Intervention of Epidemics Challenge
======================
In response to the current world-wide concern of Epidemic, we host this challenge on finding the best intervention policy to fight against the pandemic. The challenge will be evaluated on a human mobility and epidemic transmission simulator, which is provided to the global research community to apply recent advances in data science and AI to generate new insights in support of the ongoing fight against this infectious disease. 

The challenge will be evaluated on a human mobility and epidemic transmission simulator, which are based on real data. Participants will design different strategies to minimize the spread of the virus. The winners will be invited to present at the workshop.



.. only:: html

   .. figure:: visualization.gif
    :align: center
    

Goal & Metrics
--------------

.. role:: raw-latex(raw)
    :format: latex html

.. raw:: html

   <script type="text/javascript" src="http://localhost/mathjax/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

In this competition, we aim to look for effective human mobility intervention policies in a Epidemic. Generally, our goal is to minimize the total number of infected people and, at the same time, minimize the intervention on human mobility.

We first define two basic metrics:

- :math:`I`: the number of people who are infected with coronavirus.
- :math:`Q`: the total number of days that an individual has been under mobility interventions, including confined at community, quarantined at home, isolated, and hospitalized.  (We could have a weighted sum of home and isolated :math:`Q = \frac{\lambda_h Home + Isolated} {\lambda_h + 1}`.)

Based on these two basic metrics, we calculate the following score for this competition.
- :raw:: latex html

    L =\left\{
    \begin{aligned}
    & Q \quad \text{if} I< 100 \\
    & 10^6 \quad \text{else} \\
    \end{aligned}
    \right.`


The weighted sum of :math:`I` and :math:`Q`, :math:`\lambda` is a predefined factor.


+------------------+---+------------------+--------+
|     Parameter specification in this challenge    |
+------------------+---+------------------+--------+
| I                | 11|           Q      | 100000 |
+------------------+---+------------------+--------+


Support
-------

If you are having issues, please let us know.
We have a mailing list located at: epidemic-challenge@google-groups.com

License
-------

The project is licensed under the BSD license.