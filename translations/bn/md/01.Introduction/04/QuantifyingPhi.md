<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "d658062de70b131ef4c0bff69b5fc70e",
  "translation_date": "2025-05-09T13:19:50+00:00",
  "source_file": "md/01.Introduction/04/QuantifyingPhi.md",
  "language_code": "bn"
}
-->
# **ফাই পরিবার পরিমাপ**

মডেল কোয়ান্টাইজেশন বলতে একটি নিউরাল নেটওয়ার্ক মডেলের প্যারামিটারসমূহ (যেমন ওজন এবং সক্রিয়করণ মান) বড় মানের পরিসর থেকে (সাধারণত একটি ধারাবাহিক মানের পরিসর) ছোট একটি সসীম মানের পরিসরে ম্যাপ করার প্রক্রিয়াকে বোঝায়। এই প্রযুক্তি মডেলের আকার এবং গণনাগত জটিলতা কমাতে পারে এবং মোবাইল ডিভাইস বা এমবেডেড সিস্টেমের মতো সীমিত সম্পদ পরিবেশে মডেলের কার্যক্ষমতা উন্নত করতে সাহায্য করে। মডেল কোয়ান্টাইজেশন প্যারামিটারগুলির নির্ভুলতা কমিয়ে কম্প্রেশন অর্জন করে, তবে এতে কিছু নির্ভুলতার ক্ষতি হয়। তাই কোয়ান্টাইজেশন প্রক্রিয়ায় মডেলের আকার, গণনাগত জটিলতা এবং নির্ভুলতার মধ্যে সঠিক ভারসাম্য বজায় রাখা জরুরি। সাধারণ কোয়ান্টাইজেশন পদ্ধতির মধ্যে রয়েছে ফিক্সড-পয়েন্ট কোয়ান্টাইজেশন, ফ্লোটিং-পয়েন্ট কোয়ান্টাইজেশন ইত্যাদি। নির্দিষ্ট পরিস্থিতি এবং প্রয়োজন অনুযায়ী উপযুক্ত কোয়ান্টাইজেশন কৌশল বেছে নেওয়া যায়।

আমরা আশা করি GenAI মডেলগুলোকে এজ ডিভাইসে ডিপ্লয় করতে পারব এবং আরও বেশি ডিভাইসকে GenAI পরিস্থিতিতে নিয়ে আসতে পারব, যেমন মোবাইল ডিভাইস, AI PC/Copilot+PC, এবং প্রচলিত IoT ডিভাইস। কোয়ান্টাইজেশন মডেলের মাধ্যমে আমরা বিভিন্ন ডিভাইস অনুযায়ী মডেলটি এজ ডিভাইসে ডিপ্লয় করতে পারব। হার্ডওয়্যার নির্মাতাদের দ্বারা প্রদত্ত মডেল অ্যাক্সিলারেশন ফ্রেমওয়ার্ক এবং কোয়ান্টাইজেশন মডেলের সাথে মিলিয়ে আমরা আরও ভালো SLM অ্যাপ্লিকেশন পরিস্থিতি গড়ে তুলতে পারব।

কোয়ান্টাইজেশন পরিস্থিতিতে আমাদের কাছে বিভিন্ন নির্ভুলতা রয়েছে (INT4, INT8, FP16, FP32)। নিচে সাধারণত ব্যবহৃত কোয়ান্টাইজেশন নির্ভুলতাগুলোর ব্যাখ্যা দেওয়া হলো

### **INT4**

INT4 কোয়ান্টাইজেশন একটি চরম কোয়ান্টাইজেশন পদ্ধতি যা মডেলের ওজন এবং সক্রিয়করণ মানকে ৪-বিট পূর্ণসংখ্যায় কোয়ান্টাইজ করে। INT4 কোয়ান্টাইজেশনে সাধারণত নির্ভুলতার বেশি ক্ষতি হয় কারণ এর প্রতিনিধিত্বের পরিসর ছোট এবং নির্ভুলতা কম। তবে, INT8 কোয়ান্টাইজেশনের তুলনায় INT4 কোয়ান্টাইজেশন মডেলের স্টোরেজ প্রয়োজনীয়তা এবং গণনাগত জটিলতা আরও কমিয়ে দিতে পারে। লক্ষ্যণীয় যে, বাস্তব অ্যাপ্লিকেশনগুলোতে INT4 কোয়ান্টাইজেশন তুলনামূলক কম ব্যবহৃত হয়, কারণ খুব কম নির্ভুলতা মডেলের কর্মক্ষমতায় বড় ধরনের অবনতি ঘটাতে পারে। এছাড়া, সব হার্ডওয়্যার INT4 অপারেশন সমর্থন করে না, তাই কোয়ান্টাইজেশন পদ্ধতি বাছাই করার সময় হার্ডওয়্যার সামঞ্জস্য বিবেচনা করতে হয়।

### **INT8**

INT8 কোয়ান্টাইজেশন হলো মডেলের ওজন এবং সক্রিয়করণকে ফ্লোটিং পয়েন্ট সংখ্যার থেকে ৮-বিট পূর্ণসংখ্যায় রূপান্তর করার প্রক্রিয়া। যদিও INT8 পূর্ণসংখ্যার প্রতিনিধিত্ব পরিসর ছোট এবং কম নির্ভুল, এটি স্টোরেজ এবং গণনার প্রয়োজনীয়তা উল্লেখযোগ্যভাবে কমিয়ে দেয়। INT8 কোয়ান্টাইজেশনে মডেলের ওজন এবং সক্রিয়করণ মান কোয়ান্টাইজেশন প্রক্রিয়ার মধ্য দিয়ে যায়, যার মধ্যে স্কেলিং এবং অফসেট থাকে, যাতে মূল ফ্লোটিং পয়েন্ট তথ্য যতটা সম্ভব সংরক্ষিত থাকে। ইনফারেন্সের সময়, এই কোয়ান্টাইজড মানগুলো আবার ফ্লোটিং পয়েন্টে ডিকোয়ান্টাইজ করা হয় গণনার জন্য, এবং পরবর্তী ধাপে ফের INT8 এ কোয়ান্টাইজ করা হয়। এই পদ্ধতিটি বেশিরভাগ অ্যাপ্লিকেশনে যথেষ্ট নির্ভুলতা প্রদান করে এবং উচ্চ গণনাগত দক্ষতা বজায় রাখে।

### **FP16**

FP16 ফরম্যাট, অর্থাৎ ১৬-বিট ফ্লোটিং পয়েন্ট সংখ্যা (float16), ৩২-বিট ফ্লোটিং পয়েন্ট সংখ্যার (float32) তুলনায় মেমোরি ব্যবহার অর্ধেকে কমিয়ে দেয়, যা বড় আকারের ডিপ লার্নিং অ্যাপ্লিকেশনে গুরুত্বপূর্ণ সুবিধা। FP16 ফরম্যাট একই GPU মেমোরি সীমাবদ্ধতার মধ্যে বড় মডেল লোড বা বেশি ডেটা প্রক্রিয়াকরণ করতে দেয়। আধুনিক GPU হার্ডওয়্যার FP16 অপারেশন সমর্থন অব্যাহত রাখায়, FP16 ফরম্যাট ব্যবহারে কম্পিউটিং গতি বাড়তেও পারে। তবে, FP16 ফরম্যাটের কিছু অন্তর্নিহিত অসুবিধা রয়েছে, যেমন কম নির্ভুলতা, যা কিছু ক্ষেত্রে সংখ্যাগত অস্থিতিশীলতা বা নির্ভুলতার ক্ষতি ঘটাতে পারে।

### **FP32**

FP32 ফরম্যাট উচ্চতর নির্ভুলতা প্রদান করে এবং বিস্তৃত মানের পরিসর সঠিকভাবে উপস্থাপন করতে পারে। যেখানে জটিল গাণিতিক অপারেশন করা হয় বা উচ্চ নির্ভুল ফলাফল প্রয়োজন, সেখানে FP32 ফরম্যাট পছন্দ করা হয়। তবে, উচ্চ নির্ভুলতা মানে বেশি মেমোরি ব্যবহার এবং দীর্ঘ গণনা সময়। বড় আকারের ডিপ লার্নিং মডেলের ক্ষেত্রে, বিশেষ করে যেখানে অনেক প্যারামিটার এবং বিশাল পরিমাণ ডেটা থাকে, FP32 ফরম্যাট GPU মেমোরি অপর্যাপ্ততা বা ইনফারেন্স গতি হ্রাসের কারণ হতে পারে।

মোবাইল ডিভাইস বা IoT ডিভাইসে আমরা Phi-3.x মডেলগুলোকে INT4 এ রূপান্তর করতে পারি, যেখানে AI PC / Copilot PC তে উচ্চতর নির্ভুলতা যেমন INT8, FP16, FP32 ব্যবহার করা যেতে পারে।

বর্তমানে বিভিন্ন হার্ডওয়্যার নির্মাতারা জেনারেটিভ মডেল সমর্থনের জন্য বিভিন্ন ফ্রেমওয়ার্ক ব্যবহার করেন, যেমন Intel এর OpenVINO, Qualcomm এর QNN, Apple এর MLX, এবং Nvidia এর CUDA, ইত্যাদি, যেগুলো মডেল কোয়ান্টাইজেশনের সাথে মিলিয়ে লোকাল ডিপ্লয়মেন্ট সম্পন্ন করে।

প্রযুক্তিগত দিক থেকে, কোয়ান্টাইজেশনের পর আমাদের কাছে বিভিন্ন ফরম্যাট সমর্থন রয়েছে, যেমন PyTorch / Tensorflow ফরম্যাট, GGUF, এবং ONNX। আমি GGUF এবং ONNX এর মধ্যে ফরম্যাট তুলনা এবং অ্যাপ্লিকেশন পরিস্থিতি করেছি। এখানে আমি ONNX কোয়ান্টাইজেশন ফরম্যাট সুপারিশ করছি, যা মডেল ফ্রেমওয়ার্ক থেকে হার্ডওয়্যার পর্যন্ত ভালো সমর্থন পায়। এই অধ্যায়ে আমরা ONNX Runtime for GenAI, OpenVINO, এবং Apple MLX ব্যবহার করে মডেল কোয়ান্টাইজেশনের ওপর গুরুত্ব দেব (আপনার কাছে যদি ভালো কোনো উপায় থাকে, তাহলে PR জমা দিয়ে আমাদের দিতে পারেন)।

**এই অধ্যায়ে অন্তর্ভুক্ত আছে**

1. [llama.cpp ব্যবহার করে Phi-3.5 / 4 কোয়ান্টাইজ করা](./UsingLlamacppQuantifyingPhi.md)

2. [onnxruntime এর জন্য Generative AI এক্সটেনশন ব্যবহার করে Phi-3.5 / 4 কোয়ান্টাইজ করা](./UsingORTGenAIQuantifyingPhi.md)

3. [Intel OpenVINO ব্যবহার করে Phi-3.5 / 4 কোয়ান্টাইজ করা](./UsingIntelOpenVINOQuantifyingPhi.md)

4. [Apple MLX Framework ব্যবহার করে Phi-3.5 / 4 কোয়ান্টাইজ করা](./UsingAppleMLXQuantifyingPhi.md)

**দ্রষ্টব্য**:  
এই নথিটি AI অনুবাদ সেবা [Co-op Translator](https://github.com/Azure/co-op-translator) ব্যবহার করে অনূদিত হয়েছে। আমরা যথাসাধ্য সঠিকতার চেষ্টা করি, তবে স্বয়ংক্রিয় অনুবাদে ত্রুটি বা অসঙ্গতি থাকতে পারে। মূল নথিটি তার নিজ ভাষায়ই কর্তৃত্বপূর্ণ উৎস হিসেবে বিবেচিত হওয়া উচিত। গুরুত্বপূর্ণ তথ্যের জন্য পেশাদার মানব অনুবাদের পরামর্শ দেওয়া হয়। এই অনুবাদ ব্যবহারের ফলে সৃষ্ট কোনো ভুল বোঝাবুঝি বা ভুল ব্যাখ্যার জন্য আমরা দায়ী নই।