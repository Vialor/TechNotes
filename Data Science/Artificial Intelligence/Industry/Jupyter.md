```bash
# in venv
pip install ipykernel
python -m ipykernel install --user --name=LeNet-venv --display-name="Python (LeNet-venv)"
# unregister a kernel
jupyter kernelspec uninstall LeNet-venv
# launch
jupyter lab
```