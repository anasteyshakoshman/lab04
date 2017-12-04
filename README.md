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
Присваиваем переменной GITHUB_USERNAME значение
```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
```
Копируем lab03 в lab04, переходим в последнюю
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab03.git lab04
$ cd lab04
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04.git
```
Компилируем example1
```ShellSession
$ g++ -I./include -std=c++11 -c sources/print.cpp #компилируем файл print.cpp
$ ls print.o #проверяем наличие объектного файла
вывод : print.o
$ nm print.o | grep print
вывод : 
0000000000000095 t _GLOBAL__sub_I__Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000026 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
$ ar rvs print.a print.o #архивируем объектный файл
вывод :
ar: создаётся print.a
a - print.o
$ file print.a #получаем информацию об этом файле
вывод : 
print.a: current ar archive
$ g++ -I./include -std=c++11 -c examples/example1.cpp #компилируем example1.cpp
$ ls example1.o #проверяем наличие объектного файла  
вывод : example1.o
$ g++ example1.o print.a -o example1 # компилируем с учетом библиотеки print.a
$ ./example1 && echo #запускаем и печатаем
вывод : hello
```
Компилируем example2
```ShellSession
$ g++ -I./include -std=c++11 -c examples/example2.cpp #компилируем example2.cpp
$ ls example2.o #проверяем наличие объектного файла 
вывод :  example2.o 
$ nm example2.o
вывод :
                 U __cxa_atexit
                 U __dso_handle
0000000000000000 V DW.ref.__gxx_personality_v0
                 U _GLOBAL_OFFSET_TABLE_
000000000000016f t _GLOBAL__sub_I_main
                 U __gxx_personality_v0
0000000000000000 T main
                 U __stack_chk_fail
                 U _Unwind_Resume
0000000000000126 t _Z41__static_initialization_and_destruction_0ii
                 U _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
                 U _ZNSaIcEC1Ev
                 U _ZNSaIcED1Ev
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
                 U _ZNSt8ios_base4InitC1Ev
                 U _ZNSt8ios_base4InitD1Ev
0000000000000000 r _ZStL19piecewise_construct
0000000000000000 b _ZStL8__ioinit
0000000000000000 W _ZStorSt13_Ios_OpenmodeS_

$ g++ example2.o print.a -o example2  # компилируем с учетом библиотеки print.a
$ ./example2
$ cat log.txt && echo #проверяем работоспособность программы
вывод : hello
```
Очищаем временные файлы
```ShellSession
$ rm -rf example1.o example2.o print.o 
$ rm -rf print.a 
$ rm -rf example1 example2
$ rm -rf log.txt
```
Создаем файл CMakeLists.txt и редактируем его.
Настройки:
```ShellSession 
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.0)
project(print)
EOF
```
Требования:
```ShellSession
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```
Библиотеки:
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
Собираем проект
```ShellSession
$ cmake -H. -B_build
вывод :
-- The C compiler identification is GNU 7.2.0
-- The CXX compiler identification is GNU 7.2.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/stasya/anasteyshakoshman/workspace/projects/lab04/_build
$ cmake --build _build
вывод :
Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
```
Редактирование CMakeLists.txt
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
Собираем проекты
```ShellSession
$ cmake --build _build
вывод :
-- Configuring done
-- Generating done
-- Build files have been written to: /home/stasya/anasteyshakoshman/workspace/projects/lab04/_build
[ 33%] Built target print
Scanning dependencies of target example2
[ 50%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
[ 66%] Linking CXX executable example2
[ 66%] Built target example2
Scanning dependencies of target example1
[ 83%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[100%] Linking CXX executable example1
[100%] Built target example1
$ cmake --build _build --target print
вывод :
[100%] Built target print
$ cmake --build _build --target example1
вывод :
[ 50%] Built target print
[100%] Built target example1
$ cmake --build _build --target example2
вывод :
[ 50%] Built target print
[100%] Built target example2
```
Проверяем работу примеров
```ShellSession
$ ls -la _build/libprint.a
вывод :
-rw-r--r-- 1 stasya stasya 3126 дек  5 00:16 _build/libprint.a
$ _build/example1 && echo
вывод : hello
$ _build/example2 
$ cat log.txt && echo
вывод : hello
```
Копируем репозиторий и помещаем в директорию CMakeLists 
```ShellSession
$ git clone https://github.com/tp-labs/lab04 tmp
$ mv -f tmp/CMakeLists.txt .
$ rm -rf tmp
```
Просматриваем файл CMakeLists, собираем проект, отображаем проект в виде дерева
```ShellSession
$ cat CMakeLists.txt
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
вывод : 
-- Configuring done
-- Generating done
-- Build files have been written to: /home/stasya/anasteyshakoshman/workspace/projects/lab04/_build
$ cmake --build _build --target install
вывод :
[100%] Built target print
Install the project...
-- Install configuration: ""
-- Installing: /home/stasya/anasteyshakoshman/workspace/projects/lab04/_install/lib/libprint.a
-- Installing: /home/stasya/anasteyshakoshman/workspace/projects/lab04/_install/include
-- Installing: /home/stasya/anasteyshakoshman/workspace/projects/lab04/_install/include/print.hpp
-- Installing: /home/stasya/anasteyshakoshman/workspace/projects/lab04/_install/cmake/print-config.cmake
-- Installing: /home/stasya/anasteyshakoshman/workspace/projects/lab04/_install/cmake/print-config-noconfig.cmake
$ tree _install
Вывод: 
_install
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
    └── libprint.a

3 directories, 4 files

```
Загрузка созданного в репозиторий
```ShellSession
$ git add CMakeLists.txt
$ git commit -m"added CMakeLists.txt"
вывод :
[master c677482] added CMakeLists.txt
 1 file changed, 36 insertions(+)
 create mode 100644 CMakeLists.txt
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
