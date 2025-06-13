# Rapid ASKAP Continuum Survey (RACS)
A database of information on each RACS observation and the resulting data products.
This consists of a set of plain text files formatted as "comma-separated-values" (csv). The primary file is `field_data.csv`.
* Each row (line) of the primary file corresponds to an observation; multiple observations of a single field each have their own row.
* To each row there correspond several auxiliary files:
 	* beam information files: beam_inf_[SB]-[fld].csv
 	* sources matched to catalogue [cat] cat_match_[cat]_[SB]_[fld].csv
 	* PSF variation over field as images --- psf.bma.SB[SB].[fld].fits, psf.bmi.SB[SB].[fld].fits, psf.bpa.SB[SB].[fld].fits

## Contents
* [The database](#database)
* [Data columns in field_data.csv](#field_data)
* [Auxiliary files](#axiliary)
	* [Astrometry beam-to-beam `astrom_bb_<sb>-<fld>.csv`](#astro-bb)
	* [Beam information `beam_inf_sb-fld.csv`](#beam)
	* [Source matching `cat_match_cat_sb-fld.csv`](#cat)

## <a name="database"></a>1.0 The database
The elements of the database are evident on this repository page. To use the database take the following steps:
1. If you have not used git, configure it with
```
$ git config --global user.name "Your Name"
$ git config --global user.email "your_name@some_email"

```
2. Clone the database on your computer
```
$ cd your-directory
$ git clone https://bitbucket.csiro.au/scm/askap_surveys/racs.git
```
Once finished you will find 
```
.
+-- your-directory/
|  +-- racs/
|  |  +-- db/
|  |  +-- db-inputs/
|
```

3. To be consistent with usage in this document which uses `$SURVEY` to refer to the database: 
```
$ export SURVEY=your-directory/racs
```



## <a name="field_data"></a>2.0 Data columns in field_data.csv

* **INDEX** Running number from 0
* **SRC** Source number in the original observing parset 
* **FIELD_NAME** Field name in the form RACS_hhmm±ddA
* **SBID** Observing scheduling block ID
* **SCAN** Scan number in SBID for this field
* **CAL_SBID** Observing scheduling block ID for the B1934-638 observation used to calibrate this field
* **STATE** One of --NULL--, OBSERVED, CALIBRATED, IMAGED
* **RA_HMS** Right Ascension of field centre `hh mm ss.s`
* **DEC_DMS** Declination of field centre `±dd mm ss.s`
* **RA_DEG** Right Ascension of field centre in decimal degrees
* **DEC_DEG**  Declination of field centre in decimal degrees
* **GAL_LONG** Galactic longitude in decimal degrees
* **GAL_LAT** Galactic latitude in decimal degrees
* **POL_AXIS** Position of the antenna roll axis. POL_AXIS = -45 puts the cardinal axes of the footprint aligned with celestial coordinates.
* **SCAN_START** Time of scan start as Modified Julian Day (MJD) expressed in seconds.
* **SCAN_LEN** Scan duration in seconds
* **SCAN_TINT** Scan integration time. This can differ from SCAN_LEN in the case of delay for antenna slewing or rotating.
* **NBEAMS_I** Number of stokes I beam images (normally 36)
* **MOSAIC_TIME** Time stamp of mosaic file; same units as SCAN_START
* **NPIXELS_V** Number of pixels in Stokes V image (not used)
* **PSF_MAJOR** Major axis of elliptical gaussian fitted to the PSF; given here for beam 0.
* **PSF_MINOR** Minor axis of elliptical gaussian fitted to the PSF; given here for beam 0.
* **PSF_ANGLE** Position angle of elliptical gaussian fitted to the PSF; given here for beam 0.
* **MINIMUM** Minimum value of Stokes I mosaic image (Jy/beam)
* **MAXIMUM** Minimum value of Stokes I mosaic image (Jy/beam)
* **MED_RMS_uJy** Median over the mosaic of the image rms (microJy/beam)
* **MODE_RMS_uJy** Mode over the mosaic of the image rms (microJy/beam)
* **STD_RMS_uJy** Standard deviation over the mosaic of the image rms (microJy/beam)
* **MIN_RMS_uJy** Minimum over the mosaic of the image rms (microJy/beam)
* **MAX_RMS_uJy** Maximum over the mosaic of the image rms (microJy/beam)
* **SELAVY_TIME** Time stamp of Selavy file; same units as SCAN_START
* **NUM_SELAVY** Number of components identified by Selavy over the mosaic
* **SELECT** Normally set to 1; set to 0 for additional observations of a field that have lower quality.
* **DEFECT** Normally set to 0; set to 1 for observations with a known and serious problem
* **ANOMALY** Normally set to 0; set to 1 for observations with a known butr benign problem (eg. Moon in field)
* **COMMENT** Brief explanation of either DEFECT or ANOMALY
* **MinUV** Minimum length of baselines used to form image (metres).

## <a name="axiliary"></a>3.0 Auxiliary files
Auxiliary files, both `.csv' and `'.fits', are named according to the SBID and FIELD_NAME of the observation, the combination of which is always unique. CSV files are named as `X_SBID_FIELD.csv` where _X_ denotes the variety of file. At present, possible values of _X_ are 'astrom_bb', 'beam_inf' and 'cat_match'.  FITS files are named as `X.SBID.FIELD.fits` where _X_ is one of 'psf.bma', 'psf.bmi', 'psf.bpa'.

### <a name="astro-bb"></a>3.1 Astrometry beam-to-beam `astrom_bb_sb-fld.csv`
For some observation the database includes an _astrom_bb_ file. For each pair of adjacent beams isolated point sources present in both members of the pair were identified. The _astrom_bb_ table lists the mean astrometric differences over these sources, and their standard deviations; both resolved into _x_ and _y_ (&#945; cos(&#948;), &#948;), and given in arcseconds
#### Data columns
* **BEAM1** First beam of pair
* **BEAM2** Second beam of pair
* **DX_MEAN** Mean astrometric difference in &#945; cos(&#948;)
* **DX_STD** Standard deviation astrometric difference in &#945; cos(&#948;)
* **DY_MEAN** Mean astrometric difference in &#948;
* **DY_STD** Standard deviation astrometric difference in &#948;

### <a name="beam"></a>3.2 Beam information `beam_inf_sb-fld.csv`
* **BEAM_NUM** Beam number - 0-based
* **BEAM_TIME** Time stamp of beam image
* **RA_DEG** Right Ascension of field centre in decimal degrees
* **DEC_DEG**  Declination of field centre in decimal degrees
* **GAL_LONG** Galactic longitude in decimal degrees
* **GAL_LAT** Galactic latitude in decimal degrees
* **PSF_MAJOR** Major axis of elliptical gaussian fitted to the PSF
* **PSF_MINOR** Minor axis of elliptical gaussian fitted to the PSF
* **PSF_ANGLE** Position angle of elliptical gaussian fitted to the PSF
* **VIS_TOTAL** Total number of visibilities available
* **VIS_FLAGGED** Number of visibilitiees flagged

### <a name="cat"></a>3.3 Source matching `cat_match_cat_sb-fld.csv`
For each of SUMSS, NVSS, TGSS and ICRF, sources in the RACS mosaic selavy output were matched with catalogued sources. RACS sources were drawn from the Selavy file and chosen according to the selection criteria held in the file `$SURVEY/db-inputs/match_params.txt`. Sources were matched if their positions were found within the tolerance set in match_params.txt, typically 10 arceconds. The `cat_match` csv files has a row for each matched source, with columns as listed below.

* **Component_ID** Identifier given by Selavy
* **RA** Component Right Ascension in decimal degrees
* **DEC** Component Declination in decimal degrees
* **RACS_flux** Component total flux-density (mJy)
* **RA_comp** Comparison catalogue component Right Ascension in decimal degrees
* **DEC_comp** Comparison catalogue component Declination in decimal degrees
* **Comp_flux** Comparison catalogue component total flux-density (mJy)
* **Flux_Ratio_AlphaCorr** Flux density ratio corrected for an assumed spectral index given as `alpha` in `match_params.txt`
* **Flux_Ratio_NOAlphaCorr** Flux density ratio, unchanged.


