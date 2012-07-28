# 『30日でできる！ OS自作入門』 for MacOSX

『30日でできる！ OS自作入門』川合 秀実氏(著)の開発環境をMacでも扱えるようにします。ここの通りにすればWindowsとほぼ同等の環境を用意できるので、Macでも本書の内容に沿って学習を進めていくことが可能です。

## 動作環境とエディタ

以下の環境で正常に動作しているか確認しています。

- OS
	- Mac OS X Lion 10.7.4
- テキストエディタ
	- Emacs
	- 書籍で紹介されているTeraPadはMacには無いので自分の慣れたやつ使ってください
- バイナリエディタ
	- 0xED
	- [http://www.suavetech.com/0xed/0xed.html](http://www.suavetech.com/0xed/0xed.html)
		- Mac版の使いやすかったバイナリエディタです


# 1. 必要なものをインストール&用意

パパっといろいろ必要なものを揃えます。

1. `HariboteOS`という名前のディレクトリをホームディレクトリに作成
 	- ホームディレクトリはFinderのメニューバーから「移動＞ホーム」で表示された場所
2. 『30日でできる！ OS自作入門』付属のCD-ROMデータのうち、`projectsディレクトリ`と`tolsetディレクトリ`をさっき作成した`HariboteOSディレクトリ`にコピー
3. tolsetOSXをダウンロード
 	1. ダウンロードページ→[http://shrimp.marokun.net/osakkie/wiki/tolsetOSX/](http://shrimp.marokun.net/osakkie/wiki/tolsetOSX/)
 	2. tolsetOSX-070221.dmgという名前のファイルをダウンロードして解凍
 	3. 解凍したデータのうち`z_toolsディレクトリ`を`HariboteOSディレクトリ`にコピー
4. Q.appをダウンロード
	1. ダウンロードページ→[http://www.kju-app.org/](http://www.kju-app.org/)
	2. 「Download - Free」をクリックしてダウンロードして解凍
		- (現時点でバージョンはQ-0.9.0a89)
	3. 解凍したデータのうち、`Q.app`を`HariboteOSディレクトリ`にコピー
		- クセでApplicationsに入れたりしないようにね

## HariboteOSのディレクトリ構造

最終的に下記のような構造になっていれば問題無いです。

- ホームディレクトリ
	- HariboteOS
		- projects(付属CD-ROMのやつ)
		- tolset(付属CD-ROMのやつ)
		- z_tools(tolsetOSXのやつ)
		- Q.app


# 2. シェルスクリプト実行

細かい部分の調整と私が書いたMac用のMakefileというファイルをダウンロードします。これをすることでCD-ROMのサンプルを簡単に実行することができます。

めんどくさいのでこれらの設定はシェルスクリプトを使います。シェルスクリプトとは何かというと、えーと、まあ便利なやつですｗ。知らない人は調べてみてください。

それでシェルスクリプトの実行ですがターミナルを開いてください。ターミナルを知らない人は「アプリケーション＞ユーティリティ＞ターミナル」で見つかると思います。そして次の通りに実行してください。

>$ curl https://raw.github.com/sandai/30nichideosjisaku/master/bin/install.sh | sh

curl〜からの部分をコピーして、ターミナルに貼り付けて実行するだけです。$を入れないようにしてください。

# 3. 確認

うまくいっているかどうか確認してみてください。

>$ cd ~/HariboteOS/01_day/helloos0  
>$ make run

実行して以下のように表示されていれば成功です。

![image](https://github.com/sandai/30nichideosjisaku/raw/master/bin/img/qemu.png)

# 4. ちんぷんかんぷんな人のための最初だけ講座

どうやってサンプルを実行すればいいかわからない！って人のための簡易講座。細かい説明はしないので駆け足で実践的に進めます。

## 1日目
### 1 とにかくやるのだぁ…のとこ

helloos.imgをバイナリで書けという鬼畜命令が下るところですね。わからなかったらやらなくてもいいんですが、やるんだったら0xEDエディタを使って書くと良いです。

![image](https://github.com/sandai/30nichideosjisaku/raw/master/bin/img/0xed.png)

このように書きます。

書籍通りの表示にするにはウィンドウの幅を調節して右側を、

	0000000 EB 4E . . . . . .
	0000010 02 E0 . . . . . . 
	.
	.
	.
	
みたいな桁になるようにするとできます。こうして最終的にできたのが`01_day/helloos/helloos.img`になります。

自分の書いたやつを使いたいのであれば、`01_day/helloos`にある`helloos.img`を上書きしてください。(不安なら上書き前の`helloos.img`をデスクトップとかに保管するとかして)

そうしてターミナルを開いて、

>$ cd ~/HariboteOS/01_day/helloos0  
>$ make run

とすれば書籍の「最初のOS？」の画像の通りに表示されます。

### 3 アセンブラ初体験…のとこ

最初に18万行を超えるやつ書けとか言われます。著者には悪いですがスルーしましょうｗ。めんどくさいだけですからね。

次にまともな長さのソースプログラムってのがありますが、これは練習がてら書いてもいいかもしれません。`helloos.nas`というファイル名で書いてみてください。

最終的にできたのが`01_day/helloos1`にある`helloos.nas`になります。自分で書いたやつを試したいのであれば上書き保存してください。(例によって不安なら上書きする前にサンプルのやつは保管しておきましょう)

あとはターミナルからmakeします。

>$ cd ~/HariboteOS/01_day/helloos1  
>$ make run

たぶん最初と同じような画面が表示されるはずです。

### 4 もうちょっと書きなおしてみる…のとこ

ここも自分で書いたら`01_day/helloos2/`にある`helloos.nas`を上書き保存して実行してください。これまたmakeします。

>$ cd ~/HariboteOS/01_day/helloos2  
>$ make run

実は今まで書いたコードは全部同じ意味のコードです。だからQEMUの表示内容も同じです。

### ここまででわからないところがあるとしたら

たぶん`cd ~/HariboteOS/01_day/helloos0`や`make run`の意味がわからないかもしれません。詳しくは検索してください。「cd コマンド」や「make コマンド」などでヒットすると思います。

`cd`はともかく`make`は難しいですね。少しだけ補足しておきますと、`01_day/helloos0`や`01_day/helloos1`や`01_day/helloos2`の中身を見てください。「Makefile」というファイルがありますよね。

`make`というのはこの「Makefile」を実行するためのものです。ちょっとのぞいてみましょう。`01_day/helloos2`のMakefileは以下の通りです。

	run :
		../../z_tools/nask helloos.nas helloos.img
		cp helloos.img ../../z_tools/qemu/fdimage0.bin
		make -C ../../z_tools/qemu
		
これは「`make run`と入力されたらrun : 以下の指定されたことをやりなさいよ」という指示になってます。指定されたことというのは`../../z_tools/nask helloos.nas helloos.img`みたいなやつです。

これから先(27日目のharib24gあたりまで)はこの`make run`でサンプルのOSを実行できるようになってるので、よくわからなくても`make run`していけばサンプルは実行できます。そのように全てMac用に書き直しているからです。

なので困ることはないんですが、本書ではMakefileの整理が流れの中で何度が行われいます。そのときにどうなってんのかなーぐらいは把握しておくと良いと思います。

## 2日目

せっかくなので2日目もやってみましょう。……ってやることあんまりなかった。

### 1 さて開発再開…のとこ

`02_day/helloos3`にある`helloos.nas`のソースコードの解説です。一応`make run`できるようにしてますが、今までと同じ内容が起動されるだけなのでやらなくてもいいと思います。

### 3 ブートセクタだけを作るように整理…のとこ

`02_day/helloos4`をのぞいてみてください。今までは`helloos.nas`だったのが`ipl.nas`になりました。それにあわせてMakefileも少しだけ変化があります。どのように変わったかについては本書に書いてある通りです。

### 4 今後のためにMakefile導入…のとこ

本書ではここから本格的にMakefileが導入されます。どのようなコマンドが使えるのか本書を読みつつ、`02_day/helloos5`にある`Makefile`をのぞいてみてください。`make run`以外にもいくつかありますよ。

最後に`make`をやってみます。

>$ cd ~/HariboteOS/02_day/helloos5  
>$ make run  
>$ make src_only

こんな感じですね。`make src_only`が何なのかは本書を読んでください。

### 簡易講座はここまでです

分からない人にとっては言葉足らずの内容だったかもしれません。しかし、このあたりのことは知識として持っていないとこれから先厳しいと思います。知識が足りないなと思った人はがんばってください。

とりあえずこんな感じでサンプルを試しつつ学習してくことになります。

あと、自分でコードを書きながら学習していこうと考えている人はMakefileが何をやっているのか分からないと難しいです。Makefileがわからないのであれば、現状ではサンプルのコードを読みながらサンプルを実行していく学習スタイルが良いと思います。

# 5. 以上

電話帳並みに分厚い本なのでいろいろ苦労することが多いと思いますが、ぜひともがんばってやってみてください。応援しています。ではでは。

# 6. URL

- [「30日でできる！ OS自作入門」のサポートページ](http://hrb.osask.jp/)
	- 公式サポートサイト
- [tolsetOSX](http://shrimp.marokun.net/osakkie/wiki/tolsetOSX/)
	- Mac版のtolset配布ページ
- [Q - [kju:]](http://www.kju-app.org/)
	- Mac版のQEMU配布サイト
- [0xED](http://www.suavetech.com/0xed/0xed.html)
	- バイナリエディタ配布サイト
	
# 7. 連絡先

なんかうまく動かないぞーってところがあれば連絡ください。何日も待たせるようなことはないと思います。

- [https://twitter.com/sandai](https://twitter.com/sandai)
- <sandai310@gmail.com>