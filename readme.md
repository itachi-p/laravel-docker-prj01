# itachi-P's GitHub repository
##### ※http://itachi-p.com からアクセスされた方向けメッセージ
(よりセキュアなHTTPS接続に対応しました)<br>
<a href="https://itachi-p.com">(HTTPS接続)itachi-Pホーム（このページ）</a><br>
<a href="https://github.com/itachi-P/Laravel-Docker_prj01/">当GitHubリポジトリ</a>

このページはまだ**工事(準備)中**です。<br>
**タイミング次第で繋がらなかったりエラーが出たりします。ご了承下さい。**

<a href="http://itachip.ap-northeast-1.elasticbeanstalk.com/">テストページ1D AWS ElasticBeanstalk multi-container Docker & Laradock（開発中）</a><br>
<br>
~~テストページ2~~ （テスト開発完了、稼働停止済）<br>
- PHP7.2 & Laravel & AWS Elastic Beanstalk(ECS)/VPC/EC2/Route53/EBS/IAM(ユーザー・グループ権限管理)
/S3(ストレージ)/RDS(MySQL)/ACM(HTTPS接続用電子証明)/Load Balancer(負荷分散)
/CloudWatch(メトリクス監視→通知・パフォーマンス自動管理)/SimpleNotificationService(CloudWatchと連動したアラート通知)/etc.<br>

~~テストページ3B~~ GCP & Docker ※稼働停止中(AWS優先)<br>

- （参考）
  - [GCPで永久無料枠を利用してサービスを立ち上げたときにしたことの備忘録](https://qiita.com/riku-shiru/items/a870edd9dc0b132e092c)
  - [GCE上のContainer-Optimized OSでDocker Composeを使う（和訳）](https://qiita.com/gorohash/items/608da9050b32db581802)

---

### 履歴

- 2019/09/17
  - Amazon Virtual Private Cloud (VPC)にてアップロード領域確保
  - Amazon Elastic Compute Cloud (EC2)インスタンス生成（リージョン：東京ではなく北米西海岸オレゴンを選択）
  - Amazon Identity and Access Management(IAM)にて権限を制限したユーザー作成
- 2019/09/18
  - Amazon Route 53 にて独自ドメイン取得、ルーティング
  - Amazon EC2上にSSH接続で「工事中」ページをファイルアップロード
  - http://test.itachi-p.com のアドレスで仮公開
- 2019/09/19
  - http://itachi-p.com/ のアドレスにてGitHubの当リポジトリを紐付け、連動（仮）公開
- 2019/09/20〜23
  - Docker利用法の選択肢
    - ~~Amazon Elastic Container Service(ECS)~~　**次回利用予定**
    - ~~Elastic Beanstalk Docker (Single Container)~~ *Multiの方が柔軟に対応可能*
    - **Elastic Beanstalk Multi-Container Docker 今回はこれに決定**
  - CircleCI連動（設定段階）※実装優先度低（後でよい）
    - [（公式）CircleCI を設定する](https://circleci.com/docs/ja/2.0/configuration-reference/)
    - [（公式）2.0 config.yml の設定例](https://circleci.com/docs/ja/2.0/sample-config/)
    - [実践で学ぶ、Laravelをローカルから本番環境にデプロイするまで](https://logmi.jp/tech/articles/321252)
      - .circleci/config.ymlの設定(今ここ)→CircleCIのWebサイトに行って設定→start building
  - Amazon CloudFormationによりここまでの構築環境をスタックとして記録
  - Amazon CloudWatchによる監視利用開始
    - Amazon CloudWatch(Logs)利用→S3にログ保存
    - Amazon Simple Notification Service(SNS)利用→重大な影響が発生した際にメール通知する設定を有効化
- 2019/09/24
  - ローカルでPHP7.3/Laravel/MySQLにてテストアプリ作成
  - 上記アプリケーションをAWS Elastic Beanstalkにデプロイするためにソースバンドル(zip)を作成
  - Elastic Beanstalk 管理ページからデプロイ
  - <a href="http://tutorials-env.6mt7peepvf.ap-northeast-1.elasticbeanstalk.com/">テストページ2 PHP & Laravel</a>でDB(RDS)接続までを動作確認
  - 上記ローカルで作成したアプリケーションのデータベース設定をローカルのMySQL(Ver5.7.27)からAWS RDSのMySQL(Ver5.7.26)に設定変更
  - ソースバンドル再作成→手動デプロイを`eb deploy`で自動化
  - AWS EB上からRDSのｍMySQLに接続確認
  - AWS EC2上で新規Application Load Balancerを作成し、ACM電子証明書によるセキュアなHTTPS接続に対応
  - [（公式）Elastic Beanstalk への Laravel アプリケーションのデプロイ](https://docs.aws.amazon.com/ja_jp/elasticbeanstalk/latest/dg/php-laravel-tutorial.html)
  - Amazon Simple Storage Service (S3)に静的ファイル配置
  - Amazon Relational Database Service(RDS)連動
  - Amazon CloudWatch(Logs)利用
    - 監視→アラート（Eメール、SMS等）| 設定に基づく何らかのアクション
      - スケールアップ（パフォーマンス向上）
      - スケールダウン（コスト削減）
      - イベント発生→自己トリガーにより自動アクション設定
      - 一定時間ごとのバッチ処理　など
- 2019/09/25
  - SSH接続のトラブル
    - セキュリティグループのインバウンド設定、キーペアとの関連付け等見直し・再設定
  - Docker非使用のLaravel & データベース接続(RDS上のMySQL)を含むAWS各種マネージドサービスのテスト駆動が完了した為Elastic Beanstalk環境インスタンス停止・終了
  - DockerベースでのLaravel開発（Laradock）再開（このreadme.mdを含むリポジトリの本体）
    - **最終的には開発したLaravelアプリも含めたカスタムイメージ＆Dockerfileを作成し、それをCircleCIを通して自動ビルド・テスト・デプロイする**
- 2019/09/26,27
  - 新規Elastic Beanstalk multi-container Docker環境を立ち上げ
    - *Auto Scaling, Load Balancing*設定により状況に合わせて最低2〜最高4のインスタンスを稼働し負荷分散する設定
    - Load Balancerによって新規に生成された複数インスタンス稼働を確認
  - Elastic Beanstalk multi-container Docker, 及びDocker学習(Udemy及び書籍)
- 2019/09/28~29
  - Dockerチュートリアルの一環としてGoogle Cloud Platform(GCP)アカウント作成
    - `docker-machine`コマンドによりGCEインスタンス生成
    - ウェブページ公開して動作確認
    - 引き続きGCEインスタンスへのSSH接続、インスタンス設定等　及びUdemy・購入電子書籍によるDocker学習継続
- 2019/09/30
  - GCP（GCE)上にOregonリージョンの無料永久枠で新規インスタンス作成、Dockerイメージ紐付け、起動確認
- 2019/10/01~02
  - テストページ3（GCP＆Docker) | テストページ１（AWS＆Docker)上でfortunesを走らせるところまで実装
  - インスタもどき作成開始（1週間以内、10/７(月)まで）
- 2019/10/03
  - Docker、AWS、Terraterm学習
    - Docker Machine - WindowsやMac上のVMで動くDockerホストを作成(Docker Engineを実行)できるVirtualBox,Vagrant互換VM環境
      1. ローカル(リモートも)にDocker Machineを使い、Docker Engineが動く仮想マシンをプロビジョニングする
      2. その仮想環境に接続すると、その後はdockerコマンドが実行可能になる
    - (参考)[Docker Machine 概要(Docker ドキュメント日本語化プロジェクト)](http://docs.docker.jp/machine/overview.html)
    - Docker Swarm - 標準 Docker API で操作できるDockerに対応するネイティブなクラスタリング用ツール
      - Udemy動画に従い軽めに流す（ざっと全体を一通り理解する程度で充分）→詳細に拘らずアプリの中身開発を優先
    - Dockerのデータ管理 / 3種類のマウント方法と特性・使い分け
      - volume (実際にはホスト上のディレクトリ)
      - bind mount (実際にはホスト上のファイルやディレクトリ)
      - tmpfs (実際にはホスト上のメモリ)
    - Docker Compose
  - 2019/10/04
    - インスタもどき簡易作成
      - DB設計
        1. 概念設計　エンティティ抽出、リレーションシップ設定（1：1、1：多、多対多）
        2. 論理設計　概念モデルから多対多を除外しリレーショナルモデル（表）に変換→更に正規化（一意性）
        3. 物理設計　リレーショナルモデルから実際にテーブルを作成（データ型、制約の設定）
      - (参考)[4ステップで作成する、DB論理設計の手順とチェックポイントまとめ](https://qiita.com/nishina555/items/a79ece1b54faf7240fac)

---

### (以後の予定)

- AWSマネージドサービスのうち今回利用する選定済み各サービスについての理解及び設計思想強化
  - [AWS Well-Architected Training (Japanese) (AWS による優れた設計トレーニング)](https://www.aws.training/Details/Curriculum?id=12033)
    - 7個のオンライントレーニング＋**評価テスト**
- Dockerrun.aws.jsonファイルの記述（バージョン１ - シングルコンテナ、バージョン２ - マルチコンテナ）
  - シングルコンテナの場合のみDockerfileによるカスタムイメージ使用可能、マルチコンテナはDockerrun.aws.jsonのみ
  - Amazon Elastic Container Registry(Amazon ECR、後述)にイメージを保存する場合はカスタムイメージ保存制限なし
- [（公式）単一コンテナのDocker設定](https://docs.aws.amazon.com/ja_jp/elasticbeanstalk/latest/dg/single-container-docker-configuration.html)
- [（公式）複数コンテナのDocker設定](https://docs.aws.amazon.com/ja_jp/elasticbeanstalk/latest/dg/create_deploy_docker_v2config.html#create_deploy_docker_v2config_dockerrun)
- [入門Docker](https://y-ohgi.com/introduction-docker/)
- Amazon Elastic Container Registry (Amazon ECR)を使用して(有料)AWS にカスタムDockerイメージを保存するか、それともDockerHubから'docker login'するか(ECRを使わない場合は認証情報のS3への保存が必要）要検討
- 認証にAmazon Cognito利用を検討
  - サーバレスアーキテクチャにより一切コードを書かずに認証（サインアップ・サインイン）機能実装も可能
  - [プログラミングせずにCognitoで新規ユーザー登録＆サインインを試してみる](https://dev.classmethod.jp/cloud/aws/sign-up-and-sign-in-by-cognito-with-awscli/)
- 肝心のポートフォリオの中身の作成
  - 上記をDockerイメージと共に、または上記も含めたカスタムイメージをAWSにデプロイ
  - CircleCI連動で自動ビルド、テスト、デプロイ確認
    - GitHubの該当リポジトリ上でPR（プルリク）が作成されたら自動的にビルド・テストを実行
    - masterブランチにPRがmergeされたら `eb deploy`コマンドを実行し、自動でデプロイ
- Dockerイメージを公開リポジトリではなくプライベートリポジトリに置くよう変更
  - [Amazon S3をDockerプライベートレポジトリにしてAWS ElasticBeanstalk環境にデプロイ](https://aws.typepad.com/sajp/2014/06/eb-docker-private-repo.html)
- 更に余裕ができればECSへの理解を深め、Elastic Beanstalk → (ECR &) ECSに移行
- 更に更にECS → （Kubernetesについて学習した上で）EKS？
- *いずれにせよ、今後k8s(やGCPやGolang)にも対応していくが、まずはLaravel & AWS & Dockerにある程度習熟する*

(その他)
- **継続的デリバリー/継続的インテグレーション**
  - ツールとしてのCircleCIの利用だけでなく、アプリ開発及び運用、機能追加、リファクタリング等全般においてCI/CD及びその最適ツールの運用を検討する。

  - (参考)
    - [既存のAWS環境を後からTerraformでコード化する](https://dev.classmethod.jp/cloud/aws/aws-with-terraform/)
    - [10分で理解するTerraform](https://qiita.com/Chanmoro/items/55bf0da3aaf37dc26f73)
- サーバレス・イベントドリブン化
  - CloudWatchにより複数のメトリクス監視→何かしらのイベント（アクセス数・パフォーマンス変化や定時実行イベント等）・障害等の発生→*Lambda*によりAPI起動→タスク、スケジュール、バッチ処理、障害対応、スケールアップ（パフォーマンス向上）、スケールダウン（コスト削減）を実行
- MySQL → Amazon Database Aurora（MySQL5.6互換）の利用
  - データベースもAmazon RDS/Aurora等のマネージドサービスを利用し、インフラ導入及び運用のコストカット、インフラ層（レイヤー）の管理をAmazonに運用委任して最上位のアプリケーション開発だけに集中する選択肢も選べるように
- Amazon 開発者用ツール群の使用検討
  - AWS Developer Tools
　アクセスコントロール用のIAM、監査ログ用のAWS CloudTrail、イベントトリガー用のAmazon CloudWatch等と連携  
    - CodePipeline - リリース自動化サービス、他のツール群の連携(CircleCI)
    - CodeCommit - ソース管理 (GitHub)
    - CodeBuild - ビルドテスト実行ツール
    - CodeDeploy - デプロイオーケストレーションツール
    - CodeStar - 継続的デリバリーデリバリーツールチェーン全体を設定
    　　ビルドするアプリケーションのタイプと使用するプログラミング言語を選択すると包括的なツールチェーンが作成され最初の土台となるコードと共にプリロードされる

---


<p align="center"><img src="https://laravel.com/assets/img/components/logo-laravel.svg"></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/d/total.svg" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/v/stable.svg" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/license.svg" alt="License"></a>
</p>
