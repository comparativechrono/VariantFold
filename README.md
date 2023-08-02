# VariantFold
A repository for work tailoring alphafold2 to generate variant protein models. We call this project "VariantFold". 

The repository currently contains 2 versions of VariantFold available; `VariantFold_dev` - a full version with various tests for dataset/GCN model validation and `VariantFold` - a simple version demonstrating how the final product could look like. 

(`VariantFold_v1` is an **OLD** version of the script.)    
  
### Aim  
Create a tool that will model pathogenic variants in proteins and use this information to inform classification of VUS's. 

We aim to achieve that by leveraging protein structure predictions from AlphaFold and utilising a publicly available Graph Neural Network (GNN) to classify VUS based on the standardised ACMG-AMP variant classification system.




### Workflow:
The figure below is demonstrating the workflow of a VariantFold tool:
![image](https://github.com/comparativechrono/VariantFold/assets/128153504/746882ed-a1c9-4101-ab9b-8b11e232c74c)

In short, VariantFold requires an input of a variant dataset from ClinVar database. The data is then pre-processed in Step 1 using the Mutator script, followed by passing the processed data to ColabFold in Step 2. ColabFold generates 3D protein models in .pdb format for each variant. In Step 3, the generated models are transformed into a dataset, which is then converted into PyTorch Geometric graphs in Step 4 using the Converter script. The prepared dataset is used for model training and testing in Step 5, where a Graph Convolutional Network (GCN) model is employed. In Step 6, the model's performance is evaluated and can be utilized for Variant of Unknown Significance (VUS) classification. 

**Step 1. Mutator**

The first part of the script creates the required directories and installs the necessary modules for the VariantFold application. It also prompts you to enter the name of your protein and download its amino acid sequence from ClinVar (An option to manually input the sequence is also available). After that, you will need to download a variant dataset from the ClinVar database, following the instructions provided in the script.

Once you have obtained your original protein sequence and both lists of benign and pathogenic variants, you can initiate ColabFold. The Mutator script will extract the necessary information from ClinVar datasets and update the protein sequence for each variant. These updated sequences will then be fed into ColabFold for structure prediction.

**Step 2. ColabFold**

ColabFold will process each variant one by one and generate 3D models for each variant in the dataset. It is important to note that generating 3D models can be time-consuming; for instance, generating around 100 models for the VHL gene of 213 amino acids took approximately 7 hours. Additionally, please consider Google Colab's maximum runtime limit of 12 hours for the free version of the platform. For larger genes, you may need to split your data and run it separately multiple times to accommodate the runtime constraints.

**Step 3. Dataset assembly**

When ColabFold has finished generating your variant structure models, you can proceed to dataset preparation. Part II of VariantFold includes a script that will create new directories and move the best-fit model of each variant into the library folder, which will serve as the storage directory for your dataset.

Currently, the dataset assembly has been combined with a converter script.

**Step 4. Converter**

The main task of the converter script is to transform the .pdb files from libraries into file formats acceptable by PyTorch Geometrics (PyG) GCNs. Currently, it accomplishes this in two steps. First, it converts the .pdb files into DGL (Deep Graph Library) format graphs by extracting the atom coordinates from the pdb files. Then, it converts these DGL graphs into PyTorch Geometrics objects, which can be later fed into GCNs for further processing.   

**Step 5. Machine Learning**

In this step, the converted graphs are loaded into the GCN model using PyG DataLoader module. The data is split into 80% for the training dataset and 20% for the testing dataset. The model is trained using the training dataset and then tested with the testing set. The accuracy of the prediction can then be assessed using confusion matrices.

**Step 6. Data Analysis**

The performance of the model can be assessed using various tools and if the results are acceptable VUS variant dataset can be loaded for classification.

### Development state  
Currently in active development.  
Version 2 is available on this page. July 2023.   
