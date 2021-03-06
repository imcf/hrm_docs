.. include:: global_directives.inc

.. _stop_criteria:

Stopping criteria
=================

The restoration algorithms offered in HRM, **CMLE** and **QMLE**, are iterative methods. This means that the algorithm computes sequential solutions which converge to a stable deconvolution result. 

Deconvolution will stop when either of the following two conditions is met.

|StopCriteriaScreenshot|

* **Number of iterations**: sets the maximum number of iterations that Huygens will compute.

* **Quality change**: how much the results of two consecutive iterations differ. If two subsequent results differ less than this factor then convergence has been reached.
