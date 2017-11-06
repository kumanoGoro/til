# DevOps の技術要素
## 構成管理
主流はGit。SVNと違い分散型のリポジトリ
GitHubやBitBucketなどのホスティングサービスがある。
公開したくないものを自前で管理する場合
→[GitLab](https://about.gitlab.com/)、[GitBucket](https://github.com/gitbucket/gitbucket)

## 変更・障害管理
チケットで管理。
対象のソース・ドキュメントをRedmineで紐付け。
受入時にdiffまで見えるツールを作ることもある。

CI/CD の目的
通常はリードタイム短縮・回帰テスト自動化
CD までするかどうかは deploy を慎重にしたいかどうか。

変更時の再テスト範囲
受け入れテスト
→計画とチケット・設計情報との依存関係明確に

hotdeploy
Jenkinsから決まったポートめがけてdeploy要求
Webブラウザのコンソールでdeployするようなイメージ

納品管理、影響範囲の見える化
→明朗会計

CI/CDのターゲットはどこ？
UT/IT/E2E


