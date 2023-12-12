Fine-tunning CAMB Accuracy
==========================

The accurate computation of many CMB and large-scale-structure data vectors requires high ``AccuracyBoost`` values in CAMB. However, this parameter is particularly inefficient, causing an exponential increase in CAMB's runtime. This issue has been frequent enough that we provide below a simple but partial remedy.

The underlying reason for ``AccuracyBoost`` inefficiency is that this flag raises the required accuracy of multiple modules in CAMB. The appropriate boost must be fine-tuned until the x :sup:`2` of the adopted experiments remain stable. However, we do not need to raise the boost factor in all CAMB modules by the same amount to achieve such stability.

The Python function ``set_accuracy``, located in the file ``$ROOTDIR/external_modules/code/CAMB/camb``, can be modified for a more fine-tuned change to CAMB accuracy. Below is an example of possible modifications:

.. code-block:: bash

  def set_accuracy(self, AccuracyBoost=1., lSampleBoost=1., lAccuracyBoost=1., DoLateRadTruncation=True): 
      #COCOA: BEGINS
      self.Accuracy.AccuracyBoost = AccuracyBoost       
      
      #COCOA: below, actual boost = 1.0 + 2*(AccuracyBoost-1.0)
      self.Accuracy.BessIntBoost = (1.0 + 2*(AccuracyBoost-1.0))/AccuracyBoost
      self.Accuracy.IntkAccuracyBoost = (1.0 + 2*(AccuracyBoost-1.0))/AccuracyBoost
      self.Accuracy.TransferkBoost = (1.0 + 2*(AccuracyBoost-1.0))/AccuracyBoost
      self.Accuracy.KmaxBoost = (1.0 + 2*(AccuracyBoost-1.0))/AccuracyBoost
      self.Accuracy.TimeStepBoost = (1.0 + 2*(AccuracyBoost-1.0))/AccuracyBoost
      
      #COCOA: below, actual boost = 1.0 + 5*(AccuracyBoost-1.0)
      self.Accuracy.SourcekAccuracyBoost = (1.0 + 5*(AccuracyBoost-1.0))/AccuracyBoost
      self.Accuracy.BesselBoost = (1.0 + 5*(AccuracyBoost-1.0))/AccuracyBoost
      #COCOA: ENDS
      
      self.Accuracy.lSampleBoost = lSampleBoost
      self.Accuracy.lAccuracyBoost = lAccuracyBoost
      self.DoLateRadTruncation = DoLateRadTruncation
      return self

With the code above, the theoretical error in Simons Observatory x :sup:`2` seems to be under control (i.e., Δx :sup:`2` = O(few)) with ``AccuracyBoost: 1.06`` and ``lens_potential_accuracy: 4`` even a bit away from the best-fit model so that chains can be later corrected via Importance Sampling. As a reminder, corrections based on Importance Sampling are much faster when compared to running MCMC chains with insane accuracy because they can be computed on thinned versions of converged chains and are trivially parallelizable.

Out of caution, we have not implemented these changes in ``$ROOTDIR/external_modules/code/CAMB/``.
