# Amazon Linux 2をオンプレミスで実行する

## 公式ドキュメント

- [Amazon Linux 2](https://aws.amazon.com/jp/amazon-linux-2/)
- [Amazon Linux 2 に関するよくある質問](https://aws.amazon.com/jp/amazon-linux-2/faqs/)
- [Amazon Linux 2 を仮想マシンとしてオンプレミスで実行する](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html)

## VMWare ESXiでAmazon Linux 2を動かす

公式ドキュメントの『[Amazon Linux 2を仮想マシンとしてオンプレミスで実行する](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html)』を読む。Amazon Linux 2をESXiで動かすには以下の2つが必要なことに注意。

- seed.iso
- amzn2-vmware_esx-2.0.20181114-x86_64.xfs.gpt.ova

seed.isoの作り方はドキュメントに載っている通り。seedconfigディレクトリの構造は次のようになる。

```
seedconfig
├── meta-data
└── user-data
```

meta-dataとuser-dataの例を載せる。

### meta-data

```
local-hostname: amazonlinux
network-interfaces: |
  iface eth0 inet static
  address 172.16.0.3
  network 172.16.0.0
  netmask 255.255.0.0
  broadcast 172.16.255.255
  gateway 172.16.0.1
```

### user-data

```
#cloud-config
users:
  - default
chpasswd:
  list: |
    ec2-user:PASSWORD
```

OVAをデプロイしたあと、seed.isoを仮想マシンに挿入するためにCD/DVDドライブを追加し、seed.isoを参照するよう設定する。
