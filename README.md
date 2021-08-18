# Summer Research Work

This repositor contains insights and information on Zongyi Li's Fourier Neural Operator for Parametric Partial Differential Equations paper/code. Code: https://github.com/zongyi-li/fourier_neural_operator Paper: https://arxiv.org/abs/2010.08895

***Goals***
- Run zongyi-li'a Fourier Neural Operator scripts
- Use given datasets and models to train and evalute (Navier Stokes)
  -  fourier_2d_time.py
  -  eval.py
-  Train own model and use traditional CFD solvers to construct input files to evalute

*Running Eval.py*

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
![predFinal](https://user-images.githubusercontent.com/57377860/129657092-1075e9ce-c7b5-4216-abd9-2b81373c155c.gif)
