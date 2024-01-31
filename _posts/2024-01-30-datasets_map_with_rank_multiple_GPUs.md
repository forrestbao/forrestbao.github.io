---
layout: post
title: "How to do `dataset.Datasets.map()` with `rank` on multiple GPUs"
date: 2024-01-30
author: Forrest Sheng Bao/鮑盛
---

I just spent 2 hours on following the official [example](https://huggingface.co/docs/datasets/main/en/process#multiprocessing) of HuggingFace's `datasets` library to do `dataset.Datasets.map()` with `rank` on multiple GPUs. It did not work as complained by many others on the Internet: [datasets issue #6186](https://github.com/huggingface/datasets/issues/6186), [datasets PR #6415](https://github.com/huggingface/datasets/pull/6415), and [datasets PR #6550](https://github.com/huggingface/datasets/pull/6550). Many wanted complete working code. 

After digging information here and there from the discussions (especially [this reply](https://github.com/huggingface/datasets/pull/6415#issuecomment-1872269530) in PR#6415), I finally got it working. So allow me to write a complete example down here first. Hopefully it can help others before the official documents get updated. 

Known issues: 
* The code does not work well inside a Jupyter notebook. I haven't figured out why. 
* It is very important to leave the last two lines under `if __name__ == '__main__':` as they are. Otherwise, the code will not work. I am still looking how to fix that because in my case, I have other code to run after the `map()` function.

Here is the beef: 

```python

#%% 
import torch 
from datasets import Dataset, load_dataset
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM 

# get an example dataset
a_dataset = load_dataset("snli", split="test") # 10k rows 

# get an example model and its tokenizer 
model_identifier = "facebook/nllb-200-distilled-600M"
tokenizer = AutoTokenizer.from_pretrained(model_identifier)
model = AutoModelForSeq2SeqLM.from_pretrained(model_identifier)

#%% 
def unit_translate(examples: Dataset, rank: int): 
    # the function to be mapped onto dataset 
        
    device = f"cuda:{rank}"
    
    model.to(device) # this sounds weird though. Why moving the model again and again along with every batch of data? 
    inputs = tokenizer(examples['hypothesis'], return_tensors="pt").to(device)
    
    outputs = model.generate(**inputs) # this is buggy for NLLB because BOS token is not set but it is enough to show the idea of mapping with rank
    
    examples["new_column"] = tokenizer.batch_decode(outputs, skip_special_tokens=True)
    
    return examples 
    
#%% 
from multiprocess import set_start_method

if __name__ == '__main__':
        
    set_start_method('spawn')

    mapped_dataset = a_dataset.map(
        unit_translate, 
        with_rank=True,
        num_proc=torch.cuda.device_count(), # use all GPUs on the machine
        batched=True, # optional
        batch_size=12, # optional 
    )

```