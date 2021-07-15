# Kitchensink

This is a sample application for EAP 7.3 on OpenShift.

The original of this project is copied from
[kitchensink directory](https://github.com/jboss-developer/jboss-eap-quickstarts/tree/7.3.x-openshift/kitchensink)
on openshift branch of the quickstarts project.

# GitHub Action "build-and-scan.yml" の動作の前提条件

*現在はテストをしやすいようプッシュ時の自動トリガーはコメントアウトしている。
代わりに "Actions > Build and Scan > Run workflow" で手動で開始する。*

"Settings > Environments" で下記の名前のEnvironmentを作成する。

- Environment: testenv

さらに、その中に以下の5つのSecretを作成する。

| 名前                | 値の内容                       | 備考                                                       |
|---------------------|--------------------------------|------------------------------------------------------------|
| OPENSHIFT_SERVER    | OpenShiftのAPIサーバのURL      | `oc whoami --show-server` で取得可能。                     |
| OPENSHIFT_USERNAME  | OpenShiftのログインユーザ名    | プロジェクト作成権限があること。                           |
| OPENSHIFT_TOKEN     | 上記ユーザのトークン           | ログイン後に `oc whoami -t` で取得可能。パスワードは不可。 |
| OPENSHIFT_NAMESPACE | ビルドが行われるプロジェクト名 | 存在しない場合は作成される。                               |
| OPENSHIFT_REGISTRY  | OpenShiftレジストリのサーバ名  | `oc registry info` で取得可能。                            |

OPENSHIFT_TOKEN の期限がデフォルトで24時間なので、定期的に更新が必要になる。
この期間を伸ばしたい場合は以下のドキュメントに従って設定する。

- [内部 OAUTH サーバーのトークン期間の設定](https://access.redhat.com/documentation/ja-jp/openshift_container_platform/4.7/html/authentication_and_authorization/oauth-configuring-internal-oauth_configuring-internal-oauth)

厳密にはSecretである必要があるのは OPENSHIFT_TOKEN のみ。
その他のSecretはログ中でマスクされてしまい確認に不便だが、
ソースコードを変えることなく内容を変えられるのでSecretに設定するようにした。

# ローカルマシンでのビルドの仕方

How to build:

    mvn package

How to deploy to an already running EAP:

    nvm wildfly:deploy

How to find help documentation for a plugin:

    mvn mvn help:describe -Ddetail -Dplugin=wildfly

## To deploy on OpenShift

Use "openshift" profile to rename the WAR archive to "ROOT.war".

    mnv package -Popenshift

## Run Arquillian test using an already running server

    mvn integration-test -Parq-remote

## Run Arquillian test using with EAP to be started

    JBOSS_HOME=/path/to/eap-home mvn integration-test -Parq-managed

