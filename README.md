# andForensics
## *Introduction*
andForensics is a open-source **Mobile forensic tool for Android device**, written python 3.7.4, runs in Windows environment. This tool used several open-source projects, [TSK](https://www.sleuthkit.org/sleuthkit/download.php) (The Sleuth Kit) bianries for file system analysis, and [JADX](https://github.com/skylot/jadx) (Dex to Java decompiler) for APK decompilation.

## *Features*
andForensics is composed 3 phases.
### 1. Scanning
 - File system analyzing with `tsk_loaddb.exe` binary of TSK (The Sleuth Kit).
 - Extracting the file system status.
 - Extracting Android system log information.
### 2. Pre-processing
 - Grouping all files in a file system by application (system, third party).
 - Classifying all files in the filesystem by signature, file name, and extension.
 - Decompiling APK files using `JADX`. (optional)
 - Extracting user information (timestamp, ID, geodata, etc.) by analyzing all app logs in SQLite database format.
### 3. Analyzing
 - Analyzing system logs and file system information to extract all app information installed on userdata image file of Android device(including deleted apps).
 - Extracting information about the original files in the compressed file. (For now, only analyzes the APK file format)
 - Analyzing all app logs existing in userdata image file and extracting contents including user information as below.
   - Extracting contents containing the user account. (ID, PW, etc)
   - Extracting contents containing the call history.
   - Extracting contents containing the geodata(latitude, longitude).
   - Extracting contents containing the web browser history.
   - Extracting contents containing the file history.
   - *Items will continue to be added...*
 
## *Dependencies*
- `Python3.x` for overall procedure.
- `JAVA` for APK decompiling with [JADX](https://github.com/skylot/jadx).

## *Usage*
```
usage: andForensics.py [-h] [-i INPUT_DIR] [-o OUTPUT_DIR] [-p PHASE]
                       [-d DECOMPILE_APK] [-proc NUMBER_PROCESS] [-v]

optional arguments:
  -h, --help            show this help message and exit
  -i INPUT_DIR          Input directory containing the Android userdata image file with EXT4 file system.
  -o OUTPUT_DIR         Output directory to store analysis result files.
  -p PHASE              Select a single investigation phase. (default: all phases)
                         - [1/3] Scanning: "scan"
                         - [2/3] Pre-processing: "preproc" (only after "Scanning" phase)
                         - [3/3] Analyzing: "analysis" (only after "Pre-processing" phase)
                         e.g., andForensics.py -i <INPUT_DIR> -o <OUTPUT_DIR> -p preproc
  -d DECOMPILE_APK      Select whether to decompile the APK file. This operation is time-consuming and 
                        requires a large capacity. (1:enable, 0:disable, default:disable)
  -proc NUMBER_PROCESS  Input the number of processes for multiprocessing. (default: single processing)
  -v                    Show verbose (debug) output.
```

> *A simple example is as follows:*
```
python3 andForensics.py -i c:\case_image_dir\ -o c:\output_dir -proc 8
```
The above command means:
- The **<c:\case_image_dir>** directory contains the image file of the userdata partition on the Android device with the Ext4 filesystem.
- andForensics will save the output files to the **<c:\output_dir>** directory.
- andForensics will multiprocess using **eight processors**.
