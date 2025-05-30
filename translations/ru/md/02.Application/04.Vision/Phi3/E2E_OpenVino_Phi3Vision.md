<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "d7d7afa242a4a041ff4193546d4baf16",
  "translation_date": "2025-05-07T13:44:26+00:00",
  "source_file": "md/02.Application/04.Vision/Phi3/E2E_OpenVino_Phi3Vision.md",
  "language_code": "ru"
}
-->
Это демо показывает, как использовать предобученную модель для генерации Python-кода на основе изображения и текстового запроса.

[Sample Code](../../../../../../code/06.E2E/E2E_OpenVino_Phi3-vision.ipynb)

Пошаговое объяснение:

1. **Импорты и настройка**:
   - Импортируются необходимые библиотеки и модули, включая `requests`, `PIL` для обработки изображений и `transformers` для работы с моделью и процессингом.

2. **Загрузка и отображение изображения**:
   - Файл изображения (`demo.png`) открывается с помощью библиотеки `PIL` и отображается.

3. **Определение запроса**:
   - Создается сообщение, которое включает изображение и просьбу сгенерировать Python-код для обработки изображения и сохранения результата с помощью `plt` (matplotlib).

4. **Загрузка процессора**:
   - `AutoProcessor` загружается из предобученной модели, расположенной в каталоге `out_dir`. Этот процессор будет обрабатывать текст и изображение.

5. **Создание запроса**:
   - Метод `apply_chat_template` используется для форматирования сообщения в запрос, подходящий для модели.

6. **Обработка входных данных**:
   - Запрос и изображение преобразуются в тензоры, которые модель может понять.

7. **Настройка аргументов генерации**:
   - Определяются параметры для процесса генерации модели, включая максимальное количество новых токенов и выбор способа выборки вывода.

8. **Генерация кода**:
   - Модель генерирует Python-код на основе входных данных и параметров генерации. `TextStreamer` используется для обработки вывода, пропуская запрос и специальные токены.

9. **Результат**:
   - Выводится сгенерированный код, который должен включать Python-код для обработки изображения и сохранения результата, как указано в запросе.

Это демо демонстрирует, как использовать предобученную модель с помощью OpenVino для динамической генерации кода на основе пользовательского ввода и изображений.

**Отказ от ответственности**:  
Этот документ был переведен с использованием сервиса автоматического перевода [Co-op Translator](https://github.com/Azure/co-op-translator). Несмотря на наши усилия обеспечить точность, просим учитывать, что автоматический перевод может содержать ошибки или неточности. Оригинальный документ на исходном языке следует считать авторитетным источником. Для критически важной информации рекомендуется обращаться к профессиональному человеческому переводу. Мы не несем ответственности за любые недоразумения или неправильные толкования, возникшие в результате использования данного перевода.