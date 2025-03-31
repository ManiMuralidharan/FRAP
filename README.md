# FRAP
FRAP analysis 
Data Preparation Instructions:

"Before running the MATLAB script, you'll need to prepare your data using ImageJ and a spreadsheet program (like Microsoft Excel or Google Sheets). Please follow these steps carefully:

ImageJ Intensity Measurements:

Open your image sequence in ImageJ.
For each region of interest (ROI) you want to analyze, use the same, standardized ROI size. This is crucial for consistent data.
Measure the intensity values for each ROI across all time points.
Record these intensity values.
Spreadsheet Formatting:

Open a new spreadsheet.
Column A: Time Values: In the first column (Column A), paste the time values corresponding to each intensity measurement.
Column B and onwards: Intensity Data: Starting with Column B, paste the intensity values you recorded from ImageJ.
Each subsequent column (C, D, etc.) should contain the intensity values for a different ROI.
Header Row: In the first row of your spreadsheet, label each intensity data column (Column B, C, D, etc.) with a unique identifier (e.g., ROI 1, ROI 2, or simply B, C, D...).
Paired Data: The MATLAB script requires paired data. If you have reference data for each ROI, make sure to add that reference data as the column immediately following the ROI intensity data. For example, if column B is ROI 1 intensity, column C should be ROI 1 reference intensity.
Example:
Time, ROI1, ROI1_Ref, ROI2, ROI2_Ref, ...
0, 120, 100, 150, 110, ...
0.995, 115, 98, 145, 108, ...
1.990, 110, 95, 140, 105, ...
...
Save your spreadsheet as a CSV file (.csv).
File Naming:

Name your CSV files Control.csv and Reference.csv (or adjust the filenames in the MATLAB script if you prefer different names).
File Location:

Place the CSV files in the same directory as your MATLAB script.
Run the MATLAB Script:

Once your data is prepared, run the MATLAB script. The script will load the data, normalize it, fit a double exponential model, calculate half-lives, and generate plots
This MATLAB code for data analysis, focusing on how to use it step-by-step.

1. Save the Code as a MATLAB Script:

Open MATLAB.
Create a new script (File > New > Script).
Copy and paste the entire code into the script.
Save the script (e.g., halfLifeAnalysis.m).
2. Prepare Your CSV Data:

The code expects two CSV files: Control.csv.csv and Reference.csv.
Crucially: The first row of each CSV file should be headers.
The CSV files should contain paired columns: "bleach" data and "reference" data.
Ensure that the number of columns is even, as the doubleNormalize function relies on this.
Place the CSV files in the same directory as your MATLAB script or provide the full path to the files in the readmatrix calls.
3. Adjust Header Lines (If Necessary):

The lines:
Matlab

WTdata = readmatrix('Control.csv', 'NumHeaderLines', 1);
RLdata = readmatrix('Reference.csv', 'NumHeaderLines', 1);
tell MATLAB to skip the first row of each CSV file (the headers).
If your CSV files have a different number of header lines, change the NumHeaderLines value accordingly.
4. Run the Script:

In the MATLAB command window, type the script's name:
Matlab

halfLifeAnalysis
Press Enter.
5. Understand the Output:

Data Size Checks:
The code will display the size of the WTdata and RLdata matrices. This is vital to ensure that the data was loaded correctly.
The code will also display the first few rows of the WTdata and RLdata matrices.
Half-Life Calculations:
The code will display the calculated half-lives for each ROI in both the WT and RL datasets.
R-Squared Values:
The code will display the R-squared values for the fits. R-squared indicates how well the double exponential model fits the data.
If a fit fails, a warning and "Fit failed" message will be shown.
Plots:
A new figure will open, displaying the original data and fitted curves for each ROI in both datasets.
6. Code Details and Explanation:

readmatrix:
Loads the data from the CSV files into MATLAB matrices.
Time Vector:
Creates a time vector based on the number of rows in the data matrices and a time interval (0.995 seconds, adjust as needed).
doubleNormalize Function:
Normalizes the bleach data using the reference data.
Subtracts the mean reference value from the bleach data.
Handles potential negative values after the background subtraction.
Divides the corrected bleach data by the mean reference value.
Further normalizes by the mean of the first 10 data points.
doubleExp Function:
Defines the double exponential model used for fitting.
fitAndGetHalfLife Function:
Fits the double exponential model to each ROI's data using the fit function.
Calculates the half-life numerically by finding the time when the fitted curve reaches 50% of the initial amplitude.
Stores the fit results, half-lives, fitted parameters, and goodness-of-fit (gof) structures in cell arrays.
Handles potential fitting errors using a try-catch block.
Plotting:
Creates subplots to display the original data and fitted curves for each ROI.
Adds legends to the plots.
Displays the time, normalized fluorescence, and ROI labels.
R-squared display:
Displays the R-squared value, a measure of how well the data fits the model.
7. Important Notes:

File Paths: Ensure that the CSV files are in the correct location or provide the full file paths in the readmatrix calls.
Header Lines: Adjust the NumHeaderLines parameter in the readmatrix calls if your CSV files have different header structures.
Time Interval: Adjust the time interval (0.995 seconds) in the time vector calculation if needed.
Error Handling: The try-catch blocks in the fitAndGetHalfLife function help handle fitting errors.
Data Structure: The code assumes that the CSV files have paired columns (bleach and reference).
Fitting: The quality of the half-life calculations depends on how well the double exponential model fits the data. Adjust the initial parameter guesses in the fitAndGetHalfLife function if necessary.
Double Exp Function: The double exp function is neccesary for the half life calculation.
R-squared: R-squared values closer to 1 indicate a better fit.
Negative Values: The code handles negative values after background subtraction by setting them to zero. This may need adjustment depending on the data.
