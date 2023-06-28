# Easy

**B**ourne-**a**gain **sh**ell - «возрождённый» shell

**Bash** - самая популярная оболочка командной строки для Linux, имеет широкий набор встроенных функций, таких как циклы, условия и прочие
```bash
#!/bin/bash
echo "Creating file"
touch file
echo "Done"
```

**Переменная** - именованный участок памяти, в котором содержатся какие-либо данные: текст, числа и т. д

Способы присваивания переменных
- Прямое: a=2
- Чтение ввода пользователя: read a
- Как результат выполнения команды: a=`cat /etc/hostname`, a=$(cat /etc/hostname)
```bash
#!/bin/bash
botname=Linux
echo "What is your name?"
read name
server=`cat /etc/hostname`
echo "Hello $name, my name is $botname"
echo "Welcome to the $server"
```

Переменные и математическиеоперации
- Обычные переменные (текст): a=2+2
- Математические операции в переменных: let a=2+2 или ((a=2+2))
- Подстановка математических операций в строку: echo "4 + 4 = $((4+4))"
```bash
a=2+2; echo $a
2+2
let a=2+2 или ((a=2+2)); echo $a
4
echo "4 + 4 = $((4+4))"
4+4 = 8
```
В Bash существует сокращённый синтаксис для инкрементов и математической модификации существующих переменных
```
((a++)) 
((a--)) 
((a*=3))
((a/=4)) 
```

**Bash** - умеет работать только с целыми числами, при делении получаем целую часть от числа

Специальные символы языка шаблонов
- Звёздочка (*)
  - Означает любое количество любых символов
  - Заменяется на список всех имён файлов в директории
  - Можно использовать как фильтр: ```echo *.jpg  /  echo /etc/*```
- Знак вопроса
  - Вопросительный знак означает один любой символ
  - Выведет в консоль файлы, в названии которых две буквы и расширение .avi ```echo ??.avi```
 
**Экранирование символов** - замена в тексте управляющих символов на соответствующие текстовые подстановки

- Для экранирования специальных символов используется знак обратного слеша \ - ```echo \?\?.avi``` = ```??.avi```
- Это позволяет интерпретировать специальные символы как текст ```echo 100\$moneys``` = ```100$moneys```
- Экранировать можно пробелы, на каждый пробел нужен экран ```echo \ \ two \ \ space \ \``` = ```two space```
- Для экранирования специальных символов используются правильные кавычки ```Это важно для специального символа $```
- В двойных кавычках происходит подстановка переменной ```moneys=11l;  echo"100$moneys"``` = ```100111```
- В одинарных кавычках — нет echo ```'100$moneys'``` = ```100$moneys```
- Для других символов достаточно заключить текст в одинарные или двойные кавычки, он будет интерпретирован как текст, а не как специальный символ

**Код возврата (exit code)** - код, который возвращает команда или функция после выполнения

Код возврата предыдущей команды хранится в переменной **$?**
- 0 — успех
- Любое другое число, как правило, — код ошибки (свой у каждого приложения)
```
cat /etc/not_existing_file
echo $?
```

Условные операторы в Bash
- if - используется для бинарной проверки (true/false)
- case - используется для сравнения строки, содержащейся в переменной с шаблоном

Логические операторы
- и - $$
- или - ||
```
if [[ cmd1 ]] && [[ cmd2 ]]; then
elif [[ cmd3 || cmd4 ]]; then
else
fi
```
**Работа с файлами**
- -e — проверить, существует ли файл или директория
- -f — файл существует (!-f — не существует)
- -d — каталог существует (!-d — не существует)
- -s — файл существует, и он не пустой
- -r — файл существует и доступен для чтения
- -w — ... для записи
- -x — ... для выполнения
- -h — символьная ссылка
**Работа со строками **
- -z — пустая строка
- -n — не пустая строка
- == — равно
- != — не равно
**Операции с числами**
- -eq — равно
- -ne — не равно
- -lt — меньше
- -le — меньше или равно
- -gt — больше
- -ge — больше или равно

**[[ Двойные квадратные скобки ]]** - ключевые слова языка Bash, в зависимости от своего содержимого, они возвращают код выхода
```
[[ -d /tmp ]]
[[ $a -eq 8 ]]
```
Эти условия возвращают код 0 или 1 в зависимости от того, верно условие или ложно

Инверсия результата достигается с помощью «!».

Для упрощения записи команд есть, 3 оператора позволяющие избежать ненужного усложнения с помощью if
- команда1 ; команда2 - команда2 выполняется после команды1 независимо от результата её работы
- команда1 && команда2 - команда2 выполняется только после успешного выполнения команды1 (т. е. с кодом завершения 0) — это аналог операции AND (логическое И)
- команда1 || команда2 - команда2 выполняется только после неудачного выполнения команды1 (т. е. код завершения команды1 будет отличным от 0) — это аналог операции OR (логическое ИЛИ)

В cаse можно использовать:
- Специальные символы и * ?
- Несколько шаблонов через разделитель |

```
case $variable in condition1)
test.png|*.jpg)
esac
```
```
if [[ $port == '80' || $port == '8080' ]]; then
echo "This is HTTP"
elif [[ $port == '22' ]]; then
echo "This is SSH"
else if [[ $port == '53' ]]; then
echo "And this is DNS"
else
echo "I dont know what is that"
fi;
```
```
case "$port" in
('80'|'8080')
echo "This is HTTP"
;;
'22')
echо "This is SSH"
;;
'53')
echo "And this is DNS"
*)
echo "I dont know what is that"
;;
esac
```

**Отладка** - этап разработки программы, на котором разработчик обнаруживает, локализует и устраняет ошибки

Интерпретатору /bin/bash можно передавать параметры работы:
- –v - выводить все строки по мере их обработки интерпретатором
- –x - выводить все команды и их аргументы по мере их выполнения
```
bash -x script.sh
```
Внутри скрипта
```
#!/bin/bash
set -x
```
