# Container Insights Prometheus

[Container Insights](https://docs.aws.amazon.com/ja_jp/AmazonCloudWatch/latest/monitoring/ContainerInsights.html)はECSやEKSの環境で手っ取り早くメトリクスをモニタリングするのに役立ちます。しかし、標準のContainer Insightsでモニタリングするメトリクスは数が多く、費用が高くなりがちです。また、収集するメトリクスの調整もできません。

Container InsightsはPrometheusメトリクスをモニタリングすることもできます。これを使えば狙ったメトリクスのみモニタリングできます。

参考：[Container Insights の Prometheus メトリクスのモニタリング](https://docs.aws.amazon.com/ja_jp/AmazonCloudWatch/latest/monitoring/ContainerInsights-Prometheus.html)

EKSの環境でContainer InsightsによるPrometheusメトリクスをモニタリングする例を解説します。

# 全体像

今回の例ではDeploymentのActive Podの割合をモニタリングしてみます。ActiveなPodの数がしきい値を下回るとCloudWatchアラームが検知する仕組みです。

EKSにPrometheusメトリクスを収集できるCloudWatch Agent入りのPodを作成します。このPodはクラスタ内のPrometheusメトリクスをスクレイピングします。スクレイピングしたメトリクスはCloudWatchメトリクスに登録されます。Deploymentのstateで定義したreplica数とactive Pod数のメトリクスを収集するため、クラスタに[kube-state-metrics](https://github.com/kubernetes/kube-state-metrics)のPodもデプロイします。監視対象としてNginxのDeploymentをデプロイします。

AWSはContainer Insigtsにより登録されるメトリクスをモニタリングするCloudWatchアラームを設定します。

絵はる

# 構築方法

## 前提

VPC、EKSをデプロイしておいてください。

## 監視対象Deployment(Nginx)のデプロイ

readinessだけ設定してlivenssは設定しない。readinessはindex.htmlを設定する。

## kube-state-metricsのデプロイ

## Container Insights Prometheusのデプロイ

## Cloudwatch Alarmの設定

## 動作確認

Podに入りindex.htmlを消す。

DeploymentのREADYが1/2になる。

CloudWatchアラームを確認する。アラーム状態になっている。

Podに入りindex.htmlを作成する。

DeploymentのREADYが2/2になる。

CloudWatchアラームを確認する。正常状態になっている。
