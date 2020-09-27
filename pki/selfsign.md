# 自己署名によるルートCAの構築

## ルートCA構築手順

### ルートCA用ディレクトリ構造をつくる

以下の構造になるようにファイルやディレクトリを作成する。

```
pki
├── certs/
├── csr/
├── index.txt
├── newcerts/
├── openssl.cnf
├── private/
└── serial
```

```
mkdir -p ./pki/{certs,csr,newcerts,private}
touch ./pki/serial
echo "00" > ./pki/index.txt
```

### openssl.cnfを設定する

詳細は省く。ここではCAの設定が上で作ったディレクトリ構造と合致していればよい。

```
####################################################################
[ ca ]
default_ca    = CA_default        # The default ca section

####################################################################
[ CA_default ]

dir           = .              # Where everything is kept
certs         = $dir/certs     # Where the issued certs are kept
crl_dir       = $dir/crl       # Where the issued crl are kept
database      = $dir/index.txt # database index file.
new_certs_dir = $dir/newcerts  # default place for new certs.

certificate   = $dir/cacert.pem        # The CA certificate
serial        = $dir/serial            # The current serial number
crlnumber     = $dir/crlnumber         # the current crl number
                                       # must be commented out to leave a V1 CRL
crl           = $dir/crl.pem           # The current CRL
private_key   = $dir/private/cakey.pem # The private key
```

この設定が含まれた `openssl.cnf` をルートCA用のディレクトリへ配置する。

### ルートCAの秘密鍵を作成する

```
openssl genrsa -out ./private/cakey.pem 4096
```

### ルートCAのCSRを発行する

```
openssl req -new \
            -utf8 \
            -key ./private/cakey.pem \
            -out ./csr/ca.csr \
            -config ./openssl.cnf
```

### 自己署名しルートCAの証明書を発行する

```
openssl x509 -req \
             -days 3650 \
             -in ./csr/ca.csr \
             -signkey ./private/cakey.pem \
             -out ./cacert.pem
```

## 自己署名ルートCAでサーバ証明書を発行する

### 発行する証明書用の秘密鍵とCSRをつくる

この操作はサーバ証明書をデプロイするマシン上でおこなう。
生成されたCSRはルートCAのマシンへ転送する。

```
openssl genrsa -out server.key 4096
openssl req -new -key server.key -out server.csr
```

### ルートCAで署名する

署名はルートCA用ディレクトリ直下でする。

```
openssl ca -in server.csr \
           -out ./certs/server.crt \
           -config ./openssl.cnf \
           -days 365
```

生成された証明書とルートCAの証明書 `cacert.pem` をサーバ証明書をデプロイするマシンへ転送する。

### TLS化の基礎

サーバ証明書をデプロイするマシンには以下の3つのファイルが存在するはず。 

- ルートCAの証明書
- サーバ証明書
- サーバ証明書のCSRを作った秘密鍵

上の例をもとにすると、Apacheなどの設定はおおよそ次のように指定すればいい。

| 設定項目                   | 値         |
| :--                        | :--        |
| Server Certificate         | server.crt |
| Server Private Key         | server.key |
| Certificate Authority (CA) | cacert.pem |
| Server Certificate Chain   | cacert.pem |
