---
search:
  exclude: true
---

<a class="pencil-link" href="https://github.com/MobilityData/gtfs.org/edit/main/docs/schedule/examples/translations.md" title="Edit this page" target="_blank">
    <svg class="pencil" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M10 20H6V4h7v5h5v3.1l2-2V8l-6-6H6c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h4v-2m10.2-7c.1 0 .3.1.4.2l1.3 1.3c.2.2.2.6 0 .8l-1 1-2.1-2.1 1-1c.1-.1.2-.2.4-.2m0 3.9L14.1 23H12v-2.1l6.1-6.1 2.1 2.1Z"></path></svg>
  </a>

# 翻訳

<hr>

## 駅名を翻訳する

ベルギー国鉄（オランダ語でNationale Maatschappij der Belgische Spoorwegen、フランス語でSociété nationale des chemins de fer belges、現地ではNMBS/SNCBとして知られています）は、乗り換えサービスを多言語で行っているプロバイダがあります。同社は駅名を多言語で提供しており、ユーザーの言語や位置情報の設定に応じて表示されます。

NMBS/SNCBは、以下のファイルのようにフランス語でGTFSデータを公開しています。

[**agency.txt**](../../reference/#agencytxt)

    agency_id,agency_name,agency_url,agency_timezone,agency_lang
    NMBS/SNCB,NMBS/SNCB,http://www.belgiantrain.be/,Europe/Brussels,fr

代理店言語がフランス語に設定されているので、[stops.txtには](../../reference/#stopstxt)駅名がフランス語でリストアップされています。

[**stops.txt**](../../reference/#stopstxt)

    stop_id,stop_code,stop_name,stop_desc,stop_lat,stop_lon,zone_id,stop_url,location_type,parent_station,platform_code
    S8815040,,Bruxelles-Ouest,,50.8485600,4.32104000,,,1,,
    S8821006,,Anvers-Central,,51.2172000,4.42109800,,,1,,

そして、[translations.txt](../../reference/#translationstxt)ファイルは、駅名をデフォルトのエージェンシー言語（この場合はフランス語）からオランダ語に翻訳するために使用されます。

[**translations.txt**](../../reference/#translationstxt)

    table_name,field_name,record_id,language,translation
    stops,stop_name,S8815040,nl,Brussel-West

- この例では、[stops.txt](../../reference/#stopstxt)の駅名が翻訳され、[stops.txt](../../reference/#stopstxt)のレコードが`stop_id` で識別されます。そのため
  - `stopsは`テーブル名の下に記載されています（[stops.txtを](../../reference/#stopstxt)参照）。
  - `stop_nameは` `field_nameの`下に記載される（駅名が翻訳されるため）。
  - フランス語から翻訳される駅名の`stop_idは` `record_idの`下に記載されている（この場合、Bruxelles-Ouestの`stop_id`）。
- 名称はオランダ語（nl）に翻訳されています。
- 最後に翻訳された名称を翻訳に記載

GTFSで名前を翻訳するには、[translations.txtという](../../reference/#translationstxt)ファイルを使う方法もあり、ここでは`record_idの`代わりに`field_valueという`フィールドが使われる。この場合、[stops.txt](../../reference/#stopstxt)xtから翻訳するレコードを探すのに駅名が使われる。

[**translations.txt**](../../reference/#translationstxt)

    table_name,field_name,field_value,language,translation`
    stops,stop_name,Bruxelles-Ouest,nl,Brussel-West

[ソース例](http://gtfs.irail.be/mivb/mivb-gtfs.zip)