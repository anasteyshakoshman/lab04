## Laboratory work III

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab03** и с лиценцией **MIT**
- [X] 2. Ознакомиться со ссылками учебного материала
- [X] 3. Выполнить инструкцию учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

 Cоздание и установка значений переменных окружения
```ShellSession
$ export GITHUB_USERNAME=Talkytitan5127   
$ export GITHUB_EMAIL=pachagorbunoff@yandex.ru
$ alias edit=nano
```

Создание и инициализация директории lab03
```ShellSession
$ mkdir lab03 && cd lab03  #создаем директорию lab03 и переходим в нее
$ git init #инициализируем директорию
$ git config --global user.name ${GITHUB_USERNAME}  #первоначальная настройка пользователя
$ git config --global user.email ${GITHUB_EMAIL}
$ git config -e --global
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03  #добавляем удаленный репозиторий
$ git pull origin master #извлекаем,а затем cливаем данные из удаленной ветки в текущую ветку
$ touch README.md #создаем файл README.md
$ git status #определяем состояние файлов
$ git add README.md #добавляем README.md в версионный контроль, теперь он отслеживается
$ git commit -m"added README.md" #делаем коммит с комментариями
$ git push origin master #отправляем изменения на удаленный репозиторий
```

Добавить на сервисе **GitHub** в репозитории **lab03** файл **.gitignore**
со следующем содержимом:

```ShellSession
*build*/
*install*/
*.swp
```

```ShellSession
$ git pull origin master #получение данных с удаленного репозитория
$ git log #просмотр истории коммитов
```


создаем в папки разные файлы: либо заголовочный файлы С++, либо исполняемые файлы С++
Создаем папки "sources", "include", "examples"

```ShellSession
$ mkdir sources   #с дирректории для хранения файлов исходного кода
$ mkdir include   #создание дирректории для хранения заголовочных файлов
$ mkdir examples  #создание дирректории для хранения примеров

Заполнения файла исходного кода

$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out) {
  out << text;
}

void print(const std::string& text, std::ofstream& out) {
  out << text;
}
EOF
```

Заполнение заголовочного файла

```ShellSession
$ cat > include/print.hpp <<EOF
#include <string>
#include <fstream>
#include <iostream>

void print(const std::string& text, std::ostream& out = std::cout);
void print(const std::string& text, std::ofstream& out);
EOF
```

Заполнение файла с примером1

```ShellSession
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv) {
  print("hello");
}
EOF
```

Заполнение файла с примером2

```ShellSession
$ cat > examples/example2.cpp <<EOF
#include <fstream>
#include <print.hpp>

int main(int argc, char** argv) {
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```ShellSession
$ edit README.md    #Редактирование файла README.md
```

```ShellSession

#Отправка изменения на удаленный репозитория
$ git status   #Проверка состояния файлов
$ git add      #Добавление всех файлов в версионный контроль
$ git commit -m"added sources"    #Делаем коммит изменений
$ git push origin master     #Отправляем изменения на удаленный репозиторий
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2017 Братья Вершинины
```
