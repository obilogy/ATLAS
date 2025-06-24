## Automated Tracking of Learned Actin Structures (ATLAS)
ATLAS is a machine learning-enhanced routine designed to detect, segment and track actin filaments in the context of the *in vitro* motility assay.
Link to paper: https://doi.org/10.1016/j.bpr.2025.100221
![ATLAS_github](https://github.com/user-attachments/assets/fad47fc2-18a8-4e4b-831f-72bbbbb876f0)

**Important Notes:** 

#1 This routine is built using Python and R. Please make sure your computer meets the requirements to run and install both.

#2 ATLAS is designed to work on any operating system, using either CPU/GPU. To unlock ATLAS's fastest performance :rocket:, the use of an NVIDIA CUDA GPU is recommended. (NVIDIA CUDA driver 12.X or higher).




## ATLAS File Diagram:
```
ATLAS
├── LICENSE.txt
├── README.md
├── requirements.txt
├── run_atlas.bat
├── run_atlas.sh
├── scripts
│   └── R_msd_analysis.R
├── setup.py
├── src
│   ├── __init__.py
│   ├── components
│   │   ├── ATLAS.py
│   │   ├── CustomDataset.py
│   │   ├── DoubleConv.py
│   │   ├── MergeDetector.py
│   │   ├── UNet.py
│   │   └── __init__.py
│   ├── gui.py
│   ├── main.py
│   └── processor.py
├── weights
│   ├── UNET.pth
│   ├── YOLO_II_1.pt
│   ├── YOLO_II_2.pt
│   ├── YOLO_II_3.pt
│   ├── YOLO_II_4.pt
│   └── YOLO_II_5.pt
└── models
    └── ultralytics_yolov5_master (from https://github.com/ultralytics/ultralytics)
```
## Requirements (If you encounter any issues please see FAQ section below):

- RAR File Extractor, such as WinRAR (https://www.win-rar.com/download.html)
- Python version: Python 3.11 | hint: for Windows, it might be easier to install from the Microsoft Store or here: https://www.python.org/downloads/release/python-3110/
- R version: 4.1 or higher (required packages: ggplot2, gridExtra, zoo, stringr, dplyr, readr) | hint: easier to install through R studio: https://posit.co/downloads/
- In the R script (scripts/R_msd_analysis.R) change the following line to YOUR own library path: .libPaths("C:/Users/user/Documents/R/win-library/4.1") | hint #1: This is the path to the package library of the R version used by Rstudio! | hint #2: If you have multiple R versions, choose the one used by Rstudio!
- In the ATLAS folder go to src/components/, open ATLAS.py in notepad/mousepad, then search for "rscript_exe" using Ctrl+F, or scroll all the way down. 
You will see something like this: "C:\\Program Files\\R\\R-4.1.2\\bin\\Rscript.exe", then change the path to YOUR working Rscript executable file. | hint: in Windows, you might only need to change the version number. 
  
## Data Preparation and Output Nomenclature:

- ATLAS is designed to analyze all TIFF image stacks and .avi files within the selected folder (strictly one level down).
- The ATLAS output is saved into the "output" subfolder, according to the image stack and .avi filenames.
- The "output" folder contains:
	- CSV files with the raw trajectory data (*_data.csv), analyzed data with IDs, velocities and lengths (*_msd_analysis.csv), analyzed data with IDs and lengths (*_all_filament_lengths.csv), and average processing time per frame (*_pt.csv).
	- A PDF graphical summary file per movie, showing velocity and length histograms and calculated statistics. 

## Installation (If you encounter any issues please see FAQ section below): 

- Download the ATLAS compressed file (.rar) from Zenodo: https://zenodo.org/records/15320358

- Extract the ATLAS.rar file. Now you should have a folder named "ATLAS" that contains all necessary files.
  Important Note: **(must be in the same hard drive as the files to be analyzed)**.

- **Windows with CUDA GPU:** 
- Open the Command Prompt, navigate to the ATLAS folder, then run the following lines of code: 

     ```bash
     pip install -r requirements.txt
     ```
- Run the following line to install the following GPU-optimized packages:

     ```bash
     pip install torch==2.0.0+cu118 torchvision==0.15.1+cu118 --extra-index-url https://download.pytorch.org/whl/cu118
     ```

- **Linux with CUDA GPU:** 
- Open the Terminal, navigate to the ATLAS folder, then run the following lines of code: 

     ```bash
     pip install -r requirements.txt
     ```
- Run the following line to install the following GPU-optimized packages:

     ```bash
     pip install torch==2.0.0+cu118 torchvision==0.15.1+cu118 --extra-index-url https://download.pytorch.org/whl/cu118
     ```

- **Windows/Linux/Mac with CPU only or Mac with Metal GPU:**
-  Open the Terminal, navigate to the ATLAS folder and run:

     ```bash
     pip install -r requirements.txt
     ```
- Run the following line to install the following packages:

     ```bash
     pip install torch==2.0.0 torchvision==0.15.1
     ```

## Analyzing your first movie with ATLAS:

- **Windows:** 
- Double-click on `run_atlas.bat` and follow instructions.
  Important Note: it might take a minute to initialize the first time.

- **Linux/Mac:**
- Make `run_atlas.sh` executable by running the following line of code in the terminal:

     ```bash
     chmod +x run_atlas.sh	
     ```
     Important Note: it might take a minute to initialize the first time.
  
  Then you can run it with:

     ```bash
     ./run_atlas.sh
     ```
- Important Note for Linux/Mac users: if the above method does not work, you can run the ATLAS graphical interface directly using the following line of code:

     ```bash
     python src/gui.py		
     ```
     Important Note: it might take a minute to initialize the first time.


## Additional Usage Notes:

- #1: Everytime ATLAS analyzes a movie, it generates a log file, both in the ATLAS folder and in the folder containing the movies to be analyzed. This feature will prevent any wasteful re-analysis in
case that the process gets interrupted. For example, say you have 1000 movies in a folder, then the computer loses power when analyzing the movie 430. If you enter "resume", it will
start on the movie 430 instead of movie 1. Whenever there is a log file, ATLAS will show the following:

     ```bash
     Enter 'fresh' for a fresh start, or 'resume' to continue from last checkpoint:
     ```
Enter "fresh" if you want to run the analysis on all present movies, otherwise enter "resume".

- #2: ATLAS was designed to analyze one folder (which can contain as many .avi movies or tiff stack subfolders as you wish) per running cycle or each time you double click the run_atlas file.
Thus, if you have analyzed a folder with ATLAS and you would like to analyze a different folder or re-analyze the same folder for whatever reason, please close the ATLAS graphical interface and its correspondent CMD/Terminal
window and fire up ATLAS again using the appropiate "run_atlas" file.

## FAQ

- Will ATLAS work on all NVIDIA GPUs? Please make sure you have a GPU that supports the CUDA 12.X driver (recommended: 12.6.65).
- Why I am having issues with the "python" or "pip" commands? Depending on your operative system and number of python installations, you might have differents command for python 3.11 and pip other than "python" and "pip", for example "python3", "py" or "py3", and similarly "pip3" instead of "pip".
- Why I am having issues to fire up the "run_atlas" file? Depending on your operative system and number of python installations, you might have a different command for python 3.11 other than "python", for example "python3", "py" or "py3".
If that is your case, please change it inside the "run_atlas" file.
- What should I do if I have followed the instructions and still have issues running ATLAS? Please contact the authors by email (see below).

## Citation:

If you use this software in your research, please cite:

```
TY  - JOUR
T1  - Automated Tracking of Learned Actin Structures (ATLAS) in the Motility Assay
AU  - Duno-Miranda, Sebastian
AU  - Warshaw, David M.
AU  - Nelson, Shane R.
N1  - doi: 10.1016/j.bpr.2025.100221
DO  - 10.1016/j.bpr.2025.100221
T2  - Biophysical Reports
JF  - Biophysical Reports
PB  - Elsevier
SN  - 2667-0747
M3  - doi: 10.1016/j.bpr.2025.100221
UR  - https://doi.org/10.1016/j.bpr.2025.100221
Y2  - 2025/06/24
ER  - 

```

## License

This project is licensed under the GNU General Public License v3.0 - see the LICENSE file for details.

## Contact

Sebastian Duno-Miranda, sebastian.duno-miranda@uvm.edu
