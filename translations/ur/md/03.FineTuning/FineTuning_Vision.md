<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "a5a67308d3b2c5af97baf01067c6f007",
  "translation_date": "2025-07-17T08:35:42+00:00",
  "source_file": "md/03.FineTuning/FineTuning_Vision.md",
  "language_code": "ur"
}
-->
# Phi-3.5-vision finetuning recipe

یہ huggingface لائبریریز کے ذریعے Phi-3.5-vision finetuning کی سرکاری حمایت ہے۔  
براہ کرم مندرجہ ذیل کمانڈز چلانے سے پہلے کوڈ ڈائریکٹری [vision_finetuning](../../../../code/03.Finetuning/vision_finetuning) میں `cd` کریں۔

## Installation

```bash
# create a new conda environment
conda create -n phi3v python=3.10
conda activate phi3v

# install pytorch
conda install pytorch==2.1.2 torchvision==0.16.2 torchaudio==2.1.2 pytorch-cuda=12.1 -c pytorch -c nvidia

# other libraries needed to run the example code
pip install -r requirements.txt

# (optional) flash attention -- Ampere+ GPUs (e.g., A100, H100)
pip install ninja
MAX_JOBS=32 pip install flash-attn==2.4.2 --no-build-isolation

# (optional) QLoRA -- Turing+ GPUs (e.g., RTX 8000)
pip install bitsandbytes==0.43.1
```

## Quick start

ہم دو مثالیں فراہم کرتے ہیں، ایک DocVQA کے لیے اور ایک hateful meme classification کے لیے۔

کم از کم ہارڈویئر جس پر ٹیسٹ کیا گیا ہے: 4x RTX8000 (ہر GPU کے لیے 48GB RAM)

```bash
# minimal script on a mini-train split of DocVQA
torchrun --nproc_per_node=4 finetune_hf_trainer_docvqa.py
```

Phi-3.5-vision اب باضابطہ طور پر ملٹی-امیج ان پٹس کی حمایت کرتا ہے۔ یہاں NLVR2 کے لیے finetuning کی ایک مثال ہے۔

```bash
torchrun --nproc_per_node=8 finetune_hf_trainer_nlvr2.py
```

## Usage guide

ہارڈویئر کے مطابق، صارفین مختلف finetuning حکمت عملیاں منتخب کر سکتے ہیں۔ ہم  
full-finetuning (Deepspeed Zero-2 کے ساتھ) کی حمایت کرتے ہیں جس میں بصری پیرامیٹرز کو فریز کرنا اختیاری ہے، اور LoRA (جس میں 4bit QLoRA شامل ہے) بھی۔  
عمومی طور پر، ہم flash attention اور bf16 کے ساتھ full finetuning استعمال کرنے کی سفارش کرتے ہیں جہاں ممکن ہو۔

### اپنے کسٹم ڈیٹاسیٹ کو مطلوبہ فارمیٹ میں تبدیل کرنے کے لیے رہنمائی

ہم ایک کم از کم ویڈیو کلاسیفیکیشن ڈیٹاسیٹ (UCF-101 کا ایک ذیلی حصہ) کو ایک مکمل مثال کے طور پر استعمال کرتے ہیں تاکہ دکھایا جا سکے کہ آپ اپنے کسٹم ڈیٹاسیٹ کو مطلوبہ فارمیٹ میں کیسے تبدیل کریں اور اس پر Phi-3.5-vision کو finetune کریں۔

```bash
# convert data
python convert_ucf101.py --out_dir /path/to/converted_ucf101

# training
torchrun --nproc_per_node=4 finetune_hf_trainer_ucf101.py --data_dir /path/to/converted_ucf101
```

تبدیل شدہ ڈیٹا اس طرح نظر آئے گا:

```bash
> tree --filelimit=10 /path/to/converted_ucf101
/path/to/converted_ucf101
├── images
│   ├── test
│   │   ├── ApplyEyeMakeup [48 entries exceeds filelimit, not opening dir]
│   │   ├── ApplyLipstick [32 entries exceeds filelimit, not opening dir]
│   │   ├── Archery [56 entries exceeds filelimit, not opening dir]
│   │   ├── BabyCrawling [72 entries exceeds filelimit, not opening dir]
│   │   ├── BalanceBeam [32 entries exceeds filelimit, not opening dir]
│   │   ├── BandMarching [72 entries exceeds filelimit, not opening dir]
│   │   ├── BaseballPitch [80 entries exceeds filelimit, not opening dir]
│   │   ├── Basketball [88 entries exceeds filelimit, not opening dir]
│   │   ├── BasketballDunk [48 entries exceeds filelimit, not opening dir]
│   │   └── BenchPress [72 entries exceeds filelimit, not opening dir]
│   ├── train
│   │   ├── ApplyEyeMakeup [240 entries exceeds filelimit, not opening dir]
│   │   ├── ApplyLipstick [240 entries exceeds filelimit, not opening dir]
│   │   ├── Archery [240 entries exceeds filelimit, not opening dir]
│   │   ├── BabyCrawling [240 entries exceeds filelimit, not opening dir]
│   │   ├── BalanceBeam [240 entries exceeds filelimit, not opening dir]
│   │   ├── BandMarching [240 entries exceeds filelimit, not opening dir]
│   │   ├── BaseballPitch [240 entries exceeds filelimit, not opening dir]
│   │   ├── Basketball [240 entries exceeds filelimit, not opening dir]
│   │   ├── BasketballDunk [240 entries exceeds filelimit, not opening dir]
│   │   └── BenchPress [240 entries exceeds filelimit, not opening dir]
│   └── val
│       ├── ApplyEyeMakeup [24 entries exceeds filelimit, not opening dir]
│       ├── ApplyLipstick [24 entries exceeds filelimit, not opening dir]
│       ├── Archery [24 entries exceeds filelimit, not opening dir]
│       ├── BabyCrawling [24 entries exceeds filelimit, not opening dir]
│       ├── BalanceBeam [24 entries exceeds filelimit, not opening dir]
│       ├── BandMarching [24 entries exceeds filelimit, not opening dir]
│       ├── BaseballPitch [24 entries exceeds filelimit, not opening dir]
│       ├── Basketball [24 entries exceeds filelimit, not opening dir]
│       ├── BasketballDunk [24 entries exceeds filelimit, not opening dir]
│       └── BenchPress [24 entries exceeds filelimit, not opening dir]
├── ucf101_test.jsonl
├── ucf101_train.jsonl
└── ucf101_val.jsonl

34 directories, 3 files
```

`jsonl` اینوٹیشن کے لیے، ہر لائن ایک ڈکشنری ہونی چاہیے جیسا کہ:

```json
{"id": "val-0000000300", "source": "ucf101", "conversations": [{"images": ["val/BabyCrawling/v_BabyCrawling_g21_c04.0.jpg", "val/BabyCrawling/v_BabyCrawling_g21_c04.1.jpg", "val/BabyCrawling/v_BabyCrawling_g21_c04.2.jpg", "val/BabyCrawling/v_BabyCrawling_g21_c04.3.jpg", "val/BabyCrawling/v_BabyCrawling_g21_c04.4.jpg", "val/BabyCrawling/v_BabyCrawling_g21_c04.5.jpg", "val/BabyCrawling/v_BabyCrawling_g21_c04.6.jpg", "val/BabyCrawling/v_BabyCrawling_g21_c04.7.jpg"], "user": "Classify the video into one of the following classes: ApplyEyeMakeup, ApplyLipstick, Archery, BabyCrawling, BalanceBeam, BandMarching, BaseballPitch, Basketball, BasketballDunk, BenchPress.", "assistant": "BabyCrawling"}]}
{"id": "val-0000000301", "source": "ucf101", "conversations": [{"images": ["val/BabyCrawling/v_BabyCrawling_g09_c06.0.jpg", "val/BabyCrawling/v_BabyCrawling_g09_c06.1.jpg", "val/BabyCrawling/v_BabyCrawling_g09_c06.2.jpg", "val/BabyCrawling/v_BabyCrawling_g09_c06.3.jpg", "val/BabyCrawling/v_BabyCrawling_g09_c06.4.jpg", "val/BabyCrawling/v_BabyCrawling_g09_c06.5.jpg", "val/BabyCrawling/v_BabyCrawling_g09_c06.6.jpg", "val/BabyCrawling/v_BabyCrawling_g09_c06.7.jpg"], "user": "Classify the video into one of the following classes: ApplyEyeMakeup, ApplyLipstick, Archery, BabyCrawling, BalanceBeam, BandMarching, BaseballPitch, Basketball, BasketballDunk, BenchPress.", "assistant": "BabyCrawling"}]}
```

نوٹ کریں کہ `conversations` ایک فہرست ہے، اس لیے اگر ایسا ڈیٹا دستیاب ہو تو ملٹی ٹرن گفتگو کی حمایت کی جا سکتی ہے۔

## Azure GPU Quota کی درخواست

### ضروریات

ایک Azure اکاؤنٹ جس میں Contributor رول ہو (یا کوئی اور رول جس میں Contributor رسائی شامل ہو)۔

اگر آپ کے پاس Azure اکاؤنٹ نہیں ہے، تو [شروع کرنے سے پہلے مفت اکاؤنٹ بنائیں](https://azure.microsoft.com)۔

### کوٹا میں اضافہ کی درخواست کریں

آپ My quotas سے براہ راست کوٹا میں اضافے کی درخواست جمع کرا سکتے ہیں۔ کوٹا میں اضافے کی درخواست کے لیے نیچے دیے گئے مراحل پر عمل کریں۔ اس مثال کے لیے، آپ اپنی سبسکرپشن میں کوئی بھی قابل ایڈجسٹ کوٹا منتخب کر سکتے ہیں۔

[Azure portal](https://portal.azure.com) میں سائن ان کریں۔

سرچ باکس میں "quotas" لکھیں، پھر Quotas منتخب کریں۔  
![Quota](https://learn.microsoft.com/azure/quotas/media/quickstart-increase-quota-portal/quotas-portal.png)

Overview صفحے پر، کوئی فراہم کنندہ منتخب کریں، جیسے Compute یا AML۔

**نوٹ** Compute کے علاوہ تمام فراہم کنندگان کے لیے، آپ کو Adjustable کالم کی بجائے Request increase کالم نظر آئے گا۔ وہاں آپ مخصوص کوٹا کے لیے اضافہ کی درخواست کر سکتے ہیں، یا اضافہ کے لیے سپورٹ درخواست بنا سکتے ہیں۔

My quotas صفحے پر، Quota name کے تحت وہ کوٹا منتخب کریں جسے آپ بڑھانا چاہتے ہیں۔ یقینی بنائیں کہ Adjustable کالم میں اس کوٹا کے لیے Yes دکھایا گیا ہو۔

صفحے کے اوپر، New Quota Request منتخب کریں، پھر Enter a new limit منتخب کریں۔  
![Increase Quota](https://learn.microsoft.com/azure/quotas/media/quickstart-increase-quota-portal/enter-new-quota-limit.png)

New Quota Request پین میں، اپنے نئے کوٹا کی حد کے لیے عددی قدر درج کریں، پھر Submit کریں۔

آپ کی درخواست کا جائزہ لیا جائے گا، اور آپ کو اطلاع دی جائے گی کہ آیا درخواست پوری کی جا سکتی ہے۔ یہ عام طور پر چند منٹوں میں ہوتا ہے۔

اگر آپ کی درخواست پوری نہیں ہوتی، تو آپ کو سپورٹ درخواست بنانے کے لیے ایک لنک نظر آئے گا۔ اس لنک کا استعمال کرتے ہوئے، ایک سپورٹ انجینئر آپ کی درخواست میں مدد کرے گا۔

## Azure Compute GPU مشین SKU کی تجاویز

[ND A100 v4-series](https://learn.microsoft.com/azure/virtual-machines/nda100-v4-series)

[ND H100 v5-series](https://learn.microsoft.com/azure/virtual-machines/nd-h100-v5-series)

[Standard_ND40rs_v2](https://learn.microsoft.com/azure/virtual-machines/ndv2-series)

یہاں کچھ مثالیں ہیں:

### اگر آپ کے پاس A100 یا H100 GPUs ہیں

عام طور پر full finetuning بہترین کارکردگی دیتا ہے۔ آپ hateful memes classification پر Phi-3-V کو finetune کرنے کے لیے درج ذیل کمانڈ استعمال کر سکتے ہیں۔

```bash
torchrun --nproc_per_node=8 --nnodes=<num_nodes> \
  --master_addr=$MASTER_ADDR --master_port=$MASTER_PORT --node_rank=$NODE_RANK \
  finetune_hf_trainer_hateful_memes.py \
  --output_dir <output_dir> \
  --batch_size 64 \
  --use_flash_attention \
  --bf16
```

### اگر آپ کے پاس Standard_ND40rs_v2 8x V100-32GB GPUs ہیں

Phi-3-V کو hateful memes classification پر مکمل طور پر finetune کرنا اب بھی ممکن ہے۔ تاہم، flash attention کی حمایت نہ ہونے کی وجہ سے A100 یا H100 GPUs کے مقابلے میں تھروپٹ بہت کم ہوگی۔  
bf16 کی حمایت نہ ہونے کی وجہ سے درستگی پر بھی اثر پڑ سکتا ہے (اس کی جگہ fp16 مکسڈ-پریسیژن ٹریننگ استعمال ہوتی ہے)۔

```bash
torchrun --nproc_per_node=8 --nnodes=<num_nodes> \
  --master_addr=$MASTER_ADDR --master_port=$MASTER_PORT --node_rank=$NODE_RANK \
  finetune_hf_trainer_hateful_memes.py \
  --output_dir <output_dir> \
  --batch_size 64
```

### اگر آپ کو ڈیٹا سینٹر GPUs تک رسائی نہیں ہے

LoRA آپ کا واحد انتخاب ہو سکتا ہے۔ آپ hateful memes classification پر Phi-3-V کو finetune کرنے کے لیے درج ذیل کمانڈ استعمال کر سکتے ہیں۔

```bash
torchrun --nproc_per_node=2 \
  finetune_hf_trainer_hateful_memes.py \
  --output_dir <output_dir> \
  --batch_size 64 \
  --use_lora
```

Turing+ GPU کے لیے، QLoRA کی حمایت موجود ہے۔

```bash
torchrun --nproc_per_node=2 \
  finetune_hf_trainer_hateful_memes.py \
  --output_dir <output_dir> \
  --batch_size 64 \
  --use_lora \
  --use_qlora
```

## تجویز کردہ ہائپر پیرامیٹرز اور متوقع درستگی

### NLVR2

```bash
torchrun --nproc_per_node=4 \
  finetune_hf_trainer_nlvr2.py \
  --bf16 --use_flash_attention \
  --batch_size 64 \
  --output_dir <output_dir> \
  --learning_rate <lr> \
  --num_train_epochs <epochs>

```

Training method | Frozen vision model | data type | LoRA rank | LoRA alpha | batch size | learning rate | epochs | Accuracy  
--- | --- | --- | --- | --- | --- | --- | --- | --- |  
full-finetuning |  |bf16 | - | - | 64 | 1e-5 | 3 | 89.40 |  
full-finetuning | ✔ |bf16 | - | - | 64 | 2e-5 | 2 | 89.20 |  
LoRA results comming soon |  |  |  |  |  |  |  |  |

### NOTE  
نیچے دیے گئے DocVQA اور Hateful memes کے نتائج پچھلے ورژن (Phi-3-vision) پر مبنی ہیں۔  
Phi-3.5-vision کے ساتھ نئے نتائج جلد اپ ڈیٹ کیے جائیں گے۔

### DocVQA (NOTE: Phi-3-vision)

```bash
torchrun --nproc_per_node=4 \
  finetune_hf_trainer_docvqa.py \
  --full_train \
  --bf16 --use_flash_attention \
  --batch_size 64 \
  --output_dir <output_dir> \
  --learning_rate <lr> \
  --num_train_epochs <epochs>

```

Training method | data type | LoRA rank | LoRA alpha | batch size | learning rate | epochs | ANLS  
--- | --- | --- | --- | --- | --- | --- | --- |  
full-finetuning | bf16 | - | - | 64 | 5e-6 | 2 | 83.65 |  
full-finetuning | fp16 | - | - | 64 | 5e-6 | 2 | 82.60 |  
frozen image model| bf16 | - | - | 64 | 1e-4 | 2 | 79.19 |  
frozen image model| fp16 | - | - | 64 | 1e-4 | 2 | 78.74 |  
LoRA | bf16 | 32 | 16 | 64 | 2e-4 | 2 | 82.46 |  
LoRA | fp16 | 32 | 16 | 64 | 2e-4 | 2 | 82.34 |  
QLoRA | bf16 | 32 | 16 | 64 | 2e-4 | 2 | 81.85 |  
QLoRA | fp16 | 32 | 16 | 64 | 2e-4 | 2 | 81.85 |

### Hateful memes (NOTE: Phi-3-vision)

```bash
torchrun --nproc_per_node=4 \
  finetune_hf_trainer_hateful_memes.py \
  --bf16 --use_flash_attention \
  --batch_size 64 \
  --output_dir <output_dir> \
  --learning_rate <lr> \
  --num_train_epochs <epochs>

```

Training method | data type | LoRA rank | LoRA alpha | batch size | learning rate | epochs | Accuracy  
--- | --- | --- | --- | --- | --- | --- | --- |  
full-finetuning | bf16 | - | - | 64 | 5e-5 | 2 | 86.4 |  
full-finetuning | fp16 | - | - | 64 | 5e-5 | 2 | 85.4 |  
frozen image model| bf16 | - | - | 64 | 1e-4 | 3 | 79.4 |  
frozen image model| fp16 | - | - | 64 | 1e-4 | 3 | 78.6 |  
LoRA | bf16 | 128 | 256 | 64 | 2e-4 | 2 | 86.6 |  
LoRA | fp16 | 128 | 256 | 64 | 2e-4 | 2 | 85.2 |  
QLoRA | bf16 | 128 | 256 | 64 | 2e-4 | 2 | 84.0 |  
QLoRA | fp16 | 128 | 256 | 64 | 2e-4 | 2 | 83.8 |

## Speed benchmarking (NOTE: Phi-3-vision)

Phi-3.5-vision کے ساتھ نئے benchmarking نتائج جلد اپ ڈیٹ کیے جائیں گے۔

Speed benchmarking DocVQA ڈیٹاسیٹ پر کی گئی ہے۔ اس ڈیٹاسیٹ کی اوسط سیکوئنس لمبائی 2443.23 ٹوکنز ہے (امیج ماڈل کے لیے `num_crops=16` استعمال کیا گیا ہے)۔

### 8x A100-80GB (Ampere)

Training method | \# nodes | GPUs | flash attention | Effective batch size | Throughput (img/s) | Speedup | Peak GPU mem (GB)  
--- | --- | --- | --- | --- | --- | --- | --- |  
full-finetuning | 1 | 8 |  | 64 | 5.041 |  1x | ~42  
full-finetuning | 1 | 8 | ✔ | 64 | 8.657 | 1.72x | ~36  
full-finetuning | 2 | 16 | ✔ | 64 | 16.903 | 3.35x | ~29  
full-finetuning | 4 | 32 | ✔ | 64 | 33.433 | 6.63x | ~26  
frozen image model | 1 | 8 |  | 64 | 17.578 | 3.49x | ~29  
frozen image model | 1 | 8 | ✔ | 64 | 31.736 | 6.30x | ~27  
LoRA | 1 | 8 |  | 64 | 5.591 | 1.11x | ~50  
LoRA | 1 | 8 | ✔ | 64 | 12.127 | 2.41x | ~16  
QLoRA | 1 | 8 |  | 64 | 4.831 | 0.96x | ~32  
QLoRA | 1 | 8 | ✔ | 64 | 10.545 | 2.09x | ~10  

### 8x V100-32GB (Volta)

Training method | \# nodes | GPUs | flash attention | Effective batch size | Throughput (img/s) | Speedup | Peak GPU mem (GB)  
--- | --- | --- | --- | --- | --- | --- | --- |  
full-finetuning | 1 | 8 | | 64 | 2.462 |  1x | ~32  
full-finetuning | 2 | 16 |  | 64 | 4.182 | 1.70x | ~32  
full-finetuning | 4 | 32 |  | 64 | 5.465 | 2.22x | ~32  
frozen image model | 1 | 8 |  | 64 | 8.942 | 3.63x | ~27  
LoRA | 1 | 8 |  | 64 | 2.807 | 1.14x | ~30  

## Known issues

- fp16 کے ساتھ flash attention نہیں چل سکتا (جب بھی دستیاب ہو bf16 ہمیشہ تجویز کیا جاتا ہے، اور flash attention کی حمایت کرنے والے تمام GPUs bf16 کی بھی حمایت کرتے ہیں)۔  
- ابھی تک درمیانی checkpoints کو محفوظ کرنے اور ٹریننگ کو دوبارہ شروع کرنے کی حمایت نہیں ہے۔

**دستخطی نوٹ**:  
یہ دستاویز AI ترجمہ سروس [Co-op Translator](https://github.com/Azure/co-op-translator) کے ذریعے ترجمہ کی گئی ہے۔ اگرچہ ہم درستگی کے لیے کوشاں ہیں، براہ کرم آگاہ رہیں کہ خودکار ترجمے میں غلطیاں یا عدم درستیاں ہو سکتی ہیں۔ اصل دستاویز اپنی مادری زبان میں معتبر ماخذ سمجھی جانی چاہیے۔ اہم معلومات کے لیے پیشہ ور انسانی ترجمہ کی سفارش کی جاتی ہے۔ اس ترجمے کے استعمال سے پیدا ہونے والی کسی بھی غلط فہمی یا غلط تشریح کی ذمہ داری ہم پر عائد نہیں ہوتی۔