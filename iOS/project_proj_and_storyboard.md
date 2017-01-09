# 概要
行き当たりバッタリでプロジェクト進行してきたので、  
問題が発生してきた。  

一旦、モック作成を0に戻し、手順をまとめてから進行していく。  
発生した問題は主に**git**でのconflict問題。

今後の、このまま進行しても  
いつか必ず大きな問題に発展すると思われるため、  
プロジェクト初期段階である、今、手を打つ。

## 今回発生した問題の詳細
* リモートリポジトリにcommitしようとした際にconflict発生。
* conflictの解消が容易ではない。
* conflict発生時にgitでの解消方法が分からない。
* conflictを起こしたファイルは今後もcommitするごとにconflictを起こすことが予測される。
* ディレクトリ構成、コーディング時の決まり事がなさすぎるため、このままではいつか破綻する。

# 各問題への対応策
## project.pbxprojファイルのconflictを極力無くす
###まず、project.pbxprojファイルとは何か？
プロジェクト全体を管理しているファイル。

* プロジェクトの設定を変更した時
* プロジェクトにファイルを追加した時

上記の場合、ファイルが書き換えられる。  
よって、

* クラスを追加した時
* 画像ファイルを追加した時

などで書き換わるため、複数人の開発ではほぼ確実にconfictすることが予想される。

根本的な解決はできないため、以下の対策を行い、極力conflictが発生しない運用方法とする。

### gitの運用方法
1. リモートリポジトリは常にビルド(リリース)できる状態を保持する。

	プロジェクトの破壊を防ぐため  
※リリースブランチは**master**とする。

2. 朝(9:30)と夕方(17:30)に、masterブランチをpullする。

	masterを最新の状態に保ち、merge、pushの際の不要なconflictを防ぐため

3. 施策ごとにbranchを切る

	細目にmasterにpushするため

4. ファイルの追加が事前にわかっている場合は、即masterに反映する。

	project.pbxprojファイルのconflictを防ぐため。

5. branch内ではこまめにcommitする。

	バックアップをこまめに取るため。

6. masterにmergeする際は、変更のあった全てのファイルの差分を確認する。

	予期しない変更をpushすることを防ぐ。

## さらにmergepbxを使用する
参考サイト
[mergepbxを使ってみる](http://qiita.com/kiarina/items/5f7a8d04e84a20cdad87)

## その他不要なファイルをcommitして無駄にconflictしないように設定する
###.gitignore設定
プロジェクトフォルダ直下に**.gitignore**ファイルを作成し、以下を記述。
```
*.xcodeproj/*
!*.xcodeproj/project.pbxproj
!*.xcworkspace/contents.xcworkspacedata
.DS_Store
```
* .xcodeproj

	Xcode上でどのファイルが開かれているかなどの  
	管理しなくてもいい情報が含まれていたりするので、  
	ひとまず除外

* project.pbxproj

	プロジェクトの構成情報を格納するファイル

* xcworkspacedata

	プロジェクトのワークスペースのデータを格納するXMLファイル

* DS_Store

	Mac OSが自動で生成するファイル情報格納用ファイル

