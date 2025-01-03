# Azure Applicaiton Gateway

https://docs.microsoft.com/ja-jp/azure/application-gateway/overview

- レイヤー 7 (HTTP/HTTPS 層) で動作
- [正常性プローブ](https://docs.microsoft.com/ja-jp/azure/application-gateway/application-gateway-components#health-probes)
- [URLパスベースのルーティング](https://docs.microsoft.com/ja-jp/azure/application-gateway/features#url-based-routing)
- [複数サイトのホスティング](https://docs.microsoft.com/ja-jp/azure/application-gateway/features#multiple-site-hosting)
- [TLSターミネーション / エンドツーエンドTLS](https://docs.microsoft.com/ja-jp/azure/application-gateway/ssl-overview)
- WAFを利用可能
- ゾーン冗長をサポート. 複数の可用性ゾーンにまたがってデプロイ。


■Application Gatewayの特徴（Azure Load Balancerとの主な違い）

(1)**URLパス** や **ホスト ヘッダー** など、HTTP リクエストの属性に基づいてルーティングを決定することができる。

HTTPリクエストの例:

```
GET /index.php HTTP/1.1
Host: www.example.com
```

※1行目の「/index.php」が **URLパス** 。2行目の「Host: www.example.com」が **ホストヘッダー** 。

これらの値に基づいて、リクエストを（異なる）バックエンドへルーティングすることができる。

- [URLパスに基づくルーティング](https://docs.microsoft.com/ja-jp/azure/application-gateway/create-url-route-portal)
  - URLパスに基づき、異なるバックエンドへトラフィックをルーティングできる
    - 例
      - /video/* → videoBackendPool
      - /images/* → imageBackendPool
      - /* → appBackendPool
- [ホスト名に基づくルーティング](https://docs.microsoft.com/ja-jp/azure/application-gateway/create-multiple-sites-portal)
  - 異なるドメインの複数のWebサイトを1つのApplication Gatewayでホスティング可能

(2)TLS(SSL)による暗号化通信に対応。

- [TLS終端(TLS termination)](https://docs.microsoft.com/ja-jp/azure/application-gateway/ssl-overview#tls-termination)
  - TLSセッションがApplication Gatewayで解除される
  - Application Gatewayからバックエンドの間では暗号化なし（HTTP）で通信。
  - バックエンドの負荷を削減できる
- [エンドツーエンドTLS暗号化](https://docs.microsoft.com/ja-jp/azure/application-gateway/ssl-overview#end-to-end-tls-encryption)
  - TLSセッションがApplication Gatewayでいったん解除され、ルーティングが行われる。バックエンドに要求が送られる際に別のTLSセッションを使用する。
  - 用途
    - セキュリティ要件、コンプライアンス要件により、エンドツーエンド暗号化が必要な場合
    - バックエンドがTLS通信しか受け付けない場合

TLS証明書は[Azure Key Vaultに格納することができる](https://docs.microsoft.com/ja-jp/azure/application-gateway/key-vault-certs)。

■レベル（SKU）

※登場順

- Standard v1 (Azure Portal上は「Standard」と表示)
- WAF v1 (Azure Portal上は「WAF」と表示)
- Standard v2
- WAF v2

※「Standard v1」: 2015/6/25 一般提供開始. https://azure.microsoft.com/en-us/updates/general-availability-azure-application-gateway/

※「WAF v1」: WAF機能が付いたApplication Gateway。2017/3/29 一般提供開始. https://azure.microsoft.com/ja-jp/updates/application-gateway-web-application-firewall-generalavailability/

※「Standard v2」と「WAF v2」: 「Standard」「WAF」にない追加の機能（自動スケーリングやゾーン冗長等）を提供する。[2019/4/30に追加](https://azure.microsoft.com/ja-jp/updates/azure-application-gateway-standardv2-wafv2-skus-generally-available/)。

■WAFとは？

- Web Application Firewall
- SQL Injection攻撃などの一般的なWebアプリケーションに対する攻撃を防御

■v2の機能

- [自動スケーリング](https://docs.microsoft.com/ja-jp/azure/application-gateway/application-gateway-autoscaling-zone-redundant)
- ゾーン冗長
- など
- [完全な比較表](https://docs.microsoft.com/ja-jp/azure/application-gateway/application-gateway-autoscaling-zone-redundant#feature-comparison-between-v1-sku-and-v2-sku)

■インスタンス数(v2)

- 最小インスタンス数(0～100)
  - トラフィックに関係なく常に保持されるインスタンスの数
- 最大インスタンス数(1～125)
  - トラフィックの増加に合わせて増やされるインスタンス数の上限

※最小インスタンス数＜最大インスタンス数である必要がある。


※インスタンス数とコストは単純には比例しない。以下「料金」を参照。

■料金(v2)

「固定コスト」＋「容量ユニットのコスト」

(1)固定コスト

- Application Gatewayがプロビジョニングされた時間に基づいて課金。

(2)容量ユニット(Capacity Unit: CU)のコスト

「予約されたCU」＋「追加で使用されたCU」に基づいて課金。

CUとは:
- スループット、コンピューティング、永続的な接続数(TCPコネクション数)から算出される
- [1CU](https://docs.microsoft.com/ja-jp/azure/application-gateway/understanding-pricing):
  - 2500 個の永続的な接続(TCPコネクション数)
  - 2.22 Mbps のスループット
  - 1 コンピューティング ユニット
- いずれかのパラメータが超過した場合、追加のCUが使用される。
- 1インスタンスで最低10CUを保証。
  - 実際に1インスタンスでサポートできるCUは、トラフィックパターンにより変動

「予約されたCU」:
- 1インスタンスあたり10CU
- 実際の消費されたCUに関係なく課金

「追加で使用されたCU」:
- 予約されたCUでは不足する場合の、追加で消費されたCU。

※たとえ

- Application Gateway: お店
- 固定コスト: お店の家賃
- インスタンス: お店で働いてもらうアルバイトさん
- 最小インスタンス数: 常にお店にいてもらうアルバイトさんの数
- 最大インスタンス数: アルバイトさんの最大数
- 予約されたCU: アルバイトさんに毎月支払う最低給与
- 追加で使用されたCU: アルバイトさんが残業をした場合に支払う残業代


■HTTP/2のサポート

- 無効(デフォルト)
- 有効

[HTTP/2](https://ja.wikipedia.org/wiki/HTTP/2): 
- HTTPの高速化技術。
- 2015/5に[RFC 7540](https://datatracker.ietf.org/doc/html/rfc7540)として文書化。
- [普及率は 2021/1 で 50%を超えた](https://news.mynavi.jp/article/20210112-1635821/)
- 主要なWebブラウザ、Webサーバーですでにサポートされている。
- 参考: IETFでは[HTTP/3](https://ja.wikipedia.org/wiki/HTTP/3)を開発中。


■サブネット

- Application Gateway専用のサブネットが必要
  - 名称: 任意
  - Application Gatewayと、その他のリソース（VMのNIC等）は同居できない
  - 1サブネットに複数のApplication Gatewayをデプロイすることは可能
    - [1つのサブネットにv1とv2のSKUを含めることはできない](https://docs.microsoft.com/ja-jp/azure/application-gateway/application-gateway-faq#1-------------application-gateway---------------)

■Application Gatewayのコンポーネント

```
            フロントエンドIP 
            ↓
            リスナー(HTTP, 80)
            ↓
            ルーティング規則
            ↓            ↓ 
バックエンドターゲット     バックエンドターゲット
└HTTP設定(HTTP, 80, /)    └HTTP設定(HTTP, 80, /)
  ↓                         ↓
 バックエンドプール         バックエンドプール
  ↓                         ↓
バックエンド                バックエンド 
```
