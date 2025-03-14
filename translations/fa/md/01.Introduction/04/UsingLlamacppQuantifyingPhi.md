# **کم کردن حجم خانواده فی با استفاده از llama.cpp**

## **llama.cpp چیست؟**

llama.cpp یک کتابخانه نرم‌افزاری متن‌باز است که عمدتاً با زبان C++ نوشته شده و برای استنتاج روی مدل‌های زبانی بزرگ (LLMs) مثل Llama استفاده می‌شود. هدف اصلی این کتابخانه ارائه عملکردی پیشرفته برای استنتاج مدل‌های زبانی بزرگ روی انواع سخت‌افزارها با حداقل پیکربندی است. همچنین، این کتابخانه دارای بایندینگ‌های پایتون است که یک API سطح بالا برای تکمیل متن و یک سرور وب سازگار با OpenAI ارائه می‌دهند.

هدف اصلی llama.cpp این است که استنتاج مدل‌های زبانی بزرگ را با حداقل پیکربندی و عملکرد پیشرفته روی سخت‌افزارهای متنوع - چه به‌صورت محلی و چه در فضای ابری - امکان‌پذیر کند.

- پیاده‌سازی ساده با C/C++ بدون وابستگی‌ها
- پشتیبانی کامل از Apple Silicon - بهینه‌سازی شده با استفاده از ARM NEON، Accelerate و Metal
- پشتیبانی از AVX، AVX2 و AVX512 برای معماری‌های x86
- کم کردن حجم مدل‌ها به صورت اعداد صحیح 1.5 بیت، 2 بیت، 3 بیت، 4 بیت، 5 بیت، 6 بیت و 8 بیت برای استنتاج سریع‌تر و کاهش استفاده از حافظه
- هسته‌های سفارشی CUDA برای اجرای مدل‌های زبانی بزرگ روی GPUهای NVIDIA (پشتیبانی از GPUهای AMD از طریق HIP)
- پشتیبانی از backendهای Vulkan و SYCL
- استنتاج ترکیبی CPU+GPU برای تسریع نسبی مدل‌هایی که بزرگ‌تر از ظرفیت کلی VRAM هستند

## **کم کردن حجم Phi-3.5 با استفاده از llama.cpp**

مدل Phi-3.5-Instruct می‌تواند با استفاده از llama.cpp کم‌حجم شود، اما مدل‌های Phi-3.5-Vision و Phi-3.5-MoE هنوز پشتیبانی نمی‌شوند. فرمتی که توسط llama.cpp تبدیل می‌شود GGUF نام دارد که همچنین پرکاربردترین فرمت برای کم کردن حجم است.

تعداد زیادی مدل با فرمت GGUF کم‌حجم‌شده در Hugging Face موجود است. AI Foundry، Ollama و LlamaEdge به llama.cpp متکی هستند، بنابراین مدل‌های GGUF نیز اغلب استفاده می‌شوند.

### **GGUF چیست؟**

GGUF یک فرمت باینری است که برای بارگذاری و ذخیره سریع مدل‌ها بهینه‌سازی شده و برای اهداف استنتاج بسیار کارآمد است. این فرمت برای استفاده با GGML و سایر اجراکننده‌ها طراحی شده است. GGUF توسط @ggerganov توسعه داده شده که همچنین توسعه‌دهنده llama.cpp است؛ یک فریم‌ورک محبوب استنتاج مدل‌های زبانی بزرگ با C/C++. مدل‌هایی که ابتدا در فریم‌ورک‌هایی مثل PyTorch توسعه داده شده‌اند، می‌توانند به فرمت GGUF برای استفاده با این موتورها تبدیل شوند.

### **ONNX در مقابل GGUF**

ONNX یک فرمت سنتی برای یادگیری ماشین/یادگیری عمیق است که به خوبی در فریم‌ورک‌های مختلف هوش مصنوعی پشتیبانی می‌شود و در دستگاه‌های لبه‌ای کاربردهای خوبی دارد. از طرف دیگر، GGUF مبتنی بر llama.cpp است و می‌توان گفت که در عصر GenAI تولید شده است. این دو کاربردهای مشابهی دارند. اگر به عملکرد بهتر در سخت‌افزارهای جاسازی‌شده و لایه‌های کاربردی نیاز دارید، ONNX ممکن است انتخاب شما باشد. اگر از فریم‌ورک‌ها و تکنولوژی‌های مشتق شده از llama.cpp استفاده می‌کنید، GGUF ممکن است گزینه بهتری باشد.

### **کم کردن حجم Phi-3.5-Instruct با استفاده از llama.cpp**

**1. پیکربندی محیط**


```bash

git clone https://github.com/ggerganov/llama.cpp.git

cd llama.cpp

make -j8

```


**2. کم کردن حجم**

تبدیل Phi-3.5-Instruct به GGUF با دقت FP16 با استفاده از llama.cpp


```bash

./convert_hf_to_gguf.py <Your Phi-3.5-Instruct Location> --outfile phi-3.5-128k-mini_fp16.gguf

```

کم کردن حجم Phi-3.5 به INT4


```bash

./llama.cpp/llama-quantize <Your phi-3.5-128k-mini_fp16.gguf location> ./gguf/phi-3.5-128k-mini_Q4_K_M.gguf Q4_K_M

```


**3. تست**

نصب llama-cpp-python


```bash

pip install llama-cpp-python -U

```

***توجه*** 

اگر از Apple Silicon استفاده می‌کنید، لطفاً llama-cpp-python را به این شکل نصب کنید:


```bash

CMAKE_ARGS="-DLLAMA_METAL=on" pip install llama-cpp-python -U

```

تست


```bash

llama.cpp/llama-cli --model <Your phi-3.5-128k-mini_Q4_K_M.gguf location> --prompt "<|user|>\nCan you introduce .NET<|end|>\n<|assistant|>\n"  --gpu-layers 10

```



## **منابع**

1. اطلاعات بیشتر درباره llama.cpp [https://onnxruntime.ai/docs/genai/](https://onnxruntime.ai/docs/genai/)

2. اطلاعات بیشتر درباره GGUF [https://huggingface.co/docs/hub/en/gguf](https://huggingface.co/docs/hub/en/gguf)

**سلب مسئولیت**:  
این سند با استفاده از خدمات ترجمه ماشینی مبتنی بر هوش مصنوعی ترجمه شده است. در حالی که ما برای دقت تلاش می‌کنیم، لطفاً توجه داشته باشید که ترجمه‌های خودکار ممکن است حاوی خطاها یا نادرستی‌هایی باشند. سند اصلی به زبان اصلی آن باید به عنوان منبع معتبر در نظر گرفته شود. برای اطلاعات حساس، ترجمه انسانی حرفه‌ای توصیه می‌شود. ما هیچ مسئولیتی در قبال سوءتفاهم‌ها یا تفسیرهای نادرست ناشی از استفاده از این ترجمه نداریم.