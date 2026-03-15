## pyenv

| 功能                       | 说明                                            |
| ------------------------ | --------------------------------------------- |
| `pyenv install 3.11.7`   | 安装 Python 3.11.7（从源码自动构建）                     |
| `pyenv global 3.11.7`    | 设置全局默认版本                                      |
| `pyenv local 3.9.17`     | 当前目录下用 Python 3.9.17（写入 `.python-version` 文件） |
| `pyenv versions`         | 查看已安装的版本                                      |
| `pyenv uninstall 3.8.10` | 卸载某个版本                                        |
## Virtualenv
Create an isolated environment
```Shell
# method 1
python -m venv .venv
# method 2
pip install virtualenv # globally
cd your_project_folder
virtualenv -p </path/to/python/executable> .venv
```
Activate and deactivate the isolated environment

> Might need to restart virtualenv after installing new dependencies
```Python
source .venv/Scripts/activate # activate project environment (windows)
source .venv/bin/activate # (Linux)
deactivate
```
Dependencies
```Shell
# you:
pip freeze > requirements.txt
# This include ALL effective packadges, to remove global ones, consider pipreqs
# other developers:
pip install -r requirements.txt
```
## Conda
```shell
# virtual env
conda env list
conda list python

conda create --name langgraph python=3.11
conda env create -f environment.yml # create env based on environment.yml
conda env update -f environment.yml # update env based on environment.yml
conda remove -n ENV_NAME --all

conda env export > requirement.yml

conda activate env-name
conda deactivate

# Dependencies
conda list
conda install jupyter
conda remove --name bio-env toolz boltons
```
## Poetry
`pyproject.toml`: project meta and dependencies
`poetry.lock`: detail meta of the dependencies
```bash
## global shell
# create pyproject.toml
poetry init --python=3.9
# install dependencies
poetry install

## poetry shell and virtual env
# enter a nested shell & activate virtual env
poetry shell
# exit vitual env and shell 
exit
# exit virtual env, not the shell
deactivate

# when you are not in shell, you need poetry run
poetry run python your_script.py
```
