# ExKeyMoサーバーの構築方法（Mac版）
参考サイト：[【Mac向け完全版】Androidキーボード改造の第一歩！ExKeyMoサーバーを自前で立てる環境構築ガイド](https://note.com/xiaoxiongmao/n/neaec943587d2)

## 0. 必要なツール・データ
環境構築に必要なツール・データは以下の4つです。初回に設定が完了したら、次回以降の操作は不要になります。
- Homebrew: macOS上で動作するパッケージ管理ツール
- Git: プログラムをダウンロードするためのツール
- Java 17: ExKeyMoを動かすためのプログラミング言語パック
- ExKeyMoサーバー: ローカル環境で作業できるようにダウンロードして使用する

### Gitの導入
ターミナルを開き、Gitを入手します。
> brew install git

### Javaの導入
Java 17のインストールし、MacがJavaを認識できるようにします。
（システムにJavaを登録するためにパスワードの入力が求められるため入力します）
> brew install openjdk@17
> 
> sudo ln -sfn $(brew --prefix)/opt/openjdk@17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-17.jdk

設定ファイルに追記して反映させます。
> echo 'export PATH="/opt/homebrew/opt/openjdk@17/bin:$PATH"' >> ~/.zshrc source ~/.zshrc

バージョンの確認のため、以下を入力してopenjdk version 17.〜と表示されていることを確認します。
> java -version

### ExKeyMoのサーバープログラムの導入
ExKeyMoサーバーをローカル環境にダウンロードする。
> cd ~/Documents
> 
> git clone https://github.com/ris58h/exkeymo-server.git

（[公式サイト](https://github.com/ris58h/exkeymo-server.git)からzipファイルでダウンロードしてもOK）

## 1. ローカルサーバーの起動
上記設定が完了したら、ExKeyMoをローカルサーバーとして起動させ、自分オリジナルのキーレイアウトを設定できます。

> cd exkeymo-server
>
> ./mvnw clean install

ローカルサーバーのポートは指定することも可能です。
指定しない場合
> java -jar target/exkeymo-server-*-jar-with-dependencies.jar

サーバー配置先：http://localhost/

指定のポート番号に構築する場合（例ではポート番号：8080）
> java -Dserver.port=8080 -jar target/exkeymo-server-*-jar-with-dependencies.jar

サーバー配置先：http://localhost:8080

## 2. Simple/Complex UI
Simple UI: Key Layoutファイル（KL）をプルダウン項目から選択し、割り当てたいキーを指定して、ハードウェアのキー入力（スキャンコード）を、Android 内部の「キーコード（Key Code）」に変換します。

Complex UI: Key Character Mapファイル（KCM）ファイルの内容を入力し、Android 内部の「キーコード」を、画面に出力される「文字（Character）」や「機能（Function）」に変換します。

## 3. Complex UI での注意事項
- 物理キーボードを押下した時に何が入力されている確認するため、スキャンコードを調べる必要があります。

- 基本書式は以下の通り
  - 物理キーのリマッピング

  map key [スキャンコード] [Androidキーコード]

  - キーの動作定義

  key [Androidキーコード] {

  label:        [...]
      
  base:         [...]
      
  ctrl:     [...]
      
  }

- Android キーに文字コード以外のキー（修飾キー）を入れた場合、ctrl キーなどの組み合わせで修飾キーを設定することはできません。
- 
※ Clicks キーを　左カーソルに割り当て、ctrl キーを押している間だけ上カーソルにしようと思っても機能しませんので注意が必要です。

## 4. ローカルサーバーの終了
ターミナルで Ctrl + C を入力し、ローカルサーバーを止め、終了するときは以下を入力する。
> exit


