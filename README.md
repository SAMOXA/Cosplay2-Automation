# Предыстория от основателя

Однажды наш тёплый ламповый [Аниме-фест Yuki no Odori](http://tulafest.ru) решил переехать на убер-крутую
платформу для организации Аниме-фестов [Cosplay2](http://cosplay2.ru). Мы начали принимать заявки, а потом выяснилось,
что программа [Генератор слайдов](https://vk.com/cosplay2ru?w=wall-64774987_208%2Fall), которая по идее призвана
скачивать файлы участников не очень то работает... А еще, очень очень хотелось иметь все данные заявок данные в
одной базе данных, чтобы писать к ней SQL-запросы и генерировать всякие полезные списки (встроенная генерилка списков
гибкостью не блещет). Так и родился этот репозиторий.

# Основные скрипты
* [get_data.py](get_data.py)
    * Загрузка всех данных из всех заявок на фестиваль.
    * Генерация базы данных SQLite из этих данных.
* [get_files.py](get_files.py)
    * Загрузка всех файлов из всех заявок.
    * Для каждой номинации своя папка 
    * Для сбора всех файлов в одну папку для [FestEngine](https://github.com/Himura2la/FestEngine), применяется скрипт [files_extractor.py](files_extractor.py)
    * Имена файлов составляются из номера заявки и названия карточки. Если у вас нет названий карточек, сгенерируйте их или нагородите LEFT JOIN'ов в SQL (не рекомендуется, но вот [пример](https://github.com/Himura2la/Cosplay2-Downloader/blob/cr17/get_files.py#L51)).
* [image_list_gen.py](image_list_gen.py) 
    * Генерилка списка путей к картинкам для вставки в CSV файл, который
используется для определения переменных фотошопе при генерации задников.
    * Если вы ничего не поняли, [начните отсюда](http://www.richmediacs.com/user_manuals/RMCS_PS_Training/Using%20PS%20Variables/UsingVariablesInPS_EXTERNAL.html)
* [mktex.py](mktex.py)
    * Открытая часть нашего с [@Oreolek](https://github.com/Oreolek) секретного проекта для фотовыставки.
    * Пишите [в Gitter](https://gitter.im/FestEngine/Lobby?utm_source=share-link&utm_medium=link&utm_campaign=share-link) за подробностями.

## Папка `etc`
Тут хранятся легаси и ad-hoc скрипты для специфических задач.

* [gimp-scripts](etc/gimp-scripts) -- попытки привнести в GIMP работу с переменными данными. Использовалось только на одном фесте, а потом я узнал про переменные в фотошопе.
* [checker.py](etc/checker.py) -- скрипт, проверяющий все ли файлы на месте (обязательно перепровертье вручную в самом
конце, **это архи-важно**. Я уже два феста подряд просераю AMV-шки из за пренебрежения ручной проверкой). Возможно легаси, вроде бы там сейчас на каждом шагу проверки.
* [compare_folders.py](etc/compare_folders.py) - скрипт сверки папок, можете заменить на [meld](http://meldmerge.org/) или обычный diff. А можете нет.
* [gui.py](etc/gui.py) - Заготовка GUI на wxWidgets. Очень легаси, но убивать жалко, а читать код страшно.
* [regex_renamer.py](etc/regex_renamer.py) - переименователь по регэкспам, можно заменить на GNU reanme. Но не нужно.
* [sql_query.py](etc/sql_query.py) - выполняет SQL в указанной базе SQLite. Там есть очень полезная фича - делать плоские тексты с длинными полями, для пожеланий по стаффу волонтёрам и светосценариев.
* [csv_renamer.py](etc/csv_renamer.py) - для фестов, которые не через к2. Делает имена файлов для FestEngine из таблицы
* [program_cleaner.py](etc/program_cleaner.py) -- скрипт для очистки программы феста от лишних строк (для сверки выложенной
ВК программы с программой, из раздела к2 "Планирование расписания").
* Куча полезных SQL запросов, можно хватать идеи по работе с БД. Особенно полезен файл [задники.sql](sql/задники.sql),
мне очень мешало то, что я вспомнил про **JOIN** не сразу и по-началу городил **CREATE TEMP TABLE AS SELECT**...
* Про остальное лень рассказывать, вам это скорее всего не нужно.

# Как запускать
* Cкопировать `config.yml.example` в `config.yml` и отредактировать под себя. Там есть комментарии.
* Создать папку `домен_фестиваля`, указать её в конфиге
* Запустить `python3 get_data.py`
* Проверить, что всё в порядке.

Если стрёмно хранить пароль админа без шифрования, можно его не вводить в конфиге, тогда программа будет сама спрашивать (не чаще 1 раза в ~10 минут).


# Благодарности от основателя

* [Евгений Политов](https://vk.com/snark13) за создание и посильную поддержку сайта [Cosplay2](http://cosplay2.ru). Всегда донатьте ему больше, чем Ваша стоимость SMS.
* [@Oreolek](https://github.com/Oreolek) за вклад в автоматизацию на фестах и конвентах!

# Смежные проекты

* [FestEngine](https://github.com/Himura2la/FestEngine)
    * Устали от "звукач говно"? 
    * Работаете в режиме одного монитора и зрители видят вашу мышку?
    * Надоело путать и терять треки во время шоу?
    * У нас есть решение!