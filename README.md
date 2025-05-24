# Lumbar MRI Data Processing and Deep Learning Pipeline

This repository contains scripts and instructions for processing lumbar MRI data from DICOM format to a format suitable for deep learning models. The pipeline includes steps for organizing patient data, filtering lumbar cases, converting MRI scans to NIfTI and HDR/IMG formats, and preparing the final dataset for model training.

---

## Table of Contents

- [Data Processing Pipeline](#data-processing-pipeline)
- [Folder Structure](#folder-structure)
- [How to Execute the Code](#how-to-execute-the-code)
- [Additional Notes](#additional-notes)

---

## Data Processing Pipeline

Follow these steps to process your MRI data for training:

1. **Organize Raw MRI Data**
   - **Initial Data:** All patient MRI scans are stored in any folder (e.g., `x`).
   - **Patient Separation:** Use the **Radiant application** to export each patient's MRI scans into a separate folder named with the patient’s ID.
     - **Result:** You will have a folder named `Dicoms`, containing subfolders for each patient (e.g., `Dicoms/PatientID_1`, `Dicoms/PatientID_2`, etc.), each with the MRI scans for that patient.

2. **Filter Lumbar Cases**
   - **Filtering:** Run the script `LumbarFind` with the following inputs:
     - **Input:** Path to the `Dicoms` folder.
     - **Input:** Path to your `.excel` file (containing information about which patients have lumbar disease).
   - **Output:**  
     - A new folder named `Lumbar` is created under the `Dicoms` folder.
     - **Only** patients with lumbar disease are copied into the `Lumbar` folder, preserving the original folder structure.

3. **Standardize Folder Names**
   - **Renaming:** Run the script `rename` with the following input:
     - **Input:** Path to the `Lumbar` folder.
   - **Output:**  
     - Each patient folder inside `Lumbar` is renamed to contain only the patient ID (removing the patient name).

4. **Convert MRI to NIfTI Format**
   - **Conversion:** Run the script `NifitGet` with the following input:
     - **Input:** Path to the `Lumbar` folder.
   - **Output:**  
     - A new folder named `NIFTIOUTPUT` is created under the `Lumbar` folder.
     - The MRI scans for each lumbar patient are converted to NIfTI format and stored in patient-specific subfolders within `NIFTIOUTPUT`.

5. **Generate HDR and Disk Image Files**
   - **Conversion:** Run the script `HDRChange` with the following input:
     - **Input:** Path to the `NIFTIOUTPUT` folder.
   - **Output:**  
     - A new folder named `HDR_OUTPUT` is created under `NIFTIOUTPUT`.
     - The NIfTI files are converted to `.hdr` and `.img` (disk image) format and stored in patient-specific subfolders within `HDR_OUTPUT`.
     - **Note:** This `HDR_OUTPUT` folder is the final input for the deep learning model.

---

## Folder Structure

After all processing steps, your folder structure will look like this:

MainFolder/
│
├── Dicoms/
│ ├── PatientID_1/
│ ├── PatientID_2/
│ └── ...
│
├── Dicoms/Lumbar/
│ ├── PatientID_1/
│ ├── PatientID_2/
│ └── ...
│
├── Dicoms/Lumbar/NIFTIOUTPUT/
│ ├── PatientID_1/
│ ├── PatientID_2/
│ └── ...
│
└── Dicoms/Lumbar/NIFTIOUTPUT/HDR_OUTPUT/
├── PatientID_1/
├── PatientID_2/
└── ...


---

## How to Execute the Code

1. **Organize raw MRI data into patient-specific folders using Radiant.**
2. **Run `LumbarFind` to filter lumbar cases.**
3. **Run `rename` to standardize folder names.**
4. **Run `NifitGet` to convert MRI to NIfTI.**
5. **Run `HDRChange` to generate `.hdr` and `.img` files.**
6. **Use the `HDR_OUTPUT` folder as input for your deep learning model.**

---

## Additional Notes

- **Ensure all scripts and dependencies are installed before running.**
- **Always check the output folder structure and file integrity after each step.**
- **Refer to the documentation of each script for detailed usage and parameter options.**
