
## Dataset:
#### Rocstories: https://github.com/snigdhac/StoryComprehension_EMNLP/tree/master/Dataset/RoCStories


## BART:
The dataset and code we used is in seq2seq folder.

The basic command for training bart is:
```
python finetune_trainer.py data_dir dataset/key2story --learning_rate=1e-4 --warmup_steps 500  \
--gradient_accumulation_steps 8 --num_train_epochs 200  --output_dir epoch_200_lr_1e-4_key2story \
--max_source_length=512 --max_target_length=128 --do_train --overwrite_output --fp16 \
--model_name_or_path facebook/bart-base --per_device_train_batch_size 8 --save_total_limit 3
```

The basic command for evaluating is:
```
python run_eval.py epoch_200_lr_1e-5_line2story/checkpoint-245000 \
dataset/line2story/test10.source epoch_200_lr_1e-5_line2story/results10 --fp16 --bs 32
```
## Plan-and-Write:
The basic command for training the title to storyline model is:
```
python pytorch_src/main.py --cuda --batch_size 20 --train-data rocstory_plan_write/ROCStories_all_merge_tokenize.titlesepkey.train --valid-data rocstory_plan_write/ROCStories_all_merge_tokenize.titlesepkey.dev --test-data rocstory_plan_write/ROCStories_all_merge_tokenize.titlesepkey.test --dropouti 0.4 --dropouth 0.25 --seed 141 --epoch 500 --emsize 200 --nhid 300 --save model1.pt --vocab-file vocab.pkl
```

The basic command for training the storyline to story model is:
```
python pytorch_src/main.py --cuda --batch_size 20 --train-data rocstory_plan_write/ROCStories_all_merge_tokenize.titlesepkeysepstory.train --valid-data rocstory_plan_write/ROCStories_all_merge_tokenize.titlesepkeysepstory.dev --test-data rocstory_plan_write/ROCStories_all_merge_tokenize.titlesepkeysepstory.test --dropouti 0.4 --dropouth 0.25 --seed 141 --epoch 100 --emsize 1000 --nhid 1000 --save model2.pt --vocab-file vocab.pkl
```

The basic command for generating storyline / story planning is:
```
python pytorch_src/generate.py --checkpoint model1.pt  --vocab vocab.pkl --task cond_generate --conditional-data rocstory_plan_write/ROCStories_all_merge_tokenize.title.test --cuda --temperature 0.5 --sents 100 --dedup --outf OUTPUT_FILE
```

The basic command for story generatiion is:
```
python pytorch_src/generate.py --vocab vocab.pkl --checkpoint model2.pt --task cond_generate --conditional-data OUTPUT_FILE --cuda --temperature 0.3 --sents 100 --outf generation_results/cond_generated_keywords_test_e500_h1000_edr0.4_hdr0.1_t0.15.txt_lm_e1000_h1500_edr0.2_hdr0.1_t0.3.txt
```
