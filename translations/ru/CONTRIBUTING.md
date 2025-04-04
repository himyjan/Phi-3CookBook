<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "212531c5722978740dcfb73e3995cbba",
  "translation_date": "2025-04-03T06:02:57+00:00",
  "source_file": "CONTRIBUTING.md",
  "language_code": "ru"
}
-->
# Участие в проекте

Этот проект приветствует вклад и предложения. Большинство вкладов требуют вашего согласия с Соглашением о лицензировании вклада (CLA), подтверждающим, что вы имеете право и действительно предоставляете нам права на использование вашего вклада. Подробности смотрите на [https://cla.opensource.microsoft.com](https://cla.opensource.microsoft.com).

Когда вы отправляете pull request, бот CLA автоматически определит, нужно ли вам предоставить CLA, и отметит PR соответствующим образом (например, проверка статуса, комментарий). Просто следуйте инструкциям, предоставленным ботом. Вам нужно будет сделать это только один раз для всех репозиториев, использующих наше CLA.

## Кодекс поведения

Этот проект принял [Кодекс поведения Microsoft Open Source](https://opensource.microsoft.com/codeofconduct/). Для получения дополнительной информации ознакомьтесь с [Часто задаваемыми вопросами о Кодексе поведения](https://opensource.microsoft.com/codeofconduct/faq/) или свяжитесь с [opencode@microsoft.com](mailto:opencode@microsoft.com) для любых дополнительных вопросов или комментариев.

## Предостережения при создании задач

Пожалуйста, не открывайте задачи на GitHub для общих вопросов поддержки, так как список GitHub должен использоваться для запросов новых функций и отчетов об ошибках. Это позволяет нам легче отслеживать реальные проблемы или баги в коде и отделять общие обсуждения от работы с кодом.

## Как внести вклад

### Рекомендации по Pull Requests

При отправке pull request (PR) в репозиторий Phi-3 CookBook, используйте следующие рекомендации:

- **Сделайте форк репозитория**: Всегда создавайте форк репозитория в свой аккаунт перед внесением изменений.

- **Разделяйте pull requests (PR)**:
  - Отправляйте каждый тип изменений в отдельном PR. Например, исправления багов и обновления документации должны быть отправлены в отдельных PR.
  - Исправления опечаток и небольшие обновления документации могут быть объединены в один PR, если это уместно.

- **Разрешайте конфликты слияния**: Если ваш pull request показывает конфликты слияния, обновите вашу локальную ветку `main`, чтобы она соответствовала основному репозиторию, перед внесением изменений.

- **Отправка переводов**: При отправке PR с переводом убедитесь, что папка перевода содержит переводы всех файлов из оригинальной папки.

### Рекомендации по переводу

> [!IMPORTANT]
>
> При переводе текста в этом репозитории не используйте машинный перевод. Предлагайте помощь с переводами только на тех языках, которыми вы владеете.

Если вы владеете языком, отличным от английского, вы можете помочь с переводом контента. Чтобы ваши переводы были правильно интегрированы, используйте следующие рекомендации:

- **Создайте папку для перевода**: Перейдите в соответствующую папку раздела и создайте папку перевода для языка, на который вы переводите. Например:
  - Для раздела "введение": `PhiCookBook/md/01.Introduce/translations/<language_code>/`
  - Для раздела "быстрый старт": `PhiCookBook/md/02.QuickStart/translations/<language_code>/`
  - Продолжайте этот шаблон для других разделов (03.Inference, 04.Finetuning и т.д.).

- **Обновите относительные пути**: При переводе добавьте `../../` в начало относительных путей в файлах Markdown, чтобы ссылки работали корректно. Например, измените следующим образом:
  - Измените `(../../imgs/01/phi3aisafety.png)` на `(../../../../imgs/01/phi3aisafety.png)`.

- **Организуйте переводы**: Каждый переведенный файл должен быть размещен в соответствующей папке перевода раздела. Например, если вы переводите раздел "введение" на испанский, создайте следующее:
  - `PhiCookBook/md/01.Introduce/translations/es/`.

- **Отправляйте полный PR**: Убедитесь, что все переведенные файлы для раздела включены в один PR. Мы не принимаем частичные переводы разделов. При отправке PR с переводом убедитесь, что папка перевода содержит переводы всех файлов из оригинальной папки.

### Рекомендации по написанию

Для обеспечения согласованности во всех документах используйте следующие рекомендации:

- **Форматирование URL**: Оборачивайте все URL в квадратные скобки, за которыми следуют круглые скобки, без лишних пробелов вокруг или внутри. Например: `[example](https://www.microsoft.com)`.

- **Относительные ссылки**: Используйте `./` для относительных ссылок на файлы или папки в текущем каталоге и `../` для ссылок на родительский каталог. Например: `[example](../../path/to/file)` или `[example](../../../path/to/file)`.

- **Не используйте локали, специфичные для страны**: Убедитесь, что ваши ссылки не содержат локали, специфичные для страны. Например, избегайте `/en-us/` или `/en/`.

- **Хранение изображений**: Храните все изображения в папке `./imgs`.

- **Описательные имена изображений**: Давайте изображениям описательные имена, используя английские символы, цифры и дефисы. Например: `example-image.jpg`.

## Рабочие процессы GitHub

Когда вы отправляете pull request, запускаются следующие рабочие процессы для проверки изменений. Следуйте инструкциям ниже, чтобы ваш pull request прошел проверки рабочего процесса:

- [Проверка сломанных относительных путей](../..)
- [Проверка URL без локалей](../..)

### Проверка сломанных относительных путей

Этот рабочий процесс проверяет, чтобы все относительные пути в ваших файлах были корректными.

1. Чтобы убедиться, что ваши ссылки работают правильно, выполните следующие действия в VS Code:
    - Наведите курсор на любую ссылку в ваших файлах.
    - Нажмите **Ctrl + Click**, чтобы перейти по ссылке.
    - Если вы нажимаете на ссылку, и она не работает локально, это вызовет сбой рабочего процесса и не будет работать на GitHub.

1. Чтобы исправить эту проблему, выполните следующие действия с использованием предложений путей, предоставляемых VS Code:
    - Введите `./` или `../`.
    - VS Code предложит вам выбрать из доступных вариантов на основе введенного.
    - Следуйте по пути, кликнув на нужный файл или папку, чтобы убедиться, что ваш путь корректен.

После добавления правильного относительного пути сохраните и отправьте ваши изменения.

### Проверка URL без локалей

Этот рабочий процесс проверяет, чтобы ни один веб-URL не содержал локали, специфичной для страны. Поскольку этот репозиторий доступен глобально, важно, чтобы URL не содержали локали вашей страны.

1. Чтобы убедиться, что ваши URL не содержат локалей страны, выполните следующие действия:

    - Проверьте текст типа `/en-us/`, `/en/` или любую другую локаль языка в URL.
    - Если их нет в ваших URL, вы пройдете эту проверку.

1. Чтобы исправить эту проблему, выполните следующие действия:
    - Откройте путь файла, указанный в рабочем процессе.
    - Удалите локаль страны из URL.

После удаления локали страны сохраните и отправьте ваши изменения.

### Проверка сломанных URL

Этот рабочий процесс проверяет, чтобы любой веб-URL в ваших файлах работал и возвращал статус-код 200.

1. Чтобы убедиться, что ваши URL работают корректно, выполните следующие действия:
    - Проверьте статус URL в ваших файлах.

2. Чтобы исправить сломанные URL, выполните следующие действия:
    - Откройте файл, содержащий сломанный URL.
    - Обновите URL на правильный.

После исправления URL сохраните и отправьте ваши изменения.

> [!NOTE]
>
> Могут быть случаи, когда проверка URL завершится неудачно, даже если ссылка доступна. Это может произойти по нескольким причинам, включая:
>
> - **Ограничения сети**: Серверы GitHub Actions могут иметь сетевые ограничения, препятствующие доступу к определенным URL.
> - **Проблемы с тайм-аутом**: URL, которые слишком долго отвечают, могут вызывать ошибку тайм-аута в рабочем процессе.
> - **Временные проблемы сервера**: Временные простои сервера или обслуживание могут сделать URL временно недоступным во время проверки.

**Отказ от ответственности**:  
Этот документ был переведен с использованием сервиса автоматического перевода [Co-op Translator](https://github.com/Azure/co-op-translator). Хотя мы стремимся к точности, пожалуйста, учитывайте, что автоматические переводы могут содержать ошибки или неточности. Оригинальный документ на его исходном языке следует считать авторитетным источником. Для критически важной информации рекомендуется профессиональный перевод человеком. Мы не несем ответственности за любые недоразумения или неправильные интерпретации, возникающие в результате использования данного перевода.