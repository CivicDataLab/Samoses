# Samoses
[![License: MIT](https://img.shields.io/badge/License-MIT-lightgrey.svg)](https://github.com/CivicDataLab/Samoses/blob/master/LICENSE)

Wrapper for Moses


## Setup

### Installation of Moses

Clone the Moses repository [here](https://github.com/moses-smt/mosesdecoder.git)

Follow the instructions [here](http://www.statmt.org/moses/?n=Development.GetStarted) to install Moses

## DATASET

Find the nyu corpus for English-Hindi [here](https://drive.google.com/drive/u/2/folders/16Yjgq_osF2j-Ws0vWBp2i59kA094nFWP)

## Pipeline Creation Language (PCL)

PCL is a Domain Specific Language to construct non-recurrent software pipelines.
We are using PCL to build pipeline for the Statistical Machine Translation.


## RUN

```
sudo nice experiment.perl -config config_nyu -exec
```

The phrase table generated is `phrase-table.1.gz` and not `phrase-table.1`. Convert the phrase table to the compact format using:

```
sudo nice $WORKSPACE/mosesdecoder/bin/processPhraseTableMin -in $WORKSPACE/experiment/model/phrase-table.1.gz -nscores 4 -out $WORKSPACE/experiment/model/phrase-table
```

Similarly convert the reodering table:

```
sudo nice $WORKSPACE/mosesdecoder/bin/processLexicalTableMin -in $WORKSPACE/experiment/model/reordering-table.1.wbe-msd-bidirectional-fe.gz -out $WORKSPACE/experiment/model/reordering-table
```

Modify `moses.tuned.ini.1` under `tuning` directory with:

```
# PhraseDictionaryMemory name=TranslationModel0 num-features=4 path=/home/ubuntu/mosesdecoder/experiment/model/phrase-table.1 input-factor=0 output-factor=0

PhraseDictionaryCompact name=TranslationModel0 num-features=4 path=/home/ubuntu/mosesdecoder/experiment/model/phrase-table.minphr input-factor=0 output-factor=0

#LexicalReordering name=LexicalReordering0 num-features=6 type=wbe-msd-bidirectional-fe-allff input-factor=0 output-factor=0 path=/home/ubuntu/mosesdecoder/experiment/model/reordering-table.1.wbe-msd-bidirectional-fe.gz

LexicalReordering name=LexicalReordering0 num-features=6 type=wbe-msd-bidirectional-fe-allff input-factor=0 output-factor=0 path=/home/ubuntu/mosesdecoder/experiment/model/reordering-table
```

### Translate

```
$WORKSPACE/mosesdecoder/bin/moses -f $WORKSPACE/experiment/tuning/moses.tuned.ini.1
```
