# VariantFold
A repository for work tailoring alphafold2 to generate variant protein models. We call this project "VariantFold". 

The repository currently contains 2 versions of VariantFold available; `VariantFold_dev` - a full version with various tests for dataset/GCN model validation and `VariantFold` - a simple version demonstrating how the final product could look like.    
  
### Aim  
Create a tool that will model pathogenic variants in proteins and use this information to inform classification of VUS's. 

We aim to achieve that by leveraging protein structure predictions from AlphaFold and utilising a publicly available Graph Neural Network (GNN) to classify VUS based on the standardised ACMG-AMP variant classification system.




### Workflow:
The figure below is demonstrating the workflow of a VariantFold tool:
![image](https://github.com/comparativechrono/VariantFold/assets/128153504/746882ed-a1c9-4101-ab9b-8b11e232c74c)

In short, VariantFold requires an input of a variant dataset from ClinVar database. The data is then pre-processed in Step 1 using the Mutator script, followed by passing the processed data to ColabFold in Step 2. ColabFold generates 3D protein models in .pdb format for each variant. In Step 3, the generated models are transformed into a dataset, which is then converted into PyTorch Geometric graphs in Step 4 using the Converter script. The prepared dataset is used for model training and testing in Step 5, where a Graph Convolutional Network (GCN) model is employed. In Step 6, the model's performance is evaluated and can be utilized for Variant of Unknown Significance (VUS) classification. 
  
### Development state  
Currently in active development.  
Version 1 available on this page. March 2023.   
