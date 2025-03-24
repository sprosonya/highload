# Почтовый сервис Mail.ru
## Содержание
- [Тема, целевая аудитория и MVP](#Тема,-целевая-аудитория-и-MVP)
  - [Тема](#Тема)
  - [MVP](#MVP)
  - [Аудитория и трафик](#Аудитория-и-трафик)
- [Список источников](#Список-источников)
## Тема, целевая аудитория и MVP
### Тема 
Mail.ru - сервис, предоставляющий пользователям возможность обмена электронными письмами.
### MVP
Функционал:
1) Просмотр писем в папках
2) Сохранение черновика
3) Отправка письма (в том числе в ответ и пересылкой)
4) Удаление писем и черновиков
5) Создание папки (добавление писем в папку)
6) Фильтрация писем
7) Модерация спама
### Сведения
1) 50M MAU [^1]
2) 16,2 DAU [^2]
3) 20 000 000 писем отправляются в день [^3]
5) Средняя продолжительность посещения - 7 мин 30 сек [^4]
6) Почта mail.ru заблокировала более 6 млрд спам-писем за квартал [^5]
9) 37% респондентов проверяют почту несколько раз в сутки, у 22% она всегда открыта в течение дня. Каждый пятый (20%) открывает сервисы раз в день, 11% – раз в неделю. 7% пользуются электронными письмам крайне редко, а 3% уже не помнят, когда проверяли папку «Входящие» последний раз. [^6]
10) 50 пб - хранилище mail.ru. Из них 15 пб - письма, 35 пб - вложения (файлы) [^7]
11) 12 млрд файлов в хранилищах [^7]
12) 1 млн писем в минуту [^8]
14) 8 гб - бесплатный размер хранилища писем mail.ru [^8]
15) Средний вес письма без вложения 75 кб [^9]
16) Максимальный размер вложений 25 мб
17) 100 млн активных пользователей [^10]
18) По данным специалистов Почты Mail.Ru около 50% всех писем в Мобильной Почте Mail.Ru отправляются с вложениями. В каждом пятом письме — два и более прикрепленных файла. Примерно в 80% случаях — это фотографии, 19% — документы, 1% приходится на видеоролики и архивы. [^11]

### Продуктовые показатели
| Название | Показатель | Расчет |
| -------- | ---------- | ------ |
| MAU | 50 млн/месяц | из данных |
| DAU | 17 млн/день | из данных |
| Отправка писем | 20 млн/день | из данных |
| Получение писем | 500 тыс/мин | из данных (в данных - пиковая нагрузка с коэффициентом 2) |
| Письмо без вложения | 75 кб | из данных |
| Письмо с вложением | 1125 кб | Из данных: фотографии: 80% (2000 кб), документы: 19% (500 кб), видеоролики и архивы: 1% (10000 кб) <br> Средний вес вложения = (0.8×2000) + (0.19×500) + (0.01×10000) = 1795 кб <br> 80% писем с вложениями содержат один файл 1795 кб + 75 кб = 1870 кб <br> 20% писем с вложениями содержат два и более файла (возьмем 2): 2×1795 кб + 75 кб = 3665 кб <br> Средний вес с вложением = (0.8×1870) + (0.2×3665) = 2229 кб <br> Средний вес = (0.5×75) + (0.5×2229) = 1152 кб |
| Хранилище | 0,5 гб/пользователь | В открытых источниках нет данных о среднем хранилище пользователей и о количестве пользователей. Предположим, что 50 млн MAU - количество хранилищ, а ящики неактивных пользователей удаляются. 50 пб / 100 млн пользователей = 0,52 гб |
| Проверка почты | 3.5 раз/день | 37% — проверяют почту несколько раз в сутки (3 раза в день), 22% — почта всегда открыта в течение дня (10 проверок в день), 20% — проверяют почту раз в день, 11% — проверяют почту раз в неделю (0.14 проверки в день), 7% — пользуются почтой крайне редко (предположим, это 0.01 проверки в день), 3% — не помнят, когда проверяли почту (0 проверок в день). <br> Среднее количество проверок в день = (0.37×3) + (0.22×10) + (0.20×1) + (0.11×0.14) + (0.07×0.01) + (0.03×0) = 3.5261 |
| Пользователи | 100 млн | В источнике указаны активные пользователи, однако если принять, что неактивные ящики удаляются (полгода бездействия) - можно взять это значение как количество пользователей |

### Расчет нагрузки
| Запрос | RPS | Расчет RPS | Traffic | Расчет трафика |
| ------ | --- | ---------- | ------- | -------------- |
| Создание и отправка письма | 231 | Отправка писем / 86400 = 20 млн / 86400 | 2034 мбит/с | Отправка писем * Письмо / 86400 = 20 млн * 1125 кб / 86400 |
| Просмотр страницы писем | 689 | Проверка почты * DAU / 86400 = 3.5 * 17 млн / 86400 | 20175 мбит/с | Предположим, что при проверке почты открываются входящие на 1 странице (50 писем). Для отображения входящих не требуются вложения, поэтому вес одного письма базовый без вложений <br> Проверка почты * DAU * 50 * Письмо без вложения / 86400 = 3.5 * 17 млн * 50 * 75 / 86400 |
| Просмотр письма с вложениями | 2065 | Предположим, что при проверке почты пользователь открывает 3 письма с вложениями, скачивая их. <br> 3 * Проверка почты * DAU / 86400 = 3 * 3.5 * 17 млн / 86400 | 18157 мбит/с | Скачивание 3 писем <br> 3 * Проверка почты * DAU * Письмо с вложением / 86400 = 3 * 3.5 * 17 млн * 1125 кб / 86400 |
| Сохранение черновика | 4166 | Пусть черновик сохраняется автоматически каждые 10 секунд, а письмо в среднем пишется 3 минуты <br> Отправка писем * (3 мин / 10 сек) / 86400 = 20 млн * (3 мин / 10 сек) / 86400 | 123 мбит/с | Пусть при написании черновика сохраняется изменение - 75 кб / (3 мин / 10 сек) = 4 кб. <br> Сохранение вложения из черновика примем уже учтенным при расчете трафика при отправке <br> Отправка писем * ( 3 мин / 10 сек) * 4 кб / 86400 = 20 млн * ( 3 мин / 10 сек ) * 4 кб / 86400 |
| Модерация спама | 8133 | Из данных о работе антиспам сервиса почты mail.ru, RPS на модерацию спама = RPS получения письма <br> Получение писем / 60 = 1 млн / 60 | 4882 мбит/с | Так как антиспам-системы сканируют текстовое содержание, вес письма возьмем за 75 кб (письмо без вложения) <br> Получение писем * Письмо без вложения / 60 = 500 тыс * 75 кб / 60 |

### Объем хранилищ :
| Тип | Объем | Расчет |
| --- | ----- | ------ |
| Письма | 15 пб | Из данных |
| Вложения | 35 пб | Из данных |
| Личная информация | 96 тб | Личная информация - данные (ФИО, пароль, техническая информация, почта) 10 кб + аватарка 1 мб = 1034 кб <br> Пользователи * 1034 кб = 100 млн * 1034 кб |

### Глобальная балансировка нагрузки
Распределение трафика по странам [^4]
| Процент | Страна |
| ------- | ------ |
| 90 % | Россия |
| 4 % | Казахстан |
| 4 % | Белоруссия | 
| 1 % | Азербайджан |
| 1 % | Германия |  

Исходя из крупных городов этих стран и распределения нагрузки целесообразно ДЦ разместить в Москве
Для балансировки нагрузки используется Anycast

### Локальная балансировка нагрузки
Будем использовать L4 и L7 балансировщики:  
1. Балансировка SMTP-трафика (L4):  
NGINX  
Алгоритм: Least Connections    
Задачи: равномерное распределение нагрузки между SMTP-серверами. Используется L4 балансировка, так как анализ содержимого при отправке не происходит    
2. Балансировка IMAP/POP3-трафика (L4):  
NGINX  
Алгоритм: Source IP Hash    
Задачи: сохранение сессий пользователей, так как сессии IMAP/POP3 длительные  
3. Балансировка HTTP/HTTPS-трафика (L7):  
NGINX   
Алгоритм: Round Robin для статики и Least Connections для API   
Задачи: разделение трафика на статический контент и API  
4. Оркестрация сервисами с помощью Kubernetes  
5. Обеспечение отказоустойчивости - keepalived  
6. SSL Termination - сертификаты SSL будет проверять балансировщик, для того, чтобы снизить нагрузку на сервера  

### Логическая схема данных
<img width="361" alt="Снимок экрана 2025-03-24 в 19 49 13" src="https://github.com/user-attachments/assets/eaa25981-a071-47e1-84d5-08edb36b834e" />

#### Таблица User (Пользователи)
| Поле         | Тип данных   | Размер (байты) |
|--------------|--------------|----------------|
| id           | int          | 4              |
| email        | text         | 255            |
| username     | text         | 255            |
| password     | text         | 255            |
| avatar_url   | text         | 255            |
| created_at   | timestamp    | 8              |
| updated_at   | timestamp    | 8              |
| **Итого**    |              | **1040**       |

#### Таблица email (Письма)
| Поле              | Тип данных   | Размер (байты) |
|-------------------|--------------|----------------|
| id                | int          | 4              |
| title             | text         | 255            |
| description       | text         | 255            |
| sender_email      | text         | 255            |
| recipient_email   | text         | 255            |
| message_id        | int          | 4              |
| parent_id         | int          | 4              |
| date              | timestamp    | 8              |
| isRead            | bool         | 1              |
| folder_id         | int          | 4              |
| created_at        | timestamp    | 8              |
| updated_at        | timestamp    | 8              |
| isDraft           | bool         | 1              |
| **Итого**         |              | **1062**       |

#### Таблица folder (Папки)
| Поле              | Тип данных   | Размер (байты) |
|-------------------|--------------|----------------|
| id                | int          | 4              |
| user_id           | int          | 4              |
| order_number      | int          | 4              |
| name              | text         | 255            |
| parent_folder_id  | int          | 4              |
| created_at        | timestamp    | 8              |
| updated_at        | timestamp    | 8              |
| is_system         | bool         | 1              |
| num               | int          | 4              |
| **Итого**         |              | **292**        |

#### Таблица attachment (Вложения)
| Поле              | Тип данных   | Размер (байты) |
|-------------------|--------------|----------------|
| id_in_email       | int          | 4              |
| email_id          | int          | 4              |
| filename          | text         | 255            |
| contentType       | text         | 255            |
| filename          | text         | 255            |
| file_id           | int          | 4              |
| **Итого**         |              | **777**        |

#### Таблица file (Файлы)
| Поле              | Тип данных   | Размер (байты) |
|-------------------|--------------|----------------|
| id                | int          | 4              |
| content           | text         | 255            |
| created_at        | text         | 255            |
| **Итого**         |              | **514**        |

### Нагрузка на чтение/запись
| Таблица           | Запросы  | RPS |
|-------------------|----------|-----|
| User (чтение)     | подгрузка профиля (в локал сторадж), вход | (DAU + DAU/2) / 86400 = 295 |
| User (запись)     | регистрация, обновление профиля | MAU / 86400 = 578 |
| Email (чтение)    | просмотр страницы писем | 689 |
| Email (запись)    | отправка писем, сохранение черновика | 231 + 4166 = 4397 |
| Folder (чтение)   | просмотр страницы писем | 689 |
| Attachment (чтение) | просмотр письма с вложением | 2065 |
| Attachment (запись) | отправка писем с вложениями | Отправка письма / 2 = 231/2 = 115 |
| File (чтение) | просмотр письма с вложением | 2065 |
| File (запись) | отправка писем с вложениями | (Отправка письма / 2) * 0.75 (предположим, что 25% писем с вложениями пересылаются и используется тот же файл)  = (231/2)*0.75 = 87 |

### Физическая схема данных
<img width="456" alt="Снимок экрана 2025-03-24 в 21 28 06" src="https://github.com/user-attachments/assets/8822609f-6c88-45c1-b8cf-61c6f3bb4c6a" />   

### Индексы
**USER**  
1) Уникальный индекс для email для быстрой авторизации

**EMAIL**
1) Индекс по date, folder_id дя быстрой загрузки папки с письмами
2) Индекс для поиска по отправителю sender_email
3) Индекс для сортировки писем по дате
4) Полнотекстовый индекс для тела писем
5) Частичный индекс прочитанные/непрочитанные, черновик

**ATTACHMENTS**
1) Индекс по email_id для быстрой загрузки

### Выбор баз данных
| Таблица | БД | Комментарий | Шардирование | Резервирование |
| ------- | -- | ----------- | ------------ | -------------- |
| Email, Folder, Attachments | PostgreSQL с мультитенантной схемой хранения | Так как таблица писем в классическом виде будет занимать слишком много ресурсов для простейших запросов, необходимо организовать отдельную систему хранения для каждого пользователя. Существует 2 варианта: одна БД + схемы, отдельная БД на пользователя. Будем использовать 1 вариант, так как он обеспечивает легскость миграции данных, общие системные таблицы и экономию ресурсов. | Шардирование по хэшу от почты пользователя, 1024 шарда | Синхронная репликация для каждого шарда (1 мастер и 2 реплики) |
| User | PostgreSQL | Таблица редко изменяемая и небольшая, вне схем | Без шардирования | Синхронная репликация (1 мастер и 2 реплики) |
| File | S3 хранилище | Надежное, удобное хранение для файлов | - | Резервное копирование через AWS Backup |

### Денормализация
1) Поле unread_count в USER для хранения количества непрочитанных писем (избегаем COUNT при каждом входе)
2) sender_username, recipient_username, folder_name в EMAIL (избегаем JOIN)
3) has_attachments в EMAIL (избегаем JOIN)
4) unread_count в FOLDER (избегаем COUNT)

### Клиентские библиотеки / Интеграции
1) Redis для кэширования
2) pgx библиотека для PostgreSQL для go
3) aws/aws-sdk-go/service/s3 для S3 дя go

## Список источников
[^1]: [Годовой отчет VK Group](https://corp.vkcdn.ru/media/files/VK_AR2023_RUS_06.06_3KIGigO.pdf)
[^2]: [Квартальный отчет VK Group](https://corp.vkcdn.ru/media/files/RUS_Press_Release_9M_2024.pdf)
[^3]: [Регистрация](https://new.mail.ru/?ysclid=m798h0dap763791521)
[^4]: [Анализ трафика](https://www.similarweb.com/ru/website/mail.ru/#overview)
[^5]: [Новость](https://vk.company/ru/press/releases/11941/)
[^6]: [Исследование](https://news.rambler.ru/internet/47307976-issledovanie-rambler-co-65-rossiyskih-polzovateley-proveryayut-elektronnuyu-pochtu-s-noutbuka-ili-kompyutera/?ysclid=m7j44cvhnp258702465)
[^7]: [Highload++](https://m.vkvideo.ru/video-152308462_456241099?from=video)
[^8]: [Highload++](https://conf.ontico.ru/videos/5537851)
[^8]: [Тарифы](https://help.mail.ru/cloud_web/size/tariffs/)
[^9]: [Статистика](https://www.emailtooltester.com/en/blog/email-usage-statistics/)
[^10]: [Анализ рынка](https://investim.guru/obzory/auditorii-osnovnyh-pochtovyh-servisov-v-rossii-statistika-i-analitika?utm_referrer=https%3A%2F%2Fya.ru%2F)
[^11]: [Новость](https://hi-tech.mail.ru/news/123435-rossiyan-predupredili-o-neochevidnoj-ulovke-v-elektronnyh-pismah/?from=swap&swap=2)
[^12]: [История почты](https://quokka.media/istorii-brendov/mail-ru/)
