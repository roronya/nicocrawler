#nicocrawler

##what?

設定ファイルに書かれたニコニコ動画のマイリストを監視して、動画が追加されると自動でmp3かm4aでダウンロードしてきます。

cronに登録すれば自動でマイリストの更新を確認して音楽ファイルを生成してくれます。

サーバーでなくクライアントなPCでも手動で実行しても便利だと思います。

他の人のマイリスト以外にも自分のマイリストを公開マイリストとして、マイリストIDを登録すれば、音楽ファイルが欲しい動画をまとめてダウンロード出来ます。

動画ファイルに含まれたmp3かm4aを抽出するだけなので音質の劣化はありません。しかしながら、一般会員だと混雑時はエコノミーになるため、音質の劣化した音楽ファイルが生成されます。使用にはプレミアムアカウントでの利用をおすすめします。

##Download & Install

    $ git clone https://github.com/roronya/nicocrawler
	$ sudo mv nicocrawler /usr/local/bin
	$ cd /usr/local/bin/nicocrawler
	$ chmod 755 nicocralwer nicocrawler.cron

/usr/local/bin以外のディレクトリだと動作しません。すみません。

##Update

    $ git pull

##Uninstall

nicocrawlerディレクトリを削除してください。

##Usage

**javaとffmpegとswftoolsとsqlite3を内部で使っています。インストールしておいてください。**

設定ファイルにアカウント情報と監視したいマイリストのIDを書く必要があります。

設定ファイルはnicocrawler/conf/nicocrawler.confです。

初期状態は以下のようになっています。

    {
	:mail "your mailaddless"
	:pass "your password"
	:path "save directory"
    :mylists [
	          {:mylist-id ""
			   :artist ""
			   :album ""}
			   ]
    }

:mailと:passにはニコニコ動画に登録したメールアドレスとパスワードを書いてください。

:pathは動画の保存先です。例えば/home/user/Musicに保存したい場合は以下のように記述します。

    :path "/home/user/Music/"

:mylistが監視するマイリストの情報を書く場所です。たとえば[sasakure.UK氏のマイリスト](http://www.nicovideo.jp/mylist/6280579)を監視したい場合は以下のように書きます。

    :mylists [
              {:mylist-id "6280579"
			   :artist "sasakure.UK"
			   :album "niconico"}
			   ]
			   
このように書くとマイリストの全曲が/home/user/Music/sasakure.UK/niconico/以下に保存されます。

複数のマイリストを監視したい場合は以下のように書きます。

    :mylists [
              {:mylist-id "6280579"
			   :artist "sasakure.UK"
			   :album "niconico"}
			  {:mylist-id "13954828"
			   :artist "buzzG"}
			  ]

:albumを指定しないとartist名直下に保存されます。すなわち/home/user/Music/buzzG/以下に保存されます。

設定をしたら実行します。

    ./nicocrawler

cron用のシェルスクリプト(nicocrawler.cron)も用意しました。cronに登録するときはこっちを使ってください。

nicocrawler.cronはcronに登録した時間までに、前回のcron分の処理が終わってない場合は起動しないようになっています。

