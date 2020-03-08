[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/ulf1/sparsity-pattern/master?urlpath=lab)

# sparsity-pattern
Generate different types of sparsity pattern for sparse matrices.


## Installation
The `sparsity-pattern` [git repo](http://github.com/ulf1/sparsity-pattern) is available as [PyPi package](https://pypi.org/project/sparsity-pattern)

```
pip install sparsity-pattern
pip install git+ssh://git@github.com/ulf1/sparsity-pattern.git
```


## Usage
The `block`-diagonal pattern for tensorflow

```
import sparsity_pattern
import tensorflow as tf

n_rows, n_cols = 10, 12
idx = sparsity_pattern.get('block', min(n_rows, n_cols), block_sizes=[3, 1, 2])

mat = tf.sparse.SparseTensor(
    dense_shape=(n_rows, n_cols),
    indices=tf.convert_to_tensor(idx, dtype=tf.int64),
    values=range(1, len(idx)+1))

print(tf.sparse.to_dense(mat))
```

The `circle` pattern for pytorch

```
import sparsity_pattern
import torch

n_rows, n_cols = 5, 7
idx = sparsity_pattern.get('circle', min(n_rows, n_cols), offsets=[1, 2])

mat = torch.sparse_coo_tensor(
    indices=torch.tensor(idx).transpose(0, 1), 
    values=range(1, len(idx)+1),
    size=[n_rows, n_cols])

print(mat.to_dense())
```

Check the [examples](http://github.com/ulf1/sparsity-pattern/examples) folder for more notebooks.


## Appendix

### Install a virtual environment

```
python3.6 -m venv .venv
source .venv/bin/activate
pip3 install --upgrade pip
pip3 install -r requirements.txt
```

(If your git repo is stored in a folder with whitespaces, then don't use the subfolder `.venv`. Use an absolute path without whitespaces.)

### Python commands

* Jupyter for the examples: `jupyter lab`
* Check syntax: `flake8 --ignore=F401 --exclude=$(grep -v '^#' .gitignore | xargs | sed -e 's/ /,/g')`
* Run Unit Tests: `pytest`
* Upload to PyPi with twine: `python setup.py sdist && twine upload -r pypi dist/*`

### Clean up 

```
find . -type f -name "*.pyc" | xargs rm
find . -type d -name "__pycache__" | xargs rm -r
rm -r .pytest_cache
rm -r .venv
```


### Support
Please [open an issue](https://github.com/ulf1/sparsity-pattern/issues/new) for support.


### Contributing
Please contribute using [Github Flow](https://guides.github.com/introduction/flow/). Create a branch, add commits, and [open a pull request](https://github.com/ulf1/sparsity-pattern/compare/).
