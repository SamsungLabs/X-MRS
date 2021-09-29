[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/)

# Cross-modal Retrieval and Synthesis (X-MRS): Closing the modality gap in shared representation learning

This project provides the code associated with the paper:

*"Ricardo Guerrero, Hai Pham Xuan, and Vladimir Pavlovic. Cross-modal Retrieval and Synthesis (X-MRS): Closing the modality gap in shared representation learning. ACM Multimedia 2021."*


#
## Installation
Environment and requirements setup:

* `conda create -n saicc-xmrs python=3.8`
* `source activate saicc-xmrs`
* `pip install -r requirements.txt`


#
## Data preparation
Download and copy the following contents from the [Recipe1M dataset](http://pic2recipe.csail.mit.edu/) into \<path-to-project\>/data/Recipe1M:

* layer1.json, layer2.json, det_ingrs.json
* train_keys.pkl, val_keys.pkl, test_keys.pkl
* classes1M.pkl

Run `python translate_text.py <LANGUAGE>` to generate translated versions of "layer1.json". LANGUAGE can be one of the following:
* 'ru' -> English to Russian
* 'de' -> English to German
* 'fr' -> English to French
* 'ru-en' -> Russian to English (used in back-translation)
* 'de-en' -> German to English (used in back-translation)

Run `python create_lmdb.py` to generate train, val and test LMDB files.


#
## Train encoders:
To train encoder for the joint space embedding, run `python train.py`

Results are stored in \<path-to-project>/tensorboard/\<timestamp>

For a complete list or arguments see `python train.py --help`

#
## Test retrieval
Run  `python test.py --model_init_path=<path-to-project>/tensorboard/<timestamp>/models/<model>.pth.tar`

Results are printed on screen.


#
## Train synthesis:
First train ecoders and store embeddings, i.e. run `python test.py --model_init_path=<path-to-project>/tensorboard/<timestamp>/models/<model>.pth.tar`, this will create the necesary pre-calculated embedding files as well as extract the image encoder part from FoodSpaceNet. Each data split will have its own corresponding embedding file. Note that test.py needs to run through all data splits. Then to train synthesis

run

`python GAN_train.py --img_path [IMAGE_DIR] --data_path [DATA_DIR] --encoder_dir [ENCODER_PATH]`

where [IMAGE_DIR] and [DATA_DIR] are the same as in training the retrieval model.

#
## Test synthesis

`python GAN_test.py --img_path [IMAGE_DIR] --data_path [DATA_DIR] --encoder_dir [ENCODER_PATH] -trained_G_model_path [PATH_TO_GENERATOR_MODEL]`

#
## NOTES
* *X-MRS (ACM-MM'21)* also uses translations into Korean done by a propietary system not included here.
* "All the third-party libraries (python, cython, pytorch, torchvision, numpy, opencv-python, transformers, tensorboard, lmdb, pillow, omegaconf, hydra-core, subword-nmt, fastBPE, tqdm and simplejson) are owned by their respective authors, and must be used according to their respective licenses."
