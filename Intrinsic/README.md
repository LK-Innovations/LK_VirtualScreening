<!--
*** Thanks for checking out this README Template. If you have a suggestion that would
*** make this better, please fork the repo and create a pull request or simply open
*** an issue with the tag "enhancement".
*** Thanks again! Now go create something AMAZING! :D
-->




<!-- PROJECT LOGO -->
<br />
<p align="center">

  <h3 align="center">AI-enabled discovery of small molecules targeting complementary pathways for hair follicle rejuvenation</h3>

  <p align="center">
    Supporting code for the paper
  </p>
</p>



<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About the Code](#about-the-code)
* [Running the code](#running-the-code)
  * [Checkpoint files](#checkpoint-files)
  * [Usage](#usage)
* [Contact](#contact)



<!-- ABOUT THE PROJECT -->
## About the Code

The files in this repository contain training data and final Chemprop checkpoints for the models described in the paper "AI-enabled discovery of small molecules targeting complementary pathways for hair follicle rejuvenation". The code requires <a href="https://github.com/chemprop/chemprop">ChemProp</a> and Python with the appropriate packages installed. 
<ul>
<li><a href="https://github.com/chemprop/chemprop">Chemprop</a> (commit 2bcfcfe47b704d73ce7c48f177254fedeb5c0316 was used)</li>
<li><a href="https://www.rdkit.org/">RDKit</a> (version 2023.09.6 was used)</li>
</ul>
For more details on Chemprop, see <a href="https://github.com/felixjwong/protocol">here</a>.

<!-- GETTING STARTED -->
## Running the code

### Chemprop model training

Models were trained as detailed in the paper; for more context, see <a href="https://github.com/felixjwong/protocol">here</a>. Key files used for model training are as follows:


<ul>
<li>requirements.txt: A Python requirements file detailing all the package dependencies needed to successfully execute all commands in this notebook.
</li>
<li>
hyperparameters.json: A JSON file containing key hyperparameters specifying the architecture of the Chemprop model used. By default, the parameters used are: depth=2, dropout=0.0, ffn_num_layers=3, and hidden_size=600, as detailed further in the Methods section of the main text.
</li>
<li>{hdpc}_preds: Folders containing the SMILES strings of compounds for which Chemprop predictions of activity were made.
</li>
</ul>

### Checkpoint files

In each of the folders entitled {hdpc}, there are 10 Chemprop checkpoint files, which correspond to the ensemble of 10 final Chemprop models used for each cell type in the paper. These checkpoint files were generated as described <a href="https://github.com/felixjwong/protocol">previously</a>.


