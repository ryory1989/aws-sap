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

## Amazon Kinesis Data Streams

高スループットのバッファリングに適している。

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

標準ではマルチライター昨日は提供されていない。

## Amazon RDS for MySQL

クロスリージョンリードレプリカは非同期レプリケーションなので、レプリケーションラグが数十秒～数分になるケースもある。
定期スナップショットは通常、5分～24時間間隔で取得される。
スナップショットからのリストアは数十分～数時間を要する。

## Amazon RDS Proxy

データベースの前段で接続プールを維持し、同時実行が急増した場合でも新規TCP/SSLハンドシェイクを削減する。

## Amazon API Gateway

Edge-Optimizedエンドポイントに切り替えると、自動で前段にCloudFrontが配置される。

## CodeDeploy

### Blue/Greenモード

旧環境と新環境を並列稼働し、ALBのルーティングを自動で切り替える

## Amazon Config

状態変化（コンプライアンス違反）の評価を行うサービス

## Lambda

実行環境はリクエスト後でも一定時間アイドル保持され、次回呼び出し時に同じコンテナが再利用される。

## その他

レプリケーションはエンドユーザー体験の改善ではなく、バックアップ/整合性目的

## 後で調べる

- CloudFront + Shield Advanced とALB直付けWAFの違い
- AWS WAFのデプロイ戦略（Count → Blockの段階的適用）






