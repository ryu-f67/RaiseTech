# 第6回課題
## 1.CloudTrailでモニタリングされるAWS操作記録をリストアップ
![CloudTrailイベント詳細例](images/lecture06imgs/cloudtrail_for_lecture06.png)
```sh
{
    "eventVersion": "1.09",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDA3FLDXL62UQIY5ZCVG",
        "arn": "arn:aws:iam::767397683125:user/RYUYA_RT",
        "accountId": "767397683125",
        "accessKeyId": "ASIA3FLDXL62RLOESRNZ",
        "userName": "RYUYA_RT",
        "sessionContext": {
            "attributes": {
                "creationDate": "2024-04-26T08:09:39Z",
                "mfaAuthenticated": "true"
            }
        }
    },
    "eventTime": "2024-04-26T08:18:13Z",
    "eventSource": "ec2.amazonaws.com",
    "eventName": "StopInstances",
    "awsRegion": "ap-northeast-1",
    "sourceIPAddress": "XXX.XXX.XX.XX",
    "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36",
    "requestParameters": {
        "instancesSet": {
            "items": [
                {
                    "instanceId": "i-0b9a1384b93517c36"
                }
            ]
        },
        "force": false
    },
    "responseElements": {
        "requestId": "4239a667-ac80-43e0-bff7-9a47a467b7ba",
        "instancesSet": {
            "items": [
                {
                    "instanceId": "i-0b9a1384b93517c36",
                    "currentState": {
                        "code": 64,
                        "name": "stopping"
                    },
                    "previousState": {
                        "code": 16,
                        "name": "running"
                    }
                }
            ]
        }
    },
    "requestID": "4239a667-ac80-43e0-bff7-9a47a467b7ba",
    "eventID": "b221eb8f-4405-403d-a68a-257c07a73c27",
    "readOnly": false,
    "eventType": "AwsApiCall",
    "managementEvent": true,
    "recipientAccountId": "767397683125",
    "eventCategory": "Management",
    "tlsDetails": {
        "tlsVersion": "TLSv1.3",
        "cipherSuite": "TLS_AES_128_GCM_SHA256",
        "clientProvidedHostHeader": "ec2.ap-northeast-1.amazonaws.com"
    },
    "sessionCredentialFromConsole": "true"
}
```

- イベント名
  - StopInstances
      EC2インスタンスの停止
- 内容(例として3点)
  - userIdentityIAM
      アイデンティティに関する情報
  - eventTime
      イベントが完了した日時
  - responseElements
      変更を行うアクションのレスポンスの要素(今回はEC2が起動状態から停止状態へ変更された)


## 2.CloudWatchアラームを設定
ALBに対してアラームを作成し、アラーム状態とOK状態の両方で通知が来るように設定した。
- アラーム状態
![ALBアラ－ム](images/lecture06imgs/ALB_alarm.png)
![ALBアラ－ム通知メール](images/lecture06imgs/ALB_alarm_mail.png)
- OK状態
![ALB OK](images/lecture06imgs/ALB_OK.png)
![ALB OK通知メール](images/lecture06imgs/ALB_OK_mail.png)


## 3.AWS利用料の見積もりを作成
AWS利用料の見積もり [URL](https://calculator.aws/#/estimate?id=6931555d537df7b1f7d275b482cd82405d7a4d1a)


## 4.マネジメントコンソールから現在の利用料を確認
- 請求のページから現在の利用料金を確認した(2024.4.28)
    EC2の起動時間は750時間以下であり無料枠で収まったが、EBSが無料枠の30GBを超えたため料金が発生している。
    また、VPCのpublic IPv4 addressの使用でも料金が発生している。
    ![コスト](images/lecture06imgs/20240428_cost.png)
    ![請求詳細(1)](images/lecture06imgs/20240428_costdetail(1).png)
    ![請求詳細(2)](images/lecture06imgs/20240428_costdetail(2).png)
    
    金額は発生しているがAWSクレジットから消費されるため、合計税額は USD 0.00 になっている

- 請求のページから先月(2024.03)のEC2の利用料金を確認した
    EC2の起動時間は750時間以下であり無料枠で収まっている。
    また、EBSも無料枠の30GBを下回っているため無料枠で収まっており、料金がかかっていない。
    ![請求詳細(先月)](images/lecture06imgs/202403_costdetail.png)


## 5.課題に取り組んで感じたこと
- 現在は12か月間無料の対象期間であるため金額が抑えられているが、期間が終わってからのことを想定して、定期的にリソースの稼働状況を確認し、無駄に動いているリソースがないかチェックする習慣をつけなければならないと感じた。