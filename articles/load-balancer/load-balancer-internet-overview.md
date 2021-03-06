
---
title: インターネットに接続するロード バランサーの概要 | Microsoft Docs
description: インターネットに接続するロード バランサーとその機能の概要ロード バランサーが仮想マシンとクラウド サービスを使用して Azure で機能する方法
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: tysonn

ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2016
ms.author: sewhee

---
# インターネットに接続するロード バランサーの概要
Azure Load Balancer は、着信トラフィックのパブリック IP アドレスとポート番号を仮想マシンのプライベート IP アドレスとポート番号にマップし、仮想マシンからの応答トラフィックはその逆にマップします。負荷分散規則を使用すると、複数の仮想マシンまたはサービスに特定の種類のトラフィックを分散させることができます。たとえば、複数の Web サーバーまたは Web ロール間で Web 要求のトラフィックの負荷を分散できます。

> [!NOTE]
> Azure Load Balancer は、既定の設定を使用して、複数の仮想マシン インスタンス間でハッシュ分散ネットワーク トラフィックを提供します (ハッシュ分散の詳細については、「[ロード バランサーの機能](load-balancer-overview.md#load-balancer-features)」をご覧ください)。セッション アフィニティの詳細については、「[ロード バランサー分散モード](load-balancer-distribution-mode.md)」をご覧ください。
> 
> 

クラウド サービスに Web ロールまたは worker ロールのインスタンスが含まれている場合、サービス定義 (.csdef) ファイルでパブリック エンドポイントを定義できます。

Servicedefinition.csdef ファイルにはエンドポイント構成が含まれ、Web ロールまたは Worker ロールのデプロイメント用の複数のロール インスタンスがある場合は、ロード バランサーをセットアップできます。インスタンスをクラウド デプロイメントに追加するには、サービス構成ファイル (.csfg) でインスタンス数を変更します。

次の図は、暗号化された Web トラフィック用の負荷分散されたエンドポイントを示しています。このエンドポイントは、パブリックとプライベートの TCP ポートが 443 である 3 台の仮想マシン間で共有されています。この 3 台の仮想マシンは、1 つの負荷分散セット内にあります。

![パブリックのロード バランサーの例](./media/load-balancer-internet-overview/IC727496.png))

複数のインターネット クライアントが TCP ポート 443 でクラウド サービスのパブリック IP アドレスに Web ページ要求を送信すると、Azure Load Balancer は負荷分散セット内の 3 台の仮想マシンにこれらの要求を分散します。ロード バランサーのアルゴリズムの詳細については、[ロード バランサーの概要に関するページ](load-balancer-overview.md#load-balancer-features)を参照してください。

## 次のステップ
インターネットに接続するロード バランサーについて学習したら、[内部ロード バランサー](load-balancer-internal-overview.md)に関する記事を読み、クラウド デプロイメントに適したロード バランサーについて理解を深めます。

[インターネットに接続するロード バランサーの作成を開始](load-balancer-get-started-internet-arm-ps.md)し、特定のロード バランサーのネットワーク トラフィックの動作に適した種類の[分散モード](load-balancer-distribution-mode.md)を構成することもできます。

アプリケーションでロード バランサーの背後にあるサーバーの接続を維持する必要がある場合は、[ロード バランサーの TCP アイドル タイムアウトの設定](load-balancer-tcp-idle-timeout.md)に関する詳細を確認してください。Azure Load Balancer 使用時にアイドル接続動作について理解するのに役立ちます。

<!---HONumber=AcomDC_0831_2016-->