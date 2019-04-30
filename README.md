# Example workflow with Azure ML and Azure Devops

[Azure ML Service](https://docs.microsoft.com/de-de/azure/machine-learning/service/) blah [Azure DevOps](https://azure.microsoft.com/de-de/services/devops/) blah blah [TDSP](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) blah blah

## 1. Creating and preparing the repo

Create a new project and repository in Azure DevOps and clone it to your local machine in VS code

[alt text][clone_to_vscode]

Create a .gitignore file, and configure it not to check in data (CSV files in this example) or Jupyter Notebook checkpoints. For example:

```
# No notebook checkpoints
.ipynb_checkpoints/
# No data files
**/*.csv
```

Configure your repository to only check in code, and not state or output for the Jupyter notebooks. There are different approaches to achieve this objective, in the appendix of this document you can see some reference links on this subject. For the sake of simplicity we will use a package called nbstripout. It is not the fastest, but probably one of the easiest to use.

First, install nbstripout in your local machine if you did not have it: `conda install -c conda-forge nbstripout` if you are using Anaconda, or just `pip install --upgrade nbstripout`. Now you can navigate to the directory where you cloned your repository and configure it to use nbstripout (it leverages git-filters)

```
pip install --upgrade nbstripout
nbstripout --install --attributes .gitattributes
```

## 2. Create a Jupyter Notebook for data exploration

### 2.1. Using local notebooks

We will start using a Jupyter Notebook in your local machine. Obviously you need to have installed some Python distribution such as Anaconda, and virtual environments are recommended. You can use any python prompt to launch Jupyter Notebooks, or the integrated Python prompt in vscode, what we are doing in this example(using the Microsoft Python extension for VS Code):

![alt text][open_jupyter]

You can now use your favorite statistical exploratory methods to browse through your data. Example: some data representations using the Python package Seaborn:

![alt text][seaborn]

After you are done with your exploration, you can check-in your notebook, either using the git CLI or the git extensions from VS Code.

### 2.1. Using Azure Notebooks

Azure Notebooks is not too well suited for working in teams, because of the following reasons:

* No integration with CI/CD pipelines such as Azure DevOps. You can download repositories from GitHub, that is essentially it
* No integrated mechanism to push changes back to the repository.

That being said, Azure Notebooks has as well some nice advantages:

* It is free! (albeit running in VMs with limited performance)
* Does not require any Python installed in your notebook

Hence it might be worthy hacking our way through Azure Devops after all in some circumstances. What you can do is have a Notebook that you will use to issue your git commands. Note that Jupyter Notebooks have no interactive console, so you will have to give username and password in the command line. Hence, make sure that you create your Azure Notebook as "private".

You can use now the git CLI to clone an existing git repository.

![alt text][aznb_clone]

You can now navigate in Azure Notebooks to the directory where you cloned the git repo (`mlops-toyota` in this example), open the notebooks and do any required changes. When you are done, you could check-in your changes, possibly to a new branch:

![alt text][aznb_push]

We did not install here nbstripout, so make sure you clear the notebook output before saving it, so that you are not checking in your variables into the git repository.


## Appendix: Additional documentation

Notebooks and git:
* [http://timstaley.co.uk/posts/making-git-and-jupyter-notebooks-play-nice/]
* [https://blogs.msdn.microsoft.com/uk_faculty_connection/2018/11/07/using-vsts-azure-devops-for-data-science-source-control-creating-a-collection-of-interactive-notebooks-and-outputs/]
* [http://mateos.io/blog/jupyter-notebook-in-git/]

Notebooks in Azure Devops
* [https://developercommunity.visualstudio.com/content/idea/365561/add-support-for-viewing-jupyter-ipython-notebooks.html]

[nbstripout](https://github.com/kynan/nbstripout):


or (Windows)

```
git filter-branch -f --tree-filter "find . -name \"*.ipynb\" -exec nbstripout \"{}\" +"
```

### 1.a. Using a local notebook
![alt text][seaborn]


[clone_to_vscode]: images/02-clone_to_vscode.jpg "Cloning a new Azure DevOps repository to VS Code"
[open_jupyter]: images/05-open_python_terminal.jpg "Open Jupyter Notebooks from VS Code"
[seaborn]: images/08_seaborn.jpg "Visualizing data with the seaborn Python module"
[aznb_clone]: images/10-azure_notebooks_clone.jpg "Cloning an Azure Devops repo into Azure Notebooks"
[aznb_push]: images/11-azure_notebooks_push.jpg "Committing code from Azure Notebooks to a git repository"
