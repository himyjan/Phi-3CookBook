Phi-3-mini-এর প্রেক্ষাপটে, ইনফারেন্স বলতে বোঝানো হয় মডেল ব্যবহার করে ইনপুট ডেটার ভিত্তিতে পূর্বাভাস দেওয়া বা আউটপুট তৈরি করার প্রক্রিয়া। আসুন Phi-3-mini এবং এর ইনফারেন্স সক্ষমতা সম্পর্কে আরও বিস্তারিত জানাই।

Phi-3-mini হল Microsoft-এর দ্বারা প্রকাশিত Phi-3 সিরিজের মডেলের একটি অংশ। এই মডেলগুলো ছোট ল্যাঙ্গুয়েজ মডেল (SLMs)-এর সম্ভাবনাগুলো নতুনভাবে সংজ্ঞায়িত করার জন্য ডিজাইন করা হয়েছে।

Phi-3-mini এবং এর ইনফারেন্স ক্ষমতা সম্পর্কে কিছু মূল বিষয় এখানে উল্লেখ করা হলো:

## **Phi-3-mini ওভারভিউ:**
- Phi-3-mini-এর প্যারামিটার সাইজ ৩.৮ বিলিয়ন।
- এটি কেবলমাত্র ঐতিহ্যবাহী কম্পিউটিং ডিভাইসে নয়, মোবাইল ডিভাইস এবং IoT ডিভাইসের মতো এজ ডিভাইসেও চালানো যায়।
- Phi-3-mini-এর রিলিজ ব্যক্তি এবং এন্টারপ্রাইজকে বিভিন্ন হার্ডওয়্যার ডিভাইসে, বিশেষ করে রিসোর্স সীমাবদ্ধ পরিবেশে, SLM মোতায়েন করতে সক্ষম করে।
- এটি বিভিন্ন মডেল ফরম্যাটকে কভার করে, যার মধ্যে রয়েছে ঐতিহ্যবাহী PyTorch ফরম্যাট, gguf ফরম্যাটের কোয়ান্টাইজড সংস্করণ, এবং ONNX-ভিত্তিক কোয়ান্টাইজড সংস্করণ।

## **Phi-3-mini অ্যাক্সেস করা:**
Phi-3-mini অ্যাক্সেস করতে, আপনি [Semantic Kernel](https://github.com/microsoft/SemanticKernelCookBook?WT.mc_id=aiml-138114-kinfeylo) ব্যবহার করতে পারেন একটি Copilot অ্যাপ্লিকেশনে। Semantic Kernel সাধারণত Azure OpenAI Service, Hugging Face-এর ওপেন-সোর্স মডেল এবং লোকাল মডেলের সঙ্গে সামঞ্জস্যপূর্ণ।
আপনি [Ollama](https://ollama.com) বা [LlamaEdge](https://llamaedge.com) ব্যবহার করেও কোয়ান্টাইজড মডেল কল করতে পারেন। Ollama ব্যক্তিগত ব্যবহারকারীদের বিভিন্ন কোয়ান্টাইজড মডেল কল করার অনুমতি দেয়, যেখানে LlamaEdge GGUF মডেলের জন্য ক্রস-প্ল্যাটফর্ম উপলব্ধতা প্রদান করে।

## **কোয়ান্টাইজড মডেল:**
অনেক ব্যবহারকারী লোকাল ইনফারেন্সের জন্য কোয়ান্টাইজড মডেল ব্যবহার করতে পছন্দ করেন। উদাহরণস্বরূপ, আপনি সরাসরি Ollama রান Phi-3 চালাতে পারেন বা একটি Modelfile ব্যবহার করে এটি অফলাইনে কনফিগার করতে পারেন। Modelfile GGUF ফাইলের পথ এবং প্রম্পট ফরম্যাট নির্দিষ্ট করে।

## **জেনারেটিভ AI-এর সম্ভাবনা:**
Phi-3-mini-এর মতো SLMs একত্রিত করে জেনারেটিভ AI-এর নতুন সম্ভাবনা উন্মোচিত হয়। ইনফারেন্স শুধুমাত্র প্রথম ধাপ; এই মডেলগুলো রিসোর্স সীমাবদ্ধ, লেটেন্সি-নির্ভর, এবং খরচ-সীমাবদ্ধ পরিস্থিতিতে বিভিন্ন কাজের জন্য ব্যবহার করা যেতে পারে।

## **Phi-3-mini দিয়ে জেনারেটিভ AI উন্মোচন: ইনফারেন্স এবং ডিপ্লয়মেন্টের জন্য একটি গাইড** 
Semantic Kernel, Ollama/LlamaEdge, এবং ONNX Runtime ব্যবহার করে কীভাবে Phi-3-mini মডেল অ্যাক্সেস এবং ইনফারেন্স করতে হয় তা শিখুন এবং বিভিন্ন অ্যাপ্লিকেশন পরিস্থিতিতে জেনারেটিভ AI-এর সম্ভাবনা আবিষ্কার করুন।

**বৈশিষ্ট্যসমূহ**
Phi-3-mini মডেল ইনফারেন্স:

- [Semantic Kernel](https://github.com/Azure-Samples/Phi-3MiniSamples/tree/main/semantickernel?WT.mc_id=aiml-138114-kinfeylo)
- [Ollama](https://github.com/Azure-Samples/Phi-3MiniSamples/tree/main/ollama?WT.mc_id=aiml-138114-kinfeylo)
- [LlamaEdge WASM](https://github.com/Azure-Samples/Phi-3MiniSamples/tree/main/wasm?WT.mc_id=aiml-138114-kinfeylo)
- [ONNX Runtime](https://github.com/Azure-Samples/Phi-3MiniSamples/tree/main/onnx?WT.mc_id=aiml-138114-kinfeylo)
- [iOS](https://github.com/Azure-Samples/Phi-3MiniSamples/tree/main/ios?WT.mc_id=aiml-138114-kinfeylo)

সংক্ষেপে, Phi-3-mini ডেভেলপারদের বিভিন্ন মডেল ফরম্যাট অন্বেষণ করতে এবং বিভিন্ন অ্যাপ্লিকেশন পরিস্থিতিতে জেনারেটিভ AI কাজে লাগাতে সক্ষম করে।

**অস্বীকৃতি**:  
এই নথিটি মেশিন-ভিত্তিক এআই অনুবাদ পরিষেবা ব্যবহার করে অনুবাদ করা হয়েছে। আমরা যথাসম্ভব সঠিক অনুবাদের চেষ্টা করি, তবে দয়া করে মনে রাখবেন যে স্বয়ংক্রিয় অনুবাদে ভুল বা অসঙ্গতি থাকতে পারে। নথিটির মূল ভাষায় রচিত সংস্করণটিকেই প্রামাণিক সূত্র হিসেবে গণ্য করা উচিত। গুরুত্বপূর্ণ তথ্যের জন্য, পেশাদার মানব অনুবাদ পরামর্শযোগ্য। এই অনুবাদ ব্যবহারের ফলে সৃষ্ট কোনো ভুল বোঝাবুঝি বা ভুল ব্যাখ্যার জন্য আমরা দায়ী নই।