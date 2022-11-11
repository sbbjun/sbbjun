# Homework 2 ADL NTU by p08922002

## Environment
- requirements.in
```
torch evaluate datasets sentencepiece accelerate protobuf
```

## Input format
- context example
```
[
  "第一個句子", 
  "第二個句子",
  ...
]
```
- QA example
```
[
  {
    "id": "593f14f960d971e294af884f0194b3a7",
    "question": "舍本和誰的數據能推算出連星的恆星的質量？",
    "paragraphs": [
      2018,
      6952,
      8264,
      836
    ],
    "relevant": 836,
    "answer": {
      "text": "斯特魯維",
      "start": 108
    }
  },
  ...
]
```

## Part 1: choose question-related paragraph
### Preprocessing
- convert_swag.py usage
```
convert_swag.py [context-input.json] [qa-input.json]

output: swag_[qa-input.json]
``` 
- preprocessing commands
```shell
convert_swag.py context.json train.json
convert_swag.py context.json valid.json
convert_swag.py context.json test.json
```
### Training
- program parameters
```shell
run_swag_no_trainer.py -h
```
- generate final model by run_swag_no_trainer.py
```shell
run_swag_no_trainer.py \
  --model_name_or_path bert-base-chinese \
  --train_file swag_train.json \
  --validation_file swag_valid.json \
  --output_dir model_choice \
  --max_length 512 \
  --per_device_train_batch_size 1 \
  --per_device_eval_batch_size 1 \
  --gradient_accumulation_steps 2 \
  --num_train_epochs 1 \
  --learning_rate 3e-5
```
### Predict
```shell
run_swag_no_trainer.py \
  --model_name_or_path model_choice \
  --test_file swag_test.json \
  --output_dir predict \
  --max_length 512
```
- output: \[output-dir\]/predict_swag.json



## Part 2: QA system
### Preprocessing
- convert_squad.py usage
```
convert_squad.py [context-input.json] [qa-input.json] ([predict-from-part-1.json])

output: squad_[qa-input.json]
```
- preprocessing commands
```
  convert_squad.py context.json train.json
  convert_squad.py context.json valid.json
  convert_squad.py context.json test.json predict/predict_swag.json
``` 
### Training
- program parameters
```shell
run_squad_no_trainer.py -h
```
- generate final model by run_squad_no_trainer.py
```shell
run_qa_no_trainer.py \
  --model_name_or_path ckiplab/albert-base-chinese \
  --train_file squad_train.json \
  --validation_file squad_valid.json \
  --output_dir model_squad_albert \
  --max_seq_length 512 \
  --per_device_train_batch_size 1 \
  --per_device_eval_batch_size 1 \
  --gradient_accumulation_steps 2 \
  --num_train_epochs 3 \
  --learning_rate 3e-5
```
### Predict
```shell
run_qa_no_trainer.py \
  --model_name_or_path model_squad_albert \
  --test_file squad_test.json \
  --do_predict \
  --output_dir predict \
  --max_seq_length 512
```
- output: \[output-dir\]/predict_squad.csv
  
  
