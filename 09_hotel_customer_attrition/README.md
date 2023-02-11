# Прогнозирование оттока клиентов в сети отелей "Как в гостях".

Заказчик этого исследования — сеть отелей «Как в гостях». 

Чтобы привлечь клиентов, эта сеть отелей добавила на свой сайт возможность забронировать номер без предоплаты. Однако если клиент отменял бронирование, то компания терпела убытки. Сотрудники отеля могли, например, закупить продукты к приезду гостя или просто не успеть найти другого клиента.

Бизнес-метрика и другие данные.

Основная бизнес-метрика для любой сети отелей — её прибыль. Прибыль отеля — это разница между стоимостью номера за все ночи и затраты на обслуживание: как при подготовке номера, так и при проживании постояльца. 
В отеле есть несколько типов номеров. В зависимости от типа номера назначается стоимость за одну ночь. Есть также затраты на уборку. Если клиент снял номер надолго, то убираются каждые два дня. 

Стоимость номеров отеля:
- категория A: за ночь — 1 000, разовое обслуживание — 400;
- категория B: за ночь — 800, разовое обслуживание — 350;
- категория C: за ночь — 600, разовое обслуживание — 350;
- категория D: за ночь — 550, разовое обслуживание — 150;
- категория E: за ночь — 500, разовое обслуживание — 150;
- категория F: за ночь — 450, разовое обслуживание — 150;
- категория G: за ночь — 350, разовое обслуживание — 150.

В ценовой политике отеля используются сезонные коэффициенты: весной и осенью цены повышаются на 20%, летом — на 40%.
Убытки отеля в случае отмены брони номера — это стоимость одной уборки и одной ночи с учётом сезонного коэффициента.
На разработку системы прогнозирования заложен бюджет — 400 000. При этом необходимо учесть, что внедрение модели должно окупиться за тестовый период. Затраты на разработку должны быть меньше той выручки, которую система принесёт компании.

Описание данных.
В таблицах hotel_train и hotel_test содержатся одинаковые столбцы:
- id — номер записи;
- adults — количество взрослых постояльцев;
- arrival_date_year — год заезда;
- arrival_date_month — месяц заезда;
- arrival_date_week_number — неделя заезда;
- arrival_date_day_of_month — день заезда;
- babies — количество младенцев;
- booking_changes — количество изменений параметров заказа;
- children — количество детей от 3 до 14 лет;
- country — гражданство постояльца;
- customer_type — тип заказчика:
- Contract — договор с юридическим лицом;
- Group — групповой заезд;
- Transient — не связано с договором или групповым заездом;
- Transient-party — не связано с договором или групповым заездом, но связано с бронированием типа Transient.
- days_in_waiting_list — сколько дней заказ ожидал подтверждения;
- distribution_channel — канал дистрибуции заказа;
- is_canceled — отмена заказа;
- is_repeated_guest — признак того, что гость бронирует номер второй раз;
- lead_time — количество дней между датой бронирования и датой прибытия;
- meal — опции заказа:
  SC — нет дополнительных опций;
  BB — включён завтрак;
  HB — включён завтрак и обед;
  FB — включён завтрак, обед и ужин.
- previous_bookings_not_canceled — количество подтверждённых заказов у клиента;
- previous_cancellations — количество отменённых заказов у клиента;
- required_car_parking_spaces — необходимость места для автомобиля;
- reserved_room_type — тип забронированной комнаты;
- stays_in_weekend_nights — количество ночей в выходные дни;
- stays_in_week_nights — количество ночей в будние дни;
- total_nights — общее количество ночей;
- total_of_special_requests — количество специальных отметок.

## Цель исследования:

Необходимо разработать систему, которая предсказывает отказ от брони. Если модель покажет, что бронь будет отменена, то клиенту предлагается внести депозит. Размер депозита — 80% от стоимости номера за одни сутки и затрат на разовую уборку. Деньги будут списаны со счёта клиента, если он всё же отменит бронь.

## Итог исследования:

В ходе исследования был проведен исследовательский анализ признака, которые влияют на бронирование номеров в отелях.
Сеть отелей «Как в гостях», чтобы привлечь клиентов, эта сеть отелей добавила на свой сайт возможность забронировать номер без предоплаты. Однако если клиент отменял бронирование, то компания терпела убытки.
Был выяснен портрет прдполагаемого "ненадёжного" клиента, Клиент, который заблаговременно забронировал номер, указав необходимость в парковочном месте

Lead_time — количество дней между датой бронирования и датой прибытия наиболее тесно связано с тем, отменено бронирование или нет. Количество необходимых парковочных мест (requires_car_parking_spaces) также является функцией, которая наиболее сильно коррелирует с нашей целью отмены. По мере увеличения количества запросов на парковочные места вероятность отмены бронирования уменьшается.

В ходе исследования были разработаны модели ML, которые учитывая специфику и условия заказчика рассчитали возможную прибыль от внедрения депозитной системы оплаты, в ходе работы были использованы такие методы как, проверка уникальных значений и дубликатов, проверка корреляции признаков, OneHotEncoder и OrdinalEncoder, масштабирование, кросс-валидация. Проверялись модели, Решающее дерево, логистическая регрессия, случайный лес и классификация линейных опорных векторов, с проверкой на адекватность.

Максимальную прибыль показала модель DecisionTreeClassifier - 40,54 млн против 32,52 млн рублей до введения системы. Минимальный прирост прибыли дала модель Logistic Regression. Основная цель нашей модели — максимизировать прибыль, путем минимизации убытков, в контексте моделей - это означает минимизировать количество ложноотрицательных ответов, то есть основная метрика для нас стала Recall. Модель DecisionTreeClassifier показывала 0.7396 на обучающей выборке и на тестовой выборке результат были отличные - 0.9513.

Чтобы добится наилучших показателей, для модели Дерева решений, необходимо установить следующие параметры:

- 'class_weight' - 'balanced',
- 'criterion' - 'entropy',
- 'max_depth' - 2,
- 'min_samples_leaf' - 40

В совокупности с другими показателями, рекомендуется использовать модель DecisionTreeClassifier, Модель правильно определяет "надежность" клиента в 50 % бронирований. Рассматривая более конкретно каждую категорию, правильно классифицирует 93 % отмененных бронирований и 38 % не отмененных бронирований.
В то же время прибыль предсказанная составляет 40,54 млн, что казалось бы в значительной степени превосходит значение прибыли без внедрения депозита и точно позволит окупить затраты на разработку системы прогнозирования равную 0.4 млн., однако если учесть вероятность худшего сценария, при котором лояльным клиентам, которых будут ложно определять в "ненадёжных", будут предлагать внести депозит, то тогда предсказанная прибыль резко сокращается, составляя всего 24 млн.. Рекомендовать, однозначно, внедрение новой системы очень рисковано.

## Стек технологий:

`pandas`, `matplotlib`, `numpy`, `seaborn`, `scikit-learn`, `imbalanced-learn`

## Статус проекта:

Завершен.