# AWSをコマンドから**あれやこれやしたいため**の基本的な設定メモ

## 概要

awsへローカルからdeployする方法を勉強したので、そのまとめ1。

このリポジトリでは、ローカルの環境からawsをいじれるようにするための基本的な設定までのメモ。

mfa(他要素認証)の設定も含む。

## 目次

1. 実行環境
2. 使用するツールをインストールする
3. awsの認証情報を入力する
   1. プロファイルを割り当てない
   2. プロファイルを割り当てる
4. awsのmfa認証に対応させる
5. 次回からのmfa認証を楽にさせる

## 1. 実行環境

2021/09/23現在

実行環境

- MacOS Big Sur ver:11.5.2
- Homebrew 3.2.13
- python 3.9.6

## 2. 使用するツールのインストール

### awscliのインストール

awscli: AWSコマンドラインインターフェイス(CLI)

> awscliは、AWS サービスを管理するための統合ツールです。ダウンロードおよび設定用の単一のツールのみを使用して、コマンドラインから AWS の複数のサービスを制御し、スクリプトを使用してこれらを自動化することができます。

引用元: <https://aws.amazon.com/jp/cli/>

#### コマンド

以下のコマンドを実行して、awscliをインストールする。

```bash
brew install awscli
```

### aws-mfaのインストール

aws-mfa: 

#### コマンド

以下のコマンドを実行して、aws-mfaをインストールする。

```bash
pip3 install aws-mfa
```

## 3. awsの認証情報を入力する

コマンドラインから、自分のAWSのサービスを使用するためにはAWSアカウントをローカルの環境と紐づける必要がある。

### 3.1 プロファイルを割り当てない場合

mfa認証をしない場合、とりあえずAWSを使いたい場合はこの設定だけで大丈夫。

4以降はやらなくて良い。

#### コマンド

以下のコマンドを実行して、自分のAWSアカウントと紐付ける。

```bash
aws configure
```

上記のコマンドを実行することで、以下の入力が一行ずつ出力される。

```bash
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
Default region name [None]: 
Default output format [None]:
```

補足

- AWS Access Key ID: 
  - セキュリティ認証情報 > アクセスキー > アクセスIDに書かれているの文字列
    - (ex. AKIAXXXXXXXX)
- AWS Secret Access Key: 
  - アクセスキーを作成した際にダウンロードしたキー
    - (ファイル名を変えてなければ、rootkey.csvに書かれている)
    - AWSSecretKey={AWS Secret Access Key}%
    - (ex. i5gLPW4QrH2KL6kfswY788dY3GAV)
- Default region name:
  - 自分が登録したリージョン
    - (ex. ap-northeast-1): 東京の場合
- Default output format
  - awsコマンドを使用した際に返ってくる形式
    - 指定しない場合は、jsonとなる
    - (ex. json)

### 3.2 プロファイルを割り当てる場合

mfa認証をしない場合、とりあえずAWSを使いたい場合はこの設定だけで大丈夫。

4以降はやらなくて良い。

### コマンド

以下のコマンドを実行して、自分のAWSアカウントと紐付ける。

```bash
aws configure --profile [profile_name]
```

もしくは

```bash
aws configure --profile [profile_name]-long-term
```


補足

- -- profile
  - >--profile オプションを指定して名前を割り当てることによって、さまざまな認証情報と設定を持つ追加の名前付きプロファイルを作成して使用できます。
    - prfile_name は、適当な文字列
    - (ex. --profile mfa)
- [profile_name]-long-term

上記のコマンドを実行することで、以下の入力が一行ずつ出力される。

```bash
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
Default region name [None]: 
Default output format [None]:
```

補足(3.1と同じ)

- AWS Access Key ID: 
  - セキュリティ認証情報 > アクセスキー > アクセスIDに書かれているの文字列
    - (ex. AKIAXXXXXXXX)
- AWS Secret Access Key: 
  - アクセスキーを作成した際にダウンロードしたキー
    - (ファイル名を変えてなければ、rootkey.csvに書かれている)
    - AWSSecretKey={AWS Secret Access Key}%
    - (ex. i5gLPW4QrH2KL6kfswY788dY3GAV)
- Default region name:
  - 自分が登録したリージョン
    - (ex. ap-northeast-1): 東京の場合
- Default output format
  - awsコマンドを使用した際に返ってくる形式
    - 指定しない場合は、jsonとなる
    - (ex. json)

## 4. awsのmfa認証に対応させる

mfa認証(他要素認証)の場合、更にMFAデバイスのARN情報を入力する。

```bash
aws-mfa --profile [profile_name] --device arn:aws:iam::

```

## 5. 次回からのmfa認証を楽にさせる

再度、アクセスキーを取得する際に、デバイス名を一々打つのが面倒なため、デバイス名をcredientialsに追記する

今後、アクセスキーを取得する際は、デバイス名を抜いたコマンドで済む。

```bash
aws-mfa --profile [profile_name]
```




