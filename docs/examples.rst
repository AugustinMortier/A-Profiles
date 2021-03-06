Examples
========

.. image:: _static/images/rocket-solid.svg
   :class: awesome-svg


Some basic examples for getting you started using *aprofiles*. For more information, check out the :ref:`API`.

Data reading
------------

For reading ceilometers and lidars data, the :class:`~aprofiles.reader.ReadProfiles` class must be instantiated with the path of the NetCDF file to be read.
The :meth:`~aprofiles.reader.ReadProfiles.read()` method applied to this instance returns a :class:`~aprofiles.profiles.ProfilesData` object whose `data` attribute contains the NetCDF file content as a :class:`xarray.Dataset`.

.. literalinclude:: ../examples/01_data_reading.py
.. figure:: ../examples/images/attenuated_backscatter.png
   
      Attenuated Backscatter Profiles

Basic corrections
-----------------

Here is a non exhaustive list of basic corrections available to correct profile measurements.

Extrapolation lowest layers
^^^^^^^^^^^^^^^^^^^^^^^^^^^

It is frequent to observe negative values in the lowest layers of the profiles due to instrumental artifacts.
It is recommended to eliminate those outliers prior to detect some parameters such as the planetary boundary layer height, or before retrieving the aerosol profiles.
The :meth:`~aprofiles.profiles.ProfilesData.extrapolate_below()` method of the :class:`~aprofiles.profiles.ProfilesData` class allows to extrapolate values of the selected variable of a :class:`aprofiles.profiles.ProfilesData` object.

.. figure:: ../examples/images/lowest.png

      Negative values are observed in the lowest layers

.. literalinclude:: ../examples/02_01_extrapolation.py
.. figure:: ../examples/images/lowest_extrap.png

      Extrapolation below 150m of altitude

Gaussian Filtering
^^^^^^^^^^^^^^^^^^

The application of a gaussian filter can help increasing the SNR (that can be determined with the :meth:`~aprofiles.profiles.ProfilesData.snr()` method).

.. figure:: ../examples/images/attenuated_backscatter.png

      Original attenuated backscatter profiles

.. literalinclude:: ../examples/02_02_gaussian_filtering.py
.. figure:: ../examples/images/gaussian_filter.png

      Backscatter profiles with gaussian filter (`sigma=0.5`)

More advanced detection
-----------------------


Fog or condensation detection
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Fog or condensation prevents the laser beam to propagate into the atmosphere. It is important to detect those cases for filtering the data when trying to quantifying the aerosol content.
The default method of :meth:`~aprofiles.profiles.ProfilesData.foc()` relies on the constructor clouds detection, which provides clouds bases.

.. literalinclude:: ../examples/03_01_foc.py
.. figure:: ../examples/images/foc.png

      Fog or Condensation detection

Clouds detection
^^^^^^^^^^^^^^^^

The clouds module aims to detect clouds in each single profiles, individually. This detection relies on the analysis of the vertical gradient of the attenuated backscatter profiles.

.. literalinclude:: ../examples/03_02_clouds.py
.. figure:: ../examples/images/clouds.png

      Clouds detection

Planetary Boundary Layer tracking
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Planetary Boundary Layer module identifies the PBL heigh as the strongest vertical gradient of the attenuated backscatter profiles.

.. literalinclude:: ../examples/03_03_pbl.py
.. figure:: ../examples/images/pbl.png

      Planetary Boundary Layer height detection

Aerosol retrieval
-----------------

Extinction
^^^^^^^^^^

The aerosol extinction module retrieves extinction profiles by using a prescribed a priori (Lidar ratio, or AOD).
Both backward and forward methods have been implemented and can be used in this module.

.. literalinclude:: ../examples/04_01_extinction.py
.. figure:: ../examples/images/forward.png

      Aerosol extinction profiles retrieved with an iterative forward approach.

Concentration
^^^^^^^^^^^^^^^^^^

Aerosol mass concentration is calculated by :meth:`~aprofiles.profiles.ProfilesData.inversion`, if `mass_conc=True` (default),  for different aerosol types when calculating the extinction profiles.
Together with `extinction`, other variables are added to the instance of the :class:`~aprofiles.profiles.ProfilesData` class: `mass_concentration:[aer_type]`

.. literalinclude:: ../examples/04_02_mass_concentration.py
.. figure:: ../examples/images/mass_conc-dust.png

      Mass concentration profiles in the case of desert dust particles.