---
search:
  exclude: true
---

<a class="pencil-link" href="https://github.com/MobilityData/gtfs.org/edit/main/docs/schedule/examples/routes-stops-trips.md" title="Edit this page" target="_blank">
    <svg class="pencil" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M10 20H6V4h7v5h5v3.1l2-2V8l-6-6H6c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h4v-2m10.2-7c.1 0 .3.1.4.2l1.3 1.3c.2.2.2.6 0 .8l-1 1-2.1-2.1 1-1c.1-.1.2-.2.4-.2m0 3.9L14.1 23H12v-2.1l6.1-6.1 2.1 2.1Z"></path></svg>
  </a>

# Маршруты, остановки и поездки

<hr/>

## Маршруты

Маршруты являются основой работы маршрутных транспортных средств, поскольку они описывают географический охват транзитной сети. В GTFS определение маршрутов - это первый шаг к описанию работы транзитного агентства.

Первым шагом является добавление информации об агентстве, как показано в файле [agency.txt](../../reference/#agencytxt) ниже. Этот файл содержит высокоуровневую информацию об агентстве.

[**agency.txt**](../../reference/#agencytxt)

    agency_id,agency_name,agency_url,agency_timezone,agency_lang,agency_phone
    CT,Calgary Transit,http://www.calgarytransit.com,America/Edmonton,,403-262-1000

Calgary Transit управляет LRT, BRT, регулярным автобусным сообщением, паратранзитом и транспортом по требованию в Калгари, AB. В данном примере определены два маршрута, первый - автобусный, а второй - LRT. Используя файл [routes.txt](../../reference/#routestxt), каждому маршруту присваивается уникальный id, короткое и длинное название для удобства чтения.

[**routes.txt**](../../reference/#routestxt)

    agency_id,route_id,route_short_name,route_long_name,route_type,route_url,route_color,route_text_color
    CT,303-20670,303,MAX Orange Brentwood/Saddletowne,3,www.calgarytransit.com/content/transit/en/home/rider-information/max.html,#ff8000,#ffffff
    CT,202-20666,202,Blue Line - Saddletowne/69 Street CTrain,0,www.calgarytransit.com/content/transit/en/home/rider-information/lrt-and-bus-station-maps.html,#ff0000,#ffffff

Пятое поле`route_type`) используется для различения типов маршрутов:

- Первый - автобусный, следовательно, `route_type=3`.
- Второй - LRT, следовательно, `route_type=0`.
- Полный список значений для параметра `route_type` можно найти [здесь](../../reference/#routestxt).

Остальные поля содержат дополнительную информацию, такую как url, специфичный для маршрута, а также цвета, характерные для конкретного агентства, для представления услуг на карте.

<hr/>

## Остановки

В GTFS остановки и станции описываются с помощью файла [stops.txt](../../reference/#stopstxt), ниже, в первой записи определена автобусная остановка, а во второй - станция ЛРТ.

[**stops.txt**](../../reference/#stopstxt)

    stop_id,stop_code,stop_name,stop_lat,stop_lon,location_type
    8157,8157,44th Avenue NE (SB),51.091106,-113.958565,0
    6810,6810,NB Marlborough CTrain Station,51.058990,-113.981582,0

- `stop_id` - уникальный идентификатор
- `stop_code` и `stop_name` обычно содержат информацию, ориентированную на пассажиров.
- Точное местоположение предоставляется с помощью координат`(stop_lat` и `stop_lon`).
- Шестое поле`(location_type`) используется для отличия остановок от станций.
- Первая запись соответствует автобусной остановке, следовательно, `location_type=0`
- Вторая запись соответствует станции, следовательно, `location_type=1`
- Полный список значений для ` location_type  `можно найти [здесь](../../reference/stopstxt)

<hr/>

## Поездки

После описания маршрутов агентства теперь можно описать поездки, которые обслуживает каждый маршрут.

Сначала необходимо определить период обслуживания с помощью файла [calendar.txt](../../reference/#calendartxt).

[**calendar.txt**](../../reference/#calendartxt)

    service_id,monday,tuesday,wednesday,thursday,friday,saturday,sunday,start_date,end_date
    weekend_service,0,0,0,0,0,1,1,20220623,20220903

Здесь описывается услуга, которая работает только по субботам и воскресеньям, поэтому поля для этих дней заполнены 1, а поля для остальных дней заполнены нулями. Эта услуга работает с 23 июня 2022 года по 3 сентября 2022 года, как показано в полях `start_date` и `end_date`.

В этом примере файл [trips.txt](../../reference/#tripstxt) описывает 3 поездки по выходным, которые обслуживаются маршрутом MAX Orange, описанным выше.

[**trips.txt**](../../reference/#tripstxt)

    route_id,service_id,trip_id,trip_headsign,direction_id,shape_id
    303-20670,weekend_service,60270564,"MAX ORANGE SADDLETOWNE",0,3030026
    303-20670,weekend_service,60270565,"MAX ORANGE BRENTWOOD",1,3030027
    303-20670,weekend_service,60270566,"MAX ORANGE BRENTWOOD",1,3030027

- В файле routes [routes.txt](../../reference/#routestxt) указан `route_id`, соответствующий MAX Orange.
- Перечислен `service_id` из [calendar.txt](../../reference/#calendartxt), соответствующий выходным.
- Каждая запись содержит уникальный id для каждой trip

Представлен text заголовка, который обычно отображается на табличках внутри и снаружи автобуса.

- Поле `direction_id` позволяет различать поездки по одному и тому же маршруту, следующие в разных направлениях. Например, различие между входящими и исходящими поездками - или поездками в южном направлении и поездками в северном направлении.
  - В данном случае поездки в направлении Saddletowne имеют `direction_id=0`, а поездки в направлении Brentwood - `direction_id=1`. Значения в direction_id не имеют собственного значения, они используются только для определения одного направления движения по сравнению с другим.
- Для первой записи указан `shape_id` из [shapes.txt](../../reference/#shapestxt), который соответствует маршруту MAX Orange в направлении Saddletowne, а для второй и третьей записей - для маршрута MAX Orange в направлении Brentwood.

`shape_id=3030026` соответствует MAX Orange в направлении Saddletowne. В приведенном ниже файле содержится информация о точках, очерчивающих маршрут, а также расстояние между этими точками. С помощью этой информации можно нанести маршрут на карту для планирования trip или аналитических целей.

[**shapes.txt**](../../reference/#shapestxt)

    shape_id,shape_pt_lat,shape_pt_lon,shape_pt_sequence,shape_dist_traveled
    3030026,51.086506,-114.132259,10001,0.000
    3030026,51.086558,-114.132371,10002,0.010
    3030026,51.086781,-114.132865,10003,0.052
    3030026,51.086938,-114.133179,10004,0.080
    3030026,51.086953,-114.133205,10005,0.083
    3030026,51.086968,-114.133224,10006,0.085
    3030026,51.086992,-114.133249,10007,0.088
    3030026,51.087029,-114.133275,10008,0.093
    3030026,51.087057,-114.133286,10009,0.096
    3030026,51.087278,-114.133356,10010,0.121
    3030026,51.087036,-114.132864,10011,0.165
    3030026,51.086990,-114.132766,10012,0.173
    3030026,51.086937,-114.132663,10013,0.183

<hr/>

## Исключения услуг

Можно определить исключения для услуги, такие как added дни обслуживания (специальные дни) или УБРАННЫЕ дни обслуживания (например, отсутствие обслуживания в праздничные дни).

Например, если в воскресенье 17 июля 2022 года нет scheduled обслуживания - тогда эту дату можно удалить из `weekend_service` в [calendar.txt](../../reference/#calendartxt), разбив обслуживание на две части:

| Сервис             | start      | end        |
| ------------------ | ---------- | ---------- |
| `weekend_service1` | `20220623` | `20220716` |
| `weekend_service2` | `20220718` | `20220903` |

Однако это усложняет файл, так как `service_id` разбивается на две части, и это разбиение будет каскадировать на [trips.txt](../../reference/#tripstxt). Вместо этого можно сделать это более простым способом, используя [calendar_dates.txt](../../reference/#calendar_datestxt), как показано ниже:

[**calendar_dates.txt**](../../reference/#calendar_datestxt)

    service_id,date,exception_type
    weekend_service,20220623,2

- Указан `service_id` `weekend_service`
- Дата удаленной или added услуги указывается под `date` (17 июля 2022 года).
- Поле `exception_type` установлено в 2, что означает, что услуга удалена на этот день.

[Пример источника](https://data.calgary.ca/download/npk7-z3bj/application%2Fzip)