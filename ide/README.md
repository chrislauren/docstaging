# Visual Studio Code Tools for AI (Azure Machine Learning Integration)

**NOTE:** This integration with Visual Studio Code Tools for AI is currently available externally and may be used in customer engagements and/or production use cases yet. However, it does not yet use the One RP model and will need to be updated soon to leverage these changes in Azure ML. 

## Main features
 * Debug & iterate Python code using 
 * Track history and models during experimentation (Available) 
 * Deploy models to production as Docker containers (Work in progress)
 * Train models on scalable compute: local, DSVM, DLVM, ACI, Batch AI. (Available)
 * Use high-level APIs for training deep learning models. (Available)
 * Use high-level APIs for automated hyperparameter tuning. (Work in progress)
 * Use Azure ML services from common Python environments: Jupyter, VSCode, standalone python.exe. (Available)
 * Automate machine learning by accessing Azure ML services from CI/CD workflows (Work in progress)

## Prerequisites
### 1. Upgrade NVidia GPU drivers (optional)  
If you are going to use a deep learning framework like Tensorflow or CNTK and have an NVidia GPU card, you should upgrade to the latest driver for the best performance and framework compatibility. You should also install [CUDA 9.0](https://developer.nvidia.com/cuda-90-download-archive) and [cuDNN 7.0](https://developer.nvidia.com/cudnn). 

### 2. Install Python 3.5 or 3.6 
Install the latest [Python 3.5](https://www.python.org/downloads/release/python-354/) and add it to %Path% environment variable. Python 3.6 should work too.

### 3. Add Microsoft.MachineLearning.Scoring (Sonoma) Nuget reference 
To generate C# code from trained TensorFlow and ONNX models via  new Inferencing Library projects, you need to add a NuGet reference to the current private location in the [Bing NuGet feed](https://msasg.pkgs.visualstudio.com/_packaging/Bing/nuget/v3/index.json). 

To do this, add a NuGet package source in Visual Studio. From menu "Tools" => "NuGet Package Manager" => "Package Manager Settings" => "Package Sources".
![](images\NuGetSettings.png)

```shell
$ az login
$ az account set -s "<subscription_id>"

# register the new RP
$ az provider register -n Microsoft.MachineLearningServices

# check the registration status
$ az provider show -n Microsoft.MachineLearningServices
```

Note: we currently have a bug where the registration status might get stuck on `registering`, but the registration actually has already succeeded.

### Enable EUAP (optional)
Some SDK functionalities might initially be only available in the Azure Canary Region (eastus2euap, aka EUAP). To gain access to this region, please submit a request here: https://aka.ms/canaryintwhitelist. 

Note it appears that only subscriptions belonging to Microsoft tenant are approved. MSDN-based Azure subscriptions appeared to be now allowed.

## Installation

It is best if you create a new conda environment locally to try this SDK, so it doesn't mess up with your existing Python environment. 

**WARNING**: Please do **NOT** install this SDK in the Python environment installed by Azure ML Workbench, or run SDK code through Azure ML Workbench. Things will go horribly wrong.

  1. Install mini-conda from [here](https://conda.io/miniconda.html).
  2. In a terminal window, type the following commands.
  
  ```shell
  # create a new conda environment with Python 3.6
  $ conda create -n myenv Python=3.6

  # activate the conda environment
  $ conda activate myenv

  # install azureml-core 
  (myenv) $ pip install --extra-index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/rc/D72EBD8984244C659BDE1CFECDC3435 azureml-core

  # install azureml-train
  (myenv) $ pip install --extra-index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/rc/D72EBD8984244C659BDE1CFECDC3435 azureml-train
  ```
  
  Note: In near future `azureml-core` and `azureml-train` will be unified to single `azureml-sdk` meta-package.

  Also Note: If you have older version of the SDK installed, it is best to remove them before installing the new version. To remove the old versions:

  ```shell
  (myenv) $ pip uninstall azureml-train
  (myenv) $ pip uninstall azureml-core
  ```

  3. If you're using Jupyter Notebooks, install a conda-aware Jupyter Notebook server, and install and enable run history widgets:
  ```shell
  # install Jupyter 
  (myenv) $ conda install nb_conda

  # install run history widget
  (myenv) $ jupyter nbextension install --py --sys-prefix azureml.train.widgets

  # enable run history widget
  (myenv) $ jupyter nbextension enable --py --sys-prefix azureml.train.widgets
  ```

## Sample Notebooks

Once you've installed the packages, start Jupyter Notebook or your favorite Python editor, and start experimenting! 

  ```
  # start Jupyter Notebook locally
  (myenv) $ jupyter notebook
  ```

**IMPORTANT**: Please make sure you use the `Python [conda env:myenv]`
kernel when trying the sample Notebooks.

* [101 Core SDK Sample Notebook](notebooks/101-Core-SDK.ipynb) 

The above sample Notebook includes basic end-to-end steps of creating Workspace and Project, training a model while tracking run history, and deploying the model as web service.

Then try advanced examples:
 * [Train on remote VM](notebooks/March-20-Example.ipynb)
 * [Train CNTK model using a BatchAI cluster](notebooks/March-27-Example.ipynb)
 * [Tune CNTK model using HyperDrive](notebooks/HyperDrive-Training-Example.ipynb)
 * [Tune a model using AutoML service](notebooks/AutoML-Training-Example.ipynb)

## Clean up

If you want to remove the SDK from the conda environment, type the following commands:
```shell
(myenv) $ pip uninstall azureml-train
(myenv) $ pip uninstall azureml-core
```

If you want to also delete the conda enviornment, type the following commands:
```shell
(myenv) $ conda deactivate myenv
$ conda env delete -n myenv
```

## What about CLI?

The CLI provides largely the same functionality as the `azureml.core` sub-package of SDK. The intended user interface is different: CLI is for command line, while SDK is for Notebooks and Python IDEs. The SDK also has additional functionality in `azureml.train` namespace for high-level interfaces for training of deep learning models, and hyperparameter tuning.

If you want to try CLI, follow [these instructions](../cli/README.md).

## Feedback, issues, suggestions

Please email viennadogfood@service.microsoft.com.

