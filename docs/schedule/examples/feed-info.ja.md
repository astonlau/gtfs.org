---
search:
  exclude: true
---

<a class="pencil-link" href="https://github.com/MobilityData/gtfs.org/edit/main/docs/schedule/examples/feed-info.md" title="Edit this page" target="_blank">
    <svg class="pencil" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M10 20H6V4h7v5h5v3.1l2-2V8l-6-6H6c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h4v-2m10.2-7c.1 0 .3.1.4.2l1.3 1.3c.2.2.2.6 0 .8l-1 1-2.1-2.1 1-1c.1-.1.2-.2.4-.2m0 3.9L14.1 23H12v-2.1l6.1-6.1 2.1 2.1Z"></path></svg>
  </a>

# フィード情報

<hr>

## GTFSデータセットに関する情報の提供

機関やそのサービスに関する情報を提供するだけでなく、[feed_info.txt](../../reference/#feed_infotxt)ファイルを使って、GTFSデータセットに関する情報を提供することも可能です。これには

- パブリッシャーの詳細
- フィードの言語
- フィードの有効性
- バージョン

以下は、Transport for Cairioの例です。

[**feed_info.txt**](../../reference/#feed_infotxt)

    feed_publisher_name,feed_publisher_url,feed_lang,feed_start_date,feed_end_date,feed_version
    Transport for Cairo,http://transportforcairo.com/,en,20160101,20161201,0.5

[ソース例](https://github.com/transportforcairo/Metro-GTFS/archive/master.zip#Metro-GTFS-master)