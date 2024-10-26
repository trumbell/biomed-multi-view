---
{{ card_data }}
---

# {{ hf_ft_model_path }}
**SmallMoleculeMultiView**, multi-view molecular foundation model.

- **Developers:** IBM Research
- **GitHub Repository:** [{{ repo_url }}]({{ repo_url }})
- **Paper:** [Multi-view biomedical foundation models for molecule-target and property prediction](https://arxiv.org/abs/TBD)
- **Release Date**: Oct 29th, 2024
- **License:** [{{ license }}](https://www.apache.org/licenses/LICENSE-2.0).

## Model Description

This model contains the implementation of the Multi-view Molecular Embedding with Late Fusion (MMELON) architecture. MMELON combines molecular representations from three views — image, 2-dimensional chemically-bonded graph, and text (SMILES) —to learn a joint embedding that can be finetuned for downstream tasks in chemical and biological property prediction.

It was introduced in the paper [Multi-view biomedical foundation models for molecule-target and property prediction](https://arxiv.org/) by authors and first released in [this repository]({{ repo_url }}).

![SmallMoleculeMultiView Overview]({{ repo_url }}/blob/main/docs/overview.png?raw=true)

* Image Representation: Captures the 2D visual depiction of molecular structures, highlighting features like symmetry, bond angles, and functional groups. Molecular images are generated using RDKit and undergo data augmentation during training to enhance robustness.
* Graph Representation: Encodes molecules as undirected graphs where nodes represent atoms and edges represent bonds. Atom-specific properties (e.g., atomic number, chirality) and bond-specific properties (e.g., bond type, stereochemistry) are embedded using categorical embedding techniques.
* Text Representation: Utilizes SMILES strings to represent chemical structures, tokenized with a custom tokenizer. The sequences are embedded using a transformer-based architecture to capture the sequential nature of the chemical information.

The embeddings from these single-view pre-trained encoders are combined using an attention-based aggregator module. This module learns to weight each view appropriately, producing a unified multi-view embedding. This approach leverages the strengths of each representation to improve performance on downstream predictive tasks.


## Usage

Using `SmallMoleculeMultiView` requires [{{ repo_url }}]({{ repo_url }})

## Installation
Follow these steps to set up the `biomed.multi-view` codebase on your system.

### Prerequisites
* Operating System: Linux or macOS
* Python Version: Python 3.11
* Conda: Anaconda or Miniconda installed
* Git: Version control to clone the repository


### Step 1: Set up the project directory
Choose a root directory where you want to install biomed.multi-view. For example:

```bash
export ROOT_DIR=~/biomed-multiview
mkdir -p $ROOT_DIR
```

### Step 2: Install anaconda3
If you have Anconda in your system you can skip this step.
``` bash
cd $ROOT_DIR
# Download the Anaconda installer
wget https://repo.anaconda.com/archive/Anaconda3-2023.03-Linux-x86_64.sh

# Run the installer
bash Anaconda3-2023.03-Linux-x86_64.sh
# After installation, initialize Conda:
source activate $ROOT_DIR/anaconda3/bin/activate
```

#### Step 3: Create and activate a Conda environment
```bash
conda create -y python=3.11 --prefix $ROOT_DIR/envs/biomed-multiview
```
Activate the environment:
```bash
conda activate $ROOT_DIR/envs/biomed-multiview
```

#### Step 4: Clone the repository
Navigate to the project directory and clone the repository:
```bash
mkdir -p $ROOT_DIR/code
cd $ROOT_DIR/code

# Clone the repository using HTTPS
git clone https://github.com/BiomedSciAI/biomed-multi-view.git

# Navigate into the cloned repository
cd biomed.multi-view
```
Note: If you prefer using SSH, ensure that your SSH keys are set up with GitHub and use the following command:
```bash
git clone git@github.com:BiomedSciAI/biomed-multi-view.git
```

#### Step 5: Install package dependencies
Install the package in editable mode along with development dependencies:
``` bash
pip install -e .['dev']
```
Install additional requirements:
``` bash
pip install -r requirements.txt
```

#### Step 6: macOS-Specific instructions (Apple Silicon)
If you are using a Mac with Apple Silicon (M1/M2/M3) and the zsh shell, you may need to disable globbing for the installation command:

``` bash
noglob pip install -e .[dev]
```
Install macOS-specific requirements optimized for Apple’s Metal Performance Shaders (MPS):
```bash
pip install -r requirements-mps.txt
```

#### Step 7:  Installation verification (optional)
Verify that the installation was successful by running unit tests

```bash
python -m unittest bmfm_sm.tests.all_tests
```


### Get embedding example

A simple example:
```python
# Necessary imports
from bmfm_sm.api.smmv_api import SmallMoleculeMultiViewModel
from bmfm_sm.core.data_modules.namespace import LateFusionStrategy

# Load Model
model = SmallMoleculeMultiViewModel.from_pretrained(
    LateFusionStrategy.ATTENTIONAL,
    model_path="{{ hf_model_path }}",
    huggingface=True
)

# Load Model and get embeddings for a molecule
example_smiles = "CC(C)CC1=CC=C(C=C1)C(C)C(=O)O"
example_emb = SmallMoleculeMultiViewModel.get_embeddings(
    smiles=example_smiles,
    model_path="{{ hf_model_path }}",
    huggingface=True,
)
print(example_emb.shape)
```

### Get prediction example

``` python
from bmfm_sm.api.smmv_api import SmallMoleculeMultiViewModel
from bmfm_sm.api.dataset_registry import DatasetRegistry

# Initialize the dataset registry
dataset_registry = DatasetRegistry()

# Example SMILES string
example_smiles = "CC(C)CC1=CC=C(C=C1)C(C)C(=O)O"

# Get dataset information for dataset
ds = dataset_registry.get_dataset_info("{{ hf_dataset }}")

# Load the finetuned model for the dataset
finetuned_model_ds = SmallMoleculeMultiViewModel.from_finetuned(
    ds,
    model_path="{{ hf_ft_model_path }}",
    inference_mode=True,
    huggingface=True
)

# Get predictions
prediction = SmallMoleculeMultiViewModel.get_predictions(
    example_smiles, ds, finetuned_model=finetuned_model_ds
)

print("Prediction:", prediction)
```

##### Output:
```bash
Prediction: {'prediction': [0.85], 'label': None}
```

For more advanced usage, see our detailed examples at: {{ repo_url }}


## Citation

If you found our work useful, please consider to give a star to the repo and cite our paper:
```
@article{TBD,
  title={TBD},
  author={IBM Research Team},
  jounal={arXiv preprint arXiv:TBD},
  year={2024}
}
```
