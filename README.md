# FUNBOX
# [Квалификационные задания для Ruby разработчиков](https://dl.funbox.ru/qt-ruby.pdf)

# Level 1
## начало в 2023-10-09 16:30

### 1. Чем отличается proc от lambda?
   lamdba ведёт себя как обычный метод:
   * вызывает ошибку при несовпадении количества переданных аргументов
   * возвращет управление и результат в место, откуда была вызвана,\
     при неявном и явном возврате с помощью return
   
   proc имеет следующие отличия:
   * игнорирует несовпадение количества переданных аргументов при вызове, заменяя недостающие на nil и игнорируя лишние
   * при неявном возврате значения - возвращет управление и результат в место, откуда была вызвана
   * при явном возврате с помощью return - осуществляет выход из метода,\
     где был вызван, передавая результат proc, как результат вызывающего метода


### 2. Чем отличается && от and?
   приоритет `&&` выше чем у `and`. На практике `and` стараются не использовать. Кажется больше ничем.

### 3. Какие плюсы и минусы Ruby по сравнению с другими современными языками программирования вы выделяете для себя?
   Плюсы:
   * богатая и функциональная stdlib 
   * лаконичность самого языка (после perl и javascript это `полёт шмеля` для меня)
   * хорошая обратная совместимость
   * внятная объектная система и система типов\
     (особенно в 2.4+, я про объединение Fixnum и Bignum в Integer и поддержку огромных Float)
   * наличие rails

   Минусы:
   * Наличие GIL в варианте MRI
   * Недостаточная производительность для нагруженных проектов в варианте MRI
   * Пока ещё недостаточное проникновение fibers в экосистему ruby и rails, для исправления недостатка выше

### 4. В каком случае вы бы стали использовать Ruby on Rails для разработки web-приложения, а в каком — другой фреймворк? 
   * На данный момент я бы выбрал Ruby on Rails как дефолтный фреймворк\
     для задач любой сложности, пока речь идёт о проектах в рамках монолитов,\
     либо несильно нагруженных микросервисов, т.к. схожая структура кодовой базы снижает\
     когнитивную нагрузку и даёт возможность переключаться с проекта на проект\
     без больших трудозатрат
   * Одним из вариантов, где можно было бы отказаться от Ruby on Rails в пользу других фреймворков:\
     тяжелонагруженный микросервис с множеством инстансов,
     где требуется многозадачность или требования по потребляемой памяти\
     от ActiveRecord или Rails в целом, будут превалировать над удобством поддержания однородной кодовой базы

### 5. Вы запустили большой и сложный ruby скрипт на вашем любимом дистрибутиве Linux, и в процессе работы он намертво «зависает». Как найти причину сбоя? 
   * чтение логов, если они есть
   * отладочная печать для поиска примерного места проблемы
   * точки останова через binding.irb для разбирательства по месту проблемы
   * поиск логической ошибки в коде, если всё что выше не помогает

### 6. Вы запустили большой и сложный ruby скрипт на вашем любимом дистрибутиве Linux, и в процессе работы он умирает, исчерпав память, хотя, казалось бы, не должен. Как найти причину сбоя? 
   * применял бы все методы выше для начала, если речь про разово запускаемый скрипт (отработал и завершился)
   * если речь о фактическом фоновом процессе, то пришлось бы прибегнуть к помощи профилировщиков,\
     но тут речь уже больше о практической работе с ними.\
     Был опыт на несвязанных с ruby проектах, но не очень большой\
     Просто знаю в какую сторону можно посмотреть, при такой проблеме

### 7. Некоторые проекты (Rails и не только) мы запускаем под JRuby. Как вы думаете, почему?
   1. Доступ ко всей JVM экосистеме, как самый главный фактор\
      Особенно, если требуется что-то уникальное или явно превосходящее native-ruby аналоги из библиотек этой экосистемы
   2. Несколько большая производительность по cpu, в обмен на больший расход памяти
   3. Отсутствие GIL

### 8. Какие плюсы и минусы библиотеки ActiveRecord в Rails вы знаете?
   
   Плюсы ActiveRecord:
   * Это отличная orm для простых или не очень сложных запросов
   * Высокая степень удобства, при работе с данными, пока orm тебе не мешает
   * Абстрагирование над реализацией БД (ну, такое себе, но плюсом можно считать)
   * Миграции
   * Валидации
   * Связи и отношения, не завязанные на их реализацию в конкретной БД
   * Документация

   Минусы ActiveRecord:
   * Завязка на Rails
   * Потребляемая память - больше чем у конкурентов
   * Всегда нужно отслеживать конструируемые запросы
   * Какие-то реально сложные запросы проще написать на голом SQL
   * Проблемы с многопоточкой (я это знаю, но пока не сталкивался)
   * Сложность в обучении (в целом это проблема CoC всё же)
   Общая для всех orm проблема: завязка на конкретную orm

   Какие альтернативы существуют?:
   * Ruby Object Mapper
   * Sequel

   В чем их плюсы и минусы? Какие из них вы использовали?:
   На данный момент я не смотрел в сторону альтернативных ORM,\
   т.к. прокачиваю навыки конкретно Rails стека и хватало ActiveRecord

## закончил в 2023-10-09 18:30

# Level 2

## начало в 2023-10-09 19:10

### 1. Есть Rails приложение, развернутое на N серверах. В приложение нужно добавить загрузку и хранение аватаров.
   1. Какую библиотеку использовать?\
      в рамках rails я бы использовал Active Storage, т.к. есть поддержка внешних сервисов\
      и не нужны лишние зависимости
   2. Куда складывать аватарки и почему?\
      в распределённом приложении лучше все статические данные вынести на CDN,\
      которые удобно прикручиваются с помощью Active Storage.
      Но было бы неплохо оставить fallback на свою локальную копию этих данных
   3. Как и зачем нужно обрабатывать загружаемые аватарки?
      Для валидации и обработки загружаемых изображений, чтобы ону удовлетворяли требования заказчика
   4. Какие проверки загружаемого файла и когда (например, на клиенте, в Nginx или Rails) лучше всего производить?\
      Фронт и nginx
      * размер
      * расширение
      * mime-type
      
      Rails
      * размер
      * расширение
      * mime-type
      * вредоносы в файлах
      
   5. Как отдавать аватарки пользователю? Какие плюсы и минусы описанного подхода?

      Дистрибуция через внешний web-сервер: 
      * скорость
      * снижение нагрузки на rails-сервер
      * нет возможности кастомизации по url

      Отдача через внешний rails-сервер:
      * повышает нагрузку на rails-сервер
      * работает из коробки
      * кастомизация по url
   6. Какие еще проблемы и вопросы могут возникнуть при реализации такой функциональности?
      * Ограничение доступа к аватаркам только для авторизованных пользователей в случае внешнего web-сервера
      * Обработка большого количества загрузок и высокая доступность для rails-сервера
      * Снижение нагрузки на rails-сервер, принося в жертву безопасность, при выносе на внешний web-сервер

### 2. Есть большое приложение с десятками тысяч тестов. Как лучше всего организовать код и тесты, чтобы добиться максимальной скорости прогона тестов?
   * Самый базовый вариант при написании тестов - запускать их поштучно, для проверки, а не все вместе
   * Если не требуется большой объём данных для каких-то тестов,\
     то нужно заменить для них фабрики на статичные фикстуры
   * Предзагрузка всех тестовых данных перед полным прогоном тестов
   * Запуск тестов в транзакционном режиме, чтобы не портить предзагруженные данные
   * Обязательно соблюдать независимость тестов друг от друга и от порядка запуска
   * Параллельный запуск тестов
   * Если прогон всех тестов занимает такое значительное время, то это инфраструктурный вопрос\
     Частично можно решить выносом окончательного варианта ветки для проверки перед слиянием на CI/CD
   * Мониторить время выполнения тестов на CI/CD
   
## закончил в 2023-10-09 20:10

# Level 3
### 1. Напишите скрипт nmax, который делает следующее:
   [assignment-funbox-nmax](https://github.com/qsimpleq/assignment-funbox-nmax)
   ### начало в 2023-10-10 13:30
   * читает из входящего потока текстовые данные
   * по завершении ввода выводит n самых больших целых чисел, встретившихся в полученных текстовых данных.
   
   Дополнительные указания:
   * числом считается любая непрерывная последовательность цифр в тексте
   * известно, что чисел длиннее 1000 цифр во входных данных нет
   * число n должно быть единственным параметром скрипта
   * код должен быть покрыт тестами
   * код должен быть оформлен в виде гема (содержащего исполняемый файл, код модулей и т.д.)
   * плюсом является размещение на Github и интеграция с Travis CI.
   
   Пример запуска:
   ```
   cat sample_data_40GB.txt | nmax 10000
   ```
   ### закончил в 2023-10-14 14:51
   На первый рабочий прототип потратил ~4 часа\
   Всего чистого времени ~16 часов

### 2. Реализуйте web-приложение (Rails проект), которое удовлетворяет нижеизложенным требованиям:
   [assignment-funbox-rails](https://github.com/qsimpleq/assignment-funbox-rails)   
   * Приложение содержит две страницы: / и /admin
   * На странице / отображается текущий курс доллара к рублю, известный приложению.
   * Приложение фоновым скриптом периодически обновляет курс из любого\
     выбранного вами доступного источника (сайт CBR, главная страница http://www.rbc.ru, и т.д.).
   * При обновлении курса в приложении он обновляется на всех открытых в текущий момент страницах /.
   * На странице /admin находится форма, содержащая поле для ввода числа, поле для ввода даты-времени и сабмит.
   * При сабмите введенное число делается форсированным курсом до введённого\
     времени, т.е. до этого времени реальный курс игнорируется, вместо него\
     страницах / отображается форсированный курс.
   * Страница /admin «помнит» введенные предыдущий раз значения, они
     отображаются уже введенными при загрузке страницы.
   * При сабмите форсированного курса он, конечно же, cразу обновляется на всех\
     открытых страницах /. При истечении времени действия форсированного\
     курса на всех страницах начинает отображаться реальный курс.
   * Форма содержит разумные валидации.
   * Внешний вид приложения должен быть аккуратным в рамках разумного\
     (например, использовать Twitter Bootstrap).
   * Плюсом будет использование какого-либо JS-фреймворка на клиентской стороне.
   * Web-приложение должно корректно работать в браузерах Firefox и Chrome последних версий.
   * Код должен быть покрыт тестами.
   * Все необходимое для локального запуска приложения должно быть оформлено в виде Procfile-а для Foreman.
