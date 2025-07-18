<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "743d7e9cb9c4e8ea642d77bee657a7fa",
  "translation_date": "2025-07-17T09:52:13+00:00",
  "source_file": "md/03.FineTuning/LetPhi3gotoIndustriy.md",
  "language_code": "ur"
}
-->
# **Phi-3 کو صنعت کا ماہر بنائیں**

Phi-3 ماڈل کو کسی صنعت میں لاگو کرنے کے لیے، آپ کو صنعت کے کاروباری ڈیٹا کو Phi-3 ماڈل میں شامل کرنا ہوگا۔ ہمارے پاس دو مختلف اختیارات ہیں، پہلا RAG (Retrieval Augmented Generation) ہے اور دوسرا Fine Tuning۔

## **RAG بمقابلہ Fine-Tuning**

### **Retrieval Augmented Generation**

RAG ڈیٹا بازیافت + متن کی تخلیق ہے۔ ادارے کے منظم اور غیر منظم ڈیٹا کو ویکٹر ڈیٹا بیس میں محفوظ کیا جاتا ہے۔ جب متعلقہ مواد تلاش کیا جاتا ہے، تو متعلقہ خلاصہ اور مواد مل کر ایک سیاق و سباق بناتے ہیں، اور LLM/SLM کی متن مکمل کرنے کی صلاحیت کے ساتھ مل کر مواد تیار کیا جاتا ہے۔

### **Fine-tuning**

Fine-tuning کسی خاص ماڈل کی بہتری پر مبنی ہے۔ اس کے لیے ماڈل الگورتھم سے شروع کرنے کی ضرورت نہیں ہوتی، لیکن ڈیٹا کو مسلسل جمع کرنا پڑتا ہے۔ اگر آپ صنعت کی ایپلیکیشنز میں زیادہ درست اصطلاحات اور زبان کے اظہار چاہتے ہیں، تو Fine-tuning آپ کے لیے بہتر انتخاب ہے۔ لیکن اگر آپ کا ڈیٹا بار بار تبدیل ہوتا ہے، تو Fine-tuning پیچیدہ ہو سکتا ہے۔

### **انتخاب کیسے کریں**

1. اگر ہمارے جواب میں بیرونی ڈیٹا شامل کرنے کی ضرورت ہو، تو RAG بہترین انتخاب ہے۔

2. اگر آپ کو مستحکم اور درست صنعتی معلومات فراہم کرنی ہوں، تو Fine-tuning اچھا انتخاب ہوگا۔ RAG متعلقہ مواد کو ترجیح دیتا ہے لیکن ہمیشہ مخصوص باریکیاں نہیں پکڑ پاتا۔

3. Fine-tuning کے لیے اعلیٰ معیار کا ڈیٹا سیٹ ضروری ہے، اور اگر ڈیٹا کی مقدار کم ہو تو اس کا زیادہ فرق نہیں پڑے گا۔ RAG زیادہ لچکدار ہے۔

4. Fine-tuning ایک بلیک باکس ہے، ایک مابعد الطبیعیات، اور اس کے اندرونی میکانزم کو سمجھنا مشکل ہے۔ لیکن RAG ڈیٹا کے ماخذ کو تلاش کرنا آسان بناتا ہے، جس سے ہیلوسینیشنز یا مواد کی غلطیوں کو مؤثر طریقے سے درست کیا جا سکتا ہے اور بہتر شفافیت فراہم ہوتی ہے۔

### **مناظر**

1. عمودی صنعتوں کو مخصوص پیشہ ورانہ الفاظ اور اظہار کی ضرورت ہوتی ہے، ***Fine-tuning*** بہترین انتخاب ہوگا۔

2. QA سسٹم، جو مختلف علمی نکات کے امتزاج پر مشتمل ہو، ***RAG*** بہترین انتخاب ہوگا۔

3. خودکار کاروباری عمل کے امتزاج کے لیے ***RAG + Fine-tuning*** بہترین انتخاب ہے۔

## **RAG کا استعمال کیسے کریں**

![rag](../../../../translated_images/rag.2014adc59e6f6007bafac13e800a6cbc3e297fbb9903efe20a93129bd13987e9.ur.png)

ویکٹر ڈیٹا بیس ڈیٹا کا ایک مجموعہ ہے جو ریاضیاتی شکل میں محفوظ ہوتا ہے۔ ویکٹر ڈیٹا بیس مشین لرننگ ماڈلز کے لیے پچھلے ان پٹس کو یاد رکھنا آسان بناتے ہیں، جس سے مشین لرننگ کو تلاش، سفارشات، اور متن کی تخلیق جیسے استعمال کے معاملات کی حمایت کے لیے استعمال کیا جا سکتا ہے۔ ڈیٹا کو مماثلت کے میٹرکس کی بنیاد پر شناخت کیا جا سکتا ہے نہ کہ بالکل میل کھانے کی بنیاد پر، جس سے کمپیوٹر ماڈلز کو ڈیٹا کے سیاق و سباق کو سمجھنے میں مدد ملتی ہے۔

ویکٹر ڈیٹا بیس RAG کو ممکن بنانے کی کلید ہے۔ ہم متن-ایمبیڈنگ-3، jina-ai-embedding، وغیرہ جیسے ویکٹر ماڈلز کے ذریعے ڈیٹا کو ویکٹر اسٹوریج میں تبدیل کر سکتے ہیں۔

RAG ایپلیکیشن بنانے کے بارے میں مزید جانیں [https://github.com/microsoft/Phi-3CookBook](https://github.com/microsoft/Phi-3CookBook?WT.mc_id=aiml-138114-kinfeylo)

## **Fine-tuning کا استعمال کیسے کریں**

Fine-tuning میں عام طور پر استعمال ہونے والے الگورتھمز Lora اور QLora ہیں۔ انتخاب کیسے کریں؟
- [اس سیمپل نوٹ بک کے ساتھ مزید جانیں](../../../../code/04.Finetuning/Phi_3_Inference_Finetuning.ipynb)
- [Python FineTuning سیمپل کی مثال](../../../../code/04.Finetuning/FineTrainingScript.py)

### **Lora اور QLora**

![lora](../../../../translated_images/qlora.e6446c988ee04ca08807488bb7d9e2c0ea7ef4af9d000fc6d13032b4ac2de18d.ur.png)

LoRA (Low-Rank Adaptation) اور QLoRA (Quantized Low-Rank Adaptation) دونوں تکنیکیں ہیں جو Parameter Efficient Fine Tuning (PEFT) کا استعمال کرتے ہوئے بڑے زبان کے ماڈلز (LLMs) کو Fine-tune کرنے کے لیے استعمال ہوتی ہیں۔ PEFT تکنیکیں روایتی طریقوں کے مقابلے میں ماڈلز کو زیادہ مؤثر طریقے سے تربیت دینے کے لیے ڈیزائن کی گئی ہیں۔

LoRA ایک خودمختار Fine-tuning تکنیک ہے جو وزن کی اپ ڈیٹ میٹرکس پر کم درجہ کی تخمینی اپروچ لگا کر میموری کے استعمال کو کم کرتی ہے۔ یہ تیز تربیت کے اوقات فراہم کرتی ہے اور روایتی Fine-tuning طریقوں کے قریب کارکردگی برقرار رکھتی ہے۔

QLoRA LoRA کا ایک توسیعی ورژن ہے جو میموری کے استعمال کو مزید کم کرنے کے لیے کوانٹائزیشن تکنیکوں کو شامل کرتا ہے۔ QLoRA پری ٹرینڈ LLM میں وزن کے پیرامیٹرز کی درستگی کو 4-بٹ پر کوانٹائز کرتا ہے، جو LoRA کے مقابلے میں زیادہ میموری مؤثر ہے۔ تاہم، اضافی کوانٹائزیشن اور ڈی کوانٹائزیشن مراحل کی وجہ سے QLoRA کی تربیت LoRA کی تربیت سے تقریباً 30% سست ہوتی ہے۔

QLoRA کوانٹائزیشن کی غلطیوں کو درست کرنے کے لیے LoRA کو بطور معاون استعمال کرتا ہے۔ QLoRA بڑے ماڈلز کو، جن میں اربوں پیرامیٹرز ہوتے ہیں، نسبتاً چھوٹے اور آسانی سے دستیاب GPUs پر Fine-tune کرنے کے قابل بناتا ہے۔ مثال کے طور پر، QLoRA 70B پیرامیٹر ماڈل کو 36 GPUs کی ضرورت کے بغیر Fine-tune کر سکتا ہے۔

**دستخطی نوٹ**:  
یہ دستاویز AI ترجمہ سروس [Co-op Translator](https://github.com/Azure/co-op-translator) کے ذریعے ترجمہ کی گئی ہے۔ اگرچہ ہم درستگی کے لیے کوشاں ہیں، براہ کرم آگاہ رہیں کہ خودکار ترجمے میں غلطیاں یا عدم درستیاں ہو سکتی ہیں۔ اصل دستاویز اپنی مادری زبان میں ہی معتبر ماخذ سمجھی جانی چاہیے۔ اہم معلومات کے لیے پیشہ ور انسانی ترجمہ کی سفارش کی جاتی ہے۔ اس ترجمے کے استعمال سے پیدا ہونے والی کسی بھی غلط فہمی یا غلط تشریح کی ذمہ داری ہم پر عائد نہیں ہوتی۔