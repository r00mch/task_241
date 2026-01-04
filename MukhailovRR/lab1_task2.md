# 1.Как работают команды > и >>?
## Создаст или ПЕРЕЗАПИШЕТ файл
echo "Первая строка" > file.txt
cat file.txt

## Перезапишет содержимое файла
echo "Новая строка" > file.txt
cat file.txt  # Увидим только "Новая строка", старая удалена

## >> - append(добавление)
echo "Первая строка" > file.txt
echo "Вторая строка" >> file.txt
echo "Третья строка" >> file.txt
cat file.txt  # Все три строки сохранились

# 2.Что такое перенаправление ввода? stderr, stdout, stdin?
## Три стандартных потока:
## stdin - стандартный ввод (клавиатура)
## stdout - стандартный вывод (экран)
## stderr - стандартный вывод ошибок (экран)

# 3.Вывести содержание файла не используя текстовые редакторы
## cat 
cat file.txt

# 4.Создать файл с содержимым без текстовых редакторов
echo "Содержимое" > newfile.txt
# heredoc
cat > heredoc.txt > newfile.txt
cat > heredoc.txt << 'END'
Многострочное
содержимое
END

# 5.Перенаправить stdout в stderr и обратно
## Создадим команду, которая выводит и в stdout, и в stderr
echo "Тест перенаправления" > test_output.txt
## 1. stdout в файл, stderr на экран
ping -c 2 localhost > stdout.txt
# stderr увидим на экране (если будет ошибка), а stdout в файле
## 2. stderr в файл, stdout на экран
ping -c 2 nonexistenthost 2> stderr.txt
# stdout увидим на экране, ошибки в файле
## 3. Все (stdout+stderr) в один файл
ping -c 2 localhost &> all_output.txt
## или
ping -c 2 localhost > all_output.txt 2>&1
## 4. Все в /dev/null (в никуда)
ping -c 2 localhost > /dev/null 2>&1
## 5. stderr в stdout, а потом весь stdout в файл
ping -c 2 nonexistenthost 2>&1 > combined.txt
## 6. Разделить stdout и stderr в разные файлы
ping -c 2 localhost > stdout.log 2> stderr.log

# 6.Чем отличаются stdout и stderr?
stdout — для обычного вывода программы
stderr — для сообщений об ошибках и диагностики
При перенаправлении только stdout (>) — stderr продолжает выводиться на экран
stderr обычно не буферизируется — ошибки видны сразу
В конвейер (|) по умолчанию передаётся только stdout
# 7.Что такое stdin?
stdin (стандартный ввод) - поток, из которого программа читает данные.
cat < file.txt
echo "ввод" | cat
wc -l << END
строка1
строка2
END
# 8.Как отправить весь вывод команды в пустоту?
command &> /dev/null
command > /dev/null
command 2> /dev/null
ping -c 1 google.com &> /dev/null
echo $? 
