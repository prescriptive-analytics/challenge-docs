Coronavirus Challenge
======================

In response to the current world-wide concern of coronavirus, we would like to host a challenge on prescriptive analytics for coronavirus. The challenge will be evaluated on a human mobility and epidemic transmission simulator, which are based on real data. Participants will design different strategies to minimize the spread of the virus. The winners will be invited to present at the workshop.

.. figure:: https://github.com/prescriptive-analytics/chanllenge-docs/raw/master/data/results.gif
    :align: center
    

Goal & Metrics
--------------

Our goal is to minimize the #infected and at the same time, quarantine people as few as possible. Therefore, we have defined the following metrics:


1.  \#_{infected} + \lambda \times \#_{quarantined}: The weighted sum of the number of infected people and the number of quarantined people.
2. \#_{quarantined}\ s.t. \ \#_{infected} \leq 100 : The number of people needs to be quarantined in order to keep #infected under 100.



Contribute
----------

- Issue Tracker: https://github.com/gjzheng93/COVID/issues
- Source Code: https://github.com/gjzheng93/COVID/

Support
-------

If you are having issues, please let us know.
We have a mailing list located at: coronavirus-changllenge@google-groups.com

License
-------

The project is licensed under the BSD license.