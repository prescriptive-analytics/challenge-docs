.. _replay:

Visualization/Replay
======

We provide offline visualization after each simulation.


Start
------

1. enter the ``frontend`` folder and open ``index.html`` in your browser.

2. choose the replay log file (as defined by ``City Visualization File`` field in the config file, **not 'relayLogFile'**) and wait for it to be loaded. When it has finished loading, there will be a message shown in the info box.

3. choose the replay file (as defined by ``replayLogFile`` field in the config file) .

4. choose the chart data file (optional, see section *Chart* below).

5. press ``Start`` button to start the replay.

Control
-------

- Use the mouse to navigate. Dragging and mouse wheel zooming are supported.

- Move the slider in Control Box to adjust the replay speed. You can also press ``1`` on keyboard to slow down or ``2`` to speed up.

- Press ``Pause`` button in Control Box to pause/resume. You can also double-click on the map to pause and resume.

- Press ``[`` or ``]`` on keyboard to take a step backward or forward.

- To restart the replay, just press ``Start`` button again.

- The ``debug`` option enables displaying the ID of vehicles, roads and intersections during a mouse hover. **This will cause a slower replaying**, so we suggest using it only for debugging purposes.

Chart
------

The player supports showing the change of different metrics in a chart simultaneously with the replay process.

To provide required data, a log file in a format as shown below is needed:

+----+--------------------+-----------+--------------+---------------------------------------------------------+
| #  | Name               | Data Tpye | Example Data | Description                                             |
+====+====================+===========+==============+=========================================================+
| 0  | day                | int       | 0            | Current day in simulation                               |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 1  | hospitalizeNum     | int       | 0            | # of hospitalized people                                |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 2  | isolateNum         | int       | 0            | # of isolated people                                    |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 3  | quarantineNum      | int       | 0            | # of quarantined people                                 |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 4  | confineNumfree_num | int       | 0            | # of confined people                                    |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 5  | free               | int       | 201          | # of people without intervention                        |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 6  | CurrentHealthy     | int       | 199          | # of people that are not infected                       |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 7  | CurrentInfected    | int       | 2            | # of infected cases                                     |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 8  | CurrentSusceptible | int       | 199          | # of susceptible people                                 |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 9  | CurrentIncubation  | int       | 2            | # of pre-symptomatic cases                              |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 10 | CurrentDiscovered  | int       | 0            | # of symptomatic cases                                  |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 11 | CurrentCritical    | int       | 0            | # of critical cases                                     |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 12 | CurrentRecovered   | int       | 0            | # of recovered cases                                    |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 13 | AccDiscovered      | int       | 0            | Accumulated # of symptomatic cases                      |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 14 | AccCritical        | int       | 0            | Accumulated # of critical cases                         |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 15 | AccAcquaintance    | int       | 0            | Accumulated # of infected through stranger contacts     |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 16 | AccStranger        | int       | 0            | Accumulated # of infected through acquaintance contacts |
+----+--------------------+-----------+--------------+---------------------------------------------------------+
| 17 | measurement        | int       | 2            | an example measurement                                  |
+----+--------------------+-----------+--------------+---------------------------------------------------------+


Each row stands for one day and each column stands for a specific metric.

In one row, numbers are separated by one spaces and comma.

The numbers in one column will be shown as points connected by one line in the chart.

.. note::
  Make sure that each row is corresponding with the right time step.

Notes
------

- To get the example replay files, run ``download_replay.py`` under ``frontend`` folder.

- If you create a new Engine object with same ``replayLogFile``, it will clear the old replay file first

- Using ``eng.reset()`` won't clear old replays, it will append newly generated replay to the end of ``replayLogFile``

- You can change ``replayLogFile`` during runtime using ``set_replay_file``, see :ref:`set-replay-file`
