## Laboratory work IV

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [X] 2. Ознакомиться со ссылками учебного материала
- [X] 3. Выполнить инструкцию учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

Создание переменной
```ShellSession
$ export GITHUB_USERNAME=nikitorezz
```

Клонирование репозитория
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab03.git lab04
$ cd lab04
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04.git
```

Компиляция и сборка файлов
```ShellSession
$ g++ -I./include -std=c++11 -c sources/print.cpp # компиляция print.cpp
$ ls print.o # проверка существования
$ ar rvs print.a print.o # архивирование файла в бибиотеку
$ file print.a # вывод информации о файле
$ g++ -I./include -std=c++11 -c examples/example1.cpp 
$ ls example1.o
$ g++ example1.o print.a -o example1 # сборка с библиотекой
$ ./example1 && echo # проверка
```

Компиляция второго файла
```ShellSession
$ g++ -I./include -std=c++11 -c examples/example2.cpp # аналогично первому случаю
$ ls example2.o
$ g++ example2.o print.a -o example2 
$ ./example2
$ cat log.txt && echo # проверка и запись в лог
```

Удаление временных объектов
```ShellSession
$ rm -rf example1.o example2.o print.o 
$ rm -rf print.a 
$ rm -rf example1 example2
$ rm -rf log.txt
```

Создание CMakeLists.txt
```ShellSession
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.0)
project(print)
EOF
```
Добавление строк
```ShellSession
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF 
```
Добавление библиотек
```ShellSession
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF 
```
```ShellSession
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF 
```
```ShellSession
$ cmake -H. -B_build 
$ cmake --build _build # сборка
```
Добавление выполняемых файлов
```ShellSession
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```
```ShellSession
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```


Сборка проектов
```ShellSession
$ cmake --build _build
$ cmake --build _build --target print
$ cmake --build _build --target example1
$ cmake --build _build --target example2
```

Проверка работы проекта
```ShellSession
$ ls -la _build/libprint.a
$ _build/example1 && echo
hello
$ _build/example2
$ cat log.txt && echo
hello
```

Копирование репозитория и добавление CMakeLists
```ShellSession
$ git clone https://github.com/tp-labs/lab04 tmp
$ mv -f tmp/CMakeLists.txt .
$ rm -rf tmp
```

```ShellSession
$ cat CMakeLists.txt # просмотр
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install # сборка
$ cmake --build _build --target install
$ tree _install # вывод в виде дерева

_install
├── cmake
│ ├── print-config-noconfig.cmake
│ └── print-config.cmake
├── include
│ └── print.hpp
└── lib
└── libprint.a
```


Коммит в репозиторий
```ShellSession
$ git add CMakeLists.txt
$ git commit -m"added CMakeLists.txt"
$ git push origin master
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=04
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2017 Братья Вершинины
```
