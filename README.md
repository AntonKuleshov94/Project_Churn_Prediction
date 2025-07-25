# Прогнозирование оттока пользователей с образовательного сайта

# Цель: 
Провести анализ поведения пользователей сайта образовательной платформы и построить модель прогнозирования оттока (churn prediction) на основе данных из Yandex Metrika за период с 2023-11-23 по 2024-11-22

# Задачи проекта
# 1. Исследовательский анализ (EDA)
- Подготовить и очистить данные
- Выявить закономерности в поведении пользователей
- Обнаружить аномалии и проблемы
- Выделить рекомендации к дальнейшим действиям
- Предложить рекомендации по снижению оттока

# 2. Построение модели churn prediction (предсказание, какие пользователи с высокой вероятностью не вернутся на сайт)
- Выявить ключевые признаки оттока
- Построить модели для прогнозирования вероятности не возвращения
- Сравнить качество предсказания моделей и выделить наиболее подходящую модель

# Структура
- `notebooks/1_exploration_analysis.ipynb` — очистка и подготовка данных; анализ; графики; корреляции
- `notebooks/2_modelling_and_features.ipynb` — ключевые признаки; модели; оценка качества
- `data/` — исходные данные

# Описание признаков
VisitID - уникальный идентификатор визита,
Date - дата визита,
DateTime - дата и время визита,
DateTimeUTC - дата и время визита в UTC,
IsNewUser - первое ли это посещение пользователя с установленным идентификатором clientID,
StartURL - ссылка, с которой начался визит,
EndURL - ссылка, на которой закончился визит,
PageViews - кол-во просмотренных страниц,
VisitDuration - длительность визита,
Bounce – факт отказа (см. словарь),
RegionCountry - страна визита,
RegionCity - город визита,
ClientID - уникальный идентификатор пользователя,
CounterUserIDHash - уникальный идентификатор пользователя для установленного счётчика Яндекс Метрики,
Referer - откуда пришёл пользователь,
LastTrafficSource - источник траффика; например, ad - реклама, organic - органический поиск и т. п.,
MobilePhone - модель устройства, если визит осуществлялся на телефоне,
OperatingSystemRoot - операционная система на устройстве,
OperatingSystem - операционная система + версия ОС,
Browser - браузер, который использовался для визита,
UTMCampaign - название рекламной кампании,
UTMContent - создержание рекламного баннера/описание,
UTMMedium - тип траффика (канал привлечения),
UTMSource - источник трафика,
UTMTerm - ключевое слово в баннере,
registration_left - оставил ли регистрацию в этот день,
total_regs – абсолютное кол-во регистраций пользователя, оставленных за временной период предоставляемых данных.

# Запуск
1. Установка библиотек:
pip install -r ../requirements.txt

2. Среда программирования:
jupyter notebook

# Используемые модели
- DecisionTreeClassifier
- RandomForestClassifier
- ExtraTreesClassifier
- GradientBoostingClassifier
- AdaBoostClassifier
- BaggingClassifier
- XGBoost (xgboost.XGBClassifier)
- LightGBM (lgb.LGBMClassifier)
- CatBoost (cb.CatBoostClassifier)

# Оценка моделей:
- Precision/Recall
- F1-score
- time

# Рекомендации
# Улучшение сайта и продукта:
1. Повысить конверсию новых пользователей — показывать короткие, содержательные страницы с явными УТП и призывом к регистрации.
2. Адаптировать UX для мобильных устройств — улучшить структуру, навигацию и производительность.
3. Внедрить триггерные механизмы — всплывающие окна с предложением регистрации после длительного просмотра.
4. Работа с городами с высокой конверсией — масштабировать кампании в регионы типа НН.
5. В Москве выделяться на фоне конкурентов — через УТП и сравнительные преимущества.
6. Использование моделей

# Дальнейшие исследования:
1. Причины отказов и падений регистраций — сегментировать по страницам, анализ причин по источникам, устройствам.
2. Поведение новых пользователей — сделать тепловые карты и анализ кликов, чтобы понять потребности.
3. Качество кампаний — отслеживание UTM-меток, воронок, проверка трекинга.
4. Пути пользователя — построение воронок и drop-off точек.
5. Глубинные интервью или юзабилити-тесты — чтобы понять реальные барьеры на пути к регистрации.

# Итоги по тестированию моделей:
Модель LightGBM хорошо справляется с задачей: она почти не пропускает тех, кто действительно уйдёт (высокая полнота), но иногда ошибочно помечает как "ушедших" тех, кто на самом деле остался (не идеальная точность). В целом, она даёт сбалансированные предсказания и работает быстро.
1. Precision = 0.836 - из всех, кого модель предсказала как "ушёл", 83.6% действительно ушли.
2. Recall = 0.949 - из всех реально ушедших, модель нашла 94.9%, целевая метрика качества.
3. F1-score = 0.889 - хороший баланс.
4. time = 2.24 сек

При необходимости повысить recall можно использовать LightGBM с удалением из исходных данных выбросов и нулей (recall = 0.977), но при этом падает precision (0.804)

При необходимости повысить precision можно использовать LightGBM с подбором гиперпараметров (precision = 0.861), но при этом будет падение recall (0.890), однако модель станет более сбалансированной

# ✍️ Автор
Антон Кулешов

