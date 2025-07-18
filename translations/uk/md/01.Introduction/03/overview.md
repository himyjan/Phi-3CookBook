<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "f1ff728038c4f554b660a36b76cbdd6e",
  "translation_date": "2025-07-16T21:14:03+00:00",
  "source_file": "md/01.Introduction/03/overview.md",
  "language_code": "uk"
}
-->
У контексті Phi-3-mini, inference означає процес використання моделі для створення прогнозів або генерації результатів на основі вхідних даних. Дозвольте розповісти більше про Phi-3-mini та її можливості inference.

Phi-3-mini є частиною серії моделей Phi-3, випущених Microsoft. Ці моделі створені, щоб переосмислити можливості Малих Мовних Моделей (SLM).

Ось кілька ключових моментів про Phi-3-mini та її можливості inference:

## **Огляд Phi-3-mini:**
- Phi-3-mini має розмір параметрів 3,8 мільярда.
- Вона може працювати не лише на традиційних обчислювальних пристроях, а й на edge-пристроях, таких як мобільні пристрої та IoT-пристрої.
- Випуск Phi-3-mini дає змогу окремим користувачам і підприємствам розгортати SLM на різному апаратному забезпеченні, особливо в умовах обмежених ресурсів.
- Підтримує різні формати моделей, включно з традиційним форматом PyTorch, квантизованою версією формату gguf та квантизованою версією на основі ONNX.

## **Доступ до Phi-3-mini:**
Щоб отримати доступ до Phi-3-mini, ви можете використовувати [Semantic Kernel](https://github.com/microsoft/SemanticKernelCookBook?WT.mc_id=aiml-138114-kinfeylo) у додатку Copilot. Semantic Kernel загалом сумісний із Azure OpenAI Service, відкритими моделями на Hugging Face та локальними моделями.  
Також можна використовувати [Ollama](https://ollama.com) або [LlamaEdge](https://llamaedge.com) для виклику квантизованих моделей. Ollama дозволяє окремим користувачам викликати різні квантизовані моделі, а LlamaEdge забезпечує кросплатформену доступність для моделей GGUF.

## **Квантизовані моделі:**
Багато користувачів віддають перевагу використанню квантизованих моделей для локального inference. Наприклад, ви можете безпосередньо запускати Ollama run Phi-3 або налаштувати його офлайн за допомогою Modelfile. Modelfile вказує шлях до файлу GGUF та формат prompt.

## **Можливості генеративного ШІ:**
Поєднання SLM, таких як Phi-3-mini, відкриває нові можливості для генеративного ШІ. Inference — це лише перший крок; ці моделі можна використовувати для різних завдань у середовищах з обмеженими ресурсами, обмеженнями затримки та витрат.

## **Відкриття генеративного ШІ з Phi-3-mini: Посібник з inference та розгортання**  
Дізнайтеся, як використовувати Semantic Kernel, Ollama/LlamaEdge та ONNX Runtime для доступу та inference моделей Phi-3-mini, а також досліджуйте можливості генеративного ШІ у різних сценаріях застосування.

**Особливості**  
Inference моделі phi3-mini у:

- [Semantic Kernel](https://github.com/Azure-Samples/Phi-3MiniSamples/tree/main/semantickernel?WT.mc_id=aiml-138114-kinfeylo)  
- [Ollama](https://github.com/Azure-Samples/Phi-3MiniSamples/tree/main/ollama?WT.mc_id=aiml-138114-kinfeylo)  
- [LlamaEdge WASM](https://github.com/Azure-Samples/Phi-3MiniSamples/tree/main/wasm?WT.mc_id=aiml-138114-kinfeylo)  
- [ONNX Runtime](https://github.com/Azure-Samples/Phi-3MiniSamples/tree/main/onnx?WT.mc_id=aiml-138114-kinfeylo)  
- [iOS](https://github.com/Azure-Samples/Phi-3MiniSamples/tree/main/ios?WT.mc_id=aiml-138114-kinfeylo)  

Підсумовуючи, Phi-3-mini дозволяє розробникам досліджувати різні формати моделей і використовувати генеративний ШІ у різних сценаріях застосування.

**Відмова від відповідальності**:  
Цей документ було перекладено за допомогою сервісу автоматичного перекладу [Co-op Translator](https://github.com/Azure/co-op-translator). Хоча ми прагнемо до точності, будь ласка, майте на увазі, що автоматичні переклади можуть містити помилки або неточності. Оригінальний документ рідною мовою слід вважати авторитетним джерелом. Для критично важливої інформації рекомендується звертатися до професійного людського перекладу. Ми не несемо відповідальності за будь-які непорозуміння або неправильні тлумачення, що виникли внаслідок використання цього перекладу.