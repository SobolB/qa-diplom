# Дипломный проект профессии «Тестировщик»

Дипломный проект представляет собой автоматизацию тестирования комплексного сервиса, взаимодействующего с СУБД и API Банка.

## Описание приложения

### Бизнес часть

Приложение представляет из себя веб-сервис.

![](pic/service.png)

Приложение предлагает купить тур по определённой цене с помощью двух способов:
1. Обычная оплата по дебетовой карте
1. Уникальная технология: выдача кредита по данным банковской карты

Само приложение не обрабатывает данные по картам, а пересылает их банковским сервисам:
* сервису платежей (далее - Payment Gate)
* кредитному сервису (далее - Credit Gate)

Приложение должно в собственной СУБД сохранять информацию о том, каким способом был совершён платёж и успешно ли он был совершён (при этом данные карт сохранять не допускается).

*Важно: в реальной жизни приложение не должно через себя даже пропускать данные карт, если у него нет PCI DSS, но мы сделали именно так 😈.*

### Техническая часть

Само приложение расположено в файле [`aqa-shop.jar`](aqa-shop.jar) и запускается стандартным способом `java -jar aqa-shop.jar` на порту 8080.

В файле [`application.properties`](application.properties) приведён ряд типовых настроек:
* учётные данные и url для подключения к СУБД
* url-адреса банковских сервисов

### СУБД

Заявлена поддержка двух СУБД (вы это должны проверить):
* MySQL
* PostgreSQL

Учётные данные и url для подключения задаются в файле [`application.properties`](application.properties).

### Банковские сервисы

Доступа к живым банковским сервисам вам не дают, поэтому разработчики подготовили симулятор банковских сервисов, который может принимать запросы в нужном формате и генерировать ответы.

Симулятор написан на Node.js, поэтому для запуска вам нужен либо Docker, либо установленный Node.js. Симулятор расположен в каталоге [gate-simulator](gate-simulator). Для запуска необходимо перейти в этот каталог. 

Запускается симулятор командой `npm start` на порту 9999.

Симулятор позволяет для заданного набора карт генерировать предопределённые ответы.

Набор карт представлен в формате JSON в файле [`data.json`](gate-simulator/data.json).

Обратите внимание, разработчики сделали один сервис, симулирующий и Payment Gate, и Credit Gate.

## Задача

Ваша ключевая задача - автоматизировать сценарии (как позитивные, так и негативные) покупки тура.

Задача разложена на 4 этапа:
1. Планировании автоматизации тестирования
1. Непосредственно самой автоматизации
1. Подготовке отчётных документов по итогам автоматизированного тестирования
1. Подготовка отчётных документов по итогам автоматизации

Все материалы (документы, авто-тесты, открытые issue, отчёты и т.д.) должны быть размещены в одном публичном репозитории, ссылку на который вы и будете отправлять дипломному руководителю.

### Планирование

После начала работы над дипломом в течение 3 рабочих дней вы должны сдать дипломному руководителю план автоматизации, в котором описано:

* Перечень автоматизируемых сценариев
* Перечень используемых инструментов с обоснованием выбора
* Перечень и описание возможных рисков при автоматизации
* Интервальная оценка с учётом рисков (в часах)
* План сдачи работ (когда будут авто-тесты, результаты их прогона и отчёт по автоматизации)

Отчёт оформляется в виде файла с именем `Plan.md` и заливается в репозиторий вашего проекта.

### Автоматизация

На этом этапе вы непосредственно пишете авто-тесты и прогоняете их. Требования по подключению CI нет, но есть требования к самим тестам:
* Обязательно должны быть тесты UI
* Обязательно должны быть репорты (Gradle/Allure/Report Portal)
* Обязательно должны быть запросы в базу, проверяющие корректность внесения приложением информации

Код авто-тестов заливается в репозиторий вашего проекта вместе с отчётными документами, всеми необходимыми для запуска файлами и конфигурациями.

В файле `README.md` должна быть описана процедура запуска авто-тестов (если для запуска необходимо заранее установить, настроить, запустить какое-то ПО - это тоже должно быть описано).

**Важно: дипломный руководитель не будет за вас "допридумывать" как запускать ваши тесты, если после `git clone` и выполнения шагов, описанные в `README.md` авто-тесты не запускаются, то диплом отправляется на доработку.**

### Отчётные документы по итогам тестирования

В качестве отчётных документов прикладываются issue со скриншотами и описанием багов + формируется документ `Report.md`, в котором содержится отчёт о проведённом тестировании:
* Краткое описание
* Количество тест-кейсов
* % успешных/не успешных
* Общие рекомендации

Не забудьте о том, что помимо документа, в систему автоматизации должны быть интегрированы отчёты: Gradle, Allure или Report Portal.

### Отчётные документы по итогам автоматизации

В качестве отчётных документов формируется документ `Summary.md`, в котором содержится отчёт о проведённой автоматизации:
* Что было запланировано и что было сделано
* Причины, по которым что-то не было сделано
* Сработавшие риски
* Общий итог по времени (сколько запланировали/сколько потратили с обоснованием расхождения)

## О документах

Важно: когда мы просим вас написать любые документы - мы не требуем творений на 10 страниц, документ должен, максимум, занимать один лист A4.

## Expert Level

*Важно: выполнение или не выполнение этого раздела не влияет на получение диплома*

Если вы чувствуете в себе силы, мы предлагаем вам попробовать интегрировать всю систему с Appveyor CI.

Немного подсказок:
* на Appveyor есть Docker (и, возможно даже, Docker Compose)
* на Appveyor предустановлены Node.js, MySQL, PostgreSQL
* вы можете вставлять простейшие `sleep`'ы прямо в сценариях командной строки, чтобы дать "подняться" СУБД, SUT или симулятору (хотя есть и техники получше)

Если вы это сделаете, не забудьте выставить бейджик сборки.

## Как правильно работать над дипломом?

Что следует делать, чтобы все получилось: 
1. Попробовать найти ответ сначала самому в интернете. Ведь, именно скилл поиска ответов пригодится вам на первой работе. И только после этого спрашивать дипломного руководителя.
1. В одном вопросе должна быть заложена одна проблема.
1. Как правильно оформлять вопросы:
    - публикуете самую последнюю версию вашего кода на GitHub
    - включаете в репозитории Issues
    - заводите новое Issue, в котором пишете:
        - в чём проблема;
        - прикладываете скриншот (чтобы все понимали куда смотреть);
        - если в консоли любого сервиса есть ошибки - не поленитесь скопировать их тоже (не отпринскринить, а скопировать - Ctrl + C, Ctrl + V).
1. Начинать работу над дипломом как можно раньше! Чтобы было больше времени на правки. 
1. Делать диплом по частям, а не все сразу. Иначе, есть шанс, что нужно будет все переделывать :)  

Что следует делать, чтобы ничего не получилось: 
1. Никому ничего не говорить.
1. Писать вопросы вида “Я не знаю, что делать. Я ничего не понял. Ничего не работает. Не запускается. Всё работало, а теперь не работает. Да и вообще не работало.”
1. Присылать скриншоты (или ещё хуже - фотографии экрана), не показывая код.
1. Откладывать диплом на потом.
1. Ждать того, что оно "заработает само".
1. Ждать ответ на свой вопрос моментально. Дипломные руководители - работающие специалисты, которые занимаются кроме преподавания, своими проектами. Их время ограничено, поэтому постарайтесь задавать правильные вопросы, чтобы получать быстрые ответы! 

## Спойлеры

<details>
   <summary>Спойлеры</summary>

   Смотреть спойлеры не хорошо 😈!

   Но раз уж вы посмотрели, то вот вам подсказки:
   * приложение просто усыпано багами - от безобидных до супер-критичных, поэтому если вы не нашли ни одного, то плохо искали
   * если есть баги, то тесты не должны быть зелёными
   * если есть баги, то должны быть баг-репорты в issue
   * обращайте внимание на все баги (особенно внимательно смотрите на обработку платежей и их фиксацию)
</details>
