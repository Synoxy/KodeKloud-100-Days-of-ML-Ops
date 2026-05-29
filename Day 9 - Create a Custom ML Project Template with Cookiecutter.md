🚨 **Problem:**
The xFusionCorp Industries ML platform team maintains a Cookiecutter template that new ML projects are generated from. A draft template exists at /root/code/mlops-template/, but it does not render. Correct the template and use it to generate a project.

A Cookiecutter template exists at /root/code/mlops-template/. cookiecutter is installed system-wide.

The corrected template must satisfy every one of the following:

- The cookiecutter.json declares four variables:
   - project_name (default my-ml-project)
   - author (default xFusionCorp)
   - python_version (default 3.11)
   - ml_framework with the choices sklearn, pytorch, and tensorflow
- The generated requirements.txt logic:
   - Contains scikit-learn when ml_framework is sklearn
   - Contains torch when ml_framework is pytorch
   - Contains tensorflow when ml_framework is tensorflow
- The generated README.md content:
   - Must reference both the project_name and the author from cookiecutter variables.
- The template directory structure {{cookiecutter.project_name}}/ must contain:
   - Files: README.md and requirements.txt
   - Directories: data/, models/, src/, and tests/
- Review the existing template in the VS Code explorer and correct everything that prevents it from rendering.

- Once the template renders, generate a project at /root/code/churn-model/:
  - cookiecutter /root/code/mlops-template/ -o /root/code/ --no-input project_name=churn-model ml_framework=sklearn

- The generated project must contain a requirements.txt listing scikit-learn and a README.md that mentions xFusionCorp.

🛠️ **Solution:**

1. Update **cookiecutter.json** as below:
{
    "project_name": "my-ml-project",
    "author": "xFusionCorp",
    "python_version": "3.11",
    "ml_framework": ["sklearn","pytorch","tensorflow"]
}

2. Update **requirements.txt** as below: 

{% if cookiecutter.ml_framework == 'sklearn' %}
scikit-learn
{% elif cookiecutter.ml_framework == 'pytorch' %}
torch
{% elif cookiecutter.ml_framework == 'tensorflow' %}
tensorflow
{% endif %}

3. Update **Readme.md** as below: 

# {{cookiecutter.project_name}}
Created by {{ cookiecutter.author }}.

4. Run the command: **cookiecutter /root/code/mlops-template/ -o /root/code/ --no-input project_name=churn-model ml_framework=sklearn**

5. You should the folder structure created, updated Rreadme.md & requirements.txt files:
   
