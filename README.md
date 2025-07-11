# Alert-Ticket-Grouping
# 概要
このスクリプトはCDSLで公開されている[redmine-tickets-create](https://github.com/cdsl-research/redmine-tickets-create)を改良したものです.</br>
Redmineにアラートチケットを作成する際, 同名のアラートをグルーピングしてくれます.</br>

# 導入
```
git clone https://github.com/cdsl-research/Alert-Ticket-Grouping.git
```

# 説明
同名、同ホストによるアラートの場合, 最初に作成されたチケットをルート（親）チケットとして、それ以降は子チケットとして登録されます.</br>
```
[Alert] Server ICMP-Check instance:hoge (6/17 11:23) ←（ルートチケット）
├─ [Alert] Server ICMP-Check instance:hoge (6/17 12:27)
├─ [Alert] Server ICMP-Check instance:hoge (6/17 14:08) 
└─ [Alert] Server ICMP-Check instance:hoge (6/17 15:13)
```

---

同名のアラートであるが異なるホストからの場合, 共通のルートチケットを作成し, 実際のアラートチケットを子チケットとして登録します.</br>

```
[Alert] HostMemoryUsageHigh-Usage90% instance:A (6/16 11:46) ←（共通ルートチケット）
├─ [Alert] HostMemoryUsageHigh-Usage90% instance:B (6/16 11:46)
├─ [Alert] HostMemoryUsageHigh-Usage90% instance:D (6/16 11:51)
├─ [Alert] HostMemoryUsageHigh-Usage90% instance:A (6/16 11:51)
├─ [Alert] HostMemoryUsageHigh-Usage90% instance:C (6/16 15:12)
├─ [Alert] HostMemoryUsageHigh-Usage90% instance:B (6/16 15:13)
├─ [Alert] HostMemoryUsageHigh-Usage90% instance:C (6/17 17:27)
└─ [Alert] HostMemoryUsageHigh-Usage90% instance:B (6/17 17:27)
```

親チケットが"Resolved"状態になるとルートチケットがCloseされ, それ以降にアラートチケットが作成されたら新たにルートチケットとして作成されます.
