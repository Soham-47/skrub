.. currentmodule:: skrub
.. _user_guide_data_ops_evaluating_debugging_dataops:

Evaluating and debugging the DataOps plan with :meth:`.skb.full_report() <DataOp.skb.full_report>`
==================================================================================================

All operations on DataOps are recorded in a computational graph, which can be
inspected with :meth:`.skb.full_report() <DataOp.skb.full_report>`. This method
generates a html report that shows the full plan, including all nodes, their names,
descriptions, and the transformations applied to the data. It is possible to give a
title to the evaluation report this way:
``my_data_op.skb.full_report(title="my title")``.

An example of the report can be found
`here <../../../_static/credit_fraud_report/index.html>`_.

For each node in the plan, the report shows:

- The name and the description of the node, if present.
- Predecessor and successor nodes in the computational graph.
- Where the code relative to the node is defined.
- The estimator fitted in the node along with its parameters (if applicable).
- The preview of the data at that node.

Additionally, if computations fail in the plan, the report shows the offending
node and the error message, which can help in debugging the plan.

By default, reports are saved in the ``skrub_data/execution_reports`` directory, but
they can be saved to a different location with the ``ouptut_dir`` parameter.
Note that the default path can be altered with the
``SKRUB_DATA_DIR`` environment variable. See :ref:`user_guide_configuration_parameters`
for more details.

Generating reports from a SkrubLearner
======================================

When working with a :class:`SkrubLearner`, it is also possible to generate a report
directly from the learner using :meth:`SkrubLearner.report`. This method is useful
to inspect the execution of a specific method (e.g. ``fit``, ``predict``) and
debug the pipeline.

.. code-block:: python

    learner = SkrubLearner(data_op)
    report = learner.report(environment={"X": X_train, "y": y_train}, mode="fit")
    print(report["report_path"])

The report will contain the same information as :meth:`.skb.full_report() <DataOp.skb.full_report>`,
but it will also include the result of the execution (e.g. the fitted learner) and
any error that might have occurred.
