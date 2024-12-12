# Лабораторная работа 5

## Задание 1 - Автоматизация проверки формата файлов при коммите

1. Каждый репозиторий на GitHub создается сразу с папкой ```.git```, в которой лежит папка ```hooks```, которая нам и нужна. В ней лежит файл ```pre-commit.sample```. Переместимся в эту директорию и уберем часть ```.sample``` из названия файла, чтобы мы могли с ним работать.

```
cd .git/hooks
```

```
mv pre-commit.sample pre-commit
```
С помощью команды ```ls``` можно проверить изменения:

![telegram-cloud-document-2-5390887661101671544](https://github.com/user-attachments/assets/0b5a8bda-0684-4b9d-b6ef-fe134450c499)


2. Теперь напишем функцию в этом файле, которая и будет проверять наши коммиты на соответствие формату или условию (не пустой файл).

```
#!/bin/bash

staged_files=$(git diff --cached --name-only --diff-filter=ACM)

for file in $staged_files; do
   if [[ "$file" == *.txt ]]; then
      if grep -q '.' "$file"; then
         echo "Все супер"
         exit 0
      else
         echo "Ошибка: файл $file пустой"
         exit 1
      fi
   else
      echo "Ошибка: файл $file не является текстовым"
      exit 1
   fi
done
```

В редакторе это выглядит так:

![telegram-cloud-document-2-5390887661101671806](https://github.com/user-attachments/assets/434d1850-99ce-4562-92f2-4c310715159c)

3. Проверяем нашу функцию на жизнеспособность:

   Создаем файл формата ```.py```. Коммитим:

   ![telegram-cloud-document-2-5390887661101671804](https://github.com/user-attachments/assets/e81389af-f7b7-4c23-826a-5af11debfee4)

   Создаем файл формата ```.txt```, но пустой. Коммитим:

   ![telegram-cloud-document-2-5390887661101671796](https://github.com/user-attachments/assets/8a395a53-09b8-4685-8693-b55a3b628148)

   Создаем файл формата ```.txt``` и вписываем в него что-то. Коммитим:

   ![telegram-cloud-document-2-5390887661101671795](https://github.com/user-attachments/assets/a694e38d-5875-466b-baa0-b5e4442b6bb0)

   Хук отработал верно, ура


## Задание 2 - Использование Git Flow в проекте

1. Установила Git Flow:

```
brew install git-flow
```

2. Создала ветку ```develop```:

```
git checkout develop
```

3. Выполнила инициализацию Git Flow:
   
```
git flow init
```

3. Создала ветку для новой функциональности ```task-management```:

```
git flow feature start task-management
```

4. Создала файл task_manager.py и записала в него следующую функцию:

```
def create_task(title, description):
    # Логика создания задачи
    tasks = {}
    tasks["title"] = "description"
    print(f"Создана новая задача: {title}")
```

5. Закоммитила изменения:

```
git add task_manager.py
```
```
git commit -m "Добавлен функционал управления задачами"
```

6. После завершения разработки функции завершила фичу и объединила ее с основной веткой:

```
git flow feature finish task-management

```

![telegram-cloud-document-2-5390887661101672033](https://github.com/user-attachments/assets/95db6626-ea67-4aa1-8242-55c13eb95cfd)


7. Переключилась на ветку ```develop``` и начала создание релиза:

```
git checkout develop
```
```
git flow release start v1.0.0
```

8. Создала файл ```versiom.txt``` и обновила в нем версию. Закоммитила изменения:

```
echo "v1.0.0" > version.txt
```
```
git add version.txt
```
```
git commit -m "Обновлена версия для релиза v1.0.0"
```

9. Завершила релиз и объединила его с ветками ```develop``` и ```main```:

```
git flow release finish v1.0.0
```

![telegram-cloud-document-2-5390887661101672037](https://github.com/user-attachments/assets/17ea944f-f4ec-4ee8-a134-8bcd5ee2154d)


10. Создала hotfix:

```
git flow hotfix start hotfix-1.0.1
```

11. Создала файл ```file_with_error.py``` и внесла в него код. Закоммитила изменения:

![telegram-cloud-document-2-5390887661101672039](https://github.com/user-attachments/assets/8b703742-5666-4ee7-ba98-b5e814780205)

```
git add file_with_error.py
```
```
git commit -m "Исправлена критическая ошибка"
```

12. Завершила hotfix и объединила его с ветками ```develop``` и ```main```:

```
git flow hotfix finish hotfix-1.0.1
```

![telegram-cloud-document-2-5390887661101672040](https://github.com/user-attachments/assets/78b618c2-8015-40b5-9a7b-0e4a8d4c2f17)


13. Завершенла работы и отправила изменений на удаленный репозиторий:

```
git push origin develop
```
```
git push origin main

```








Что вам нужно знать, чтобы успешно защитить работу:

Основные команды Git, как возникают и как решать конфликты, Git Hooks, Git Flow. 

## Ресурсы

1. [Git Documentation](https://git-scm.com/doc)
2. [Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials)
3. [Pro Git Book](https://git-scm.com/book/en/v2)
4. [Markdown Guidelines](https://docs.github.com/ru/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
