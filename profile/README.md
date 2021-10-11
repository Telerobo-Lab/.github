## Git利用時のルールについて
研究室でのソフトウェア開発において混乱を防ぐためにgit-flowをベースに管理を行いましょう！

### 参考ページ
[A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)

[トピックブランチと統合ブランチでの運用例](https://backlog.com/ja/git-tutorial/stepup/05/)

### ブランチ定義
- master

  リリース用のブランチ．リリースしたらタグ付けを行う．タグ付けは**vX.Y.Z**のように付ける．
  - X: **メジャーバージョン** 見た目や操作性に影響を及ぼすような大きな変更
  - Y: **マイナーバージョン** 細かな機能向上や部分的な追加
  - Z: **パッチバージョン** バグ修正など
  
  ※このブランチ上での作業は行わない
  
- develop
 
  開発用ブランチ．コードが安定し，リリース準備が出来たらreleaseブランチへマージする．
  
  ※このブランチ上での作業は行わない
  
- feature
  
  機能の追加用ブランチ．developから分岐し，developにマージする．
  
- hotfixes
  
  リリース後の緊急修正用のブランチ．masterから分岐し，masterにマージすると共にdevelopにマージする．
  
- release

  リリース準備用のブランチ．リリース予定の機能やバグ修正が反映されたdevelopから分岐する．リリースの準備が整ったらmasterにマージすると共にdevelopにマージする．
  
メリットとして...
  1. 本番リリースのリポジトリと開発中・修正中のリポジトリを明確に区分することで紛れ込みを防ぐ．
  1. 開発・修正毎にリポジトリを作成でき，リリースタイミングにかかわらず並行して作業ができる．
  1. 履歴によりリリース内容を後から追跡が可能になる．
  
### 開発の流れ
(1) developブランチからfeatureブランチに分岐
```
$ git checkout develop
$ git checkout -b feature/XXXXX
```
(2) 開発した機能をdevelopブランチに取り込む
```
$ git checkout develop
$ git merge --no-ff feature/XXXXX
$ git branch -d feature/XXXXX
$ git push origin develop
```
(3) リリースを行う
```
$ git checkout master
$ git merge --no-ff release/vX.Y.Z
$ git tag -a vX.Y.Z -m 'version X.Y.Z'
$ git push origin vX.Y.Z
```

## まとめ
チームで１つのシステムを開発することはとても難しいことです．熟練者の集まりではないので，ルールを決めることは非常に役に立つことだと思います．
また，システム開発を行う上でたくさん問題が発生すると思います．しかしマルチタスクは効率を落とします．
「なにが問題なのか」「どんな機能を追加するのか」などをしっかり決定して，１つずつ解決していきましょう．
git-flowがその助けになれば幸いです．
