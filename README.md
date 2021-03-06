# GIFAnalysis
GIF++ Analysis Code

__1. Make MAP file - MAP*__ 

  DetectorName'\t'     StripPerPartition'\t'   PartitionCut'\n'  
  TDC_start'\t'        TDC_end'\t'     Strip_start'\t' Strip_end'\n'  
  ...  
  TDC_start'\t'        TDC_end'\t'     Strip_start'\t' Strip_end'\n'  
  '\n' (when detector setting is end)  
  
  ex)  
  T3S5 (KODEL_DG12)       32      AB  
  2000    2015    1       16  
  2016    2031    17      32  
  2032    2047    64      49  
  2048    2063    48      33  
  
  You can find TDC channel & strip number for each test beam  
  on https://drive.google.com/open?id=1LyIAcLzgN0jdvHnPx1cIScB__B4uFL9U5UqvBO9ENrg  
  
__2. Make Calibration file - calibrate.C -> CAL*__
  
  InputRootFile : DAQ.root file - NoSource(or at least ABS220) with Beam   
  MAPConfigFile : Map file(#1)  
  Cut_PercentageAfterCut : Check entries after cut  
  Cut_TimeWindow : Check difference between peek after cut and chamber mean  
  Cut_RMSAfterCut : Check broad peak  
  (GIF++ do not use calibration time)  
  
__3. Clustering - KODEL.C -> DAQ-KODEL.root__ 
  
  InputRootFile : DAQ.root file  
  MAPConfigFile : Map file(#1)  
  Delta_Strip : Definition of adjacent strip for clustering (GIF++ clustering : 1)  
  Delta_Partition : Definition of adjacent partition for clustering (GIF++ clustering : 0 - do not use)  
  Delta_Time : Definition of adjacent time for clustering (GIF++ clustering : do not use adjacent time / using beam timing cut)  
  
__4. Make trolly SET file - SET*__
  
  Trolly & Beam SETTING config file  
  
  BEAM'\t' X_mean'\t' X_error'\t' Y_mean'\t' Y_error'\t' Time_mean'\t' Time_error'\n'
  
  ex)  
  BEAM    1.5       0.5       27.5      5       0       30  
  
  DetectorName'\t' Order'\t' DetectorWidth(x)'\t' DetectorHeight(y)'\t' DetectorPosition(x)'\t' DetectorPosition(y)'\t' RotationAngle'\n'  
  
  ex)  
  T3S5 (KODEL_DG12)       4       2       32      1       7      0  
  T3S1 (GT_16)    3       2       48      0       0       180  
  
  DetectorWidth, Height, PositionX, PositionY on example file is not real value. (just for tracking test)  
  You have to update based on  
  https://drive.google.com/drive/folders/0BwXFTEDFLcaVelc2SEV2UmRfa1U  
  (Source base coordinate : https://drive.google.com/open?id=0B3Uwh2qCP96vM2s2OFhoVkFmSDg)  
  
__5. Tracking - Tracking.C -> DAQ-Trk.root__
  
  InputRootFile : DAQ-KODEL.root  
  MAPConfigFile : MAP file(#1)  
  SETConfigFile : SET file(#4)  
  Cut_X : Distance in X-axis  
  Cut_Y : Distance in Y-axis  
  Cut_Time : Difference in Time  
  
