## Google Doc Link:
https://docs.google.com/document/d/1Lb-sFeqdVITwLK2e73MgK_pP85W05VcUY5S65OL2WgM/edit

## Dataset:
#### Rocstories: https://github.com/snigdhac/StoryComprehension_EMNLP/tree/master/Dataset/RoCStories

## Code:
Plan-and-Write: <br>
* Code: https://bitbucket.org/VioletPeng/language-model/src/master/ <br>
* Paper: https://arxiv.org/pdf/1811.05701.pdf

Content Planning for Neural Story Generation with Aristotelian Rescoring: <br>
* Code: https://github.com/PlusLabNLP/story-gen-BART <br>
* Paper: https://aclanthology.org/2020.emnlp-main.351.pdf <br>

Automatic Story Generation: Challenges and Attempts: <br>
* Paper: https://aclanthology.org/2021.nuse-1.8.pdf
(A good paper helps you get to know Story Generation)

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
