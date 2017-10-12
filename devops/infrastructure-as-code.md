# Infrastructure as Code とは
インフラ構成を人が手作業で作るマニュアルで管理するのではなく、宣言的なコードでインフラ構成を管理する。

## Infrastructure as Code のプラクティス
- バージョン管理(Git)
- Pull Request(レビュー)
- TDI(テスト駆動インフラストラクチャ)
- CI(継続的インテグレーション)
- CD(継続的デプロイ)

#### ブートストラッピング
- Docker

#### コンフィグレーション
- Ansible
- Puppet
- Chef

||Ansible|Puppet|Chef|
|開発言語|Python|Ruby|Ruby|
|開発開始|2012年|2005年|2009年|
|書式|YAML|独自DSL|Ruby DSL|
|冪等性|○|○|○|
|構成管理方法|Push型|Pull型|Pull型|
|エージェント|不要|必要|必要|

## コード化によるメリット
- オペレーション品質の向上
- システム運用の標準化
- 再利用性の向上
- 作業工数削減
- セキュリティの向上
