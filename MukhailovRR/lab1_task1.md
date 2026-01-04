# 1.Переместиться между директориями
## Перейти в домашнюю директорию пользователя
cd ~ 
## Перейти в корневую директорию
cd /
## Перейти на один уровень вверх (в родительскую папку)
cd ..
## Переход в конкретную папку
cd ~/Downloads

# 2.Вывести список файлов в директории
## Простой список файлов и папок
ls
## Подробный список с правами, владельцем, размером и датой
ls -l
## Показать размер файлов в удобочитаемом виде (KB, MB)
ls -lh
# 3.Вывести список ВСЕХ файлов в директории
ls -a


<img width="565" height="304" alt="image" src="https://github.com/user-attachments/assets/91350429-bb27-48c2-9247-070c5bbbd4ad" />

# 4.Создать папку с подпапками
## Команда mkdir (make directory). Флаг -p создает родительские директории, если их нет
mkdir lab_work
## Создание двух папок в одной строке 
mkdir lab_work/docs lab_work/data
<img width="792" height="44" alt="image" src="https://github.com/user-attachments/assets/9a47331d-2393-45ac-bc1c-26024fb053e9" />

## Создание иерархии одной строкой 
mkdir -p lab_work/project/{docs,data,src,backup}
<img width="806" height="175" alt="image" src="https://github.com/user-attachments/assets/378f6ecc-80a0-4354-b081-8d317e2cd330" />

# 5.Внутри папки создать файлик и записать в него что-нибудь
cd ~/lab_work/project/docs
touch myfile.txt
echo "IDK" > myfile.txt
cat myfile.txt
ь<img width="797" height="129" alt="image" src="https://github.com/user-attachments/assets/a6cbcf6d-8f75-4192-b177-1e02d2f3b86f" />

# 6.Переместить файл из одной директории в другую
mv ~/lab_work/project/docs/myfile.txt ~/lab_work/project/data/
## mv - move 
## Проверим, что в docs файла нет
ls ~/lab_work/project/docs
## Проверим, что файл появился в data
ls ~/lab_work/project/data
<img width="942" height="113" alt="image" src="https://github.com/user-attachments/assets/c6e852c9-b72e-44f9-87f5-50c7c15dae95" />

# 7.Скопировать файл из одной директории в другую
## Команда cp (copy)
## Скопируем файл обратно из data в docs (теперь будет два одинаковых файла в разных местах)
cp ~/lab_work/project/data/myfile.txt ~/lab_work/project/docs/
<img width="890" height="110" alt="image" src="https://github.com/user-attachments/assets/e46822b1-4007-4430-a1de-45c1c918a8e8" />

# 8.Переименовать файл
cd ~/lab_work/project/docs
mv myfile.txt oldfile.txt
ls
<img width="764" height="132" alt="image" src="https://github.com/user-attachments/assets/315c6d4a-ec0c-4f93-8ba7-11bc05564fb9" />
# 9.Сравнить содержимое файлов
## diff
echo "I know!!!" > oldfile2.txt
diff oldfile.txt oldfile2.txt
<img width="930" height="270" alt="image" src="https://github.com/user-attachments/assets/9e3c912b-fda2-4605-86ed-0b8fbe744749" />
# 10.Сортировка содержимого по возрастанию и убыванию
## Создадим файл с неупорядоченными строками
echo -e "яблоко\nгруша\nбанан\nабрикос" > fruits.txt
cat fruits.txt
## Сортировка по возрастанию 
sort fruits.txt
## Сортировка в обратном порядке 
sort -r fruits.txt
<img width="998" height="349" alt="image" src="https://github.com/user-attachments/assets/ed58a7fe-c0f1-42cc-bf7b-6cb8b9f03c19" />
# 11.Удалить все папки и файлы
## Удалить один файл (например, oldfile.txt)
rm ~/lab_work/project/docs/oldfile.txt
## Удалить папку со всем её содержимым
rm -rf ~/lab_work
## проверим, что всё удалилось
cd ~
ls -la | grep lab_work
<img width="908" height="150" alt="image" src="https://github.com/user-attachments/assets/784f21b0-f360-4814-a09d-a8fc6cdd503b" />
