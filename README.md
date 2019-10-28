# Teracotta

Teracotta(テラコッタ)は客先にTeratermとExcelしかない可愛そうなインフラエンジニア達のためのTeratermマクロフレームワークです。

Ansibleを意識したネットワーク作業の抽象化を目的としています。

**# For in English (and more) users**

Teracotta is a Teraterm macro framework for use when the development environment given to customers has only Teraterm and Excel.

My goal is to abstract network device settings like Ansible.

Currently, most of the text in this repository is written in Japanese. If you want to support your language, please fork this repository or discuss it in an issue.

## できること

* **機器リスト、コマンドリストを読み込むための標準機能**
よく一般的に書かれているTeratermマクロでは、コマンドリストと機器リストを読み込んでそれぞれの機器にコマンドを流すのをしてると思います。Teracottaでは、機器リストを**インベントリ**、コマンドリストを**シナリオ**と呼んで標準機能としています。

* **INI形式で書くシナリオ**
機器への接続(telnet/ssh/console) から、ログ取得、コマンド実行などを自動化するためにTeratermマクロを書く必要はありません。INI形式で書くことのできるシナリオには、対応するTeracottaAPI名を書くだけで操作の自動化をすることができます。

* **CSV形式で書くインベントリ**
どの機器に入って操作するかはインベントリにCSV形式で記述します。インベントリには、ログインするために必要なIPアドレスやログイン名/パスワードだけでなく、各機器に固有の設定などを記述することにより確認作業や設定作業を自動化することもできます。

* **抽象化を目指して設計しているAPI群**
シナリオを通して呼び出すAPIは、機器によって操作方法が異なることを想定した抽象化ができるように設計してあります。そのためため、ベンダー毎にいちいちコマンドを覚え直す必要はありません。どんな機器であっても、ログインするときはログインと指定してあげるだけで済みます。

* **Teratermマクロに不足していた関数を補完**
Teratermマクロには存在しないリッチなマクロ関数を用意しています。例えばIPv4を判定するための機能やプロンプトを取得するための機能など。かゆいところに手が届きます。

* **マクロ作成で陥りがちな不具合への対応**
Teratermマクロを動かす際に陥りがちな不具合を解決しています。表示に時間のかかるコマンドを打ったら最後まで表示されずに画面が閉じてしまった！ みたいな経験はありませんか？ `waitほにゃらら` なteratermマクロの内部処理の違いがわかれば解決できることは多いです。

* **独自APIを拡張**
Teracottaはマクロでできるいろいろなことをリッチにはしていますが、それでも出来ないことは多いです。対応機器も今のところほとんどありません。なので足りない部分は独自で用意できるような仕組みにしてあります。ユーザAPIを自作することにより、今まで書いていたTeratermマクロと同程度の労力で (あるいはログイン処理などを意識しない分より省エネルギーに) 新たなマクロを書くことが出来ます。

## 動作環境

- Teraterm (v4.78以上)
- Teratermマクロが動作するWindows機
  ※Wine等エミュレーション環境での動作確認はしていません

## 使い方

1. config.iniを必要に合わせて編集します。
2. シナリオを作成し `scenario` フォルダ内に `scenario.ini` として保存します。(UTF-8)
   書き方は `scenario\scenario-example.ini` を参照してください。
3. インベントリを作成し、 `teracotta.ttl` と同じフォルダに `hosts.csv` として保存します。(UTF-8)
   書き方は `hosts-example.csv` を参照してください。
4. `teracotta.ttl` を実行

## コマンドプロンプトからの実行について

コマンドプロンプトから `teracotta.ttl` を呼び出す場合、引数からシナリオとインベントリを指定することが出来ます

```
ttpmacro.exe teracotta.ttl [INVENTORY] [SCENARIO]
```

`INVENTORY` : インベントリ保存パス (相対または絶対パス)
デフォルトは `hosts.csv`

`SCENARIO` : シナリオ名 (.ttl 抜きのファイル名のみ記述)
デフォルトは `scenario`

## ユーザAPIを作成する

作業内容に合わせてユーザ独自でAPIを作成することが出来ます(もっと別の呼び方のほうがいいような気がするが…)

シナリオから呼び出されるAPIは、`user` フォルダ内または `user-api` フォルダに格納します。
これらのフォルダはAPIの読み込み順序が異なります。

1. `user-api\[type]\[scenario].ttl`
2. `user-api\[scenario].ttl`
3. `api\[type]\[scenario].ttl`
4. `api\[scenario].ttl`

`type` はインベントリ内で指定してください。

該当するAPIが見つかったタイミングでそのマクロを読み込み、以降の検索は行いません。
また、 `user-api` はgit管理対象外となっています。

APIは `lib` フォルダ内に格納されている様々なマクロを利用することが出来ます。
libフォルダに格納されているマクロの使い方については、現在ドキュメントの用意はしていませんが、コメントにて使い方を記載しているので、ご確認ください。

## 不具合、要望など

うまく動かない、機能が不足している等、何かあったら、Issueに書き込むなどしてください。

自力でAPIを作れる人がいたら、ぜひとも開発に参加してください。