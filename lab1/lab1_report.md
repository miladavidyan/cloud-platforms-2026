University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: Облачные платформы как основа технологического предпринимательства 
Year: 2026/2027  
Group: U4225  
Author: Davidyan Milana Eduardovnа  
Lab: Lab1  
Date of create: 30.10.2026  
Date of finished: 

# Лабораторная работа №1. Обзор Google Cloud и исследование основных сервисов

## Цель работы

Ознакомиться с основными возможностями Google Cloud, научиться создавать виртуальные машины и сервисные аккаунты, работать с облачным хранилищем и управлять правами доступа.

## Ход работы

### 1. Создание сервисного аккаунта

В Google Cloud Console в разделе `IAM & Admin → Service Accounts` был создан сервисный аккаунт `mdavidyan-sa-lab1`.

Аккаунту назначена роль `Storage Admin`, предоставляющая права на управление объектами Cloud Storage.
<img width="1085" height="592" alt="image" src="https://github.com/user-attachments/assets/e2de3ef0-bd7e-4a22-b59b-b83fa71af016" />

### 2. Создание виртуальной машины

В разделе `Compute Engine → VM instances` создана виртуальная машина со следующими параметрами:

| Параметр | Значение |
|---|---|
| Название | mdavidyan-vm-lab1 |
| Регион | us-central1 |
| Зона | us-central1-a |
| Серия | E2 |
| Тип машины | e2-micro |
| Операционная система | Debian Linux |
| Модель предоставления | Spot |
| Сервисный аккаунт | mdavidyan-sa-lab1 |

Для сервисного аккаунта выбран параметр `Allow full access to all Cloud APIs`.

После создания виртуальной машины было выполнено подключение через SSH.

<img width="872" height="424" alt="image" src="https://github.com/user-attachments/assets/de012ce1-2a26-43a6-8f66-1b9a46b98f77" />
<img width="736" height="332" alt="image" src="https://github.com/user-attachments/assets/48479de1-b2a5-43df-9cab-b2296968e70b" />
<img width="632" height="142" alt="image" src="https://github.com/user-attachments/assets/d7d68eac-854d-48fe-9d49-745380105138" />
<img width="559" height="301" alt="image" src="https://github.com/user-attachments/assets/d357730c-c8a1-4eb1-acd2-3b71e4890614" />
<img width="550" height="159" alt="image" src="https://github.com/user-attachments/assets/651c871a-c18e-4df5-925c-7d762f2bcfd1" />
<img width="852" height="213" alt="image" src="https://github.com/user-attachments/assets/5e23d996-5b6b-4af3-8053-d0440b075966" />
<img width="1122" height="315" alt="image" src="https://github.com/user-attachments/assets/8a1b89d0-dae6-42f6-bf71-28ffd527c5dc" />


### 3. Работа с Cloud Storage

<img width="740" height="110" alt="image" src="https://github.com/user-attachments/assets/b289493c-c961-438f-8dd5-335e556a77f0" />
<img width="945" height="313" alt="image" src="https://github.com/user-attachments/assets/87147c4f-0a06-4feb-901f-e57eb7e1110b" />

С помощью утилиты `gcloud` был получен список файлов в бакете `lab1-bucket-itmo`:

```bash
gcloud storage ls gs://lab1-bucket-itmo/
```

В бакете обнаружены три файла:
- `pic1.jpg`
- `pic2.jpg`
- `pic3.jpeg`

Для загрузки файлов на виртуальную машину создана локальная директория:

```bash
mkdir lab1-files
```

Затем выполнено копирование:

```bash
gcloud storage cp 'gs://lab1-bucket-itmo/*' ~/lab1-files/
```

Наличие файлов проверено командой:

```bash
ls -lah ~/lab1-files/
```

Все три файла успешно скопированы. Общий объём переданных данных составил около 1,5 МиБ.


### 4. Изменение прав доступа

В разделе IAM роль сервисного аккаунта `mdavidyan-sa-lab1` изменена с `Storage Admin` на `Compute Viewer`.

<img width="603" height="255" alt="image" src="https://github.com/user-attachments/assets/c5d3bf5d-ace2-42cd-90be-4baa49f80d19" />
<img width="870" height="100" alt="image" src="https://github.com/user-attachments/assets/09657c70-3cdf-45ef-a430-66e47491d0ba" />


После изменения роли выполнена повторная попытка копирования файлов в новую директорию:

```bash
mkdir ~/lab1-test
gcloud storage cp 'gs://lab1-bucket-itmo/*' ~/lab1-test/
```

**Результат:** все три файла были повторно успешно скачаны.


Для проверки использованного аккаунта выполнены команды:

```bash
gcloud auth list
```

```bash
curl -s -H "Metadata-Flavor: Google" \
http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/email
```

Обе проверки подтвердили использование сервисного аккаунта `mdavidyan-sa-lab1`.

Дополнительно предпринята попытка просмотра IAM-политики бакета:

```bash
gcloud storage buckets get-iam-policy gs://lab1-bucket-itmo
```

Команда завершилась ошибкой `Permission denied`, указывающей на отсутствие разрешения `storage.buckets.getIamPolicy`.

При этом повторная команда `gcloud storage ls` успешно отобразила список объектов бакета.

Таким образом, после смены роли доступ к чтению файлов сохранился. Возможная причина — дополнительные разрешения на уровне бакета или проекта, однако точно установить её без просмотра соответствующих политик не удалось.

### 5. Удаление ресурсов

После завершения эксперимента были удалены:

- Виртуальная машина `mdavidyan-vm-lab1`.
- Сервисный аккаунт `mdavidyan-sa-lab1`.

## Вывод

В ходе лабораторной работы были изучены основные сервисы Google Cloud: IAM, Compute Engine и Cloud Storage.

Создана виртуальная машина, настроен сервисный аккаунт, выполнено копирование файлов из облачного хранилища и проверено их наличие средствами Linux.

Эксперимент показал, что изменение роли `Storage Admin` на `Compute Viewer` не привело к потере доступа к файлам в исследуемом бакете. Это демонстрирует, что фактические права доступа могут зависеть не только от одной назначенной роли, но и от других действующих IAM-разрешений.

Получены практические навыки управления облачными ресурсами и работы с утилитой `gcloud`.
