# Summer Research Work

![predFinal](https://user-images.githubusercontent.com/57377860/129657092-1075e9ce-c7b5-4216-abd9-2b81373c155c.gif)

**Goals**
- Run zongyi-li'a Fourier Neural Operator scripts
- Use given datasets and models to train and evalute (Navier Stokes)
  -  fourier_2d_time.py
  -  eval.py
-  Train own model and use traditional CFD solvers to construct input files to evalute

**Running Eval.py**

Eval.py uses a pretrained model and evalutes a given input file (.mat). The advantage of this is that you do not need to trian a model which is quite difficult to run on normal computers. I suggest using google colab for running majority of the files/scripts. Even on google colab the run time on training models is upwards of an hour which is why this file allows you to quickly run and evalute.
