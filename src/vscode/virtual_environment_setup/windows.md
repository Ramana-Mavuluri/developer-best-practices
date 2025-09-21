# Python virtual environment set up in PC(Windows)
This guide will help you set up with following items
* Setup python virtual environment
* Automatically switch to python virtual environment when you switch from project to project
* Automatically install required packages when you switch to project
* Automatically load environment variables when you switch to project
* Add .gitignore file to ignore virtual environment folder

## Prerequisites
* Python installed in your PC. You can download it from [here](https://www.python)
* Visual Studio Code installed in your PC. You can download it from [here](https://code.visualstudio.com/)
* Powershell 7.4.*x or higher version is installed
* Python extension installed in Visual Studio Code. You can find it [here](https://marketplace.visualstudio.com/items?itemName=ms-python.python)


## Steps

### Setup python virtual environment
1. Open your project folder in Visual Studio Code
2. Open terminal in Visual Studio Code (Ctrl + `) and select powershell
3. Run following command to create python virtual environment in powershell
    ```powershell
    python -m venv .venv
    ```
4. Now you can see a folder named `.venv` in your project folder
5. Now activate the virtual environment by running following command in terminal
    ```powershell
    .\.venv\Scripts\Activate.ps1
    ```
    
    if you encounter any error while activating the virtual environment, you can refer to [this guide](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies) to change the execution policy in powershell; follow the instructions in the guide to set the execution policy to `RemoteSigned` or `Unrestricted` as needed.

    or If you do not have time and energy to read the guide, here is a brief summary of the steps you need to follow:
    
    Run the following command in powershell to set the execution policy to `RemoteSigned` for the current user:

    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
    ```
    Now you can run the command 
    ```powershell
    .\.venv\Scripts\Activate.ps1
    ``` to activate the virtual environment


6. You can deactivate the virtual environment by running the command
    ```powershell
    deactivate
    ```


### Automatically switch to python virtual environment when you switch from project to project
Best way to do this is via direnv plugin, you can find the installation guide [here](https://direnv.net/docs/installation.html)

First two steps are one time setup steps, you do not need to repeat them for every project

1. Install direnv; Best way to install in windows is using WinGet, you can run the following command in powershell to install direnv
    ```powershell
    winget install direnv.direnv
    ```
2. Add *direnv* hook to your powershell profile
    First, check if you have a powershell profile file by running the command
    ```powershell
    Test-Path $PROFILE
    ```
    If the command returns `True`, you have a profile file; if it returns `False`, you do not have a profile file.

    If you do not have a profile file*(i.e, when the above command returns `False`), you can create $PROFILE by running the command
    ```powershell
    New-Item -ItemType File -Path $PROFILE -Force
    ```
    Now, add the following line to your profile file

    (You can open the profile file by running the command `code $PROFILE` in powershell)
    ```powershell
    Invoke-Expression "$(direnv hook pwsh)"
    ```
3. Create a `.envrc` file in your project folder and add the following line- This step is required for every project
    ```
    layout python
    ```
4. Restart the terminal in Visual Studio Code
5. Now, whenever you open the project folder in Visual Studio Code, it will automatically switch to the python virtual environment


### Automatically install required packages when you switch to project
The main objective behind creating virtual environments is to manage dependencies for different projects separately.
There are two ways to manage dependencies in python virtual environments
1. Manually install the required packages using pip command whenever you switch to the project
2. Use a `requirements.txt` file to list all the required packages for your project and install them automatically when you switch to the project

#### Manually install the required packages
1. You can manually install the required packages using pip command
    ```powershell
    pip install <package_name>
    ```
#### Automatically install the required packages using requirements.txt file
1. You can create a requirements.txt file to list all the required packages for your project by running the command
    ```powershell
    pip freeze > requirements.txt
    ```
2. You can install all the required packages from the requirements.txt file by running the command
    ```powershell
    pip install -r requirements.txt
    ```
3. You can add the above command to the `.envrc` file to automatically install the required packages whenever you switch to the project
    ```
    layout python
    pip install -r requirements.txt
    ```
4. Now, whenever you switch to the project, it will automatically install the required packages from the requirements.txt file
    Note: It will install only those packages which are not already installed in the virtual environment 

### Automatically load environment variables when you switch to project
Simple approach is to set them in `.envrc` file when there are less number of environment variables; if there are many environment variables, you can use a `.env` file or `.env.local` file to manage them
#### Adding environment variables in `.envrc` file
1. You can set environment variables in the `.envrc` file to automatically load them whenever you switch to the project
2. You can set environment variables in the `.envrc` file by adding the following lines
    ```
    export DATABASE_URL=postgres://user:password@localhost:5432/dbname
    export SECRET_KEY=your_secret_key
    ```
3. You can add the above lines to the `.envrc` file to automatically load the environment variables whenever you switch to the project
    ```
    layout python
    pip install -r requirements.txt
    export DATABASE_URL=postgres://user:password@localhost:5432/dbname
    export SECRET_KEY=your_secret_key
    ```
4. Now, whenever you switch to the project, it will automatically load the environment variables from the `.envrc` file
5. You can also set environment variables in the terminal after activating the virtual environment by running the following commands
    ```powershell
    $env:DATABASE_URL="postgres://user:password@localhost:5432/dbname"
    $env:SECRET_KEY="your_secret_key"
    ``` 
#### Using `.env` or `.env.local` file to manage environment variables
1. You can also use a `.env` file to set environment variables for your project
    ```
    DATABASE_URL=postgres://user:password@localhost:5432/dbname

    SECRET_KEY=your_secret_key
    ```
2. You can add the above line to the `.envrc` file to automatically load the environment variables whenever you switch to the project
    ```
    layout python
    pip install -r requirements.txt
    . $PWD/.env
    ```
3. Now, whenever you switch to the project, it will automatically load the environment variables from the `.env` file
4. You can also use a `.env.local` file to set environment variables for your local development
    ```
    DATABASE_URL=postgres://user:password@localhost:5432/dbname

    SECRET_KEY=your_secret_key
    ```
5. You can add the above line to the `.envrc` file to automatically load the environment variables whenever you switch to the project
    ```
    layout python
    pip install -r requirements.txt
    . $PWD/.env
    . $PWD/.env.local
    ```
6. Now, whenever you switch to the project, it will automatically load the environment variables from the `.env.local` file

### Adding .gitignore file to ignore virtual environment folder
1. You can create a `.gitignore` file in your project folder to ignore the virtual environment folder
2. You can add the following line to the `.gitignore` file to ignore the virtual environment folder
    ```
    .venv/
    ```
3. Now, the virtual environment folder will be ignored by git and will not be pushed to the remote repository
4. You can also add other files and folders to the `.gitignore` file as per your requirement
5. Here is a sample `.gitignore` file for a python project
    ```
    .venv/
    # Distribution / packaging
    .Python
    # C extensions
    *.so
    # Byte-compiled / optimized / DLL files
    __pycache__/
    *.py[cod]
    *$py.class
    # Cython debug symbols
    cython_debug/
    # Jupyter Notebook checkpoints
    .ipynb_checkpoints
    # pyenv
    .python-version
    ```