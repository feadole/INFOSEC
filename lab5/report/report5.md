---
## Front matter
title: "Отчёта по лабораторной работе № 5"
subtitle: "Информационная безопасность"
author: "Адоле Фейт Эне"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

## Цель работы

Изучение механизмов изменения идентификаторов, применения SetUID- и
Sticky-битов. Получение практических навыков работы в консоли с дополнительными атрибутами. Рассмотрение работы механизма смены идентификатора
процессов пользователей, а также влияние бита Sticky на запись и удаление
файлов.

## Теоретическое введение

SetUID, SetGID и Sticky - это специальные типы разрешений позволяют задавать
расширенные права доступа на файлы или каталоги.
• SetUID (set user ID upon execution — «установка ID пользователя во время
выполнения) являются флагами прав доступа в Unix, которые разрешают
пользователям запускать исполняемые файлы с правами владельца исполняемого файла.
• SetGID (set group ID upon execution — «установка ID группы во время выполнения») являются флагами прав доступа в Unix, которые разрешают
пользователям запускать исполняемые файлы с правами группы исполняемого файла.
• Sticky bit в основном используется в общих каталогах, таких как /var или
/tmp, поскольку пользователи могут создавать файлы, читать и выполнять
их, принадлежащие другим пользователям, но не могут удалять файлы,
принадлежащие другим пользователям.

## Выполнение лабораторной работы

## 5.1 Создание программы
Для начала я убедилась, что компилятор gcc установлен, исолпьзуя команду
“gcc -v”. Затем отключила систему запретов до очередной перезагрузки системы
командой “sudo setenforce 0”, после чего команда “getenforce” вывела “Permissive”
(рис. 5.1).

![Рис. 5.1: Предварительная подготовка](image/lab5.1.png)

Проверила успешное выполнение команд “whereis gcc” и “whereis g++” (их расположение) (рис. 5.2).

![Рис. 5.2: Команда “whereis”](image/lab5.2.png)

Вошла в систему от имени пользователя guest командой “su - guest”. Создала программу simpleid.c командой “touch simpleid.c” и открыла её в редакторе
командой “gedit /home/guest/simpleid.c” (рис. 5.3).

![Рис. 5.3: Вход в систему и создание программы](image/lab5.3.png)

Код программы выглядит следующим образом (рис. 5.4).

![Рис. 5.4: Код программы simpleid.c](image/lab5.4.png)

Скомпилировала программу и убедилась, что файл программы был создан
командой “gcc simpleid.c -o simpleid”. Выполнила программу simpleid командой
“./simpleid”, а затем выполнила системную программу id командой “id”. Результаты, полученные в результате выполнения обеих команд, совпадают (uid=1001 и
gid=1001) (рис. 5.5).

![Рис. 5.5: Компиляция и выполнение программы simpleid](image/lab5.5.png)

Усложнила программу, добавив вывод действительных идентификаторов (рис. 5.6).

![Рис. 5.6: Усложнение программы](image/lab5.6.png)

Получившуюся программу назвала simpleid2.c (рис. 3.7).

![Рис. 5.7: Переименование программы в simpleid2.c](image/lab5.7.png)

Скомпилировала и запустила simpleid2.c командами “gcc simpleid2.c -o sipleid2”
и “./simpleid2” (рис. 3.8).

![Рис. 5.8: Компиляция и выполнение программы simpleid2](image/lab5.8.png)

От имени суперпользователя выполнила команды “sudo chown root:guest
/home/guest/simpleid2” и “sudo chmod u+s /home/guest/simpleid2”, затем выполнила проверку правильности установки новых атрибутов и смены владельца
файла simpleid2 командой “sudo ls -l /home/guest/simpleid2” (рис. 3.9). Этими
командами была произведена смена пользователя файла на root и установлен
SetUID-бит.

![Рис. 5.9: Установка новых атрибутов (SetUID) и смена владельца файла](image/lab5.9.png)

Запустила программы simpleid2 и id. Теперь появились различия в uid (рис. 5.10).

![Рис. 5.10: Запуск simpleid2 после установки SetUID](image/lab5.10.png)

Проделала тоже самое относительно SetGID-бита. Также можем заметить различия с предыдущим пунктом (рис. 5.11).

![Рис. 5.10: Запуск simpleid2 после установки SetUID](image/lab5.10.png)

Создаем программу readfile.c (рис. 5.12).

![Рис. 5.12: Код программы readfile.c](image/lab5.12.png)

Скомпилировала созданную программу командой “gcc readfile.c -o readfile”.
Сменила владельца у файла readfile.c командой “sudo chown root:guest
/home/guest/readfile.c” и поменяла права так, чтобы только суперпользователь
мог прочитать его, а guest не мог, с помощью команды “sudo chmod 700
/home/guest/readfile.c”. Теперь убедилась, что пользователь guest не может
прочитать файл readfile.c командой “cat readfile.c”, получив отказ в доступе (рис. 5.13).

![Рис. 5.13: Смена владельца и прав доступа у файла readfile.c](image/lab5.13.png)

Поменяла владельца у программы readfile и устанавила SetUID. Проверила, может ли программа readfile прочитать файл readfile.c командой “./readfile readfile.c”.
Прочитать удалось.Аналогично проверила, можно ли прочитать файл /etc/shadow.
Прочитать удалось (рис. 5.14).

![Рис. 5.14: Запуск программы readfile](image/lab5.14.png)

## 3.2 Исследование Sticky-бита
Командой “ls -l / | grep tmp” убеждилась, что атрибут Sticky на директории /tmp
установлен. От имени пользователя guest создала файл file01.txt в директории
/tmp со словом test командой “echo”test” > /tmp/file01.txt”. Просматрела атрибуты
у только что созданного файла и разрешаем чтение и запись для категории
пользователей “все остальные” командами “ls -l /tmp/file01.txt” и “chmod o+rw
/tmp/file01.txt” (рис. 5.15).

![Рис. 5.15: Создание файла file01.txt](image/lab5.15.png)

От имени пользователя guest2 попробовала прочитать файл командой “cat
/tmp/file01.txt” - это удалось. Далее попыталась дозаписать в файл слово test2,
проверить содержимое файла и записать в файл слово test3, стерев при этом всю
имеющуюся в файле информацию - эти операции удалось выполнить только в
случае, если еще дополнительно разрешить чтение и запись для группы пользователей командой “chmod g+rw /tmp/file01.txt”. От имени пользователя guest2
попробовала удалить файл - это не удается ни в каком из случаев, возникает
ошибка (рис. 5.16).

![Рис. 5.16: Попытка выполнить действия над файлом file01.txt от имени пользователя guest2](image/lab5.16.png)

Повысила права до суперпользователя командой “su -” и выполнила команду,
снимающую атрибут t с директории /tmp “chmod -t /tmp”. После чего покинула
режим суперпользователя командой “exit”. Повторила предыдущие шаги. Теперь
мне удалось удалить файл file01.txt от имени пользователя, не являющегося его
владельцем (рис. 5.17).

![Рис. 5.17: Удаление атрибута t (Sticky-бита) и повторение действий](image/lab5.17.png)

Повысила свои права до суперпользователя и вернула атрибут t на директорию
/tmp (рис. 5.18).

![Рис. 5.18: Возвращение атрибута t (Sticky-бита)](image/lab5.18.png)


## Выводы

В ходе выполнения данной лабораторной работы я изучила механизмы изменения идентификаторов, применение SetUID- и Sticky-битов. 
Получила практические навыки работы в консоли с дополнительными атрибутами. Рассмотрела
работу механизма смены идентификатора процессов пользователей, а также
влияние бита Sticky на запись и удаление файлов.

