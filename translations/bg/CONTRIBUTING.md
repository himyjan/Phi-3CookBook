<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "9f71f15fee9a73ecfcd4fd40efbe3070",
  "translation_date": "2025-05-09T03:44:46+00:00",
  "source_file": "CONTRIBUTING.md",
  "language_code": "bg"
}
-->
# Contributing

Този проект приветства приноси и предложения. Повечето приноси изискват да се съгласите с
Contributor License Agreement (CLA), който декларира, че имате правото и действително ни предоставяте
правата да използваме вашия принос. За подробности посетете [https://cla.opensource.microsoft.com](https://cla.opensource.microsoft.com)

Когато изпратите pull request, CLA бот автоматично ще определи дали трябва да предоставите
CLA и ще отбележи PR съответно (например, статус проверка, коментар). Просто следвайте инструкциите,
дадени от бота. Трябва да го направите само веднъж за всички репозиторита, използващи нашия CLA.

## Code of Conduct

Този проект е приел [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
За повече информация прочетете [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) или се свържете с [opencode@microsoft.com](mailto:opencode@microsoft.com) при допълнителни въпроси или коментари.

## Cautions for creating issues

Моля, не отваряйте GitHub issues за общи въпроси за поддръжка, тъй като списъкът в GitHub трябва да се използва за заявки за нови функции и доклади за грешки. По този начин можем по-лесно да следим реалните проблеми или бъгове в кода и да държим общата дискусия отделена от самия код.

## How to Contribute

### Pull Requests Guidelines

Когато изпращате pull request (PR) към репозитория Phi-3 CookBook, моля използвайте следните указания:

- **Fork на репозитория**: Винаги правете fork на репозитория във вашия акаунт преди да направите модификациите си.

- **Отделни pull requests (PR)**:
  - Изпращайте всеки тип промяна в отделен pull request. Например, корекции на бъгове и обновления на документацията трябва да се изпращат в отделни PR.
  - Корекции на правописни грешки и дребни обновления на документацията могат да се комбинират в един PR, когато е уместно.

- **Разрешаване на конфликти при сливане**: Ако вашият pull request показва конфликти при сливане, обновете локалния си `main` клон, за да съвпада с основния репозитория преди да направите модификациите.

- **Изпращане на преводи**: При изпращане на преводен PR, уверете се, че папката с преводи съдържа преводи за всички файлове в оригиналната папка.

### Translation Guidelines

> [!IMPORTANT]
>
> При превод на текст в този репозитория, не използвайте машинен превод. Доброволно се ангажирайте с преводи само на езици, на които владеете.

Ако владеете чужд език, различен от английски, можете да помогнете с превода на съдържанието. Следвайте тези стъпки, за да гарантирате, че приносите ви за превод са правилно интегрирани, моля използвайте следните указания:

- **Създайте папка за превод**: Навигирайте до съответната секция и създайте папка за превод на езика, на който допринасяте. Например:
  - За секцията introduction: `PhiCookBook/md/01.Introduce/translations/<language_code>/`
  - За секцията quick start: `PhiCookBook/md/02.QuickStart/translations/<language_code>/`
  - Продължете този модел и за другите секции (03.Inference, 04.Finetuning и т.н.)

- **Обновете относителните пътища**: При превод, коригирайте структурата на папките, като добавите `../../` в началото на относителните пътища във файловете markdown, за да гарантирате, че линковете работят правилно. Например, променете:
  - От `(../../imgs/01/phi3aisafety.png)` към `(../../../../imgs/01/phi3aisafety.png)`

- **Организирайте преводите си**: Всеки преведен файл трябва да бъде поставен в съответната папка за превод на секцията. Например, ако превеждате секцията introduction на испански, създайте:
  - `PhiCookBook/md/01.Introduce/translations/es/`

- **Изпратете пълен PR**: Уверете се, че всички преведени файлове за една секция са включени в един PR. Не приемаме частични преводи за секция. При изпращане на преводен PR, уверете се, че папката за превод съдържа преводи за всички файлове в оригиналната папка.

### Writing Guidelines

За да се осигури последователност във всички документи, моля използвайте следните указания:

- **Форматиране на URL**: Обвивайте всички URL в квадратни скоби, последвани от кръгли скоби, без допълнителни интервали около или вътре в тях. Например: `[example](https://www.microsoft.com)`.

- **Относителни линкове**: Използвайте `./` за относителни линкове към файлове или папки в текущата директория и `../` за такива в родителска директория. Например: `[example](../../path/to/file)` или `[example](../../../path/to/file)`.

- **Не използвайте локализирани по държава езикови настройки**: Уверете се, че вашите линкове не включват локали специфични за държава. Например, избягвайте `/en-us/` или `/en/`.

- **Съхранение на изображения**: Съхранявайте всички изображения в папката `./imgs`.

- **Описателни имена на изображения**: Давайте описателни имена на изображенията, използвайки английски букви, цифри и тирета. Например: `example-image.jpg`.

## GitHub Workflows

Когато изпратите pull request, следните workflows ще се задействат, за да валидират промените. Следвайте инструкциите по-долу, за да гарантирате, че вашият pull request преминава проверките на workflow:

- [Check Broken Relative Paths](../..)
- [Check URLs Don't Have Locale](../..)

### Check Broken Relative Paths

Този workflow гарантира, че всички относителни пътища във вашите файлове са коректни.

1. За да сте сигурни, че линковете ви работят правилно, изпълнете следните действия във VS Code:
    - Задръжте курсора върху някой линк във файловете си.
    - Натиснете **Ctrl + Click**, за да отидете на линка.
    - Ако кликнете на линк и той не работи локално, това ще задейства workflow и няма да работи и в GitHub.

1. За да поправите този проблем, изпълнете следните действия, използвайки предложенията за пътища от VS Code:
    - Въведете `./` или `../`.
    - VS Code ще ви подскаже от наличните опции, базирани на това, което сте въвели.
    - Следвайте пътя, като кликнете на желания файл или папка, за да сте сигурни, че пътят е коректен.

След като добавите правилния относителен път, запазете и изпратете промените си.

### Check URLs Don't Have Locale

Този workflow гарантира, че нито един уеб URL не съдържа локализация, специфична за държава. Тъй като този репозитория е достъпен глобално, е важно URL адресите да не съдържат локализация за дадена държава.

1. За да проверите, че вашите URL не съдържат локализации на държави, изпълнете следните действия:

    - Проверете за текст като `/en-us/`, `/en/` или други езикови локали в URL адресите.
    - Ако тези не са налични във вашите URL, ще преминете проверката.

1. За да поправите този проблем, изпълнете следните действия:
    - Отворете пътя на файла, посочен от workflow.
    - Премахнете локализацията на държавата от URL адресите.

След като премахнете локализацията, запазете и изпратете промените си.

### Check Broken Urls

Този workflow гарантира, че всеки уеб URL във файловете ви работи и връща статус код 200.

1. За да проверите дали URL адресите ви работят правилно, изпълнете следните действия:
    - Проверете статуса на URL адресите във вашите файлове.

2. За да поправите счупени URL адреси, изпълнете следните действия:
    - Отворете файла, който съдържа счупения URL.
    - Обновете URL адреса с правилния.

След като поправите URL адресите, запазете и изпратете промените си.

> [!NOTE]
>
> Може да има случаи, в които проверката на URL адресите се провали, въпреки че линкът е достъпен. Това може да се случи по няколко причини, включително:
>
> - **Ограничения на мрежата:** Сървърите на GitHub actions може да имат мрежови ограничения, които пречат на достъпа до определени URL адреси.
> - **Проблеми с таймаут:** URL адреси, които отнемат твърде много време за отговор, могат да предизвикат грешка за изтичане на времето в workflow.
> - **Временни проблеми със сървъра:** Понякога сървърът може да бъде временно недостъпен поради техническа поддръжка или прекъсвания по време на валидацията.

**Отказ от отговорност**:  
Този документ е преведен с помощта на AI преводаческа услуга [Co-op Translator](https://github.com/Azure/co-op-translator). Въпреки че се стремим към точност, моля, имайте предвид, че автоматизираните преводи могат да съдържат грешки или неточности. Оригиналният документ на неговия език трябва да се счита за авторитетен източник. За критична информация се препоръчва професионален човешки превод. Ние не носим отговорност за никакви недоразумения или неправилни тълкувания, възникнали в резултат на използването на този превод.