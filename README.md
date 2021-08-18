# Summer Research Work

This repositor contains insights and information on Zongyi Li's Fourier Neural Operator for Parametric Partial Differential Equations paper/code. Code: https://github.com/zongyi-li/fourier_neural_operator Paper: https://arxiv.org/abs/2010.08895

**Goals**
- Run zongyi-li'a Fourier Neural Operator scripts
- Use given datasets and models to train and evalute (Navier Stokes)
  -  fourier_2d_time.py
  -  eval.py
-  Train own model and use traditional CFD solvers to construct input files to evalute

**Running Eval.py**

Eval.py uses a pretrained model and evalutes a given input file (.mat). The advantage of this is that you do not need to trian a model which is quite difficult to run on normal computers. I suggest using google colab for running majority of the files/scripts. Even on google colab the run time on training models is upwards of an hour which is why this file allows you to quickly run and evalute.
1. Upload the provided code in repositor to google colab
2. Turn on GPU accelerator: Runtime > Change runtime type > Hardware accelerator > GPU. You can check there is a device connected by running:
import torch
print(torch.cuda.device_count())


![predFinal](https://user-images.githubusercontent.com/57377860/129657092-1075e9ce-c7b5-4216-abd9-2b81373c155c.gif)
