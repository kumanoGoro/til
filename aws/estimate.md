# AWSの料金を試算する

## AWS 課金体系と見積り方法について

https://aws.amazon.com/jp/how-to-understand-pricing/

#### AWSの課金体系
従量課金
|1|サーバ(Amazon EC2)|約2円〜 / 1時間|
|2|ストレージ(Amazon S3、Amazon EBS)|約10円 / 1GB / 1ヶ月|
|3|データ転送（上りは無料）|約20円 / 1GB|

##### 1.サーバ課金
- 1時間単位
→サーバを止めている間は課金されない。
- リザーブドインスタンス
→予約金を払うことで最大約70％安くなる。
　　[AWSマイスターシリーズ](http://www.slideshare.net/AmazonWebServicesJapan/aws-16524731)
- ライセンス課金
→有償OSのライセンス料金は1時間当たりの単価に含まれる。

##### 2.ストレージ課金
ストレージサービスは以下3種類
- Amazon EBS
- Amazon S3
- Amazon Glacier

##### 3.データ転送
1. 実際に使った分だけの課金
1. 上りは無料
1. ボリュームディスカウントあり

初期投資に目がいきがちだがTCOで考える。
https://www.slideshare.net/kentamagawa/tco-14340860
以下のようなものは最初から含まれている。(通常はランニングコストとしてかかる)
- サーバ機器
- NW機器
- 機器保守費用
- 電源費用
- 空調費用
- データセンターのスペース費用
- 運用人件費

### EC2の課金体系
1. Amazon EC2インスタンスの利用時間 x 時間単価
1. Amazon EBS（仮想ハードディスク）の確保量
1. Amazon EC2とAmazon EBS間のInput/Output回数
1. データ転送量（Internetへ出ていく分のみ）

 オプションで使えるもの
 - Elastic Load Balancing
 - Snapshot
 --以下無料
 - Amazon EC2の仮想Firewall機能である「Security Group」
 - 仮想プライベートクラウド環境を構築する「Amazon VPC」(VPN接続は有料）
 - AWSリソースの監視サービス「Amazon CloudWatch」
 - Amazon EC2のスケールアウト、インを?動化する「Auto scaling」
 - 付け外しが可能なグローバルIPアドレス「Elastic IP」（実行中のインスタンス毎に1つまで無料）

### S3の課金体系
1. ストレージ料金
1. リクエスト料金
1. データ転送量（Internetへ出ていく分のみ）

