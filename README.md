<!--
# Homework 1 ADL NTU by p08922002

## Environment
- requirements.in
```
torch tensorflow seqeval tqdm numpy pandas scikit-learn
```

## Part 1: Intent Classification
### Preprocessing
- preprocess_intent.py -h
```
usage: preprocess_intent.py [-h] [--data_dir DATA_DIR]
                            [--glove_path GLOVE_PATH] [--rand_seed RAND_SEED]
                            [--output_dir OUTPUT_DIR]
                            [--vocab_size VOCAB_SIZE]

options:
  -h, --help            show this help message and exit
  --data_dir DATA_DIR   Directory to the dataset.
  --glove_path GLOVE_PATH
                        Path to Glove Embedding.
  --rand_seed RAND_SEED
                        Random seed.
  --output_dir OUTPUT_DIR
                        Directory to save the processed file.
  --vocab_size VOCAB_SIZE
                        Number of token in the vocabulary 
``` 
- final model
```shell
preprocess_intent.py --data_dir ./data/intent/ --glove_path ./glove.840B.300d.txt --output_dir ./cache/intent --vocab_size 10000
```
### Training
- train_intent.py -h
```
usage: train_intent.py [-h] [--data_dir DATA_DIR] [--cache_dir CACHE_DIR]
                       [--ckpt_dir CKPT_DIR] [--comment COMMENT]
                       [--max_len MAX_LEN] [--hidden_size HIDDEN_SIZE]
                       [--num_layers NUM_LAYERS] [--dropout DROPOUT]
                       [--bidirectional BIDIRECTIONAL] [--lr LR]
                       [--batch_size BATCH_SIZE] [--device DEVICE]
                       [--num_epoch NUM_EPOCH]

options:
  -h, --help            show this help message and exit
  --data_dir DATA_DIR   Directory to the dataset.
  --cache_dir CACHE_DIR
                        Directory to the preprocessed caches.
  --ckpt_dir CKPT_DIR   Directory to save the model file.
  --comment COMMENT
  --max_len MAX_LEN
  --hidden_size HIDDEN_SIZE
  --num_layers NUM_LAYERS
  --dropout DROPOUT
  --bidirectional BIDIRECTIONAL
  --lr LR
  --batch_size BATCH_SIZE
  --device DEVICE       cpu, cuda, cuda:0, cuda:1
  --num_epoch NUM_EPOCH
```
- final model
```shell
python train_intent.py --comment GRU --hidden-size 128 --num_layers 1 --num_epoch 500 --max_len 30 --lr 0.1
```


## Part 2: Slot Tagging
### Preprocessing
- preprocess_slot.py -h
```
# same as Part 1: preprocess_intent.py
``` 
- final model
```shell
preprocess_slot.py --data_dir ./data/intent/ --glove_path ./glove.840B.300d.txt --output_dir ./cache/intent --vocab_size 10000
```
### Training
- train_slot.py -h
```
# same as Part 1: train_intent.py
```
- final model
```shell
python train_slot.py --comment GRU --hidden-size 768 --num_layers 1 --num_epoch 200 --max_len 40 --lr 0.1
```
-->
