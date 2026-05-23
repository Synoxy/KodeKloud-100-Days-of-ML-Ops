##### Problem Statement:
A teammate has configured a JupyterLab server for the xFusionCorp Industries data science team, but the server does not behave correctly. Inspect the configuration, diagnose the issues, and start the server.

JupyterLab is already installed in the virtual environment at /root/code/.env. The team's configuration file is at /root/code/jupyter_lab_config.py and is visible in the file explorer.
When JupyterLab is started, the Jupyter UI button at the top of the lab must open the notebook interface.
For this to work, the running server must meet the following requirements:
- it listens on port 8888
- it binds on 0.0.0.0 (the lab proxy cannot reach a server that is only bound on 127.0.0.1)
- the notebook root directory is /root/notebooks/, and that directory exists on disk

Open the configuration file, identify every setting that prevents the requirements above from being met, and correct it. Create any missing directories.
Start JupyterLab from within the virtual environment using the corrected configuration.

**Solution:**
1. Make below changes in `jupyter_lab_config.py` file:

c.ServerApp.token = ''
c.ServerApp.password = ''
c.ServerApp.disable_check_xsrf = True
c.ServerApp.notebook_dir = '/root/notebooks/'
c.ServerApp.port = 8888
c.ServerApp.ip = '0.0.0.0'

2. Activate the Virtual Environment: **source /root/code/ml-env/bin/activate**
3. Run the jupyter notebook using: **jupyter lab --config=/root/code/jupyter_lab_config.py --allow-root**