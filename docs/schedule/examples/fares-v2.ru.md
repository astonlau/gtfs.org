---
search:
  exclude: true
---
<a class="pencil-link" href="https://github.com/MobilityData/gtfs.org/edit/main/docs/schedule/examples/fares-v2.md" title="Edit this page" target="_blank">
    <svg class="pencil" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M10 20H6V4h7v5h5v3.1l2-2V8l-6-6H6c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h4v-2m10.2-7c.1 0 .3.1.4.2l1.3 1.3c.2.2.2.6 0 .8l-1 1-2.1-2.1 1-1c.1-.1.2-.2.4-.2m0 3.9L14.1 23H12v-2.1l6.1-6.1 2.1 2.1Z"></path></svg>
  </a>

# Fares V2

<hr/>

Fares V2 - это проект расширения GTFS, который направлен на устранение ограничений Fares V1. Этот проект расширения принимается итерациями, и приведенные ниже примеры показывают, что можно смоделировать, используя ту часть Fares V2, которая была принята в официальной спецификации. Более подробную информацию о проекте расширения Fares V2 можно найти в разделе " [Расширения](../../../extensions) ".

В промежуточный период производители могут реализовать Fares V2 наряду с реализацией Fares V1 в одном и том же наборе данных, поскольку между ними нет технического конфликта. Потребители будут иметь выбор, какую реализацию использовать, независимо друг от друга. После принятия и достаточного одобрения Fares V2, Fares V1 может быть отменен в будущем.

## Определение тарифа на проезд

Существует несколько способов оплаты проезда в транзитной системе TTC в Торонто. TTC предлагает два варианта стоимости проезда в зависимости от способа оплаты. Стоимость проезда взрослого составляет 3,20 CAD при использовании карты PRESTO, а при оплате наличными - 3 ,25 CAD.

Транзитные билеты или тарифы можно описать с помощью файла [fare_products.txt](../../reference/#fare_productstxt). Каждая запись соответствует определенному тарифу.

[**fare_products.txt**](../../reference/#fare_productstxt)

    fare_product_id,fare_product_name,amount,currency
    presto_fare,PRESTO Card Fare,3.2,CAD
    cash_fare,Cash Fare,3.25,CAD

[Пример источника](https://www.ttc.ca/Fares-and-passes)

<hr/>

## Описать места обслуживания в одной тарифной зоне.

Некоторые транзитные агентства используют зональную структуру тарифов. Тарифные зоны - это разделенные географические области, в которых действуют разные цены на проезд. Независимо от того, едут ли они в пределах одной зоны или из одной зоны в другую, пассажирам необходимо знать правильную стоимость проезда. В системе TTC Торонто существует только одна тарифная зона. Транзитные пассажиры просто должны платить по единому билету за любую trip в пределах города Торонто, независимо от того, какой вид транспорта используется.

Зоны оплаты проезда можно описать с помощью файла [stops_areas.txt](../../reference/#stops_areastxt), который назначает остановки из файла [stops.txt](../../reference/#stopstxt) в файл [areas.txt](../../reference/#areastxt).

Сначала определите зону (тарифную зону).

[**areas.txt**](../../reference/#areastxt)

    area_id,area_name
    ttc_service_area,TTC Fare Zone

Затем, используя `stop_id` из файла [stops.txt](../../reference/#stopstxt), сгруппируйте остановки вместе в соответствующую определенную область (тарифную зону).

[**stops_areas.txt**](../../reference/#stops_areastxt)

    area_id,stop_id
    ttc_service_area,262
    ttc_service_area,263
    ttc_service_area,264
    ttc_service_area,265
    ttc_service_area,266
    …

[Источник примера](http://opendata.toronto.ca/toronto.transit.commission/ttc-routes-and-schedules/OpenData_TTC_Schedules.zip)

<hr/>

## Создайте правила для поездок по одному билету

В GTFS, тарифный этап соответствует trip, которую пассажир совершает без пересадок между различными видами транспорта, маршрутами, сетями или агентствами. Единый тариф позволяет пассажирам совершать поездки в пределах любой пары автобусных остановок, станций метро и остановок трамвая в сети TTC.

Группы остановок определяют поездки внутри сети от пункта отправления до пункта назначения (или от набора пунктов отправления до набора пунктов назначения, если идентификаторы районов соответствуют сгруппированным остановкам). В приведенном ниже файле описаны два правила для поездки в любую точку сети TTC. Первое правило соответствует поездке по тарифу PRESTO, а второе - по наличному тарифу.

[**fare_leg_rules.txt**](../../reference/#fare_leg_rulestxt)

    leg_group_id,network_id,from_area_id,to_area_id,fare_product_id
    ttc_trip_presto,ttc_network,ttc_service_area,ttc_service_area,presto_fare
    ttc_trip_cash,ttc_network,ttc_service_area,ttc_service_area,cash_fare

[Пример источника](https://www.ttc.ca/Fares-and-passes)

<hr/>

## Создание правил для пересадок

Велосипедисты, использующие карту PRESTO для проезда в TTC, имеют 2-часовое окно неограниченных пересадок. Это означает, что в течение 2 часов они могут неограниченное количество раз пересаживаться с автобуса на автобус, метро и трамвай.

[**fare_transfer_rules.txt**](../../reference/#fare_transfer_rulestxt)

    from_leg_group_id,to_leg_group_id,transfer_count,duration_limit,duration_limit_type,fare_transfer_type,fare_product_id
    ttc_trip_presto,ttc_trip_presto,-1,7200,3,0,free_transfer

Приведенный выше файл представляет это в GTFS со следующими полями:

- Пересадка возможна на ноги и с ног, оплаченных картой PRESTO`(ttc_trip_presto`).
- `transfer_count` устанавливается в `-1`, так как нет ограничения на количество разрешенных трансферов.
- `duration_limit` установлено на `7200` секунд, что эквивалентно 2 часам
- `duration_limit_type` установлен на `3`, поскольку пассажиры должны прикоснуться к своей карте PRESTO, как только они входят в зону оплаты проезда или как только они садятся в автобус или трамвай. Это соответствует подтверждению тарифа arrival на текущем участке и подтверждению тарифа arrival на следующем участке.
- `fare_transfer_type` установлен на `0`, так как пассажиры платят только за первый проезд. Не существует платы за пересадку или второго тарифа за пересадку в течение 2 часов. Следовательно, стоимость можно смоделировать как сумму первого тарифа и суммы сборов за пересадку.
- Плата за пересадку равна нулю, так как в системе TTC она бесплатна. На это указывает `fare_product_id=free_transfer`.

[Источник примера](https://www.ttc.ca/Fares-and-passes/PRESTO-on-the-TTC/Two-hour-transfer)