University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: Облачные платформы как основа технологического предпринимательства 
Year: 2026/2027  
Group: U4225  
Author: Davidyan Milana Eduardovnа  
Lab: Lab3  
Date of create: 30.10.2026  
Date of finished: 


# Лабораторная работа №3. Исследование Cloud Storage

## Цель работы

Изучить основные возможности Google Cloud Storage, освоить создание бакетов, загрузку и организацию файлов, настройку публичного доступа и получение ссылок на объекты.

## Ход работы

### 1. Создание бакета

В Google Cloud Storage был создан бакет `mdavidyan-lab3-2026` со следующими параметрами:

| Параметр | Значение |
|---|---|
| Регион | europe-west1 (Belgium) |
| Класс хранения | Standard |
| Управление доступом | Uniform |
| Public access prevention | Disabled |
| Защита данных | None |

<img width="886" height="487" alt="image" src="https://github.com/user-attachments/assets/a198170c-d5d3-43a6-88c9-c63e9044ca51" />

### 2. Загрузка и организация файлов

В бакет были загружены три изображения:

<img width="454" height="201" alt="image" src="https://github.com/user-attachments/assets/ddf08d5d-35f2-4650-8bbb-ba8342150f5b" />

Создана папка `cats`. 

<img width="908" height="359" alt="image" src="https://github.com/user-attachments/assets/7098042a-f6d6-42b6-9cdb-d1ccc8c2306b" />

При попытке переместить изображения из корня бакета в папку через функцию `Move` возникла ошибка:
`Unable to move your object. Your billing account closed.`

В качестве альтернативы изображения были повторно загружены непосредственно в папку `cats`.

<img width="908" height="359" alt="image" src="https://github.com/user-attachments/assets/43e61b96-7db2-49a7-a24f-0ef09c8c5c1c" />

### 3. Настройка публичного доступа

Во вкладке `Permissions` для принципала `allUsers` была назначена роль `Storage Object Viewer`.

После изменения настроек бакет получил статус `Access granted to public principals`.

<img width="706" height="394" alt="image" src="https://github.com/user-attachments/assets/49788832-2638-47e0-9cc3-4876b9a78cf8" />
<img width="723" height="250" alt="image" src="https://github.com/user-attachments/assets/dc057d35-5b48-4fe4-9d32-0bb99071c011" />

Через контекстное меню файла была получена публичная ссылка на изображение.

Ссылка успешно открылась в режиме инкогнито без авторизации в Google Cloud, что подтвердило доступность объекта для внешних пользователей.

<img width="946" height="900" alt="image" src="https://github.com/user-attachments/assets/30b74eb7-0684-4d9e-bf3e-cf0d2ac99b4c" />

### 4. Удаление ресурсов

После завершения работы все загруженные файлы и созданный бакет `mdavidyan-lab3-2026` были удалены.

## Вывод

В ходе лабораторной работы были изучены основные функции Google Cloud Storage: создание бакета, загрузка и организация объектов, настройка IAM-разрешений и предоставление публичного доступа.

Успешно проверена возможность открытия изображений по публичным ссылкам без авторизации.
