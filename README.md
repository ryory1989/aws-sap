# キーワードメモ

## S3

### Inteligent-Tiering

アクセス頻度に応じて自動でデータを階層化し、最初の48時間の高トラフィックはStandard tierが処理するため、パフォーマンスを確保できる
アクセスが減った際はArchive Instant Access tierへ無停止で移行する

### Glacier

リストアには数分～数時間かかる

### Block Public Access

バケットレベルで設定できる。

- BlockPublicAcls：新たに付与されるPublicACLを拒否
- IgnorePublicAcls：すでに付与されているPublicACLを拒否

Pre-Signed URLは署名付きリクエストとして、IAM認証情報を含むので、パブリックACLを必要としない認可済みアクセスとなる。

### Replication Time Control（RTC）

最大15分以内の配信をSLAとしている。
追加料金が必要。

### S3 Batch Replication

大量の既存オブジェクトを一括で複製する。
ジョブは15分～数時間単位で開始される。

## System Manager Session Manager

HTTPS通信を行う
インターフェイスVPCエンドポイントを用意すればEC2からのAWS APO呼び出しはパブリックインターネットを経由しない
StartPortForwardingSessionでローカル8080→リモート3306のトンネリングをサポートしている

※対話シェル、ポートフォワーディング、ログ出六、KMS連携

## Elastic Disaster Recovery(DRS)

vSphereを含むオンプレのk相馬伸にエージェントをインストールするだけで、秒レベルの継続的ブロックリレーションを実現できる。
待機期間中は安価なスタンバイスナップショットと最小限のリザーブドリソースのみを使用
災害宣言時にワンクリック（or API）でEC2インスタンスとEBSボリュームが自動生成される
リバースレプリケーション機能を使い、自動同期し、短時間でオンプレに戻すことも可能

## AWS DataSync

ファイルレベル転送サービス
そのため、ディスクフォーマットが変わる可能性がある
NTFSアクセス権・所有者情報・ドメインSIDなどを維持したまま拘束に転送できるので、ACLやドメイン設定を変えずに移行可能。
TLS1.2でインフライト暗号化通信を行う。
スケジュールベースで差分転送を自動化可能。

※ファイルレプリケートとブロックレプリケートの違い

## Amazon AppStream2.0フリート

画面転送プロトコルを用いてユーザーとの往復を数十msに短縮できる

## Amazon RedShift

分析（OLAP）向け

## Amazon WorkSpaces

マネージドなクラウド型の仮想デスクトップサービス
多様なデバイスからインターネット経由でアクセスできる

## Bastion

Bastionを使うとSSH接続が残る

## S3 Transfer Acceleration

CloudFrontのエッジロケーションを経由してTCP転送を最適化し、長距離回線のレイテンシーやパケットロスの影響を大幅低減する。
クライアントが最寄りのリージョンまでしかアップロードしない。

## Amazon SQS Standard

バースト受信の場合でも、キューが無制限でバッファし、少なくとも1回の配信を行う

## Amazon Kinesis

### Amazon Kinesis Data Streams

高スループットのバッファリングに適している。

#### プロビジョニングモード

シャード数の設定とスケール調整は手動

### Amazon Kinesis Data Firehose

受信スループットに応じて内部で自動的にパーティションとバッファリングを調整する
→　シャードやキャパシティを意識した運用が不要
取り込んだデータはほぼリアるタイムでターゲットサービスに配信

## Amazon OpenSearch Service

班構造体データを動的スキーマで格納し、高速検索を提供する。
Firehoseからのストリーミング取り込みもネイティブでサポート

### OpenSearch Dashboard

サービスに格納されたデータをリアルタイムに可視化する。

## Amazon Redshift Spectrum

データレイク上のファイルをバッチ的にスキャンして分析する。

### StackSets

複数アカウント、リージョンへ一括デプロイする運用者向け機能。

## Amazon EMR

フルマネージドのHadoop/Sparkプラットフォーム

### EMRFS

mazon EMR クラスターが Amazon EMR と Amazon S3 の間で通常のファイルを直接読み書きするために使用する HDFS の実装
HDFSのパスをほぼそのままS3にマッピングできる

## Amazon Organization

### タグポリシー

「許可値の評価のみ」だと準拠状況を判定し、リソースの作成・更新はブロックされない。
準拠違反が発生すると、`Tag Policy Compliance`イベントがEventBridgeに出力される。

## Amazon FSx for Windows File Server

作成後はデプロイタイプの変更は不可。
Multi-AZデプロイタイプの場合、セカンダリAZに自動複製され、障害時には秒単位でフェイルオーバー可能。

## Load Balancer

### NLB

任意のTCPポートを利用可能
リージョン内の複数AZにまたがって配置でき、ヘルスチェックに基づき、自動でフェイルオーバーする。

### ALB

HTTP/HTTPS専用

## Amazon Aurora

Aurora Global Databaseは専用の高速ネットワークを用いたリージョン間レプリケーションを行い、通常1秒未満のレイテンシーでデータを複製する。（RPO<=1s、RTO1分未満）

### Aurora MySQL

標準ではマルチライター機能は提供されていない。

## RDS

### Amazon RDS for MySQL

クロスリージョンリードレプリカは非同期レプリケーションなので、レプリケーションラグが数十秒～数分になるケースもある。
定期スナップショットは通常、5分～24時間間隔で取得される。
スナップショットからのリストアは数十分～数時間を要する。

### Amazon RDS Proxy

データベースの前段で接続プールを維持し、同時実行が急増した場合でも新規TCP/SSLハンドシェイクを削減する。

### RDS Enhanced Monitoring

最短1秒間隔でOSメトリクスをCloudWatchに送信できる。

### RDS Performance Insights

DBLoad等のデータベース内部指標をリアルタイムに可視化できる。

## Amazon API Gateway

Edge-Optimizedエンドポイントに切り替えると、自動で前段にCloudFrontが配置される。

## CodeDeploy

### Blue/Greenモード

旧環境と新環境を並列稼働し、ALBのルーティングを自動で切り替える

## Amazon Config

状態変化（コンプライアンス違反）の評価を行うサービス

## Lambda

実行環境はリクエスト後でも一定時間アイドル保持され、次回呼び出し時に同じコンテナが再利用される。

### Reserved Concurrency

同時実行数の確保と上限を同時に設定する仕組み

## AWS Migration Hub

複数の移行ツールを横断し、単一のダッシュボードでアプリケーション単位の可視化を実現する。

Agentless Discovery Connector
AWS Systems ManagrInventory

## Application Discovery Service(ADS)

エージェントをインストールすることで、OS種別、CPU、メモリなど、移行計画に必要な詳細メタデータを1秒単位で収集する。
取得したデータはMigration Hubに統合され、TCO資産やR7分析レポートを自動的に生成できる。

## Application Migration Service

既存サーバに小さなエージェントを入れるだけでブロックレベルの継続レプリケーションを行えるフルマネージドサービス。
レプリケーション対象ディスクやネットワーク設定を確認できる。
レプリケーション後の移行カットオーバーに最適化されたサービス。
マイグレーションハブはダッシュボード。

## Server Migration Service(SMS)

オンプレの仮想マシンをAWSにレプリケーションし、最終的に一括カットオーバーを行うための移行実行ツール。
SMSコネクタはVMイメージのレプリケーションが主な目的で、取得できるメトリクスはレプリケーションに必要な最低限の情報のみ。

## Database Migration Service(DMS)

最小限のダウンタイムでデータベースを安全に移行するサービス。
自動生成するのはテーブルと主キーで、トリガーやビューなどの複雑なオブジェクトはサポート対象外。
「フルロード＋CDC」タスクを実行すると初回ロードで全データを移し、その後の変更を継続的に取り込む。
フルロードのみだと増分レプリケーションは行われない。
CDCはいつから変更を取り込むかという開始点が必要で、初回データがターゲット側に必要。
プライマリキーやインデックス、外部キー制約を`遅延作成`に設定すると、

## AWS Cloud Adoptiin Readiness Tool

クラウド導入の成熟度を組織・人材・セキュリティ・運用など6つの視点で定量評価し、ギャップ分析レポートを提供する。

## AWS X-Ray

マイクロサービスや分散トレースの可観測性を高めるためのサービスで、アプリケーションコード内のリクエストフローやレイテンシ分析を行う。
稼働後の性能分析ツール。

## AWS Total Cost of Ownership(TCO) Calculator

オンプレのサーバースペックや運用コストを手入力し、クラウド利用時のTCO削減効果を概算するwebツール。
実際のパフォーマンスデータ収集や依存関係の自動把握はできない。

## EC2

### EC2 Image Builder

AMIの作成、テスト、保守の一連のパイプラインをフルマネージドで提供。
ここで作成したAMIをService Catalogに「登録すれば権限付与だけで組織内のアカウントが参照可能

### Auto Scaling

インスタンスを待機状態にしてカスタムアクションを実行するために、ライフサイクルフックを追加できる。
カスタムアクションはインスタンスの起動時/終了前に実行される。

Default resultには以下の2つを指定する。

- CONTINUE（続行）：Auto Scalingグループは、終了前に他のライフサイクルフックなどの残りのアクションを続行できる。
- ABANDON（中止）：Auto Scaling グループはインスタンスをただちに終了する。

## Route53

### Latencyベースのルーティング

クライアントのDNSリゾルバから見た最小レイテンシーのリージョンを自動的に選択する。

### Failoverルーティング

プライマリが落ちたときのみセカンダリに切り替える。

### Geoproximity

ユーザの地理的場所と指定シェイプの重なり具合でルーティングを決める機能。

### Evaluate Target Health

有効にするとALB障害時に自動フェイルオーバーが実施される。

### Route 53 Resolver アウトバウンドエンドポイント

AWSからオンプレミスの名前解決を受け付ける。
条件付きフォワーダールールと組み合わせることがベストプラクティス。

### Route 53 Resolver アウトバウンドエンドポイント

オンプレミスからAWSの名前解決を受け付ける。

### その他

DNSには必ずTCP53を開けておく必要がる。

## Cloudformation

### Cloudformation StackSets

複数のアカウント、リージョンのスタックを1度のオペレーションで作成、更新、削除できるサービス。
リージョン間複製機能もある。

#### サービス管理型

OUターゲットで使用すると、OUに紐づくアカウントが増減しても自動的にスタックが展開・更新される。
管理ロール・実行ロールが自動で作成される。

#### 自己管理型

各ターゲットアカウントに管理ロールと実行ロールを手動で用意する必要がある。

### クロススタック参照

他のスタックで出力した情報を別のスタックで取得させる。
同一リージョン内でしか利用できない。

## AWS Transfer Family

マネージドSFTPサービスでコントロールプレーンもリージョン内のデータプレーンも複数AZに分散される。

## AWS Storage Gateway

### File Gatewav

プロトコルはSMB/NFS

## AWS CDK

コードでクラウドインフラストラクチャを定義し、 AWS CloudFormation を通じてプロビジョニングするためのオープンソースのソフトウェア開発フレームワーク。

## AWS AppSync

マネージドでスケーラブルなGrphQLサービスで、WebSocketベースのSubscription（Pub/Sub）機能を提供する。

## AWS Migration Portfolio Assessment(MPA)

Excel/CSVなどのテンプレート入力やADSなど他ツールからのデータ取り込みを前提とする「コスト試算ツール」

## Amazon ElasticCache

### Amazon ElasticCache for Redis

メモリ内データストアで、キャッシュ用途に特化したサービス。
インメモリであるので、Webサーバ層をステートレスに保ちつつ、高速にセッションを共有できる。

### Amazon ElasticCache for Mencached

シンプルでスケールアウトしやすい一方、ネイティブなレプリケーションや自動フェイルオーバー機能はない。

## AWS Resource Access Manager(RAM)

同一組織内の複数アカウントに対して、VPCサブネットをネイティブに共有できるサービス。
VPC全体を共有対象にはできない。
共有されたサブネットにワークロードアカウントからENIを作成することで、各アカウントのEC2インスタンスをネットワーク専用アカウントのVPC内に配置できる。

## Elastic BeanStalk

アプリ実行環境のデプロイを簡素化するサービス。

## Service Catalog

### Launch Constraint（起動制約）

エンドユーザが製品を起動、更新、終了するときにサービスカタログが引き受けるIAMロールを指定する。

## AWS WAF

### Reputation managedルール

AWSが継続的に更新している基既知の悪性IPアドレスを自動参照し、該当IPからのリクエストを遮断する。

## Transit Gaeway

1AZあたり約5Gbps、全AZ合計で最大50Gbpsが上限。

## PrivateLink

サービス公開（NLB/ALB経由）のリクエスト-レスポンス型通信に適している。
相互通信を行うには、サービス側・クライアント側の両方にエンドポイントを配置する必要がある。

## その他

レプリケーションはエンドユーザー体験の改善ではなく、バックアップ/整合性目的
PCI-DSSのオリジン保護で最も一般的なのが`秘匿ヘッダー方式`

## 後で調べる

- CloudFront + Shield Advanced とALB直付けWAFの違い
- AWS WAFのデプロイ戦略（Count → Blockの段階的適用）






