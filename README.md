# Summer Research Work

This repositor contains insights and information on Zongyi Li's Fourier Neural Operator for Parametric Partial Differential Equations paper/code. Code: https://github.com/zongyi-li/fourier_neural_operator Paper: https://arxiv.org/abs/2010.08895

## Goals
- Run zongyi-li'a Fourier Neural Operator scripts
- Use given datasets and models to train and evalute (Navier Stokes)
  -  fourier_2d_time.py
  -  eval.py
-  Train own model and use traditional CFD solvers to construct input files to evalute

## Running Eval.py

Eval.py uses a pre-trained model and evaluates a given input file (.mat). The advantage of this is that you do not need to train a model which is quite difficult to run on normal computers. I suggest using Google Colab for running the majority of the files/scripts. Even on Google Colab the run time on training models is upwards of an hour which is why this file allows you to quickly run and evaluate.
1. Upload the provided code in repositor to google colab
2. Turn on GPU accelerator: Runtime > Change runtime type > Hardware accelerator > GPU. You can check there is a device connected by running:  
import torch  
print(torch.cuda.device_count())
3. Go into folder directory
4. Run the code:  
!python3 eval.py

**If there is an issue with certain torch functions not being available, install an older version of pytorch:  
!pip install torch==1.7.1 torchvision==0.8.2 torchaudio==0.7.2

The output file will be located in the 'pred' folder. It should be a MatLab file (.mat). There will be 2 varibles (4 dimensional) within the file; u and pred.
- Pred: Predicted timesteps
- U: Actual timesteps

Both are 4 dimensional matrices N by R by R by T
- N: Number of examples
- R: Resolution (most commonly 64 or 128)
- T: Time step

I have provided a short script file (MatlabScript.txt) which you can use to squeeze out matrices of time steps to view the change in vorticity. You can save the heatmaps to your local machine. The error between the predicted flow and actual is usually sub 10%.

Left is actual and right is predicted.  
<img src="https://user-images.githubusercontent.com/57377860/129989723-8c32c002-d5d4-45b9-a7f6-dc43bcd72424.gif" width="425"/> <img src="https://user-images.githubusercontent.com/57377860/129989716-d7246e90-2a73-4161-b56e-707da791035b.gif" width="425"/> 

## SU2
SU2 is open-source software that is used to solve partial differential equations and performing PDE-constrained optimization numerically. A mesh file (.su2) and configuration file (.cfg) are needed to run SU2. There are many provided on the SU2 website and Github. For this example, Unsteady_NACA0012 folder was used which includes a mesh file of an airfoil.
1. Download mesh and configuration file 
2. Open output .vtk files in Paraview 
3. Convert them from unstructured to structured mesh outputs
4. Save 64x64 sections in cvs file for each time step <img align="right" width=50%  src="https://user-images.githubusercontent.com/57377860/130001787-37932e53-a420-465d-bb91-8a1fa28df4f7.PNG">

5. Open cvs files in MatLab, normalize data
6. Construct matrix of size resolution, resolution, # timesteps
7. Create 20 for appropriate batch size for code
8. Save final MatLab file to local machine
9. Upload and run on google colab using one of the files (try eval.py)

***Result***  
The predition given by the script was terrible. The error rate was above 130%, which was expected. This was simply a trial to see the direction this can be taken in. Left is actual and right is predicted.  
<img src="https://user-images.githubusercontent.com/57377860/130003166-a5adf9cc-e981-48a7-b445-00a840064355.gif" width="440"/> <img src="https://user-images.githubusercontent.com/57377860/130003170-c2346315-96ad-4d55-bd53-412e88f69d49.gif" width="425"/> 

## Data Generation
The AI model needs to be trained and evaluated using datasets. These datasets are generated using scripts that were provided in Zongyi-li's repository and are stored in MatLab files (.mat). A couple of datasets for you to train a model have been provided in a google drive folder included some pre-trained models. The Navier stokes folder contains data generation scripts for the codes that work with that PDE. The scripts use traditional computational methods to calculate accurate time steps. There are a couple of variables that can be changed or are needed for the data generation:
- w0: initial vorticity
- f: forcing term
- visc: viscosity (1/Re)
- T: final time
- delta_t: internal time-step for solve (descrease if blow-up)
- record_steps: number of in-time snapshots to record
- S: resolution
- N: number of examples  

You could manipulate these, including the forcing term which is quite interesting. This can be done using PyTorch code or MatLab. MatLab method requires you to upload the data back into the python code which can be some work. Changing the forcing term is how you can manipulate the flow of your data or try to mimic laminar flow. The initial vorticity is also quite important to understand for data generation. It uses Gaussian Random Fields.

Running these data generation scripts is quite simple. Change your directory into the folder and use !python filename to run. If there are issues with certain methods not existing, simply install an earlier version of pytorch: !pip install torch==1.7.1 torchvision==0.8.2 torchaudio==0.7.2

## Flaws
The final goal is to be able to make mesh files of our own, generate timesteps using traditional methods, and then have the AI predict timesteps using them. The initial required time steps needed for the input can be derived from traditional CFD methods, like SU2. There are multiple obstacles and constraints that have bounded the application of the new research done by Zongyi-li and his team. The code provided with his research is more like a proof of concept of Fourier Neural Operator for Parametric Partial and not be used for real applications just yet, which is undoubtedly impressive. For example, boundary conditions are periodic, which are generally used in molecular dynamics simulations to avoid issues with boundary effects caused by finite size. The system is made more like an infinite one using this. However, these conditions are not plausible for what is trying to be achieved here.

The AI model is only able to predict what it has learnt. It simply cannot start predicting things it has never seen before which is what was happening here and why there was a high error expected from the start. If we provide the neural operator with data generated by the script provided in Zongyi-li's repository, it will learn to predict flows governed under those conditions, which we cannot mimic using SU2 as of now.

- Code is not complete and bug free. Last commit on Aug 11th 2021 showing how work is still being done on it.
- Code is a proof of concept of Fourier Neural Operator for Parametric Partial Differential Equation not for real applications 
- Has limitation with; Boundary conditions, External force matrix (gaussian random field), Number of time steps needed for scripts to work, Does not translate well with actual CFD standards (SU2 configurations)

