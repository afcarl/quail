
Advanced plotting
=================

This tutorial will go over more advanced plotting functionality. Before
reading this, you should take a look at the basic analysis and plotting
tutorial. First, we'll load in some example data. This dataset is an
``egg`` comprised of 30 subjects, who each performed 8 study/test blocks
of 16 words each.

.. code:: ipython2

    import quail
    egg = quail.load_example_data()

Accuracy
--------

.. code:: ipython2

    accuracy = quail.analyze(egg, analysis='accuracy')
    accuracy.head()




.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th></th>
          <th>0</th>
        </tr>
        <tr>
          <th>Subject</th>
          <th>List</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th rowspan="5" valign="top">0</th>
          <th>0</th>
          <td>0.500</td>
        </tr>
        <tr>
          <th>1</th>
          <td>0.500</td>
        </tr>
        <tr>
          <th>2</th>
          <td>0.250</td>
        </tr>
        <tr>
          <th>3</th>
          <td>0.375</td>
        </tr>
        <tr>
          <th>4</th>
          <td>0.625</td>
        </tr>
      </tbody>
    </table>
    </div>



By default, the ``analyze`` function will perform an analysis on each
list separately, so when you plot the result, it will plot a separate
bar for each list, averaged over all subjects:

.. code:: ipython2

    ax = quail.plot(accuracy)



.. image:: advanced_plotting_files/advanced_plotting_5_0.png


We can plot the accuracy for each subject by setting
``plot_type='subject'``, and we can change the name of the subject
grouping variable by setting the ``subjname`` kwarg:

.. code:: ipython2

    ax = quail.plot(accuracy, plot_type='subject', subjname='Subject Number')



.. image:: advanced_plotting_files/advanced_plotting_7_0.png


Furthermore, we can add a title using the ``title`` kwarg, and change
the y axis limits using ``ylim``:

.. code:: ipython2

    ax = quail.plot(accuracy, plot_type='subject', subjname='Subject Number',
                    title='Accuracy by Subject', ylim=[0,1])



.. image:: advanced_plotting_files/advanced_plotting_9_0.png


In addition to bar plots, accuracy can be plotted as a violin or swarm
plot by using the ``plot_style`` kwarg:

.. code:: ipython2

    ax = quail.plot(accuracy, plot_type='subject', subjname='Subject Number',
                    title='Accuracy by Subject', ylim=[0,1], plot_style='violin')
    ax = quail.plot(accuracy, plot_type='subject', subjname='Subject Number',
                    title='Accuracy by Subject', ylim=[0,1], plot_style='swarm')



.. image:: advanced_plotting_files/advanced_plotting_11_0.png



.. image:: advanced_plotting_files/advanced_plotting_11_1.png


We can also group the subjects. This is useful in cases where you might
want to compare analysis results across multiple experiments. To do this
we will reanalyze the data, averaging over lists within a subject, and
then use the ``subjgroup`` kwarg to group the subjects into two sets:

.. code:: ipython2

    accuracy = quail.analyze(egg, analysis='accuracy', listgroup=['average']*8)
    accuracy.head()




.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th></th>
          <th>0</th>
        </tr>
        <tr>
          <th>Subject</th>
          <th>List</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <th>average</th>
          <td>0.476562</td>
        </tr>
        <tr>
          <th>1</th>
          <th>average</th>
          <td>0.960938</td>
        </tr>
        <tr>
          <th>2</th>
          <th>average</th>
          <td>0.546875</td>
        </tr>
        <tr>
          <th>3</th>
          <th>average</th>
          <td>0.757812</td>
        </tr>
        <tr>
          <th>4</th>
          <th>average</th>
          <td>0.406250</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython2

    ax = quail.plot(accuracy, subjgroup=['Experiment 1']*15+['Experiment 2']*15)



.. image:: advanced_plotting_files/advanced_plotting_14_0.png


Oops, what happened there? By default, the ``plot`` function looks to
the List column of the df to group the data. To group according to
subject group, we must tell the plot function to plot by ``subjgroup``.
This can be achieved by setting ``plot_type='subject'``:

.. code:: ipython2

    ax = quail.plot(accuracy, subjgroup=['Experiment 1']*15+['Experiment 2']*15, plot_type='subject')



.. image:: advanced_plotting_files/advanced_plotting_16_0.png


If you also have a list grouping (such as first 4 lists / second 4
lists), you can plot the interaction by setting ``plot_type='split'``.
This will create a plot with respect to both the ``subjgroup`` and
``listgroup``:

.. code:: ipython2

    accuracy = quail.analyze(egg, analysis='accuracy', listgroup=['First 4 Lists']*4+['Second 4 Lists']*4)
    ax = quail.plot(accuracy, subjgroup=['Experiment 1']*15+['Experiment 2']*15, plot_type='split')



.. image:: advanced_plotting_files/advanced_plotting_18_0.png


Like above, these plots can also be violin or swarm plots:

.. code:: ipython2

    ax = quail.plot(accuracy, subjgroup=['Experiment 1']*15+['Experiment 2']*15, plot_type='split', plot_style='violin')
    ax = quail.plot(accuracy, subjgroup=['Experiment 1']*15+['Experiment 2']*15, plot_type='split', plot_style='swarm')



.. image:: advanced_plotting_files/advanced_plotting_20_0.png



.. image:: advanced_plotting_files/advanced_plotting_20_1.png


Memory fingerprints
-------------------

The Memory Fingerprint plotting works exactly the same as the the
accuracy plots, with the except that ``plot_type='split'`` only works
for the accuracy plots, and the default ``plot_style`` is a violinplot,
instead of a barplot.

.. code:: ipython2

    fingerprint = quail.analyze(egg, analysis='fingerprint', listgroup=['First 4 Lists']*4+['Second 4 Lists']*4)
    ax = quail.plot(fingerprint, subjgroup=['Experiment 1']*15+['Experiment 2']*15, plot_type='subject')
    ax = quail.plot(fingerprint, subjgroup=['Experiment 1']*15+['Experiment 2']*15, plot_type='list')



.. image:: advanced_plotting_files/advanced_plotting_23_0.png



.. image:: advanced_plotting_files/advanced_plotting_23_1.png


Other analyses
--------------

Like the plots above, spc, pfr and lagcrp plots can all be plotted
according to ``listgroup`` or ``subjgroup`` by setting the ``plot_type``
kwarg.

Plot by list grouping
~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython2

    listgroup = ['First 4 Lists']*4+['Second 4 Lists']*4
    plot_type = 'list'
    
    spc = quail.analyze(egg, analysis='spc', listgroup=listgroup)
    ax = quail.plot(spc, plot_type=plot_type)
    
    pfr = quail.analyze(egg, analysis='pfr', listgroup=listgroup)
    ax = quail.plot(pfr, plot_type=plot_type)
    
    lagcrp = quail.analyze(egg, analysis='lagcrp', listgroup=listgroup)
    ax = quail.plot(lagcrp, plot_type=plot_type)



.. image:: advanced_plotting_files/advanced_plotting_26_0.png



.. image:: advanced_plotting_files/advanced_plotting_26_1.png



.. image:: advanced_plotting_files/advanced_plotting_26_2.png


Plot by subject grouping
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython2

    listgroup=['average']*8
    subjgroup = ['Experiment 1']*15+['Experiment 2']*15
    plot_type = 'subject'
    
    spc = quail.analyze(egg, analysis='spc', listgroup=listgroup)
    ax = quail.plot(spc, subjgroup=subjgroup, plot_type=plot_type)
    
    pfr = quail.analyze(egg, analysis='pfr', listgroup=listgroup)
    ax = quail.plot(pfr, subjgroup=subjgroup, plot_type=plot_type)
    
    lagcrp = quail.analyze(egg, analysis='lagcrp', listgroup=listgroup)
    ax = quail.plot(lagcrp, subjgroup=subjgroup, plot_type=plot_type)



.. image:: advanced_plotting_files/advanced_plotting_28_0.png



.. image:: advanced_plotting_files/advanced_plotting_28_1.png



.. image:: advanced_plotting_files/advanced_plotting_28_2.png

