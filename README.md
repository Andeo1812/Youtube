# YouTube

HighLoad system project

## Содержание

* ### [Тема и целевая аудитория](#1)
* ### [Расчет нагрузки](#2)
* ### [Логическая схема](#3)
* ### [Физическая схема](#4)
* ### [Технологии](#5)
* ### [Схема проекта](#6)
* ### [Список серверов](#7)
* ### [Источники](#8)

## 1. Тема и целевая аудитория <a name="1"></a>

**YouTube** — видеохостинг, предоставляющий пользователям услуги воспроизведения, хранения,
комментирования, оценивания видео.

### MVP

- Просмотр видео
- Регистрация
- Рекомендаций видео
- Загрузка видео
- Комментирование видео
- Лайки

### Целевая аудитория

[Статистика](https://xmldatafeed.com/statistika-youtube-stoimost-aktivnye-polzovateli-luchshie-kanaly-i-tendenczii-2022/)
(доп. источники [источник](https://www.businessofapps.com/data/youtube-statistics/),
[источник](https://ictnews.uz/03/11/2022/youtube/))
показывают, что более 2,6 миллиардов активных пользователей в месяц

В связи с отсутствием данных примем допущение о DAU на
основе [данных](https://www.statista.com/statistics/256896/frequency-with-which-us-internet-users-visit-youtube/).
98% - 2600 млн, а DAU - 62% - 1644 млн. То есть примем пропорцию между DAU и MAU для всех стран на основе соотношении
этих параметров для 1 страны - США

В целях упрощения CDN возьмем только топ-3 страны с количеством пользователей онлайн

| **Страна** | **Количество пользователей в месяц, млн** |
|------------|-------------------------------------------|
| India      | 467                                       |
| USA        | 240                                       |
| Indonesia  | 127                                       |

Месячная аудитория с упрощением:

> 467 + 240 + 127 = 834 [млн]

Промасштабируем дневную аудиторию с упрощением:

> 834 / 2600 * 1644 = 524 [млн]

## 2. Расчет нагрузки <a name="2"></a>

### Продуктовые метрики

* Месячная аудитория - 834 [млн]
* Дневная аудитория - 524 [млн]
* Усредненные метрики:

(Рассмотреть метрики сколько 1 пользователь смотрит 1 видео)

Хранилище пользователя: аватар и личные данные - 15 КБ + 5 КБ = 20 [КБ]

Картинки в webp ужимаются до такого порядка и немного данных о пользователе

[Пользовтаели тратят 23,2 часа в месяц на просмотр видео.](https://lpgenerator.ru/blog/2021/10/07/statistika-youtube-kotoruyu-nuzhno-znat-v-2021/#:~:text=YouTube%20%E2%80%94%20%D0%BB%D1%83%D1%87%D1%88%D0%B5%D0%B5%20%D0%BF%D1%80%D0%B8%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%B4%D0%BB%D1%8F%20%D0%BF%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%B2%D0%BE%D0%B9%20%D0%BF%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B8%20%D0%B2%D0%B8%D0%B4%D0%B5%D0%BE.%20%D0%A1%D1%80%D0%B5%D0%B4%D0%BD%D0%B8%D0%B9%20%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%20%D1%82%D1%80%D0%B0%D1%82%D0%B8%D1%82%2023%2C2%20%D1%87%D0%B0%D1%81%D0%B0%20%D0%B2%20%D0%BC%D0%B5%D1%81%D1%8F%D1%86%20%D0%BD%D0%B0%20%D0%BF%D1%80%D0%BE%D1%81%D0%BC%D0%BE%D1%82%D1%80%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%BD%D1%82%D0%B0)

> 23,2 * 60 / 30 = 46,4 [мин/день] - просмотры

[Cредняя длительность видео на YouTube 8.4 минуты.](https://exlibris.ru/news/statistika-youtube-2019-infografika/#:~:text=%D0%9F%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D0%B8%20%D1%81%D0%BC%D0%BE%D1%82%D1%80%D1%8F%D1%82%20%D0%B2%D0%B8%D0%B4%D0%B5%D0%BE%20%D0%BA%D0%B0%D0%B6%D0%B4%D1%8B%D0%B9%20%D0%B4%D0%B5%D0%BD%D1%8C&text=%D0%92%20%D1%81%D1%80%D0%B5%D0%B4%D0%BD%D0%B5%D0%BC%20%D0%BD%D0%B0%20%D0%BA%D0%B0%D0%B6%D0%B4%D0%BE%D0%B3%D0%BE%20%D1%87%D0%B5%D0%BB%D0%BE%D0%B2%D0%B5%D0%BA%D0%B0,%D0%BF%D0%BE%D1%8D%D1%82%D0%BE%D0%BC%D1%83%20%D1%81%D1%80%D0%B5%D0%B4%D0%BD%D0%B8%D0%B5%20%D0%BF%D0%BE%D0%BA%D0%B0%D0%B7%D0%B0%D1%82%D0%B5%D0%BB%D0%B8%20%D1%82%D0%B0%D0%BA%D0%B8%D0%B5%20%D0%B2%D0%BF%D0%B5%D1%87%D0%B0%D1%82%D0%BB%D1%8F%D1%8E%D1%89%D0%B8%D0%B5.)

> 46.4 / 8.4 = 5.5 [штук] - видео просматривает один пользователь за сутки

[Пользователи в среднем ставят 4 лайка на 100 видео.](https://tubularlabs.com/blog/3-metrics-youtube-success/#:~:text=Tips%20for%20YouTube%20Success%3A%20Aim%20for%20at%20least%2040%20Likes%20for%20every%201000%20views%20of%20your%20video)

> 5.5 * 30 = 165 [штук] - среднее число просмотров в месяц
>
> 165 / 100 * 4 = 6 [штук/месяц] - лайков в месяц ставит пользователь

[Пользователи в среднем оставляют 5 комментариев на 1000 видео.](https://tubularlabs.com/blog/3-metrics-youtube-success/#:~:text=Tips%20for%20YouTube%20Success%3A%20Aim%20for%20at%20least%205%20comments%20for%20every%201000%20video%20views)

> 165 / 1000 * 5 = 1 [штук] - комментариев оставляет пользователь в месяц

[Пользователи в среднем загружают 500 видео в минуту](https://exlibris.ru/news/statistika-youtube-2019-infografika/#:~:text=500%20%D1%87%D0%B0%D1%81%D0%BE%D0%B2%20%D0%B2%D0%B8%D0%B4%D0%B5%D0%BE%20%D0%B7%D0%B0%D0%B3%D1%80%D1%83%D0%B6%D0%B0%D0%B5%D1%82%D1%81%D1%8F%20%D0%BD%D0%B0%20YouTube%20%D0%BF%D0%BE%20%D0%B2%D1%81%D0%B5%D0%BC%D1%83%20%D0%BC%D0%B8%D1%80%D1%83%20%D0%B2%D1%81%D0%B5%D0%B3%D0%BE%20%D0%B7%D0%B0%20%D0%BC%D0%B8%D0%BD%D1%83%D1%82%D1%83.)

> 500 * 60 * 24 * 30 = 21 600 000 [штук/месяц] - загруженных видео в месяц
>
> 21 600 000 * 834 / 2600 = 6 928 615 [штук/месяц] - загруженных видео в месяц с учетом упрощения по CDN (см. выше)
>
> 6 928 615 / 834 000 000 = 0,008 [штук/месяц] - видео в месяц загружает 1 пользователь
>
> 165 / 0,008 = 20625 - раз поток просмотра превышает поток загрузок видео

Таблица действий - число операций в месяц для одного пользователя:

| **Действие**      | **Среднее количество в месяц [штук]** |
|-------------------|---------------------------------------|
| Лайк              | 6                                     |
| Комментарий       | 1                                     |
| Просмотрено видео | 165                                   |
| Загружено видео   | 0,008                                 |

### Технические метрики

#### Размер хранения

#### Видео

[1 час видео весит 8 Гб для 1080p.](https://7youtube.ru/news/skolko-vesit-video.html#:~:text=%D0%92%D1%81%D0%B5%20%D0%B2%D0%BF%D0%BE%D0%BB%D0%BD%D0%B5%20%D0%BB%D0%BE%D0%B3%D0%B8%D1%87%D0%BD%D0%BE%20%E2%80%93%20%D1%82%D0%B0%D0%BA%D0%B8%D0%B5%20%D1%84%D0%B0%D0%B9%D0%BB%D1%8B%20%D0%B8%D0%BC%D0%B5%D1%8E%D1%82%20%D0%B8%D0%B4%D0%B5%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D0%B5%20%D1%81%D0%BE%D0%BE%D1%82%D0%BD%D0%BE%D1%88%D0%B5%D0%BD%D0%B8%D1%8F%20%E2%80%93%20%D0%B2%D0%B5%D1%81/%D0%BA%D0%B0%D1%87%D0%B5%D1%81%D1%82%D0%B2%D0%BE.%20%D0%94%D0%BB%D1%8F%20%D0%B1%D1%8B%D1%81%D1%82%D1%80%D0%BE%D0%B3%D0%BE%20%D0%BF%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%B2%D0%BE%D0%B3%D0%BE%20%D0%B2%D0%BE%D1%81%D0%BF%D1%80%D0%BE%D0%B8%D0%B7%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D1%8F%20%E2%80%93%20%D1%8D%D1%82%D0%BE%20%D0%B8%D0%B4%D0%B5%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D0%B5%20%D1%80%D0%B5%D1%88%D0%B5%D0%BD%D0%B8%D0%B5.)

> 6 928 615 / 30 = 32 333 [ГБ] - в сутки
>
> 32 333 * 30 = 970 006 [ГБ] - в месяц
>
> 970 006 * 12 = 11 367 [ПБ] - за год
>
> 11 367 * 5 = 58 836 [ПБ] - за 5 лет
>
> 58 836 * 2 = 113 672 [ПБ] - с учетом сохранности данных и других причин, нужно хранить в 2 экземплярах по меньшей
> мере.

#### RPS в разбивке по типам запросов

Примем сценарий использования сервиса, по которому пользователи выбирают видео для просмотра двумя способами:

1. С главной страницы с рекомендациями: home page -> video
2. 2 Через поиск:                       home page -> search -> video

Таблица - число запросов при загрузке конкретной страницы для 1 пользователя: (добавить логин, регистрацию)

Будем отдельно вести учет работы WEB-сервера и Backend.
Т.к. приложение в 99% случаев имеет дело с публичным данными и их можно отдать WEB-сервером. WEB-сервер в данном
понимании
это отдача статики в 100% случаев. Backend - имеется ввиду именно API.

Таблица запросов при загрузке страниц:

| **Страница**                | **к API** | **За статикой в сумме** | **За картинками** | **За превью** | **За стилями** | **За анимациями и пр** |
|-----------------------------|-----------|-------------------------|-------------------|---------------|----------------|------------------------|
| Home page + recommendations | 3         | 70                      | 30                | 30            | 5              | 5                      |
| Search                      | 1         | 50                      | 20                | 20            | 5              | 5                      |
| Video                       | 4         | 51                      | 21                | 20            | 5              | 5                      |
| Load video                  | 1         | 0                       | 5                 | 0             | 2              | 2                      |

Home page + recommendations запросы к API:

- Получить рекомендации
- Получить тренды
- Получить данные пользователя

Search:

- Поиск по названию

Video:

- Само видео
- Данные по видео
- Комментарии
- Похожие видео

Load video:

- Загрузка видео

Примем сценарий использования сервиса, по которому пользователь смотрит видео 4 раза из главной страницы - рекомендаций
и 2 раза через поиск.

(Переделать от общих запросов к конкретно статике)

1. С главной страницы

Запросы на бэк:

> 3 + 4 = 7 - запросов на бэк для просмотра одного видео с главной страницы
>
> 7 * 4 = 28 - за 4 видео учитывая сценарии
>
> 28 * 524 млн = 14 672 [зап/сутки млн]

2. Через поиск

Запросы на бэк:

> 3 + 1 + 4 = 8 - запросов на бэк для просмотра одного видео с помощью поиска
>
> 8 * 2 = 16 - за 2 видео учитывая сценарии
>
> 16 * 524 = 8 384 [зап/сутки млн]

##### Запросы на бэк

> 18 864 + 8 384 = 23 056 [зап/сутки млн]

> 23 056 / (24 * 60 * 60) = 266 851 [RPS]

#### Лайки, комментарии, загрузки

> 524 млн * 5,5 = 2882 [видео/сутки млн]

> 2882 млн * 4% = 115,28 [лайки/сутки млн] - лайки
>
> 115,28 млн / (24 * 60 * 60) = 1334 [RPS]

> 2882 млн * 0.5% = 14,41 [комментариев/сутки млн] - комментарии
>
> 14,41 млн / (24 * 60 * 60) = 166 [RPS]

> 6 928 615 / 30 / (24 * 60 * 60) = 2,67 = 3 [видео/сутки] - загрузки
>
> 3 * 2 = 6 [RPS]

#### Статика

#### Картинки

> (30 + 21) * 4 + (20 + 21) * 2 + 5 * 3 = 301 [картинки/сутки] - по сценариям + загрузка видео
>
> 524 млн * 301 = 157 724 [зап/сутки млн]
>
> 157 724 / (24 * 60 * 60) = 1 825 509 [RPS]

#### Превью

> (30 + 20) * 4 + (20 + 20) * 2 = 280 [картинки/сутки] - по сценариям
>
> 524 млн * 280 = 146 720 [зап/сутки млн]
>
> 146 720 / (24 * 60 * 60) = 1 698 148 [RPS]

#### Стили

> (5 + 5) * 4 + (5 + 5) * 2 + 2 * 3 = 66 [картинки/сутки] - по сценариям + загрузка
>
> 524 млн * 66 = 34 584 [зап/сутки млн]
>
> 34 584 / (24 * 60 * 60) = 400 277 [RPS]

#### Анимации и пр.

> (5 + 5) * 4 + (5 + 5) * 2 + 2 * 3 = 66 [картинки/сутки] - по сценариям + загрузка
>
> 524 млн * 66 = 34 584 [зап/сутки млн]
>
> 34 584 / (24 * 60 * 60) = 400 277 [RPS]

| **Цель**       | **RPS**   |
|----------------|-----------|
| Картинки       | 1 825 509 |
| Превью         | 1 698 148 |
| Стили          | 400 277   |
| Анимации и пр. | 400 277   |
| Backend        | 266 851   |

## 3. Логическая схема <a name="3"></a>

![image](https://user-images.githubusercontent.com/88785411/231266235-87595f8d-1556-4ce6-953d-a500fdc18f0e.png)

## 4. Физическая схема <a name="4"></a>

### Хранение сессий пользователей

Для хранения сессий будем использовать in-memory key-value Redis. Благодаря тому, что Redis является in-memory
базой данных, то доступ к определенной записи будет практически равен доступу к оперативной памяти.
Помимо свой быстроты Redis поддерживает неблокирующую master-slave репликацию из коробки.

### Хранение видео

По типу хранения видео можно разделить на категории:

1. HDD
2. SSD

Изначально все видео будут загружаться на HDD, но те видео которые будут иметь популярность,
будем копировать на SSD, чтобы отдавать быстрее, когда популярность будет проходить они
будут удаляться с SSD. То есть в самом первом приближении "популярное" видео - это видео, которое входит
в топ 5% видео по числу просмотров за месяц.

### Профили, лайки, комментарии

Данные, для которых необходима надежность и не нужен столь быстрый доступ(профили, комментарии, лайки)
будем хранить в PostgreSQL. Для повышения доступности и надеждности сервиса будем использовать
технологию шардирования (железо -> VPS -> shard -> bucket) и в разных шардах
будем хранить информацию о разных сущностях проекта.

### Схемы хранения

![image](https://user-images.githubusercontent.com/88785411/231280353-bc752f45-3530-4a79-8d57-cae84c5fa025.png)

Где: \
Redis session - cluster хранения сессий \
Storage video - cluster хранения самого видео \
PostgreSQL video - шарды с данными о видео включая связи с другими сущностями
PostgreSQL meta - шарды с метаинформацией о видео, например, тэги \
PostgreSQL user - шарды с данными о пользователях, например личные данные, комменты, просмотры \
Prometheus - cluster хранения метрик \

![image](https://user-images.githubusercontent.com/88785411/231275421-e30fe662-3a17-427c-a379-5906f5144965.png)

Система buckets внутри шардов признана для унификации обращений. Сначала определяется шард потом бакет в нем для обращения.

## 5. Технологии <a name="5"></a>

| Технология  | Область применения             | Обоснование                                                           |
|-------------|--------------------------------|-----------------------------------------------------------------------|
| React       | Библиотека Frontend            | Ускорит процесс разработки                                            |
| Go          | Backend                        | Простота языка, удобные тулзы из коробки, высокая утилизация CPU      |
| Яндекс.Танк | Backend                        | Система нагрузочного тестирования                                     |
| GrayLog     | Backend                        | Система логирования                                                   |
| Vault       | Backend                        | Хранилище секретов                                                    |
| Jaeger      | Backend                        | Система трасировки                                                    |                                   |
| Redis       | Backend                        | In-memory БД позволяет обеспечить высокую скорость на чтение и запись |
| Prometheus  | Хранение метрик                | Хранилище метрик и работа с метриками                                 |
| Grafana     | Мониторинг                     | Мониторнг и алерты                                                    |
| PostgreSQL  | Хранение данных сущностей      | Обладает хорошей функциональностью и поддержкой                       |
| Nginx       | Web-server Reverse proxy       | Комерческий стандарт                                                  |
| GitLab      | Система контроля версий, CI/CD | Командная разработка, pipelines                                       |
| Kubernetes  | Deploy                         | Масштабируемая система deploy                                         |

## 6. Схема проекта <a name="6"></a>

In process ...
