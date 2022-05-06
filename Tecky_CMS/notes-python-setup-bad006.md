# Short notes for python with venv
&nbsp;
## Create an env
(once only or per project)
1. create a project folder and enter the folder
```bash
mkdir my_project
cd my_project
```
2. create an env for the project
```bash
python -m venv my_env
```
&nbsp;
## Activate the env session 
(every time)
1. enter the project folder
```bash
cd my_project
```
2. activate the env 
<br>

**mac**
```bash
source my_env/bin/activate
```
**windows**
```bash
my_env\Scripts\Activate.ps1
```