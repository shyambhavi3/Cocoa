Manual Blocking of Cosmolike Parameters
=======================================
  
Cosmolike Weak Lensing pipeline contains parameters with different speed hierarchies. For example, Cosmolike execution time is reduced by approximately 50% when fixing the cosmological parameters. When varying only multiplicative shear calibration, Cosmolike execution time is reduced by two orders of magnitude.

Cobaya can't automatically handle parameters associated with the same likelihood that have different speed hierarchies. Luckily, we can manually impose the speed hierarchy in Cobaya using the ``blocking:`` option. The only drawback of this method is that parameters of all adopted likelihoods need to be manually specified, not only the ones required by Cosmolike.

In addition to that, Cosmolike can't cache the intermediate products of the last two evaluations, which is necessary to exploit optimizations associated with dragging (``drag: True``). However, Cosmolike caches the intermediate products of the previous evaluation, thereby enabling the user to take advantage of the slow/fast decomposition of parameters in Cobaya's main MCMC sampler.

Below we provide an example YAML configuration for an MCMC chain that with DES 3x2pt likelihood.

.. code-block:: bash

      likelihood: 
        des_y3.des_3x2pt:
        path: ./external_modules/data/des_y3
        data_file: DES_Y1.dataset
     
     (...)
     
    sampler:
        mcmc:
            covmat: "./projects/des_y3/EXAMPLE_MCMC22.covmat"
            covmat_params:
            # ---------------------------------------------------------------------
            # Proposal covariance matrix learning
            # ---------------------------------------------------------------------
            learn_proposal: True
            learn_proposal_Rminus1_min: 0.03
            learn_proposal_Rminus1_max: 100.
            learn_proposal_Rminus1_max_early: 200.
            # ---------------------------------------------------------------------
            # Convergence criteria
            # ---------------------------------------------------------------------
            # Maximum number of posterior evaluations
            max_samples: .inf
            # Gelman-Rubin R-1 on means
            Rminus1_stop: 0.02
            # Gelman-Rubin R-1 on std deviations
            Rminus1_cl_stop: 0.2
            Rminus1_cl_level: 0.95
            # ---------------------------------------------------------------------
            # Exploiting Cosmolike speed hierarchy
            # ---------------------------------------------------------------------
            measure_speeds: False
            drag: False
            oversample_power: 0.2
            oversample_thin: True
            blocking:
            - [1,
                [
                    As_1e9, H0, omegab, omegam, ns
                ]
              ]
            - [4,
                [
                    DES_DZ_S1, DES_DZ_S2, DES_DZ_S3, DES_DZ_S4, DES_A1_1, DES_A1_2,
                    DES_B1_1, DES_B1_2, DES_B1_3, DES_B1_4, DES_B1_5,
                    DES_DZ_L1, DES_DZ_L2, DES_DZ_L3, DES_DZ_L4, DES_DZ_L5
                ]
              ]
            - [25,
                [
                    DES_M1, DES_M2, DES_M3, DES_M4, DES_PM1, DES_PM2, DES_PM3, DES_PM4, DES_PM5
                ]
              ]
            # ---------------------------------------------------------------------
            max_tries: 10000
            burn_in: 0
            Rminus1_single_split: 4
