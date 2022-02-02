# PukiWiki用プラグイン<br>Folding@home情報表示 fahstats.inc.php

Folding@home の統計情報を表示する[PukiWiki](https://pukiwiki.osdn.jp/)用プラグイン。  
ユーザー（ドナー）個人統計とチーム統計の2種類を表示できます。

統計情報は公式APIから取得します。  
負荷軽減のためと、API側の情報更新頻度が低いうえ応答を返さないこともあるため、結果はキャッシュして一定時間使い回します。

※ ご注意：APIの仕様変更により、当プラグインが機能しなくなる場合があります。

|対象PukiWikiバージョン|対象PHPバージョン|
|:---:|:---:|
|PukiWiki 1.5.3 ~ 1.5.4RC (UTF-8)|PHP 7.4 ~ 8.1|

## インストール

下記GitHubページからダウンロードした fahstats.inc.php を PukiWiki の plugin ディレクトリに配置してください。

[https://github.com/ikamonster/pukiwiki-fahstats](https://github.com/ikamonster/pukiwiki-fahstats)

## 使い方

```
&fahstats(userName|teamID);
```

userName … ユーザー名
teamID … チームID

## 使用例

```
&fahstats(FahstatsSample);
&fahstats(999999);
```

## 設定

ソース内の下記の定数で動作を制御することができます。

|定数名|値|既定値|意味|
|:---|:---:|:---:|:---|
|PLUGIN_FAHSTATS_API_INTERVAL|数値|(4 * 60 * 60)|APIアクセス間隔（秒）※短くしすぎてサーバーに負荷をかけないよう注意|
|PLUGIN_FAHSTATS_API_TIMEOUT|数値|30|APIタイムアウト時間（秒）|
|PLUGIN_FAHSTATS_RECURSIVE|0 or 1|0|1:入れ子の情報を再帰的に走査する。テキスト引数でより詳細な情報を表示したいときに使う|

## 高度な使い方：表形式ではなく、任意の文字列で表示

```
&fahstats(userName | teamId){text};
```
text … 表示する文字列。文字列内の「%キー名%」が対応する値に置換される。主なキー／値は下表の通り。


|キー|値|
|:---|:---|
|name|名前|
|score|スコア|
|wus|ワークユニット数|
|last|最終処理日時|
|rank|順位|
|active_50|過去50日間の使用クライアント数|
|active_7|過去7日間の使用クライアント数|
|users|総ユーザー数|
|id|チームID|

※ドナー統計時のみ有効、チーム統計時のみ有効な値もあります。他のキー／値など、詳しくは本プラグイン実行後に生成されるキャッシュファイル ``cache/fahstats.*.dat`` 内を参照してください。

### 使用例
```
&fahstats(FahstatsSample){%name%さんのスコアは%score%、処理したワークユニット数は%wus%です。};
&fahstats(999999){%name%チームのスコアは%score%、処理したワークユニット数は%wus%です。};
```
