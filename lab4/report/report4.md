---
## Front matter
title: "Отчёта по лабораторной работе № 4"
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

Получение практических навыков работы в консоли с расширенными атрибутами файлов.

## Теоретическое введение

В UNIX-системах, кроме стандартных прав доступа, существуют также дополнительные или специальные атрибуты файлов, которые поддерживает файловая
система. Управлять атрибутами можно с помощью команды “chattr”.
Виды расширенных атрибутов:
• a - файл можно открыть только в режиме добавления для записи
• A - при доступе к файлу его запись atime не изменяется
• c - файл автоматически сжимается на диске ядром
• C - файл не подлежит обновлению «копирование при записи»
• d - файл не является кандидатом для резервного копирования при запуске
программы dump
• D - при изменении каталога изменения синхронно записываются на диск
• e - файл использует экстенты для отображения блоков на диске. Его нельзя
удалить с помощью chattr
• E - файл, каталог или символическая ссылка зашифрованы файловой системой. Этот атрибут нельзя установить или сбросить с помощью chattr, хотя
он может быть отображён с помощью lsattr
• F -директория указывает, что все поиски путей внутри этого каталога выполняются без учёта регистра. Этот атрибут можно изменить только в пустых
каталогах в файловых системах с включённой функцией casefold
• i - файл не может быть изменён: его нельзя удалить или переименовать,
нельзя создать ссылку на этот файл, большую часть метаданных файла
нельзя изменить, и файл нельзя открыть в режиме записи
• и другие

## Выполнение лабораторной работы

От имени пользователя guest определила расширенные атрибуты файла /home/guest/dir1/file1 командой “lsattr /home/guest/dir1/file1”. Командой
“chmod 600 /home/guest/dir1/file1” установила права, разрешающие чтение и
запись для владельца файла. При попытке использовать команду “chattr +a
/home/guest/dir1/file1” для установления расширенного атрибута “a” получила
отказ в выполнении операции (рис. 3.1).

![Рис. 4.1: Расширенные атрибуты файла](image/lab4.1.png)

От имени суперпользователя установила расширенный атрибут “a” на
файл командой “sudo chattr +a /home/guest/dir1/file1” и от имени пользователям guest проверила правильность установления атрибута командой “lsattr /home/guest/dir1/file1” (рис. 3.2).

![Рис. 4.2: Установка расширенного атрибута “a” от имени суперпользователя](image/lab4.2.png)

Дозаписала в файл file1 слово “test” командой “echo”test” » /home/guest/dir1/file1”
и, используя команду “cat /home/guest/dir1/file1” убедилась, что указанное
ранее слово было успешно записано в наш файл. Аналогично записала в файл
слово “abcd”. Далее попробовала стереть имеющуюся в файле информацию
командой “echo”abcd” > /home/guest/dirl/file1”, но получила отказ. Попробовала
переименовать файл командой “rename file1 file2 /home/guest/dirl/file1” и
изменить права доступа командой “chmod 000 /home/guest/dirl/file1” и также
получила отказ (рис. 3.3).

![Рис. 4.3: : Попытка выполнить действия над файлом после установки атрибута “a”](image/lab4.3.png)

Сняла расширенный атрибут“a” с файла от имени суперпользователя командой
“sudo chattr -a /home/guest/dir1/file1” и повторила операции, которые ранее не
получилось выполнить - теперь ошибок не было, операции были выполнены (рис.
3.4).

![Рис. 4.4: Попытка выполнить действия над файлом после снятия атрибута “a”](image/lab4.4.png)

От имени суперпользователя командой “sudo chattr +i /home/guest/dir1/file1”
установила расширенный атрибут “i” и повторила действия, которые выполняла
ранее. В данном случае файл можно было только прочитать, а изменить/записать
в него что-то, переименовать и изменить его атрибуты - нельзя (рис. 3.5).

![Рис. 4.5: : Попытка выполнить действия над файлом после установки атрибута “i”](image/lab4.5.png)

## Выводы

В ходе выполнения данной лабораторной работы я получила практические
навыки работы в консоли с расширенными атрибутами файлов, на практике
опробовала действие расширенных атрибутов “a” и “i”.

## Список литературы

Права доступа к файлам в Linux [Электронный ресурс]. 2019. URL: https:
//losst.ru/prava-dostupa-k-fajlam-v-linux.