[![Build Status](https://travis-ci.org/nikitorezz/lab05.svg?branch=master)](https://travis-ci.org/nikitorezz/lab05)
## Laboratory work V

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```ShellSession
$ open https://travis-ci.org
```

## Tasks

- [x] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
- [x] 2. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
- [x] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
- [x] 6. Получить фрагмент вставки значка сервиса **Travis CI** в формате **Markdown**
- [x] 7. Установить [**Travis CLI**](https://github.com/travis-ci/travis.rb#installation)
- [x] 8. Выполнить инструкцию учебного материала
- [x] 9. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

Создание переменных
```ShellSession
$ export GITHUB_USERNAME=nikitorezz
$ export GITHUB_TOKEN=...
```
Клонирование репозитория
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 lab05
$ cd lab05
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
```
ОбЪявление языка в travis.yml
```ShellSession
$ cat > .travis.yml <<EOF
language: cpp
EOF
```
Запись скрипта в travis.yml
```ShellSession
$ cat >> .travis.yml <<EOF

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
```
Добавление аддонов в travis.yml
```ShellSession
$ cat >> .travis.yml <<EOF

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```
Запись токена
```ShellSession
$ travis login --github-token ${GITHUB_TOKEN}
```
Проверка travis
```ShellSession
$ travis lint
```
Добавление значка
```ShellSession
$ ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md
```
Коммит
```ShellSession
$ git add .travis.yml
$ git add README.md
$ git commit -m"added CI"
$ git push origin master
```
Использование комманды тревиса 
```ShellSession
$ travis lint
Warnings for .travis.yml:
[x] value for addons section is empty, dropping
[x] in addons section: unexpected key apt, dropping
$ travis accounts  # показывает аккаунты
nikitorezz (Резниченко Никита): subscribed, 6 repositories
$ travis sync       # синхронизация
synchronizing: . done
$ travis repos        # отображение репозиториев
nikitorezz/bmstu_3rd_sem (active: no, admin: yes, push: yes, pull: yes)
Description: ???
nikitorezz/bmstu_labs_alp (active: no, admin: yes, push: yes, pull: yes)
Description: ???

nikitorezz/bmstu_labs_metod (active: no, admin: yes, push: yes, pull: yes)
Description: ???

nikitorezz/lab03 (active: no, admin: yes, push: yes, pull: yes)
Description: ???

nikitorezz/lab04 (active: no, admin: yes, push: yes, pull: yes)
Description: ???

nikitorezz/lab05 (active: no, admin: yes, push: yes, pull: yes)
Description: ???


$ travis enable         # подключение travis
Detected repository as nikitorezz/lab05, is this correct? |yes| 
nikitorezz/lab05: enabled :)
$ travis whatsup          # выводит истории шагов
nikitorezz/lab05 passed: #3
$ travis branches           # выводит истории шагов в ветках 
master:  #3    passed     added CI
$ travis history           # выводит всю историю изменений и их состояние   
#3 passed:       master added CI
$ travis show           # выводит всю инфу
State:         passed
Type:          push
Branch:        master
Compare URL:   https://github.com/nikitoezz/lab05/compare/34dbbc302cc...4f32cbc1
Duration:      50 sec
Started:       2017-10-06 23:57:07
Finished:      2017-10-06 23:57:57
Allow Failure: false
Config:        os: linux
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Travis Client](https://github.com/travis-ci/travis.rb)
- [AppVeyour](https://www.appveyor.com/)
- [GitLab CI](https://about.gitlab.com/gitlab-ci/)

```
Copyright (c) 2017 Братья Вершинины
```
