# Workflow to setup LANDSAT8 OLI data to processed with Biophysical Processor (SNAP_L8_C2_BiophysicalLandsat8Op)

A set of 4 different GPF graphs is proposed with the aim of setup Landsat-8 OLI data for the processing with Biophysical Processor.
The Biophysical Processor integrated in SNAP 8 also supports Landsat-8 OLI as input data. Such processor requires zenith and azimuth angles both for sun and view to be provided together with BOA reflectances. Recently, Landsat Collection 2 (C2) datasets have been released, providing data with improved geometric accuracy and improved radiometric calibration. Landsat C2 dataset at Level 2 (L2) contains BOA reflectances, but no angles are provided. Landsat C2 dataset at Level 1 (L1) contains TOA reflectances, together with zenith and azimuth angles both for sun and view, that were not provided in the previous Collection 1 L1 dataset.
The 'Sen2like' tool will be integrated in SNAP to harmonize Landsat-8 to Sentinel-2, in order to generate denser time series and improve Earth monitoring. Since Landsat C2 reader is not yet available in SNAP 8, and Sen2Like is not yet integrated, there is the need of a procedure to easily combine BOA reflectances with zenith and azimuth angles both for sun and view, in order to use the Biophysical Processor.
I propose here a set of GPF graphs to combine Landsat-8 BOA reflectances with zenith and azimuth angles both for sun and view in order to execute the operator 'Biophysical Processor LANDSAT8' (BiophysicalLandsat8Op).

The following graphs are proposed in the following posts:

* L8_C2_L1_L2_stacking.xml      Combines Landsat-8 OLI C2 L1 (angles) with Landsat-8 OLI C2 L2 (BOA reflectances)
* L8_C2_BiophysicalOp.xml       Combines Landsat-8 OLI C2 L1 (angles) with Landsat-8 OLI C2 L2 (BOA reflectances) and execute Biophysical Processor
* L8_iCOR_BiophysicalOp.xml     Combines Landsat-8 OLI C2 L1 (angles) with Landsat-8 OLI BOA reflectances atmospherically corrected using iCOR and execute Biophysical Processor
* L8_C2_iCOR_BiophysicalOp.xml  Perform atmospheric correction of Landsat-8 OLI C1 L1 using iCOR, combines with Landsat-8 OLI C2 L1 (angles) and execute Biophysical Processor

## L8_C2_L1_L2_stacking.xml
The GPF graph combines Landsat-8 OLI C2 L1 (angles) with Landsat-8 OLI C2 L2 (BOA reflectances). Result can later be processed with the operator 'Biophysical Processor LANDSAT8' (BiophysicalLandsat8Op).

Here an example of command line

```
gpt L8_C2_L1_L2_stacking.xml -Poutput=/data/LC08_L2SP_190031_20201214_20201219_02_T1_SR.dim -Pinput_B3=/data/LC08_L2SP_190031_20201214_20201219_02_T1/LC08_L2SP_190031_20201214_20201219_02_T1_SR_B3.TIF -Pinput_B4=/data/LC08_L2SP_190031_20201214_20201219_02_T1/LC08_L2SP_190031_20201214_20201219_02_T1_SR_B4.TIF -Pinput_B5=/data/LC08_L2SP_190031_20201214_20201219_02_T1/LC08_L2SP_190031_20201214_20201219_02_T1_SR_B5.TIF -Pinput_B6=/data/LC08_L2SP_190031_20201214_20201219_02_T1/LC08_L2SP_190031_20201214_20201219_02_T1_SR_B6.TIF -Pinput_B7=/data/LC08_L2SP_190031_20201214_20201219_02_T1/LC08_L2SP_190031_20201214_20201219_02_T1_SR_B7.TIF -Pinput_SZA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_SZA.TIF -Pinput_SAA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_SAA.TIF -Pinput_VZA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_VZA.TIF -Pinput_VAA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_VAA.TIF
```

Description of command line options:

* -Poutput Output product file name
* -Pinput_B3 Input Landsat-8 OLI C2 L2 Band 3 (B3) GeoTIFF file name
* -Pinput_B4 Input Landsat-8 OLI C2 L2 Band 4 (B4) GeoTIFF file name
* -Pinput_B5 Input Landsat-8 OLI C2 L2 Band 5 (B5) GeoTIFF file name
* -Pinput_B6 Input Landsat-8 OLI C2 L2 Band 6 (B6) GeoTIFF file name
* -Pinput_B7 Input Landsat-8 OLI C2 L2 Band 7 (B7) GeoTIFF file name
* -Pinput_SZA Input Landsat-8 OLI C2 L1 Solar Zenith Angle (SZA) GeoTIFF file name
* -Pinput_SAA Input Landsat-8 OLI C2 L1 Solar Azimuth Angle (SAA) GeoTIFF file name
* -Pinput_VZA Input Landsat-8 OLI C2 L1 View Zenith Angle (VZA) GeoTIFF file name
* -Pinput_VAA Input Landsat-8 OLI C2 L1 View Azimuth Angle (VAA) GeoTIFF file name

## L8_C2_BiophysicalOp.xml
The GPF graph combines Landsat-8 OLI C2 L1 (angles) with Landsat-8 OLI C2 L2 (BOA reflectances) and execute Biophysical Processor 'Biophysical Processor LANDSAT8' (BiophysicalLandsat8Op).

Here an example of command line

```
gpt L8_C2_BiophysicalOp.xml -PcomputeLAI=true -PcomputeFapar=true -PcomputeFcover=true -Pformat=BEAM-DIMAP -Poutput=/data/LC08_L2SP_190031_20201214_20201219_02_T1_SR_Biophysical.dim -Pinput_B3=/data/LC08_L2SP_190031_20201214_20201219_02_T1/LC08_L2SP_190031_20201214_20201219_02_T1_SR_B3.TIF -Pinput_B4=/data/LC08_L2SP_190031_20201214_20201219_02_T1/LC08_L2SP_190031_20201214_20201219_02_T1_SR_B4.TIF -Pinput_B5=/data/LC08_L2SP_190031_20201214_20201219_02_T1/LC08_L2SP_190031_20201214_20201219_02_T1_SR_B5.TIF -Pinput_B6=/data/LC08_L2SP_190031_20201214_20201219_02_T1/LC08_L2SP_190031_20201214_20201219_02_T1_SR_B6.TIF -Pinput_B7=/data/LC08_L2SP_190031_20201214_20201219_02_T1/LC08_L2SP_190031_20201214_20201219_02_T1_SR_B7.TIF -Pinput_SZA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_SZA.TIF -Pinput_SAA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_SAA.TIF -Pinput_VZA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_VZA.TIF -Pinput_VAA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_VAA.TIF
```

Description of command line options:

* -PcomputeLAI Boolean. Compute LAI (Leaf Area Index)
* -PcomputeFapar Boolean. Compute FAPAR (Fraction of Absorbed Photosynthetically Active Radiation)
* -PcomputeFcover Boolean. Compute FVC (Fraction of Vegetation Cover)
* -Pformat Output product format (i.e. 'BEAM-DIMAP')
* -Poutput Output product file name
* -Pinput_B3 Input Landsat-8 OLI C2 L2 Band 3 (B3) GeoTIFF file name
* -Pinput_B4 Input Landsat-8 OLI C2 L2 Band 4 (B4) GeoTIFF file name
* -Pinput_B5 Input Landsat-8 OLI C2 L2 Band 5 (B5) GeoTIFF file name
* -Pinput_B6 Input Landsat-8 OLI C2 L2 Band 6 (B6) GeoTIFF file name
* -Pinput_B7 Input Landsat-8 OLI C2 L2 Band 7 (B7) GeoTIFF file name
* -Pinput_SZA Input Landsat-8 OLI C2 L1 Solar Zenith Angle (SZA) GeoTIFF file name
* -Pinput_SAA Input Landsat-8 OLI C2 L1 Solar Azimuth Angle (SAA) GeoTIFF file name
* -Pinput_VZA Input Landsat-8 OLI C2 L1 View Zenith Angle (VZA) GeoTIFF file name
* -Pinput_VAA Input Landsat-8 OLI C2 L1 View Azimuth Angle (VAA) GeoTIFF file name

## L8_iCOR_BiophysicalOp.xml
The GPF graph combines Landsat-8 OLI C2 L1 (angles) with Landsat-8 OLI BOA reflectances atmospherically corrected using iCOR and execute Biophysical Processor'Biophysical Processor LANDSAT8' (BiophysicalLandsat8Op).
'iCOR' should be installed in the system and imported in SNAP 8 as a module. Landsat-8 OLI C1 L1 is currently supported as iCOR input dataset.

Here an example of command line

```
gpt L8_iCOR_BiophysicalOp.xml -PcomputeLAI=true -PcomputeFapar=true -PcomputeFcover=true -Pformat=BEAM-DIMAP -Poutput=/data/LC08_L1TP_191031_20210106_20210106_01_RT_Biophysical.dim -Pinput_iCOR=/data/LC08_L1TP_190031_20201214_20201219_01_T1_iCOR.tif -Pinput_SZA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_SZA.TIF -Pinput_SAA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_SAA.TIF -Pinput_VZA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_VZA.TIF -Pinput_VAA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_VAA.TIF
```

Description of command line options:

* -PcomputeLAI Boolean. Compute LAI (Leaf Area Index)
* -PcomputeFapar Boolean. Compute FAPAR (Fraction of Absorbed Photosynthetically Active Radiation)
* -PcomputeFcover Boolean. Compute FVC (Fraction of Vegetation Cover)
* -Pformat Output product format (i.e. 'BEAM-DIMAP')
* -Poutput Output product file name
* -Pinput_iCOR Input Landsat-8 OLI BOA Reflectances dataset atmosferically corrected using iCOR
* -Pinput_SZA Input Landsat-8 OLI C2 L1 Solar Zenith Angle (SZA) GeoTIFF file name
* -Pinput_SAA Input Landsat-8 OLI C2 L1 Solar Azimuth Angle (SAA) GeoTIFF file name
* -Pinput_VZA Input Landsat-8 OLI C2 L1 View Zenith Angle (VZA) GeoTIFF file name
* -Pinput_VAA Input Landsat-8 OLI C2 L1 View Azimuth Angle (VAA) GeoTIFF file name

## L8_C1_L1_iCOR_BiophysicalOp.xml
The GPF graph perform atmospheric correction of Landsat-8 OLI C1 L1 using iCOR, combines with Landsat-8 OLI C2 L1 (angles) and execute Biophysical Processor Biophysical Processor'Biophysical Processor LANDSAT8' (BiophysicalLandsat8Op).
'iCOR' should be installed in the system and imported in SNAP 8 as a module. Landsat-8 OLI C1 L1 is currently supported as iCOR input dataset.

Here an example of command line

```
gpt L8_C1_L1_iCOR_BiophysicalOp.xml -PcomputeLAI=true -PcomputeFapar=true -PcomputeFcover=true -Poutput_iCOR=/data/LC08_L1TP_191031_20210106_20210106_01_RT_iCOR.tif -Pformat=BEAM-DIMAP -Poutput=/data/LC08_L1TP_191031_20210106_20210106_01_RT_iCOR_Biophysical.dim -Pinput=/data/LC08_L1TP_190031_20201214_20201219_01_T1/LC08_L1TP_190031_20201214_20201219_01_T1_MTL.txt -Pinput_SZA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_SZA.TIF -Pinput_SAA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_SAA.TIF -Pinput_VZA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_VZA.TIF -Pinput_VAA=/data/LC08_L1TP_190031_20201214_20201219_02_T1/LC08_L1TP_190031_20201214_20201219_02_T1_VAA.TIF
```

Description of command line options:

* -PcomputeLAI Boolean. Compute LAI (Leaf Area Index)
* -PcomputeFapar Boolean. Compute FAPAR (Fraction of Absorbed Photosynthetically Active Radiation)
* -PcomputeFcover Boolean. Compute FVC (Fraction of Vegetation Cover)
* -Poutput GeoTIFF file name for iCOR output product
* -Pformat Output product format (i.e. 'BEAM-DIMAP')
* -Poutput Output product file name
* -Pinput Input Landsat-8 OLI C1 L1 dataset ('MTL.txt' file)
* -Pinput_SZA Input Landsat-8 OLI C2 L1 Solar Zenith Angle (SZA) GeoTIFF file name
* -Pinput_SAA Input Landsat-8 OLI C2 L1 Solar Azimuth Angle (SAA) GeoTIFF file name
* -Pinput_VZA Input Landsat-8 OLI C2 L1 View Zenith Angle (VZA) GeoTIFF file name
* -Pinput_VAA Input Landsat-8 OLI C2 L1 View Azimuth Angle (VAA) GeoTIFF file name
