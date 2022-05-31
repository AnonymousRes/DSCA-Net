# DSCA-Net

## Note:

Due to the double-blind policy for other papers associated with this account, we may at any time set this project to "private" and make it invisible, in which case, if you need the code, send an email to leewu@mail.sdu.edu.cn and tensorfire@mail.sdu.edu.cn

## Requirements:

- Keras	2.3.1
- Scikit-learn	0.22
- TensorFlow	2.2.0
- NumPy 1.17.4	

## Tutorial:

1. Create the MIMIC-III database according to the official tutorial of the MIMIC project.
2. Process the MIMIC-III database using the MIMIC-III_Data_Preprocess .sql (which roughly documents our process of processing EHR features) to get the file lw_dataset_100.csv.
3. Use the file "data_process_mimic.py" to further process the data - "lw_dataset_100.csv" The data used for training the model.
4. 4. Run main.py
