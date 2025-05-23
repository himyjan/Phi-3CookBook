<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "cb5648935f63edc17e95ce38f23adc32",
  "translation_date": "2025-05-07T13:36:59+00:00",
  "source_file": "md/03.FineTuning/FineTuning_Scenarios.md",
  "language_code": "fa"
}
-->
## سناریوهای ریزتنظیم

![FineTuning with MS Services](../../../../translated_images/FinetuningwithMS.3d0cec8ae693e094c38c72575e63f2c9bf1cf980ab90f1388e102709f9c979e5.fa.png)

**پلتفرم** این شامل فناوری‌های مختلفی مانند Azure AI Foundry، Azure Machine Learning، AI Tools، Kaito و ONNX Runtime است.

**زیرساخت** این شامل CPU و FPGA می‌شود که برای فرآیند ریزتنظیم ضروری هستند. اجازه دهید آیکون‌های هر یک از این فناوری‌ها را به شما نشان دهم.

**ابزارها و چارچوب‌ها** این شامل ONNX Runtime و ONNX Runtime است. اجازه دهید آیکون‌های هر یک از این فناوری‌ها را به شما نشان دهم.  
[Insert icons for ONNX Runtime and ONNX Runtime]

فرآیند ریزتنظیم با فناوری‌های مایکروسافت شامل اجزا و ابزارهای مختلفی است. با درک و استفاده از این فناوری‌ها، می‌توانیم برنامه‌های خود را به‌طور مؤثری ریزتنظیم کرده و راه‌حل‌های بهتری ایجاد کنیم.

## مدل به‌عنوان سرویس

مدل را با استفاده از ریزتنظیم میزبانی‌شده، بدون نیاز به ایجاد و مدیریت محاسبات، ریزتنظیم کنید.

![MaaS Fine Tuning](../../../../translated_images/MaaSfinetune.3eee4630607aff0d0a137b16ab79ec5977ece923cd1fdd89557a2655c632669d.fa.png)

ریزتنظیم بدون سرور برای مدل‌های Phi-3-mini و Phi-3-medium در دسترس است که به توسعه‌دهندگان امکان می‌دهد به‌سرعت و به‌راحتی مدل‌ها را برای سناریوهای ابری و لبه‌ای سفارشی کنند بدون اینکه نیاز به تنظیم محاسبات داشته باشند. همچنین اعلام کرده‌ایم که Phi-3-small اکنون از طریق ارائه Models-as-a-Service ما در دسترس است تا توسعه‌دهندگان بتوانند به سرعت و به‌راحتی توسعه هوش مصنوعی را شروع کنند بدون اینکه زیرساخت‌های پایه را مدیریت کنند.

## مدل به‌عنوان پلتفرم

کاربران خود محاسبات را مدیریت می‌کنند تا مدل‌هایشان را ریزتنظیم کنند.

![Maap Fine Tuning](../../../../translated_images/MaaPFinetune.fd3829c1122f5d1c4a6a91593ebc348548410e162acda34f18034384e3b3816a.fa.png)

[Fine Tuning Sample](https://github.com/Azure/azureml-examples/blob/main/sdk/python/foundation-models/system/finetune/chat-completion/chat-completion.ipynb)

## سناریوهای ریزتنظیم

| | | | | | | |
|-|-|-|-|-|-|-|
|سناریو|LoRA|QLoRA|PEFT|DeepSpeed|ZeRO|DORA|
|سفارشی‌سازی مدل‌های از پیش آموزش‌دیده LLM برای وظایف یا حوزه‌های خاص|بله|بله|بله|بله|بله|بله|
|ریزتنظیم برای وظایف NLP مانند طبقه‌بندی متن، شناسایی موجودیت‌های نام‌دار و ترجمه ماشینی|بله|بله|بله|بله|بله|بله|
|ریزتنظیم برای وظایف پرسش و پاسخ|بله|بله|بله|بله|بله|بله|
|ریزتنظیم برای تولید پاسخ‌های شبیه به انسان در چت‌بات‌ها|بله|بله|بله|بله|بله|بله|
|ریزتنظیم برای تولید موسیقی، هنر یا دیگر اشکال خلاقیت|بله|بله|بله|بله|بله|بله|
|کاهش هزینه‌های محاسباتی و مالی|بله|بله|خیر|بله|بله|خیر|
|کاهش مصرف حافظه|خیر|بله|خیر|بله|بله|بله|
|استفاده از پارامترهای کمتر برای ریزتنظیم کارآمد|خیر|بله|بله|خیر|خیر|بله|
|شکل حافظه‌پسند موازی‌سازی داده که دسترسی به حافظه کل GPUهای موجود را فراهم می‌کند|خیر|خیر|خیر|بله|بله|بله|

## نمونه‌های عملکرد ریزتنظیم

![Finetuning Performance](../../../../translated_images/Finetuningexamples.a9a41214f8f5afc186adb16a413b1c17e2f43a89933ba95feb5aee84b0b24add.fa.png)

**سلب مسئولیت**:  
این سند با استفاده از سرویس ترجمه هوش مصنوعی [Co-op Translator](https://github.com/Azure/co-op-translator) ترجمه شده است. در حالی که ما در تلاش برای دقت هستیم، لطفاً توجه داشته باشید که ترجمه‌های خودکار ممکن است حاوی خطاها یا نادرستی‌هایی باشند. سند اصلی به زبان مادری خود به عنوان منبع معتبر در نظر گرفته شود. برای اطلاعات حیاتی، ترجمه حرفه‌ای انسانی توصیه می‌شود. ما مسئول هیچگونه سوءتفاهم یا برداشت نادرستی که از استفاده از این ترجمه ناشی شود، نیستیم.