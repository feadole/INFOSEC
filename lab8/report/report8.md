---
## Front matter
title: "Отчёта по лабораторной работе № 8"
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

Освоить на практике применение режима однократного гаммирования на
примере кодирования различных исходных текстов одним ключом.


## Теоретическое введение

Гаммирование - наложение (снятие) на открытые (зашифрованные) данные
последовательности элементов других данных, полученной с помощью некоторого криптографического алгоритма, для получения зашифрованных (открытых)
данных.
Основная формула, необходимая для реализации однократного гаммирования:
Ci = Pi XOR Ki, где Ci - i-й символ зашифрованного текста, Pi - i-й символ открытого
текста, Ki - i-й символ ключа.
В данном случае для двух шифротекстов будет две формулы: С1 = P1 xor K и С2 =
P2 xor K, где индексы обозначают первый и второй шифротексты соответственно.
Если нам известны оба шифротекста и один открытый текст, то мы можем
найти другой открытый текст, это следует из следующих формул: C1 xor C2 = P1
xor K xor P2 xor K = P1 xor P2, C1 xor C2 xor P1 = P1 xor P2 xor P1 = P2.
Более подробно см. в [1].

## Выполнение лабораторной работы

Код программы (рис. 8.1).

![Рис. 8.1:  Приложение, реализующее режим однократного гаммирования для двух
текстов одним ключом, Часть 1](image/lab8.1.png)

• In[1]: импорт необходимых библиотек
 In[2]: функция, реализующая сложение по модулю два двух строк
• In[3]: открытые/исходные тексты (одинаковой длины)
• In[5]: создание ключа той же длины, что и открытые тексты
• In[7]: получение шифротекстов с помощью функции, созданной ранее, при
условии, что известны открытые тексты и ключ
• In[8]: получение открытых текстов с помощью функции, созданной ранее,
при условии, что известны шифротексты и ключ

![Рис. 8.2: Приложение, реализующее режим однократного гаммирования для двух
текстов одним ключом, Часть 2](image/lab8ю2.png)

In[9]: сложение по модулю два двух шифротекстов с помощию функции,
созданной ранее
• In[10]: получение открытых текстов с помощью функции, созданной ранее,
при условии, что известны оба шифротекста и один из открытых текстов
• In[12]: получение части первого открытого текста (срез)
• In[14]: получение части второго текста (на тех позициях, на которых расположены символы части первого открытого текста) с помощью функции,
созданной ранее, при условии, что известны оба шифротекста и часть первого открытого текста

## Выводы

В ходе выполнения данной лабораторной работы я освоила на практике применение режима однократного гаммирования на примере кодирования различных
исходных текстов одним ключом.
