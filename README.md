# Яндекс.Директ

Курсовая работа в рамках 3-го семестра программы по Веб-разработке ОЦ VK x МГТУ им. Н.Э. Баумана (ex. "Технопарк") по дисциплине "Проектирование высоконагруженных сервисов"

#### Автор - [Стома Фёдор](https://park.vk.company/profile/f.stoma/ "Страница на портале VK x МГТУ")
#### Задание - [Методические указания](https://github.com/init/highload/blob/main/homework_architecture.md)

#### Содержание:
1. [Тема, функционал и аудитория](#1)
2. [Расчёт нагрузки](#2)
3. [Глобальная балансировка нагрузки](#3)
4. [Локальная балансировка нагрузки](#4)
5. [Логическая схема базы данных](#5)
6. [Физическая схема базы данных](#6)
7. [Алгоритмы](#7)
8. [Технологии](#8)
9. [Обеспечение надёжности](#9)
10. [Схема проекта](#10)
11. [Расчёт ресурсов](#11)

## Тема, функционал и целевая аудитория <a name="1"></a>

### 1.1 Тема
[Яндекс.Директ](https://direct.yandex.ru/) - система контекстной рекламы на страницах «Яндекса» и сайтах партнёров Рекламной системы Яндекса.

### 1.2 Функционал сервиса (MVP)
- Показ рекламных объявлений
Показ рекламных объявлений может совершаться по различным таргетингам, например:
 1) Ключевые слова в поисковом запросе
 2) Географический
 3) По полу и возрасту
 4) Временной

- Создание рекламных объявлений

- Модель продажи рекламы
  1) CPC (цена за клик)
  2) Количество показов объявлений
 
- Управление рекламными компаниями (настройка, просмотр статистики)

### 1.3 Целевая аудитория
Пользователи:
- Среднемесячная аудитория рекламной сети Яндекса - 93.1 млн. человек [^1]
- Среднесуточная аудитория рекламной сети Яндекса - 65 млн. человек [^1]

Рекламодатели:
- Среднесуточное количество показа рекламных объявлений - 4.96 млрд. объявлений [^2]
- Количество активных рекламодателей Яндекс Директа - 1,5 млн. человек 

## Расчёт нагрузки <a name="2"></a>
### 2.1 Расчет
Объем рекламы в Яндекс.Директ зависит от типа рекламы:
- Текстово-графическая реклама - 1 Кб [^3]
- Медиа реклама - от 150 Кб до 10 Мб [^3]
- Видео реклама - от 1 Мб до 100 Мб [^3]  
В поиске Яндекса доминируют текстово-графические объявления, которые составляют более 90% от всех рекламных показов. Примем средний размер медиа и видео рекламы за 5Мб:  
`1 Кб * 4.96 * 10^9 * 0.9 + 1 Мб * 4.96 * 10^9 * 0.1 = 2369.2 Тб` - общий объем памяти, занимаемый объявлениями

`4.96 * 10^9 / 24 / 3600 = 57407` - средний RPS показов
Средний RPS кликов: 263
Тогда сетевой трафик составит:  
`1 Кб * 57407 * 0.9 + 5 Мб * 57407 * 0.1 = 224 Гбит/c`

Допустим, каждый рекламодатель в среднем запрашивает статистику около 4 раз в сутки. Получим:  
`1.5 * 10^6 * 4 / 24 / 3600 = 69` - средний RPS по запросу статистики рекламной кампании

По данным из итогов модерации рекламы в Яндекс [^2] Пульт управления рекламой используют 112 млн. раза в месяц. Следовательно:  
`112 * 10^6 / 31 / 24 / 3600 = 41` - средний RPS по созданию рекламы

### 2.2 Продуктовые метрики
|Метрика|Значение|Обозначение|
| ------------- | -------------|--|
|MAU (чел.)|93.1 млн.|MAU|
|DAU (чел.)|65 млн.|DAU|
|Размер текстово-графической рекламы|1Кб|SIZE_GRATH|
|Размер медиа рекламы|до 10Мб|SIZE_MEDIA|
|Размер видео рекламы|до 100Мб|SIZE_VIDEO|
|Количество рекламодателей|1.5 млн.|ADVERTISER_ALL|
|Новых объявлений в месяц|112 млн.|ADVERTISEMENT_NEW|
|Количество объявлений|4.96 млрд.|ADVERTISEMENT_ALL|
|Объем памяти, занимаемый объявлениями|2369.2 Тб|SIZE_ALL|

### 2.3 Технические метрики
| Запрос                              | RPS (средний)   | RPS (пиковый) |
| ----------------------------------- | --------------- | ------------  |
| Создание объявлений                 | 41              | 82            |
| Просмотр статистики                 | 69              | 138           |
| Подбор объявлений                   | 57407           | 114814        |
| Обработка кликов                    | 263             | 526           |

| Запрос                              | Потребляемый трафик (средний) |
| ----------------------------------- | ----------------------------- |
| Подбор объявлений                   | 224 Гбит/с                    |
 
## Глобальная балансировка нагрузки <a name="3"></a>


## Локальная балансировка нагрузки <a name="4"></a>


## Логическая схема базы данных <a name="5"></a>


## Физическая схема базы данных <a name="6"></a>


## Алгоримты <a name="7"></a>


## Технологии <a name="8"></a>


## Обеспечение надёжности <a name="9"></a>


## Схема проекта <a name="10"></a>


## Расчёт ресурсов <a name="11"></a>


### Список источников:
[^1]: [Статистика аудитории Яндекс.Директ в 2024 году](https://yandex.ru/support/direct/general/yan.html)
[^2]: [Итоги модерации рекламы в Яндекс](https://ya.ru/project/admoderation/2023)
[^3]: [Технические ограничения для объявлений Яндекс.Директ](https://yandex.ru/support/direct/moderation/technical-restrictions.html)
