# Homework 3 ADL NTU by p08922002

## Environment
- requirements.in
```
torch rouge ckiptagger
```

- install
```
pip install -e tw_rouge
```

## Dataset format
- example: input.jsonl
```
{"date_publish": "2016-06-04 00:00:00", "title": "藝人張魁的兒子計誘友人被黑道擄走 妨害自由起訴", "source_domain": "udn.com", "maintext": "張魁（中）。聯合報系資料照／記者程宜華攝影\n藝人張魁的兒子、歌手張峰奇哥哥張覺引捲入黑道討債風波，（下略）", "split": "train", "id": "5"}
{"date_publish": "2016-10-21 00:00:00", "title": "喪屍確實存在？揭秘海地死人復活真相 哈佛雜誌分析案例", "source_domain": "udn.com", "maintext": "提起喪屍（Zombie），人們可能最先想起近日上映的《屍殺半島》劇情，腦海浮現它們肆意咬食人類的畫面。（下略）", "split": "train", "id": "6"}
...
```
- example(for predict): input_eval.jsonl
```
{"date_publish": "2021-01-14 00:00:00", "maintext": "Anker在此次CES 2021中，宣布以旗下Soundcore品牌推出新款真無線藍牙耳機Liberty Air 2 Pro，同時也確定引進台灣市場。\nLiberty Air 2 Pro本身採入耳式耳塞設計，（下略）", "id": "21710"}
{"date_publish": "2021-01-14 00:00:00", "maintext": "如同其他品牌選擇在CES 2021公布新款Chromebook產品，華碩也宣布推出換上Intel第11代Core處理器的Chromebook Flip CX5，並且藉由15.6吋機身設計與2 in 1使用模式擴展企業應用需求。\nChromebook Flip CX5採窄邊框、（下略）", "id": "21712"}
...
```

- example output
```
{"title": "Anker新款真無線藍牙耳機Liberty Air 2 Pro 引進台灣市場", "id": "21710"}
{"title": "華碩打造對應軍規防護與2 in 1設計的15.6吋Chromebook", "id": "21712"}
...
```


## Training
### Usage
- hw3.py
```
usage
``` 

### Model config
- Model
-- Architecture: MT5ForConditionalGeneration
-- Tokenizer: MT5Tokenizer
-- Optimizer: Adafactor
- Configuration
-- learning-rate: 3e-4
-- max-len: 256
-- max-len-target: 64
-- batch-size: 3
-- gradient accumulation: 4
```
{
  "_name_or_path": "google/mt5-small",
  "architectures": [
    "MT5ForConditionalGeneration"
  ],
  "d_ff": 1024,
  "d_kv": 64,
  "d_model": 512,
  "decoder_start_token_id": 0,
  "dense_act_fn": "gelu_new",
  "dropout_rate": 0.1,
  "eos_token_id": 1,
  "feed_forward_proj": "gated-gelu",
  "initializer_factor": 1.0,
  "is_encoder_decoder": true,
  "is_gated_act": true,
  "layer_norm_epsilon": 1e-06,
  "model_type": "mt5",
  "num_decoder_layers": 8,
  "num_heads": 6,
  "num_layers": 8,
  "pad_token_id": 0,
  "relative_attention_max_distance": 128,
  "relative_attention_num_buckets": 32,
  "tie_word_embeddings": false,
  "tokenizer_class": "T5Tokenizer",
  "torch_dtype": "float32",
  "transformers_version": "4.24.0",
  "use_cache": true,
  "vocab_size": 250112
}
```



## Predict (generate summary)
### Usage
- hw3_predict.py
```
usage: hw3_predict.py [-h] -m MODEL_PATH [-M MODEL_TOKEN_PATH] -j JSONL [-J JSONL_OUT] [-l MAX_LEN] [-L MAX_LEN_TARGET] [-b BATCH_SIZE_PREDICT] [-gs]
                      [-ges] [-gng NO_REPEAT_NGRAM_SIZE] [-gnb NUM_BEAMS] [-gt TEMPERATURE] [-gtk TOP_K] [-gtp TOP_P]

options:
  -h, --help            show this help message and exit
  -m MODEL_PATH, --model-path MODEL_PATH
                        help
  -M MODEL_TOKEN_PATH, --model-token-path MODEL_TOKEN_PATH
  -j JSONL, --jsonl JSONL
  -J JSONL_OUT, --jsonl-out JSONL_OUT
  -l MAX_LEN, --max-len MAX_LEN
  -L MAX_LEN_TARGET, --max-len-target MAX_LEN_TARGET
  -b BATCH_SIZE_PREDICT, --batch-size-predict BATCH_SIZE_PREDICT
  -gs, --do_sample
  -ges, --early_stopping
  -gng NO_REPEAT_NGRAM_SIZE, --no_repeat_ngram_size NO_REPEAT_NGRAM_SIZE
  -gnb NUM_BEAMS, --num_beams NUM_BEAMS
  -gt TEMPERATURE, --temperature TEMPERATURE
  -gtk TOP_K, --top_k TOP_K
  -gtp TOP_P, --top_p TOP_P
```

### Configuration
- Configuration
-- max-len-target: 64
-- epoch: 50

### Generate strategies
- hw3_predict.py -b 128 
- hw3_predict.py -b 128 -gng 2 
- hw3_predict.py -b 32 --num_beams 4 -ges 
- hw3_predict.py -b 16 --num_beams 8 -ges 
- hw3_predict.py -b 128 --do_sample --top_k 16 
- hw3_predict.py -b 128 --do_sample --top_k 32 
- hw3_predict.py -b 128 --do_sample --top_k 64 
- hw3_predict.py -b 128 --do_sample --top_k 128 
- hw3_predict.py -b 128 --do_sample --top_p 0.92 --top_k 0 
- hw3_predict.py -b 128 --do_sample --top_p 0.8 --top_k 0 
- hw3_predict.py -b 128 --do_sample --top_p 0.64 --top_k 0 
- hw3_predict.py -b 128 -gng 2 --do_sample --top_p 0.64 --top_k 0 
- hw3_predict.py -b 128 --do_sample --temperature 0.75 --top_p 0.64 --top_k 0 
- hw3_predict.py -b 128 --do_sample --temperature 0.25 --top_p 0.64 --top_k 0 


## Evaluate
### Usage
- eval.py
```
usage: eval.py [-h] [-r REFERENCE] [-s SUBMISSION]

options:
  -r REFERENCE, --reference REFERENCE
  -s SUBMISSION, --submission SUBMISSION  
```



