*********
Specutils
*********

Specutils is an `Astropy`_ affiliated package with the goal of providing a
shared set of Python representations of astronomical spectra and basic tools to
operate on these spectra.  The effort is also meant to be a "hub", helping to
unite the Python astronomical spectroscopy community around shared effort, much
as `Astropy`_ is meant to for the wider astronomy Python ecosystem.


First Steps
==============
The `Spectrum1D`_ class is one of the core classes of the specutils package.
You can import it like this:

.. code-block:: python

  >>> from specutils import Spectrum1D

To instantiate it you can define a wave and a flux:

.. code-block:: python

  >>> wave = np.arange(6000, 9000) * u.Angstrom
  >>> flux = np.random.random(3000) * u.Unit('W m-2 angstrom-1 sr-1')

and then call the **from_array** method:

.. code-block:: python

  >>> spec1d = Spectrum1D.from_array(wave, flux)
  >>> spec1d.wavelength
  <Quantity [ 6000., 6001., 6002.,...,  8997., 8998., 8999.] Angstrom>
  >>> spec1d.flux
  <Quantity [ 0.75639906, 0.23677036, 0.08408417,...,  0.82740303, 0.38345114,
              0.77815595] W / (Angstrom m2 sr)>

Or you can read a Spectrum from a .fits file with the **read_fits** method:

.. code-block:: python

  >>> from specutils.io import read_fits
  >>> myspec = read_fits.read_fits_spectrum1d('myfile.fits')

It supports the types of FITS formats listed in `this page`_.

**Note:** A list of spectra is returned whenever the input file is a multispec file.

Writing spectra to .fits files works in the same way with the **write_fits** method:

.. code-block:: python

  >>> from specutils.io import write_fits
  >>> write_fits.write(myspec, 'mynewfile.fits')

**Note:** write_fits.write deciphers the type of object passed and writes spectra to the given file in FITS format.

Reading a Spectrum from a FITS file with no specified units in the header will give the following warning:

.. code-block:: python

  >>> myspec = read_fits.read_fits_spectrum1d('specutils/io/tests/files/UVES.fits')
  UserWarning: Initializing a Spectrum1D WCS with units set to `None` is not recommended

the Spectrum1D.dispersion will be an array:

.. code-block:: python

  >>> myspec.dispersion
  array([ 3732.05623192,  3732.0858853 ,  3732.11553869, ...,  4999.67906915,
          4999.70872253,  4999.73837591])

and thus the Spectrum1D's wavelength, energy and frequency will not be available.
In order to be convertible, the dispersion must be an astropy Quantity, which will happen if the FITS header has specified the units or if you specify them manually like this:

.. code-block:: python

  >>> myspec = read_fits.read_fits_spectrum1d('specutils/io/tests/files/UVES.fits', dispersion_unit='angstrom')
  >>> myspec.dispersion
  <Quantity [ 3732.05623192, 3732.0858853 , 3732.11553869,...,
              4999.67906915, 4999.70872253, 4999.73837591] Angstrom>
  >>> myspec.wavelength
  <Quantity [ 3732.05623192, 3732.0858853 , 3732.11553869,...,
              4999.67906915, 4999.70872253, 4999.73837591] Angstrom>
  >>> myspec.energy
  <Quantity [ 5.32265743e-19,  5.32261514e-19,  5.32257285e-19,...,
              3.97314639e-19,  3.97312282e-19,  3.97309926e-19] J>
  >>> myspec.frequency
  <Quantity [ 8.03290303e+14,  8.03283920e+14,  8.03277538e+14,...,
              5.99623404e+14,  5.99619847e+14,  5.99616291e+14] Hz>

You can easily make a plot of the Spectrum using matplotlib in ipython with the --pylab flag and calling:

.. code-block:: python

  >>> import pylab as pl
  >>> pl.plot(myspec.wavelength, myspec.flux)

.. plot:: pyplots/plotting_example.py

`Full Documentation`_

.. image:: https://travis-ci.org/astropy/specutils.png?branch=master
  :target: https://travis-ci.org/astropy/specutils

.. image:: https://coveralls.io/repos/astropy/specutils/badge.png
  :target: https://coveralls.io/r/astropy/specutils

.. _Full Documentation: http://specutils.readthedocs.org/en/latest/specutils/index.html
.. _this page: http://iraf.net/irafdocs/specwcs.php
.. _Spectrum1D: http://specutils.readthedocs.org/en/latest/specutils/spectrum1d.html
.. _Spectrum1DPolynomialWCS: http://specutils.readthedocs.org/en/latest/api/specutils.wcs.specwcs.Spectrum1DPolynomialWCS.html#specutils.wcs.specwcs.Spectrum1DPolynomialWCS
.. _CompositeWCS: http://specutils.readthedocs.org/en/latest/api/specutils.wcs.specwcs.CompositeWCS.html#specutils.wcs.specwcs.CompositeWCS
.. _WeightedCombinationWCS: http://specutils.readthedocs.org/en/latest/api/specutils.wcs.specwcs.WeightedCombinationWCS.html#specutils.wcs.specwcs.WeightedCombinationWCS
.. _DopplerShift: http://specutils.readthedocs.org/en/latest/api/specutils.wcs.specwcs.DopplerShift.html#specutils.wcs.specwcs.DopplerShift
.. _Spectrum1DIRAFLegendreWCS: http://specutils.readthedocs.org/en/latest/api/specutils.wcs.specwcs.Spectrum1DIRAFLegendreWCS.html#specutils.wcs.specwcs.Spectrum1DIRAFLegendreWCS
.. _Spectrum1DIRAFChebyshevWCS: http://specutils.readthedocs.org/en/latest/api/specutils.wcs.specwcs.Spectrum1DIRAFChebyshevWCS.html#specutils.wcs.specwcs.Spectrum1DIRAFChebyshevWCS
.. _Spectrum1DIRAFBSplineWCS: http://specutils.readthedocs.org/en/latest/api/specutils.wcs.specwcs.Spectrum1DIRAFBSplineWCS.html#specutils.wcs.specwcs.Spectrum1DIRAFBSplineWCS
.. _MultispecIRAFCompositeWCS: http://specutils.readthedocs.org/en/latest/api/specutils.wcs.specwcs.MultispecIRAFCompositeWCS.html#specutils.wcs.specwcs.MultispecIRAFCompositeWCS -->

.. _Astropy: http://www.astropy.org/
