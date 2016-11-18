A little walkthrough for deploying Theano with GPU support in Windows 10.

**Hardware info**
+ GPU: NVIDIA GeForce GTX 750 Ti
+ CPU: Intel Core i5-4590 CPU 3.30GHz
+ Memory: 16G

#Step 1
First make sure you have Visual Studio 2012 & CUDA 8.0 installed (VS2015 doesn't support cuda 8.0 yet)
#step 2
Install Anaconda
#Step 3
Open windows CMD, type
```
conda install mingw libpython
```

to install C++ compiler for python and GCC as they are dependencies for theano (according to "Alternative: Anaconda" in [Ref1](http://deeplearning.net/software/theano/install_windows.html))
#Step 4
Install theano and lasagne (or just theano)

Open command line in windows, then
##Choice 1: Lasagne
```
pip install -r https://raw.githubusercontent.com/Lasagne/Lasagne/master/requirements.txt
pip install https://github.com/Lasagne/Lasagne/archive/master.zip
```
##Choice 2: Theano only
```
pip install theano
```
#Step 5 (Optional) 
If you type *```import lasagne```* in python environment, and got error about "cl.exe", you should check the installing directory of cl.exe and add it into environment variable **PATH**

(eg: *E:\Microsoft Visual Studio 11.0\VC\bin\amd64*) 
#Step 6
Create one **.theanorc** file in your **HOME** directory (command *```echo %USERPROFILE%```* in Windows CMD) with folloing contents:
```
[global]
device = gpu
floatX = float32

[lib]
cnmem = 0.8

[nvcc]
flags=-LC:\Users\andefjx\Anaconda2\libs
compiler_bindir=C:\Program Files (x86)\Microsoft Visual Studio 11.0\VC\bin\amd64
```
Make sure to change the path in **flags** *C:\Users\andefjx\Anaconda2\libs* into your Anaconda installation directory and **compiler_bindir** to the path with **cl.exe** in it. (according to [Ref3](https://lepisma.github.io/articles/2015/07/30/up-with-theano-and-cuda/))
#Step 7
Then if you type *```import theano```*, you can get information about your gpu. 

**cuDNN** (download by yourself, need registration) is supported only by nvidia gpus with Compatibility 3.0 or higher (https://developer.nvidia.com/cuda-gpus), 

and the value of **CNMeM** (CNMeM already included in theano) in **.theanorc** represents the start size (either in MB or the fraction of total GPU memory) of the memory pool. If more memory is needed, Theano will try to obtain more, but this can cause memory fragmentation.
#Step 8 (Optional)
Installing nolearn: open CMD, and type *```pip install https://github.com/dnouri/nolearn/archive/master.zip```* to install nolearn


## referances:
###Ref1:
http://deeplearning.net/software/theano/install_windows.html
###Ref2:
http://stackoverflow.com/questions/33440453/anaconda-python-error-importing-theano
###Ref3:
https://lepisma.github.io/articles/2015/07/30/up-with-theano-and-cuda/
##Others:
http://stackoverflow.com/questions/8125826/error-compiling-cuda-from-command-prompt
http://stackoverflow.com/questions/8985860/compile-cuda-without-visual-studio-cannot-find-compiler-cl-exe-in-path
