<p align="right">
<a href="README.md">English description</a> | Описание на русском
</p>

# TARS-CLI

[![NPM version][npm-image]][npm-url] [![Downloads][downloads-image]][npm-url] [![Build Status][travis-image]][travis-link] [![Dependency Status][deps-image]][deps-link] [![Gitter][gitter-image]][gitter-link]

TARS-CLI — Command Line Interface для сборщика верстки [TARS](https://github.com/tars/tars/blob/master/README_RU.md).

Основная проблема при разработке верстки с помощью TARS — необходимость каждый раз устанавливать все npm-зависимости. Каждый проект в результате занимает больше 200 МБ. Чтобы упростить процедуру инициализации проекта и облегчить работу с TARS в целом был создан TARS-CLI. Вся основная документация по TARS находится в оригинальном репозитории [TARS](https://github.com/tars/tars/blob/master/README_RU.md).

TARS-CLI — это только интерфейс к основному сборщику, который позволяет:

* Инициализировать проект.
* Запустить dev-сборку с перезагрузкой браузера и открытием тунеля во внешний веб.
* Запустить build-сборку с минифицированными файлами или в режиме release.
* Добавить модуль с различным набором файлов.
* Добавить страницу, как пустую, так и копию существующей.

Если у вас возникли проблемы при работе с TARS-CLI, прошу ознакомится с разделом [troubleshooting](#troubleshooting).

## Установка

Для корректной работы необходимо установить TARS-CLI глобально:

`npm i -g tars-cli`

Возможно потребуются права суперюзера. Но желательно настроить систему так, чтобы этого не требовалось.

## Команды TARS-CLI

Все команды запускаются по шаблону:

`tars` + `command-name` + `flags`

В любой момент можно запустить `tars --help` или `tars -h` или просто `tars`, без дополнительных комманд и флагов. Данная команда выведет информацию о всех доступных командах. Также можно добавить ключ `--help` или `-h` к любой команде, чтобы получить наиболее полное описание этой команды.

`tars -v` или `tars --version` выведет текущую установленную версию TARS-CLI. Также будет выведена информация по обновлению, если оно доступно.

Практически во всех командах доступен интерактивный режим. В данном режиме вы сможете взаимодействовать с CLI через подобие графического интерфейса. При использовании интерактивного режима вам не нужно знать, какие флаги за что отвечают, так как вы общаетесь с CLI на естественном языке. Интерактивный режим легко отключить, если вам необходимо проводить автоматические тестирование или что-то еще, что не требует присутствие человека.

### Command list

* [tars init](#tars-init)
* [tars re-init](#tars-re-init)
* [tars dev](#tars-dev)
* [tars build](#tars-build)
* [tars add-module](#tars-add-module-modulename)
* [tars add-page](#tars-add-page-pagename)
* [tars update](#tars-update)

### tars init

Данная команда позволяет инициализировать TARS в текущей директории. Запускает команду `gulp init` в TARS.

Данная команда скачает TARS из оригинального репозитория или по url, который будет передан с ключом `-s` или `--source`. То есть вы можете создать свой собственный форк TARS, и использовать его.

Доступен интерактивный режим по умолчанию. Вы сможете очистить текущую директорию для проекта или создать новую. Затем CLI поможет вам с настройкой конфигурации проекта. Можно выбрать шаблонизатор, препроцессор, показывать ли системные нотификации, с какой плотностью пикселей поддерживать экраны и т.д. Если интерактивный режим не нужен, команда должна запускаться со флагом `--silent`.

#### Доступные флаги

* `--silent`: запускает init без интерактивного режима.
* `-s`, `--source`: по умолчанию init скачивает из репозитория TARS последнюю версию сборщика и распаковывает в текущей директории. С помощью флага `-s` можно определить, откуда скачивать zip-архив с TARS, если у вас есть своя собственная сборка TARS. **Внимание, опция должна идти последней!**
    
#### Пример использования команды

````bash
# Запустит init в интерактивном режиме
tars init

# Запустит init без интерактивного режима
tars init --silent

# Скачает TARS с http://url.to.tars.zip и заинитит проект в интерактивном режиме
tars init -s http://url.to.tars.zip

# Скачает TARS с http://url.to.tars.zip и заинитит проект без интерактивного режима
tars init --silent --source http://url.to.tars.zip
````

[Назад, к списку команд.](#command-list)

### tars re-init

Данная команда позволяет реинициализировать TARS с новыми настройками (шаблонизатор, препроцессор). Необязательно менять эти настройки вручную, так как их можно будет изменить в интерактивном режиме. Запускает команду `gulp re-init` в TARS.

Доступен интерактивный режим, как и в команде `init`.

#### Доступные флаги

* `--silent`: запускает re-init без интерактивного режима.

#### Пример использования команды

````bash
# Запустит re-init в интерактивном режиме
tars re-init

# Запустит re-init без интерактивного режима
tars re-init --silent
````

[Назад, к списку команд.](#command-list)

### tars dev

Данная команда запускает dev-сборку, с запуском вотчеров. Запускает команду `gulp dev` в TARS.

Доступен интерактивный режим при запуске команды без флагов. Можно выбрать дополнительные опции dev-сборки, доступные через флаги. Если необходимо запустить команду без флагов и без интерактивного режима, используйте флаг `--silent`.

#### Доступные флаги

* `-l`, `--livereload`, `--lr`: запускает лайврелоад в браузере.
* `-t`, `--tunnel`: инициализация проекта с расшариванием верстки во внешний веб.
* `--ie8`: включить в сборку стили для ie8.
* `--silent`: запускает сборку без интерактивного режима.
* `--custom-flags`: позволяет использовать кастомные флаги с dev-командой. Пример использования описан ниже. В интерактивном режиме флаги перечисляются через пробел без кавычек и запятых. **Внимание, опция должна идти последней!**

#### Пример использования команды

````bash
# Будет запущен интерактивный режим
tars dev

# Команда будет запущена без флагов и без интерактивного режима
tars dev --silent

# Будет запущен сервер для livereload
tars dev -l

# Будет запущен сервер для livereload и создан туннель во внешний веб + поддержка ie8
tars dev --tunnel --ie8

# Будет запущен сервер для livereload и создан туннель во внешний веб + поддержка ie8 + два кастомных флага
tars dev --tunnel --ie8 --custom-flags '--custom-flag1 --custom-flag2'
````

[Назад, к списку команд.](#command-list)

### tars build

Данная команда запускает конечную сборку проекта, без запуска вотчеров. Запускает команду `gulp build` в TARS.

Доступен интерактивный режим при запуске команды без флагов. Можно выбрать дополнительные опции build-сборки, доступные через флаги. Если необходимо запустить команду без флагов и без интерактивного режима, используйте флаг `--silent`.

#### Доступные флаги

* `-m`, `--min`: в html подключаются минимизированные файлы.
* `-r`, `--release`: в html подключаются минимизированные файлы, в названии которых есть hash. Данный режим полезен, если вы напрямую выкладываете верстку на сервер. 
* `--ie8`: включить в сборку стили для ie8.
* `--silent`: запускает сборку без интерактивного режима.
* `--custom-flags`: позволяет использовать кастомные флаги с build-командой. Пример использования описан ниже. В интерактивном режиме флаги перечисляются через пробел без кавычек и запятых. **Внимание, опция должна идти последней!**

#### Пример использования команды

````bash
# Будет запущен интерактивный режим
tars build

# Команда будет запущена без флагов и без интерактивного режима
tars build --silent

# Будет создана версия сборки с минифицированными файлами
tars build -m

# Будет создана release-версия сборки + поддержка ie8
tars build --release --ie8

# Будет создана release-версия сборки + поддержка ie8 + два кастомных флага
tars build --release --ie8 --custom-flags '--custom-flag1 --custom-flag2'
````

[Назад, к списку команд.](#command-list)

### tars add-module %moduleName%

Команда добавляет модуль в проект. В качестве параметра принимает имя модуля. Если модуль уже существует, будет выдана соответствующая ошибка. Чтобы создать модуль с готовыми файлами — необходимо пользоваться флагами.

Доступен интерактивный режим при запуске команды без флагов. Можно выбрать файлы и папки, которые должны быть созданы вместе с модулем.

#### Доступные флаги

* `-f`, `--full`: добавляет модуль со всеми папками и файлами, которые могут быть в модуле:  папка для assets, ie, data + файл выбранного шаблонизатора, js и выбранного препроцессора.
* `-b`, `--basic`: добавляет только основные файлы.
* `-d`, `--data`: добавляет папку для data. Также создается файл с данными со следующим содержанием:
````javascript
moduleName: {}
````
* `-i`, `--ie`: добавляет папку для стилей для IE.
* `-a`, `--assets`: добавляет папку для assets.
* `-e`, `--empty`: добавляет только папку модуля, без файлов.

Ключи имеют следующий приоритет:
* `-e`
* `-f`
* `other`

Иными словами, если вы используете `-d -b` и `-e`, будет создана пустая папка для модуля, так как `-e` имеет больший приоритет. Тоже касается интерактивного режима. Если будут выбраны пункты "Полная версия модуля" и "Пустая папка", то будет создана только пустая папка.

#### Пример использования команды

````bash
# Будет запущен интерактивный режим добавления модуля с именем "sidebar"
tars add-module sidebar

# Добавит модуль "sidebar" с базовыми файлами и папкой assets
tars add-module sidebar -b -a

# Добавит модуль "sidebar" с базовыми файлами, папкой assets и папкой для данных
tars add-module sidebar -b -a -d

# Добавит модуль "sidebar" со всеми файлами и папками
tars add-module sidebar --full

# Добавит в модули пустую папку с именем "sidebar"
tars add-module sidebar -e -b -a -d -i
````
    
[Назад, к списку команд.](#command-list)

### tars add-page %pageName%

Команда добавляет новую страницу в markup/pages. В качестве параметра принимает имя страницы. Если страница уже существует, будет выдана соответствующая ошибка. Есть возможность добавить как пустую страницу, так и копию шаблонной (по умолчанию это _template.{html, jade, hbs}). Вы можете создать свой _template.{html, jade, hbs}, чтобы TARS-CLI копировал именно эту страницу.

Интерактивный режим не доступен.

#### Доступные флаги

* `-e`, `--empty`: добавит пустую страницу.

#### Пример использования команды

````bash
# Будет создана страница inner.{html, jade} на основе _template.{html, jade}
tars add-page inner

# Будет создана страница inner.html на основе _template.html
tars add-page inner.html

# Будет создана пустая страница inner.html
tars add-page inner -e
````

[Назад, к списку команд.](#command-list)

###  tars update

Обновит текущую версию TARS-CLI до последней доступной. Под капотом запускает npm update -g tars-cli.

Интерактивный режим не доступен.

#### Пример использования команды

````bash
tars update
````

## Troubleshooting

Для работы TARS-CLI на данный момент требуется git. Он должен быть установлен в системе и прописан в PATH. Если при установке вы получаете ошибку, в которй сказано, что у вас нет git'а, то прошу его установить.

В случае, если у вас Windows и git не прописан в PATH (команда git --version выдает ошибку в cmd), то TARS-CLI необходимо ставить и обновлять в git bash. Работать с TARS-CLI в git bash нельзя. Необходимо использовать стандартный cmd или любой другой терминал.

Если возникла проблема с модулем pty.js, то прошу обновится до версии 1.1.3 минимум. Обновлятся следует через npm:
```bash
npm update -g tars-cli
```

Если возникают еще какие-либо ошибки, смело пишите на [tars.builder@gmail.com](tars.builder@gmail.com) или в [gitter](https://gitter.im/tars/tars-cli?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=body_badge)

[downloads-image]: http://img.shields.io/npm/dm/tars-cli.svg
[npm-url]: https://npmjs.org/package/tars-cli
[npm-image]: http://img.shields.io/npm/v/tars-cli.svg

[travis-image]: https://travis-ci.org/tars/tars-cli.svg?branch=master
[travis-link]: https://travis-ci.org/tars/tars-cli

[deps-image]: https://david-dm.org/tars/tars-cli.svg
[deps-link]: https://david-dm.org/tars/tars-cli

[gitter-image]: https://badges.gitter.im/Join%20Chat.svg
[gitter-link]: https://gitter.im/tars/tars-cli?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=body_badge