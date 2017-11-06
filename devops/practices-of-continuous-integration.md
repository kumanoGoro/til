# CI
[toc]

## CIとは
継続的インテグレーション(Continuous Integration)
コンパイル・テストを繰り返すことで品質を向上させる取り組み
- 不具合・デグレを早期発見できる
→影響範囲、原因特定が容易になり、手戻り工数も削減できる

→何度も繰り返すことが重要なのでテスト自動化など自動化技術が必須
　テストを自動化し、集計・可視化することで品質が分析できるようになる。

## CIを実現するツール
1. CIツール
 - ビルドの設定
 - ビルドの自動実行
 - 実行結果の通知
 - 実行結果の表示
 - バージョン管理
 - テスト自動実行

 代表的なCIツール　：　Jenkins
1. ビルドツール


単体テスト：JUnit
カバレッジ測定：Jacoco
静的解析：CheckStyle、FindBugs
GUIテスト：Selenium





## 参考
[Continuous Integration](https://martinfowler.com/articles/continuousIntegration.html)
[Webアプリケーションにおける回帰テストの自動化](http://jasst.jp/symposium/jasst12tokyo/pdf/B2-3.pdf)
[自動化・システム化](http://www.exmotion.co.jp/solution/legacy-4.html)
[継続的インテグレーション（CI）によるテスト自動実行](http://itpro.nikkeibp.co.jp/article/COLUMN/20121023/431822/?rt=nocnt)

[JenkinsとGitHubを連携する](http://blog.duck8823.com/entry/2016/04/24/022923)