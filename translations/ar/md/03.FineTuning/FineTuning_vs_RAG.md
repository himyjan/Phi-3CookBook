<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "e4e010400c2918557b36bb932a14004c",
  "translation_date": "2025-03-27T15:41:21+00:00",
  "source_file": "md\\03.FineTuning\\FineTuning_vs_RAG.md",
  "language_code": "ar"
}
-->
## ضبط النموذج مقابل RAG

## توليد النص المعزز بالاسترجاع

RAG يعني استرجاع البيانات + توليد النصوص. يتم تخزين البيانات المنظمة وغير المنظمة الخاصة بالمؤسسة في قاعدة بيانات تعتمد على المتجهات. عند البحث عن محتوى ذي صلة، يتم العثور على الملخص والمحتوى المناسبين لتكوين سياق، ثم يتم دمج قدرة إكمال النص الخاصة بـ LLM/SLM لتوليد المحتوى.

## عملية RAG
![FinetuningvsRAG](../../../../translated_images/rag.36e7cb856f120334d577fde60c6a5d7c5eecae255dac387669303d30b4b3efa4.ar.png)

## ضبط النموذج
ضبط النموذج يعتمد على تحسين نموذج معين. لا يتطلب البدء من خوارزمية النموذج، ولكن يحتاج إلى تراكم البيانات بشكل مستمر. إذا كنت تبحث عن مصطلحات دقيقة وتعبيرات لغوية متخصصة في التطبيقات الصناعية، فإن ضبط النموذج سيكون الخيار الأفضل. ولكن إذا كانت بياناتك تتغير بشكل متكرر، فقد يصبح ضبط النموذج معقدًا.

## كيفية الاختيار
إذا كانت الإجابة تحتاج إلى إدخال بيانات خارجية، فإن RAG هو الخيار الأمثل.

إذا كنت بحاجة إلى إنتاج معرفة صناعية مستقرة ودقيقة، فإن ضبط النموذج سيكون خيارًا جيدًا. RAG يركز على استخراج المحتوى المناسب ولكنه قد لا يلتقط دائمًا الفروق الدقيقة المتخصصة.

ضبط النموذج يتطلب مجموعة بيانات عالية الجودة، وإذا كانت البيانات محدودة النطاق، فلن يكون هناك فرق كبير. RAG أكثر مرونة.  
ضبط النموذج يعتبر صندوقًا أسودًا، نوعًا من الميتافيزيقيا، ومن الصعب فهم آليته الداخلية. ولكن RAG يجعل من السهل العثور على مصدر البيانات، مما يساعد بشكل فعال في تعديل الهلوسة أو أخطاء المحتوى ويوفر شفافية أفضل.

**إخلاء المسؤولية**:  
تم ترجمة هذه الوثيقة باستخدام خدمة الترجمة بالذكاء الاصطناعي [Co-op Translator](https://github.com/Azure/co-op-translator). بينما نسعى لتحقيق الدقة، يُرجى العلم أن الترجمات الآلية قد تحتوي على أخطاء أو معلومات غير دقيقة. يجب اعتبار الوثيقة الأصلية بلغتها الأصلية المصدر الرسمي. للحصول على معلومات مهمة، يُوصى باللجوء إلى ترجمة بشرية احترافية. نحن غير مسؤولين عن أي سوء فهم أو تفسير خاطئ ينشأ عن استخدام هذه الترجمة.