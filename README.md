HAWKIRED Pipeline
=================

HAWKIRED is a data processing pipeline written in python for the [ESO HAWK-I](http://www.eso.org/sci/facilities/paranal/instruments/hawki/) (High Acuity Wide field K-band Imager) instrument.

Dependencies
------------

You'll need to install or have installed:

1. The [HAWKI pipeline distribution](http://www.eso.org/sci/software/pipelines/hawki/hawki-pipe-recipes.html) kit.
2. The Python bindings for CPL [python-cpl](https://github.com/olebole/python-cpl)
3. [Pyfits](http://www.stsci.edu/institute/software_hardware/pyfits)

Note: For MacOSX I had to remove the "mcheck" from the line `libraries = [ 'cplcore', 'cpldfs', 'cplui', 'cpldrs', 'gomp', 'mcheck' ]`, in the setup.py file to get PyCPL to compile.

Downloading Data
----------------

Download your data from the [ESO Archive Data Portal](http://archive.eso.org/eso/eso_archive_main.html) using the "CalSelector" option. Using this option gives you the all the calibration data for each OB (Observation Block) and and XML file which is used by HAWKIRED as input.

Running HAWKIRED
----------------

1. Copy the contents of the HAWKIRED directory to the directory containing the HAWKI data you want to reduce (`cp /path/to/HAWKIRED/* /path/to/datadir`)
2. Change to your data directory (`cd /path/to/datadir`)
3. Run `prep` (you may have to edit if you get a `Argument list too long` shell error)
4. Edit and run `mkrun`
5. Run `do_all`
6. Run `mkzpt` (e.g `mkzpt uds_ob*_hawki_y`)

Optional

7. Edit and run mkfp (makes footprints of stitched OBs)
8. Edit an run mkdist

Notes
-----

- You may want to comment out all but the first OB in do_all to do a test run.
- *combine.fits are the all images combined from an OB. The files have 4 extensions - one for each HAWK-I chip.
- Stitched files are the mosaics stitched together from the 4 chips. These are for just reference only. DON'T USE THEM FOR SCIENCE.
- The `runname_hawki_filter_zpt.json` files have the computed zeropoints from standard star observations for each OB

You can extract the zeropoint for each OB/chip using the Python json module:

    import json
    with open('uds_hawki_y_zpt.json') as f:
        zpts = json.load(f)
    for ob, chips in zpts.iteritems():
        for chip in chips:
            print '%s %s %s' % (ob, chip['CHIP'], chip['ZPOINT'])

