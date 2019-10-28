# ユーザAPI

## 概要

自作APIをこのフォルダ配下に格納します。

APIはシナリオで指定したパラメータ `API` と `type` を用いて実行するマクロファイルを検索します

## 検索順序

1. `user-api\[type]\[API].ttl`
2. `user-api\_global\[API].ttl`
3. `api\[type]\[API].ttl`
4. `api\_global\[API].ttl`

該当するマクロファイルが見つかった時点でそのマクロを読み込み、以降の検索は行いません。

## 作成方法

基本的には標準API ( `api` フォルダ配下) の書き方を参照してください。

`api\_global\raw_command.ttl` がシンプルでわかりやすいと思います。

また、APIから更にAPIを読み込むことは出来ません。
これはteraterm及びteracottaの仕様に関係しています。

teracottaでは、APIを呼び出すために `lib\api.ttl` をincludeしていますが、teratermマクロは同じマクロファイルを入れ子で読み込むことが出来ません。

そのため、どうしてもAPIを重ねて呼び出したい場合は、 `lib\api.ttl` を直接呼び出せばそれも可能になります。
ただし、動作確認はしていませんので保証は致しません。