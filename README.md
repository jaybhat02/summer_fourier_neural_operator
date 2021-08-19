# Summer Research Work

This repositor contains insights and information on Zongyi Li's Fourier Neural Operator for Parametric Partial Differential Equations paper/code. Code: https://github.com/zongyi-li/fourier_neural_operator Paper: https://arxiv.org/abs/2010.08895

## Goals
- Run zongyi-li'a Fourier Neural Operator scripts
- Use given datasets and models to train and evalute (Navier Stokes)
  -  fourier_2d_time.py
  -  eval.py
-  Train own model and use traditional CFD solvers to construct input files to evalute

## Running Eval.py

Eval.py uses a pretrained model and evalutes a given input file (.mat). The advantage of this is that you do not need to trian a model which is quite difficult to run on normal computers. I suggest using google colab for running majority of the files/scripts. Even on google colab the run time on training models is upwards of an hour which is why this file allows you to quickly run and evalute.
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

Predicted Flow:  
![pred](https://user-images.githubusercontent.com/57377860/129989716-d7246e90-2a73-4161-b56e-707da791035b.gif)  
Actual Flow:  
![u](https://user-images.githubusercontent.com/57377860/129989723-8c32c002-d5d4-45b9-a7f6-dc43bcd72424.gif)

## SU2
SU2 is an open source software that is used to solve partial differential equations and performing PDE-constrained optimization numerically. A mesh file (.su2) and configuration file (.cfg) are needed to run SU2. There are many provided on the SU2 website and github. For this example Unsteady_NACA0012 folder was used which includes a mesh file of an airfoil.
1. Download mesh and configuration file 
2. Open output .vtk files in Paraview 
3. Convert them from unstructured to structured mesh outputs
<img align="right" width=60%  src="https://user-images.githubusercontent.com/57377860/130001787-37932e53-a420-465d-bb91-8a1fa28df4f7.PNG">
4. Save 64x64 sections in cvs file for each time step
5. Open cvs files in MatLab, normalize data
6. Construct matrix of size resolution, resolution, # timesteps
7. Create 20 for appropriate batch size for code
8. Save final MatLab file to local machine
9. Upload and run on google colab using one of the files (try eval.py)


