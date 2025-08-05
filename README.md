# キーワードメモ

## S3

### Inteligent-Tiering

アクセス頻度に応じて自動でデータを階層化し、最初の48時間の高トラフィックはStandard tierが処理するため、パフォーマンスを確保できる
アクセスが減った際はArchive Instant Access tierへ無停止で移行する

### Glacier

リストアには数分～数時間かかる

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
そのため、ぢすくフォーマットが変わる可能性がある

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

## その他

レプリケーションはエンドユーザー体験の改善ではなく、バックアップ/整合性目的

## 後で調べる

- CloudFront + Shield Advanced とALB直付けWAFの違い






