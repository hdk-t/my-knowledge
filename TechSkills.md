技術系ナレッジ
# よく使うLinuxコマンド
## chmod
	1:--x
	2:-w-
	3:-wx
	4:r--
	5:r-x
	6:rw-
	7:rwx
## rsync
### 現在のサーバーから/tmp/test.txtを他のサーバーの/tmpに送る
	rsync -avz -P /tmp/test.txt {ユーザー名}@{送り先のサーバーのIP}:/tmp/
### 現在のサーバーの/tmpへ他のサーバーの/tmp/test.txtを取りに行く
	rsync -avz -P {ユーザー名}@{取りに行くサーバーのIP}:/tmp/test.txt /tmp/
## ディスク容量調査
### 現在の階層のディレクトリごとの容量を表示
	du ./ -h --max-depth=1  
### サイズ順
	du ./ -h --max-depth=1 | sort -hr  
## inode容量調査
### / 配下のファイル数
	find / -xdev -type f | cut -d "/" -f 2 | sort | uniq -c | sort -nr  
### /var 配下のファイル数
	find /var -xdev -type f | cut -d "/" -f 3 | sort | uniq -c | sort -nr  
## MySQLダンプ
### ダンプ
	mysqldump -u{USER} -p --default-character-set=binary --opt {スキーマ名} {テーブル名(無いとスキーマごとdump)} /tmp/{ダンプファイル名}.sql
### 圧縮しながらダンプ
	mysqldump -u {USER} -p --default-character-set=binary {スキーマ名} {テーブル名(無いとスキーマごとdump)} | gzip > /tmp/{ダンプファイル名}.sql.gz  
### インポート
	mysql -u{USER} -p --default-character-set=binary {スキーマ名} < /tmp/{ダンプファイル名}.sql
	※ -f を付けるとエラーを無視する
### 解凍しながらインポート
	mysql -u{USER} -p --default-character-set=binary {スキーマ名} | gzip > /tmp/{ダンプファイル名}.sql.gz
## tar関係
### tarに圧縮
	tar -cvzf {圧縮後ファイル名.tar.gz} {圧縮対象ディレクトリ}
### tarを解凍
	tar -xvzf {圧縮後ファイル名.tar.gz}
## ZIP主にファイル
### ZIPに圧縮(ディレクトリを圧縮)
	zip {ファイル名}.zip {ファイル名}
### ZIPを解凍
	unzip {ZIPファイル名}
### GZIPを解凍
	gzip -d *ファイル名.gz*
## snmp
### snmpが通るか確認
	snmpwalk -v 2c -c {SNMP_USER} {対象のIPアドレス}
## ハードウェア系
### 物理CPUの数
	cat /proc/cpuinfo | grep "physical id"
### プロセッサー数
	cat /proc/cpuinfo | grep processor
### コア数
	cat /proc/cpuinfo | grep "cpu cores"
### メモリ容量
	cat /proc/meminfo
### ディスクの詳細を確認
	hdparm -I {/dev/sda など}
### ディスクの書き込み量を確認
	smartctl -a {/dev/sda など} | grep -e Total_LBAs_Written -e "TLC_Writes_32MiB" -e "Sector Size"
## RPM
### RPMファイルからインストール
	rpm -ivh {RPMファイル名}
### RPMでインストール済一覧を表示
	rpm -qa
### RPMでインストールしたミドルウェアを削除
	rpm -e {middleware name}
## llコマンド
### 容量が大きい順位並べる
	ll -S
### 日付順に並べる
	ll -t
## サーバー負荷調査
### メモリ使用量を表示
	free -h
	vmstat 1
### CPUコアごとの負荷を表示
	mpstat -u -P ALL 1
## grep関係
### 現在のディレクトリでgrep
	grep -r 文字列 ./*
### ディレクトリ名で検索コマンド
	find /opt /home /usr /etc /bin /data /mnt -type d -iname {検索するディレクトリ名}
### ルートフォルダと拡張子と文字列を複数指定してgrep
	nice -n 10 \
	find /opt /home /usr /etc /bin /data /root /mnt \
	-type f -name "*.sh" -or -name "*.properties" -or -name "*.xml" -or -name ".env" -or -name "*.php" -or -name "*.java" -or -name "*.cs" -or -name "*.vb" | \
	xargs grep -ni -e {文字列1} -e {文字列2} -e {文字列3}
## ミドルウェアの依存関係を確認
	ldd {プログラム名}
# よく使う正規表現
## 行末とマッチ
```php
$
```
## 行頭とマッチ
```php
^
```
## 文字列以降とマッチ
```php
(?<=文字列).*$
```
## 数字と改行以外とマッチ
```php
[^0-9(\n)]
```
## 文字列を含む行全体とマッチ
```php
^.*文字列.*\n
```
## 文字列を含む行以外とマッチ
```php
^(?!.*(文字列)).*\R
```
## 複数の文字列を含む行以外とマッチ
```php
^(?!.*(文字列1|文字列2)).*\R
```
## 末尾が文字列とマッチ
```php
.*文字列$
```
## 行頭から3文字の文字列とマッチ
```php
^...
```
. の数で文字数を変更できる  
## 2連続した改行とマッチ
```php
\n{2}
```
## 囲われた文字列とマッチ
### { }に囲われた文字列とマッチ  
```php
(?<=[{]).*?(?=[}])
```
### \<img>\</img>に囲われた文字列とマッチ(複数の場合は\[ \]が不要)    
```php
(?<=\<img\>).*?(?=\<\/img\>)
```
### 囲い文字も含む場合
```php
\[img\](.*?)\[\/img\]
```
## 文字列以降とマッチ
```php
(?<=文字列)(.*)
```
## 文字列以前とマッチ
```php
(.*)(?=文字列)
```
## YouTubeの動画ページURLとマッチ(パラメータは除去)
ex) https://www.youtube.com/watch?v=9ERgjchxsiM ~~?param=1~~  
```php
https:\/\/www.youtube.com\/watch\?v=[\w\/:%#\$&\(\)~\.=\+\-]+
```
## 4桁の数値とマッチ(西暦判定とかで使える)
```php
^\d{2}$
// OR
^[0-9]{2}$
```
## 正規表現でTSVやCSVを操作する
#### 置換前
```php
(.+){区切り文字}(.+){区切り文字} ...(.*)
``` 
##### 例 3カラムのCSVの場合
```php
(.+),(.+),(.*)
```
#### 置換後
$nで変数を好きなように埋め込む  
```php
$1 $2 ...$n
```
# Elastic Search
## クエリパラメータ
### 単数(WHERE =)
```json
{
  "query": {
    "bool": {
      "must": {
        "match": {
          "_id": "123456"
        }
      }
    }
  }
}
```
### 複数(WHERE IN)
```json
{
  "query": {
    "bool": {
      "must": {
        "terms": {
          "_id": ["123", "456"]
        }
      }
    }
  }
}
```
### 複数条件(AND)
```json
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "field_name1": 123
          }
        },
        {
          "match": {
            "field_name2": "abc"
          }
        }
      ]
    }
  }
}
```
### 複数条件否定(NOT)
```json
{
  "query": {
    "bool": {
      "must": [
        {
          "match": { "field_name1": 123 }
        }
      ],
      "must_not": [
        {
          "match": { "field_name2": "abc" }
        }
      ]
    }
  }
}
```
### フィールドが存在していないもの
```json
{
  "query": {
    "bool": {
      "must_not": [
        {
          "exists": {
            "field": "field_name"
          }
        }
      ]
    }
  }
}
```
# Git
## ブランチを作成する際
- ブランチを切る際のブランチ名は{親ブランチ}\_{ブランチ内容}などにすると分かり易い
# 運用
## 一括処理に置ける動作確認/調査方法
- ログを仕込む場所を考える(切り替え点など)
- ミドルウェアの設定を見直す
- 応急処置手段を用意しておく
## サーバー負荷チェック
topコマンドでLoadAverageを確認
	top
LoadAverageが1コアにつき3.0以上割り当てられる場合だと負荷が高いと言える  
1.0以下なら余裕  
2.0弱が普通  
例 CPU:1コアのサーバーでLA:3.0だと負荷が高い  
## 実行時間が長いバッチ・Jobの大幅改修を行う際のチェック項目
- 全体のデータフローを図にする
- 失敗する可能性の高いJobの失敗時の手順を作成する
## 処理ボトルネック調査手順
まずは、ミドルウェアのログをチェック  
前回の処理との比較を行う  
- 実行時間
- 出力内容
### 確認ポイント
- サーバーの負荷状況をチェック
### もし負荷が高い場合
どの処理がネックになっているか確認する  
I/O系の処理を疑う
- DBならスロークエリ
- httpサーバーならアクセスログ
## 設定ファイルを変更
前提として、サーバーの設定ファイルはGit管理すべきだが、現実はそう甘くない  
- 編集前にバックアップを取る
    cp -p file file.yyyymmdd  

- 編集 OR 置換する
	sed -i {ファイル名} -e "s|{置換前}|{置換後}|g"

- 編集後にdiffで差分を比較する
	diff file file.yyyymmdd
# コンテナ運用
## コンテナのログ削除
### ログファイルの場所を確認
	docker inspect {コンテナ名} | grep -i log
### ログサイズを確認して削除
	ll {コンテナログファイルの場所}
	cat /dev/null > {コンテナログファイルの場所}
## 未使用のボリュームの削除
	docker volume prune 
## 全ての未使用のコンテナとコンテナイメージ(キャッシュ含む)を削除
	docker system prune -a
# Nginxログローテート設定
## 設定ファイルを編集
    vi /etc/logrotate.d/nginx

```conf
/var/log/nginx/*log {
    daily
    rotate 7
    missingok
    notifempty
    sharedscripts
    compress
    delaycompress
    postrotate
        /usr/sbin/nginx -s reopen >/dev/null 2>&1 || true
    endscript
}
```
## 反映する
    logrotate /etc/logrotate.conf
## 反映確認
    less /var/lib/logrotate/logrotate.status
# MySQL
## my.conf
### innodb_buffer_pool_size
推奨のサイズはインスタンスのメモリの8割と言われている
## INSERTが遅い場合
まず、常時遅いのか、時々遅いのかをはっきりさせる。  
常に遅い場合  
- テーブルのデータが多い
- 挿入するデータ量が多い
- DBがMyISAM
	- MyISAMだと更新が遅く、参照が早い
	- InnoDBだと更新が早く、参照が遅い

たまに遅くなる場合  
- OS自体の負荷が高い
## DEAD LOCK確認
	SHOW ENGINE INNODB STATUS;
## レプリケーション停止
### マスターとスレーブがズレることを許容するならばSKIPする
```sql
SHOW SLAVE STATUS;
SET GLOBAL SQL_SLAVE_SKIP_COUNTER = 1;
START SLAVE;
SHOW SLAVE STATUS;
```
もしMySQL5.7ならチャンネル名を指定してあげなければいけない(複数のマスターを持つことができるため)  
```sql
START SLAVE FOR channel '{チャンネル名}';
```
## レプリカのマスターの設定を変更
```sql
CHANGE MASTER TO 
MASTER_HOST='{HOST}',
MASTER_PORT={PORT},
MASTER_USER='{USER}',
MASTER_PASSWORD='{PASS}',
MASTER_LOG_FILE='{SHOW MASTER STATUS Relay_Master_Log_File}',
MASTER_LOG_POS={SHOW MASTER STATUS Exec_Master_Log_Pos};
```
## binlog系
### binlogを出力
    mysqlbinlog --no-defaults {binlog file} > binlog.sql
### binlogをSQLにデコンパイルして出力
    mysqlbinlog --no-defaults --verbose {binlog file} > binlog.sql
### 日付期間を指定して出力
    mysqlbinlog --no-defaults --start-datetime="{開始日(yyyy-mm-dd hh:mm:ss)}" --stop-datetime="{終了日(yyyy-mm-dd hh:mm:ss)}" {binlogファイル} > binlog.sql
### ポジションを指定して出力
    mysqlbinlog --no-defaults --start-position={開始ポジション} --stop-position={終了ポジション} {binlogファイル} > binlog.sql
## テーブル破損によるレプリケーション停止の復旧手順
```sql
SHOW SLAVE STATUS;
-- エラー内容に try to repair itが含まれていたらテーブル破損の可能性がある  

CHECK TABLE {テーブル名};
-- Table is marked as crashed
-- になってたらクラッシュしてる

REPAIR TABLE {テーブル名};
-- {テーブル名}	check	status	OK   
-- になっていればOK  

START SLAVE;  
```
## データベース単位でオプティマイズ
	mysqlcheck -o -u{ユーザー} -p {スキーマ名} {テーブル名};
## DBのディスク容量削減
### binlogを削除する(パージ)
```sql
	PURGE BINARY LOGS TO '{binlogファイル名}';
```
指定したbinlogを残して、これ以前のログを削除する  
※注意 レプリカのレプリケーションが追い付いていない状態で実行してはならない
### スローログを削除する
	cat /dev/null > {スローログファイル名}
## MySQLクエリチューニング
### MySQL EXPLAIN 早見表
#### type
const:  Primary keyもしくはユニークインデックスによるアクセス。最速。
eq_ref: JOIN時にPrimary keyもしくはユニークインデックスを利用したアクセス
ref:	ユニークではないインデックスによる等価検索
range:	インデックスを用いた範囲検索
index:	フルインデックススキャン
ALL:	フルテーブルスキャン
### 基本
- サブクエリがあれば、サブクエリ単位でEXPLAINする
- テンポラリテーブルを使用する
- UNION -> UNION ALL
- WHERE句は左辺に変数を置いてはならない
- SELECT句のロジックは消す
### クエリキャッシュ
DBの負荷が高い場合はクエリキャッシュの設定を疑ってみる
### 複合PKの注意
順番が左にあるPKで検索した場合はPKが効く
### IN句の注意点
複数PKの場合、IN句で複数条件を指定するよりも = で1つの条件を指定した方がパフォーマンスが向上する場合がある
### マルチカラムINDEXの注意
マルチカラムINDEXの場合は左から順にWHERE条件を付け足さないとINDEXが効かない
# システム設計
## ドキュメント(必須順)
- 画面設計書
  - 画面一覧
  	- 画面遷移図
  	- 画面デザイン図
  - 入力項目一覧
  - 表示項目一覧
  - イベント一覧
  	- フローチャート
  - メッセージ一覧
  	- エラーメッセージ
  	- ダイアログメッセージ
- テーブル定義書
	- ER図
	- エンティティ／データモデル
- 仕様書／機能一覧
	- API仕様書
	- インターフェイス設計書(アプリ＜-＞リソース)
	- フローチャート
	- CRUD図
	- ステータス遷移図
- テスト仕様書
- インフラ構成図
- サイトマップ
- WBS
- 開発ルール
	- コーディングルール
	- スタイルガイド
- 運用設計書
## テーブル設計
### 共通
- 正規化
	- これはn対nの関係だと把握して設計する
- テーブル数は可能な限り少なくする
- 中途半端な状態でレコードに保存される可能性がある場合はnull許容型にして、ビジネスロジックでバリデーションする
- リレーションする場合はリレーションが削除された場合の動きも想定しておく(いいねテーブルとユーザーテーブルが良い例)
### マスタテーブル設計
- マスタの内容をユーザーに選択させる場合は、sortカラムを入れる
- マスタの表示名と説明を必ず入れる
- マスターの変更が頻繁にあり、リレーション先でマスターに応じた分岐処理がある場合、不具合が起こりやすいので、この場合は時点データが取得される前提の設計にする(注文履歴と商品マスタが良い例)
### リレーション

## テーブル定義変更
- 定義変更処理中はテーブルがロックされる
- レコード数が多い場合は実行時間に注意
EX: xxxレコードのテーブルのVARCHARの文字数追加の実行時間 -> 5 min 10 sec
## カラム設計
- 設定項目
	- カラム名
	- 型
	- Null許容可否
	- デフォルト値
	- インデックス
	- プライマリキー
	- カラムの位置
	- コメント

- 数値でマイナス値を使用しないならUNSIGNEDにする
- 文字列系の場合は照合順序を指定できる(けどそんなに気にしなくてもよさそう)
## 処理予約テーブル設計で必要なカラム
- 処理ID
- 処理優先順位
- 処理モード
- 処理ステータス
- 処理予約時間
- 処理開始時間
- 処理終了時間
# SJISで文字化けする文字
```txt
①②③④⑤⑥⑦⑧⑨⑩⑪⑫⑬⑭⑮⑯⑰⑱⑲⑳ⅠⅡⅢⅣⅤⅥⅦⅧⅨⅩ㍉㌔㌢㍍㌘㌧㌃㌶㍑㍗㌍㌦㌣㌫㍊㌻㎜㎝㎞㎎㎏㏄㎡㍻〝〟№㏍℡㊤㊥㊦㊧㊨㈱㈲㈹㍾㍽㍼≒≡∫∮∑√⊥∠∟⊿∵∩∪纊褜鍈銈蓜俉炻昱棈鋹曻彅丨仡仼伀伃伹佖侒侊侚侔俍偀倢俿倞偆偰偂傔僴僘兊兤冝冾凬刕劜劦勀勛匀匇匤卲厓厲叝﨎咜咊咩哿喆坙坥垬埈埇﨏塚增墲夋奓奛奝奣妤妺孖寀甯寘寬尞岦岺峵崧嵓﨑嵂嵭嶸嶹巐弡弴彧德忞恝悅悊惞惕愠惲愑愷愰憘戓抦揵摠撝擎敎昀昕昻昉昮昞昤晥晗晙晴晳暙暠暲暿曺朎朗杦枻桒柀栁桄棏﨓楨﨔榘槢樰橫橆橳橾櫢櫤毖氿汜沆汯泚洄涇浯涖涬淏淸淲淼渹湜渧渼溿澈澵濵瀅瀇瀨炅炫焏焄煜煆煇凞燁燾犱犾猤猪獷玽珉珖珣珒琇珵琦琪琩琮瑢璉璟甁畯皂皜皞皛皦益睆劯砡硎硤硺礰礼神祥禔福禛竑竧靖竫箞精絈絜綷綠緖繒罇羡羽茁荢荿菇菶葈蒴蕓蕙蕫﨟薰蘒﨡蠇裵訒訷詹誧誾諟諸諶譓譿賰賴贒赶﨣軏﨤逸遧郞都鄕鄧釚釗釞釭釮釤釥鈆鈐鈊鈺鉀鈼鉎鉙鉑鈹鉧銧鉷鉸鋧鋗鋙鋐﨧鋕鋠鋓錥錡鋻﨨錞鋿錝錂鍰鍗鎤鏆鏞鏸鐱鑅鑈閒隆﨩隝隯霳霻靃靍靏靑靕顗顥飯飼餧館馞驎髙髜魵魲鮏鮱鮻鰀鵰鵫鶴鸙黑ⅰⅱⅲⅳⅴⅵⅶⅷⅸⅹ￢￤ⅰⅱⅲⅳⅴⅵⅶⅷⅸⅹⅠⅡⅢⅣⅤⅥⅦⅧⅨⅩ￢＇＂㈱№㏍℡纊褜鍈銈蓜俉炻昱棈鋹曻彅丨仡仼伀伃伹佖侒侊侚侔俍偀倢俿倞偆偰偂傔僴僘兊兤冝冾凬刕劜劦勀勛匀匇匤卲厓厲叝﨎咜咊咩哿喆坙坥垬埈埇﨏塚增墲夋奓奛奝奣妤妺孖寀甯寘寬尞岦岺峵崧嵓﨑嵂嵭嶸嶹巐弡弴彧德忞恝悅悊惞惕愠惲愑愷愰憘戓抦揵摠撝擎敎昀昕昻昉昮昞昤晥晗晙晴晳暙暠暲暿曺朎朗杦枻桒柀栁桄棏﨓楨﨔榘槢樰橫橆橳橾櫢櫤毖氿汜沆汯泚洄涇浯涖涬淏淸淲淼渹湜渧渼溿澈澵濵瀅瀇瀨炅炫焏焄煜煆煇凞燁燾犱犾猤猪獷玽珉珖珣珒琇珵琦琪琩琮瑢璉璟甁畯皂皜皞皛皦益睆劯砡硎硤硺礰礼神祥禔福禛竑竧靖竫箞精絈絜綷綠緖繒罇羡羽茁荢荿菇菶葈蒴蕓蕙蕫﨟薰蘒﨡蠇裵訒訷詹誧誾諟諸諶譓譿賰賴贒赶﨣軏﨤逸遧郞都鄕鄧釚釗釞釭釮釤釥鈆鈐鈊鈺鉀鈼鉎鉙鉑鈹鉧銧鉷鉸鋧鋗鋙鋐﨧鋕鋠鋓錥錡鋻﨨錞鍗鎤鏆鏞鏸鐱鑅鑈閒隆﨩隝隯霳霻靃靍靏靑靕顗顥飯飼餧館馞驎髙髜魵魲鮏鮱鮻鰀鵰鵫鶴鸙黑～
```
# 非機能要件
## 稼働率
逐次処理において、バッチがどれくらいの余力があるかを算出する
- 処理開始時間と処理完了時間が存在するテーブルに作成
- 期間あたりの処理開始時間と処理完了時間の差分の時間を全て合算する
- その期間の処理件数を算出する
- 場合によっては処理モードごとで分ける
### SQL例
```sql
-- 日時ごとでGROUP BY
SELECT
	DATE_FORMAT(a.update_on, '%Y/%m/%d'),
	COUNT(*)
FROM table_a AS a
	INNER JOIN table_b AS b ON a.id = b.id
WHERE a.update_on BETWEEN '2023-05-07 00:00:00' AND '2023-05-13 23:59:59'
GROUP BY DATE_FORMAT(a.update_on, '%Y/%m/%d');
```
## テスト
### テストした方が良いケース
- 認証機能とユーザー種別が存在するアプリは他の種別のユーザーでテストする
- リスト系表示の0件のパターン
- マスター系データが不整合のパターン
- 一括更新系のロジックは、無関係のデータが更新されていないか、更新日時を範囲指定して確認する
- シングルページアプリケーションの場合、画面遷移時やモーダル開閉時のグローバル変数の初期化に注意する

# コーディング
## DBに更新を行う際のバリデーションの基本
- 文字数チェック
- 型チェック
## ログ出力
```
ログの出力の基本は5W1H

When → いつその処理が実行されたか  
Who → 誰がその処理を呼び出したか  
Where → どこで実行されているのか  
What → 何をするためにその処理が呼び出されたか  
Why → なぜその処理が実行されたのか
```
https://qiita.com/tadashiro_ninomiya/items/19c774898c68add6185e
## N + 1問題の対処法
- JOIN句を使用する
- LaravelならEloquentのwith関数を使用する
## コードレビュー
- フレームワークやプロジェクトのコーディングコーディング規約に沿ってあるか
- 適切に関数を作成しているか
- ネストが深すぎないか
- N + 1が発生していないか
- よくあるバグが潜んでいないか
	- nullポ
	- 入力値チェック
	- データが0件や存在しない場合のパターンに対応しているか
# ShellScript
## PINGチェック
```sh
#!/bin/bash
# 引数で受け取ったIP達のpingが通るかのチェックをするだけのシェル 

for arg in "$@"
do
    /bin/ping $arg -c 1 > /dev/null 2> /dev/null
    if [ $? = 0 ]; then
        echo $arg "ping success!!"
    else
        echo $arg "ping failed!!"
    fi
done
```
## MySQLからデータを取得
```shell
	#!/bin/bash

	DB_IP={HOST}
	DB_PORT=3306
	DB_USER={USER}
	DB_PWD={PASS}
	DB={DATABASE}
	DUMP_FILE=/tmp/select_result.txt

	mysql -u${DB_USER} -h${DB_IP} -P${DB_PORT} -p${DB_PWD} -D${DB} --skip-secure-auth -e "
	SHOW DATABASES;
	" | tee ${DUMP_FILE}
```
## 古いディレクトリを削除
```sh
#!/bin/bash

REF_DATE=`date --date "3 years ago" '+%Y%m'`

# 対象のディレクトリに移動
cd /tmp/target_dir

# 古いディレクトリを抽出する
for DIR in `ls`; do
        if [[ $DIR < $REF_DATE ]] ; then
                OLD_DIRS+=($DIR)
        fi
done

# 抽出したディレクトリを削除
for DIR in  ${OLD_DIRS[@]}; do
        rm -rf $DIR
        echo "Deleted Directry ${DIR}"
done
```
## 引数にパスと日数を受け取って、そのパスの配下の最終更新が引数の日数より前のディレクトリを全て削除する
```sh
#!/bin/bash
# 引数にパスと日数を受け取って、そのパスの配下の最終更新が引数の日数より前のディレクトリを全て削除するシェル

PATH=/tmp       # 念のため
SUBTRACT_DAY=36500   # 念のため

PATH=$1
SUBTRACT_DAY=$2

TODAY=`date +%Y%m%d`
PAST_DAY=`date +%Y%m%d --date \"$SUBTRACT_DAY day ago\"`

# 対象ディレクトリに移動
cd $PATH

# 削除対象を抽出
TARGETS=`find ./ -type d -newermt '$PAST_DAY' -and ! -newermt '$TODAY' | xargs stat -c'%y %n'`

# 削除実行
$TARGETS | xargs rm -rf
echo $TARGETS
```
# JavaScript
### 参考文献

[配列を征する者はJSを制す。JavaScriptのスマートな配列操作テクニック](https://ics.media/entry/200825/#find2)
## 連想配列からフィルタリングして取得する方法
```javascript
var heroes = [
	{name: "Batman”, franchise: "DC"},
	{name: "Ironman”, franchise: "Marvel"},
	{name: "Thor”, franchise: "Marvel"},
	{name: "Superman”, franchise: "DC"},
];

const marvelHeroes = heroes.filter((hero) =>
	hero.franchise == "Marvel"
);

// こっちの書き方でもOK
const marvelHeroes = heroes.filter(function (hero){
	return hero.franchise == "Marvel";
});

// [ {name: "Ironman", franchise: "Marvel"}, {name: "Thor", franchise: "Marvel"} ]
```
`marvelHeroes`は必ず配列で取得されるので、1件だけ取得する事が期待される場合は
`marvelHeroes[0]`で取得する必要がある
### 配列が全て条件に一致しているか
```js
const isNoValid = dataList.every(data => data.id !== 5);

if (isNoValid === true) {
  console.log("IDが5のデータはありません");
}
```
### 配列の中に特定の条件が含まれているか
```js
const arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

arr.some(value => value < 10)
// return true
```
## 正規表現でYouTubeの動画URLとマッチ

JavaScriptではリテラルに応じてエスケープが違うので注意
```js
str.match("https:\/\/www.youtube.com\/watch\\?v=[\\w\/:%#\\$&\\(\\)~\\.=\\+\\-]+");
```
# PHP
## PHPでCSVを読む
### DBに登録する際に分割して登録する
chunk関数で配列を分割して登録する
### 5C問題
- PHPは基本UTF-8しか読み込めない
- SJISで読むと文字列によっては改行と判定される 例 ソ", が改行と判定される
https://qiita.com/Ken_tk/items/5f3715d7f97c620fa42c
## composer.json の書き方
```json
{
	"package": {
		"package-a": "1.2.1", // バージョン固定値
		"package-b": ">1.2.1", // バージョン範囲指定
		"package-c": ">=1.2.1", // バージョン範囲指定
		"package-d": "<1.2.1", // バージョン範囲指定
		"package-e": "<=1.2.1", // バージョン範囲指定
		"package-f": "!=1.2.1",  // このバージョン以外
		"package-g": ">=1.2.1 <2.1.0",  // バージョン範囲指定
		"package-h": ">=1.2.1 <2.1.0 || >=3.0.1",  // バージョン範囲指定
		"package-i": "1.2.1-2.0.0",  // バージョン範囲指定 以上 以下
		"package-j": "*", // ワイルドカードでバージョン範囲指定
		"package-k": "1.*", // ワイルドカードでバージョン範囲指定 1系の最新
		"package-l": "~1.2.1", // >=1.2.1 <1.3.0 と同じ指定 
		"package-m": "^1.2.1" // >=1.2.1 <2.0.0 と同じ指定
	}
}
```
## Laravel
### 命名規約(オリジナル含む)

| 機能名    | Controller アクション 名 | HTTPメソッド  | パス                       | ルート名             |
| ------ | ------------------ | --------- | ------------------------ | ---------------- |
| 一覧画面   | index              | GET       | /リソース名                   | resource.index   |
| 作成画面   | create             | GET       | /リソース名/create            | resource.create  |
| 作成処理   | store              | POST      | /リソース名                   | resource.store   |
| 詳細画面   | show               | GET       | /リソース名/{resource}        | resource.show    |
| 編集画面   | edit               | GET       | /リソース名/{resource}/edit   | resource.edit    |
| 編集処理   | update             | PUT/PATCH | /リソース名/{resource}        | resource.update  |
| 削除確認画面 | delete             | GET       | /リソース名/{resource}/delete | resource.delete  |
| 削除処理   | destroy            | DELETE    | /リソース名/{resource}        | resource.destroy |
### FormRequestの書き方
```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;
use Log;

class SampleForm extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * バリデーションルールに行う処理
     */
    protected function prepareForValidation()
    {
        // NOP
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array<string, mixed>
     */
    public function rules()
    {
        return [
            'name' => 'required',
        ];
    }

    /**
     * Custom varidation messages
     *
     * @return array<string, mixed>
     */
    public function messages()
    {
        return [
            'name.required' => ':attributeは、必須です。',
    }
}

```
### Faker一覧
[FakerPHP](https://fakerphp.github.io/formatters/)
[非公式リファレンス](https://fwhy.github.io/faker-docs/formatters/lorem/word.html)
### Intervertion/image ライブラリの使い方
[完全網羅！Intervention Image（PHP）で画像を編集する全実例](https://blog.capilano-fw.com/?p=1574)
#### 画像ドライバのインスタンスを取得
ImageMagickの場合はImagick関数が使えるので便利  
```php
// ImageMagick or GD のインスタンスが取得できる
$img->getCore();

// ImageMagickの場合これでEXIFデータを削除できる
$img->stripImage();
```
### 419 page expired エラー対策
ログイン画面にリダイレクトさせてあげると親切
[セッション切断時にPOST通信をし419エラーになるのを防ぐ方法](https://zenn.dev/yuzuyuzu0830/articles/3952e36dc91705)
### Eloquentで実際のSQLを見る方法
```php
$query = Model::find($id);
preg_replace_array('/\?/', $query->getBindings(), $query->toSql());
```
# CSS
## padding
値を1つ指定した場合： 指定した値が|上下左右|のパディングになります。  
値を2つ指定した場合： 記述した順に|上下|左右|のパディングになります。  
値を3つ指定した場合： 記述した順に|上|左右|下|のパディングになります。  
値を4つ指定した場合： 記述した順に|上|右|下|左|のパディングになります。  
# アイディア
## サジェスト
標準のサジェストが付いている場合は、文字列連結でヒットさせるワードを増やすことができる
# UI/UX
## ランディングページ
### 重要な情報はページを開いてすぐに見せる
(例)
- 無料/割引率などの価格アピール
- 有名人の登場
## 入力フォーム
### チェックボックス
- チェック数を表示する