⚠️ Warning ⚠️ Weak Lensing YAML files in Cobaya
================================================
The CosmoLike pipeline takes  Ω :sub:`m` and  Ω :sub:`b'` but the CAMB Boltzmann code only accepts Ω :sub:`c` h :sup:`2` and Ω :sub:`b` h :sup:`2` in Cobaya. Therefore, there are two ways of creating YAML compatible with CAMB and Cosmolike:

* CMB parameterization: (Ω :sub:`c` h :sup:`2`, Ω :sub:`b` h :sup:`2`) as primary MCMC parameters and (Ω :sub:`m`, Ω :sub:`b`) as derived quantities.

.. code-block::

 omegabh2:
     prior:
         min: 0.01
         max: 0.04
     ref:
         dist: norm
         loc: 0.022383
         scale: 0.005
     proposal: 0.005
     latex: \Omega_\mathrm{b} h^2
 omegach2:
     prior:
         min: 0.06
         max: 0.2
     ref:
         dist: norm
         loc: 0.12011
         scale: 0.03
     proposal: 0.03
     latex: \Omega_\mathrm{c} h^2
 mnu:
     value: 0.06
     latex: m_\\nu
 omegam:
     latex: \Omega_\mathrm{m}
 omegamh2:
     derived: 'lambda omegam, H0: omegam*(H0/100)**2'
     latex: \Omega_\mathrm{m} h^2
 omegab:
     derived: 'lambda omegabh2, H0: omegabh2/((H0/100)**2)'
     latex: \Omega_\mathrm{b}
 omegac:
     derived: 'lambda omegach2, H0: omegach2/((H0/100)**2)'
     latex: \Omega_\mathrm{c}

* Weak Lensing parameterization: (Ω :sub:`m`, Ω :sub:`b`) as primary MCMC parameters and (Ω :sub:`c` h :sup:`2`, Ω :sub:`b` h :sup:`2`) as derived quantities.

Adopting (Ω :sub:`m`, Ω :sub:`b`) as main MCMC parameters can create a silent bug in Cobaya. The problem occurs when the option drop: true is absent in (Ω :sub:`m`, Ω :sub:`b`) parameters, and there are no expressions that define the derived (Ω :sub:`c` h :sup:`2`, Ω :sub:`b` h :sup:`2`) quantities. The bug is silent because the MCMC runs without any warnings, but the CAMB Boltzmann code does not update the cosmological parameters at every MCMC iteration. As a result, the resulting posteriors are flawed, but they may seem reasonable to those unfamiliar with the issue. It's important to be aware of this bug to avoid any potential inaccuracies in the results.

The correct way to create YAML files with (Ω :sub:`m`, Ω :sub:`b`) as primary MCMC parameters is exemplified below

.. code-block::

    omegab:
        prior:
            min: 0.03
            max: 0.07
        ref:
            dist: norm
            loc: 0.0495
            scale: 0.004
        proposal: 0.004
        latex: \Omega_\mathrm{b}
        drop: true
    omegam:
        prior:
            min: 0.1
            max: 0.9
        ref:
            dist: norm
            loc: 0.316
            scale: 0.02
        proposal: 0.02
        latex: \Omega_\mathrm{m}
        drop: true
    mnu:
        value: 0.06
        latex: m_\\nu
    omegabh2:
        value: 'lambda omegab, H0: omegab*(H0/100)**2'
        latex: \Omega_\mathrm{b} h^2
    omegach2:
        value: 'lambda omegam, omegab, mnu, H0: (omegam-omegab)*(H0/100)**2-(mnu*(3.046/3)**0.75)/94.0708'
        latex: \Omega_\mathrm{c} h^2
    omegamh2:
        derived: 'lambda omegam, H0: omegam*(H0/100)**2'
        latex: \Omega_\mathrm{m} h^2
