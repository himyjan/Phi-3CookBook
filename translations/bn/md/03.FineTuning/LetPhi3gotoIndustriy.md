<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "743d7e9cb9c4e8ea642d77bee657a7fa",
  "translation_date": "2025-05-09T22:25:04+00:00",
  "source_file": "md/03.FineTuning/LetPhi3gotoIndustriy.md",
  "language_code": "bn"
}
-->
# **Phi-3 কে একটি শিল্প বিশেষজ্ঞ বানান**

Phi-3 মডেলকে একটি শিল্পে প্রয়োগ করতে হলে আপনাকে শিল্পের ব্যবসায়িক ডেটা Phi-3 মডেলে যুক্ত করতে হবে। আমাদের দুটি ভিন্ন অপশন আছে, প্রথমটি হলো RAG (Retrieval Augmented Generation) এবং দ্বিতীয়টি হলো Fine Tuning।

## **RAG বনাম Fine-Tuning**

### **Retrieval Augmented Generation**

RAG হলো ডেটা রিট্রিভাল + টেক্সট জেনারেশন। এন্টারপ্রাইজের স্ট্রাকচার্ড এবং আনস্ট্রাকচার্ড ডেটা ভেক্টর ডাটাবেসে সংরক্ষিত থাকে। প্রাসঙ্গিক কন্টেন্ট খোঁজার সময়, প্রাসঙ্গিক সারাংশ এবং বিষয়বস্তু খুঁজে সেটি একটি প্রসঙ্গ তৈরি করে, এবং LLM/SLM এর টেক্সট কমপ্লিশন ক্ষমতার সাথে মিলিয়ে কন্টেন্ট তৈরি করা হয়।

### **Fine-tuning**

Fine-tuning হলো একটি নির্দিষ্ট মডেলের উন্নতির ওপর ভিত্তি করে। এটি মডেল অ্যালগরিদম থেকে শুরু করতে হয় না, তবে ডেটা ধারাবাহিকভাবে সংগ্রহ করতে হয়। যদি আপনি শিল্পের অ্যাপ্লিকেশনে আরো সঠিক টার্মিনোলজি এবং ভাষার প্রকাশ চান, তাহলে Fine-tuning আপনার জন্য ভালো বিকল্প। কিন্তু যদি আপনার ডেটা বারবার পরিবর্তিত হয়, তাহলে Fine-tuning জটিল হয়ে যেতে পারে।

### **কিভাবে নির্বাচন করবেন**

1. যদি আমাদের উত্তর বহিরাগত ডেটা প্রয়োজন হয়, তাহলে RAG সেরা বিকল্প

2. যদি আপনি স্থিতিশীল এবং সঠিক শিল্প জ্ঞান আউটপুট করতে চান, Fine-tuning ভালো হবে। RAG প্রাসঙ্গিক কন্টেন্ট টেনে আনে কিন্তু সবসময় বিশেষায়িত সূক্ষ্মতা ধরতে নাও পারে।

3. Fine-tuning এর জন্য উচ্চমানের ডেটাসেট প্রয়োজন, এবং যদি ডেটা পরিসীমা ছোট হয়, তবে তেমন প্রভাব পড়বে না। RAG বেশি নমনীয়।

4. Fine-tuning হলো একটি ব্ল্যাক বক্স, একটি মেটাফিজিক্স, এবং এর অভ্যন্তরীণ প্রক্রিয়া বোঝা কঠিন। কিন্তু RAG ডেটার উৎস খুঁজে পেতে সহজ করে তোলে, ফলে হ্যালুসিনেশন বা কন্টেন্ট ভুল সংশোধনে কার্যকর এবং ভালো স্বচ্ছতা প্রদান করে।

### **পরিস্থিতি**

1. উল্লম্ব শিল্পগুলো নির্দিষ্ট পেশাদার শব্দভাণ্ডার এবং প্রকাশনা প্রয়োজন, ***Fine-tuning*** হবে সেরা বিকল্প

2. QA সিস্টেম, বিভিন্ন জ্ঞানের বিন্দু একত্রিত করার ক্ষেত্রে, ***RAG*** হবে সেরা বিকল্প

3. স্বয়ংক্রিয় ব্যবসায়িক প্রবাহের সংমিশ্রণ ***RAG + Fine-tuning*** হবে সেরা বিকল্প

## **RAG কিভাবে ব্যবহার করবেন**

![rag](../../../../translated_images/rag.36e7cb856f120334d577fde60c6a5d7c5eecae255dac387669303d30b4b3efa4.bn.png)

একটি ভেক্টর ডাটাবেস হলো এমন একটি ডেটার সংগ্রহ যা গাণিতিক আকারে সংরক্ষিত থাকে। ভেক্টর ডাটাবেস মেশিন লার্নিং মডেলগুলোকে পূর্ববর্তী ইনপুট মনে রাখতে সহজ করে তোলে, যা সার্চ, রিকমেন্ডেশন এবং টেক্সট জেনারেশনের মতো ব্যবহারে সহায়তা করে। ডেটা মিল খুঁজে পাওয়া যায় সাদৃশ্যের উপর ভিত্তি করে, সঠিক মিল নয়, ফলে কম্পিউটার মডেলগুলো ডেটার প্রসঙ্গ বুঝতে পারে।

ভেক্টর ডাটাবেস RAG বাস্তবায়নের মূল। আমরা টেক্সট-এম্বেডিং-৩, jina-ai-embedding এর মতো ভেক্টর মডেলের মাধ্যমে ডেটাকে ভেক্টর স্টোরেজে রূপান্তর করতে পারি।

RAG অ্যাপ্লিকেশন তৈরির আরও জানতে [https://github.com/microsoft/Phi-3CookBook](https://github.com/microsoft/Phi-3CookBook?WT.mc_id=aiml-138114-kinfeylo) দেখুন।

## **Fine-tuning কিভাবে ব্যবহার করবেন**

Fine-tuning এ সাধারণত ব্যবহৃত অ্যালগরিদম হলো Lora এবং QLora। কিভাবে নির্বাচন করবেন?
- [এই স্যাম্পল নোটবুক থেকে আরও জানুন](../../../../code/04.Finetuning/Phi_3_Inference_Finetuning.ipynb)
- [Python FineTuning স্যাম্পলের উদাহরণ](../../../../code/04.Finetuning/FineTrainingScript.py)

### **Lora এবং QLora**

![lora](../../../../translated_images/qlora.6aeba71122bc0c8d56ccf0bc36b861304939fee087f43c1fc6cc5c9cb8764725.bn.png)

LoRA (Low-Rank Adaptation) এবং QLoRA (Quantized Low-Rank Adaptation) দুইটি প্রযুক্তি যা Parameter Efficient Fine Tuning (PEFT) ব্যবহার করে বড় ভাষা মডেল (LLMs) ফাইন-টিউন করার জন্য ব্যবহৃত হয়। PEFT প্রযুক্তিগুলো মডেলগুলোকে ঐতিহ্যবাহী পদ্ধতির থেকে বেশি দক্ষতার সাথে ট্রেন করার জন্য ডিজাইন করা হয়েছে।

LoRA একটি স্বাধীন ফাইনটিউনিং প্রযুক্তি যা ওজন আপডেট ম্যাট্রিক্সে লো-র্যাঙ্ক আনুমানিকতা প্রয়োগ করে মেমোরি ব্যবহারের পরিমাণ কমায়। এটি দ্রুত ট্রেনিং সময় দেয় এবং ঐতিহ্যবাহী ফাইনটিউনিং পদ্ধতির কাছাকাছি পারফরম্যান্স বজায় রাখে।

QLoRA হলো LoRA এর একটি সম্প্রসারিত সংস্করণ যা মেমোরি ব্যবহারের আরও কমানোর জন্য কোয়ান্টাইজেশন প্রযুক্তি অন্তর্ভুক্ত করে। QLoRA প্রাক-প্রশিক্ষিত LLM এর ওজন প্যারামিটারগুলোর প্রিসিশন ৪-বিটে কোয়ান্টাইজ করে, যা LoRA থেকে বেশি মেমোরি সাশ্রয়ী। তবে অতিরিক্ত কোয়ান্টাইজেশন এবং ডিকোয়ান্টাইজেশন ধাপের কারণে QLoRA ট্রেনিং LoRA এর চেয়ে প্রায় ৩০% ধীর।

QLoRA কোয়ান্টাইজেশন ভুলগুলো ঠিক করার জন্য LoRA কে অ্যাকসেসরি হিসেবে ব্যবহার করে। QLoRA তুলনামূলকভাবে ছোট, সহজলভ্য GPU তে বিলিয়ন প্যারামিটারের বিশাল মডেল ফাইন-টিউনিং সক্ষম করে। উদাহরণস্বরূপ, QLoRA ৭০B প্যারামিটার মডেলকে ৩৬টি GPU ব্যবহার করে মাত্র ২...

**অস্বীকৃতি**:  
এই নথিটি AI অনুবাদ সেবা [Co-op Translator](https://github.com/Azure/co-op-translator) ব্যবহার করে অনূদিত হয়েছে। আমরা যথাসাধ্য সঠিকতার চেষ্টা করি, তবে অনুগ্রহ করে সচেতন থাকুন যে স্বয়ংক্রিয় অনুবাদে ভুল বা অসঙ্গতি থাকতে পারে। মূল নথিটি তার নিজস্ব ভাষায়ই কর্তৃপক্ষিক উৎস হিসেবে বিবেচিত হওয়া উচিত। গুরুত্বপূর্ণ তথ্যের জন্য পেশাদার মানব অনুবাদের পরামর্শ দেওয়া হয়। এই অনুবাদের ব্যবহারে সৃষ্ট কোনো ভুল বোঝাবুঝি বা ভুল ব্যাখ্যার জন্য আমরা দায়বদ্ধ নই।