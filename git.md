﻿# Дополнительный учебник и источник инфы https://githowto.com/ru. Этот конспект по курсу СБТ.

# Настройки
`git config --system`
`git config --global`
`git config --local`

`git config --global user.name "ivan ivanov"`
`git config --global user.email your@email`
`git config --global core.editor nano`

`git config --list`

# Справка
`git help`
`git help config`

# Создание репозитория в существующей директории
`git init`

# Клонирование существующего репозитория
`git clone https://github.com/libgit2/libgit2`

# Три основных состояния, в которых могут находиться файлы проекта:
* изменённое (modified) 
* подготовленное (staged) 
* зафиксированное (committed)

Определение состояния файлов в рабочей директории:
`$ git status`

Начать отслеживать файл:
`$ git add <filename>`

Индексация изменений:
`$ git add <filename>`

Просмотр неиндексированных изменений:
`$ git diff`

Просмотр индексированных изменений:
`$ git diff --staged`

Зафиксировать изменения:
`$ git commit`

Игнорирование индексации:
`$ git commit -a`

Добавление сообщения к коммиту:
`$ git commit -m 'Add description to my project'`

Ключи можно комбинировать:
`$ git commit -a -m 'Add description to my project'`

# Просмотр истории коммитов
`git log`
## Опции
`--stat` - выведет статистику для каждого коммита
`--graph` - строит текстовый граф
`--decorate` - покажет "головы" (HEAD)
`--all` - покажет все ветки
'git log --pretty=oneline' - удобная однострочная история


## Ограничение вывода
`--author`
`--grep`
Чтобы фильтровать коммиты по автору и ключевым словам одновременно: --all-match


# Удаление и перемещение файлов
## Удаление файлов
`git rm Filename` - удаляет файл с диска, и убирает его из отслеживаемых файлов.
Если файл был изменен и проиндексирован, то нужно добавить параметр -f:
`git rm -f Filename`

## Перемещение файлов
`$ git mv README.md README`

# Ветвление
Создадим новую ветку с именем “testing”:
`$ git branch testing`
В результате появится новый указатель на тот же самый коммит, в котором вы находитесь. Команда git branch только создает новую ветку. Переключения не происходит.
Как Git определяет, в какой ветке вы находитесь? Он хранит специальный указатель HEAD. git log --decorate покажет вам, куда указывают указатели веток:
`$ git log --oneline --decorate`

Давайте переключимся на ветку “testing”:
`$ git checkout testing`

Переключится обратно на “master”:
`$ git checkout master`

Давайте посмотрим где находятся указатели ваших веток, и как ветвилась история проекта:
`$ git log --oneline --decorate --graph --all`

# Основы слияния
Чтобы создать ветку и сразу переключиться на нее
`git checkout -b iss53`

Коммит с автоматическим добавлением измененных файлов
`git commit -a -m 'C3. Addition.'`

Выполнить слияние (merge) с основной веткой
`git checkout master`
`git merge hotfix `

## Удалить ненужную теперь ветку hotfix:
$ git branch -d hotfix

## Если возник конфликт:
`$ git merge iss54` 
Auto-merging Math.java
CONFLICT (content): Merge conflict in Math.java
Automatic merge failed; fix conflicts and then commit the result.
Надо зайти в Math.java и привести файл в нужный вид, после чего закоммитить его.
Можно использовать `git mergetool`, можно обычным редактором.

# Работа с метками
**Легковесная метка** — это просто указатель на определённый коммит.
**Аннотированные метки** хранятся в базе данных Git’а как полноценные объекты, которые имеют контрольную сумму, дату создания, имя и почтовый адрес создавшего, комментарий, а также быть подписаны и проверены с помощью цифровой подписи.

## Просмотр меток
Вывести список
`git tag`

Можно фильтровать
`git tag -l 'v1.8.5*'`

## Создание легковесных меток
Cоздание легковесной метки на текущий коммит:
`$ git tag v1.4-lw`

Создание легковесной метки на конкретный коммит:
`git tag v1.2 9fceb02`

## Создание аннотированных меток
`git tag -a v1.4 -m 'my version 1.4'`

Для просмотра данных метки:
`$ git show v1.4`

# Обмен метками
Для отправки определенной метки:
`git push origin [имя метки]`

Для отправки всех меток:
`$ git push origin --tags`

Для получения меток просто выполните `git pull.`

# Работа с удаленными репозиториями

Просмотр списка удаленных репозиториев
`git remote` - список будет не пуст, если мы, например, клонировали свой репозиторий из сети.
`git remote -v` - просмотр списка с адресами репозиториев

`git remote show [remote-name]` - получение подробной информации об удаленном репозитории

`git remote add [shortname] [url]:`
`git remote add tr https://localhost:7990/testrepo`
Теперь вместо указания полного пути вы можете использовать tr:
`git fetch tr`

Переименование
`git remote rename tr testrepo`

Удаление
`git remote rm testrepo`

## Получение изменений из удалённого репозитория
`git fetch [remote-name]` - Получает список изменений в удаленном репозитории, а также сами изменения, без слияния с вашими изменениями.
`git pull` -  получает изменения из удалённой ветви и сливает их со текущей ветвью.

## Отправка изменений в удаленный репозиторий
`git push [remote-name] [branch-name]`
`git push origin master` - Если кто-то внес изменения в удаленном репозитории с момента последнего получения изменений, то push будет отклонен. Нужно будет сначала выполнить получение и слияние изменений из удаленного репозитория.