---
title: "Logic Apps の価格モデル | Microsoft Docs"
description: "Logic Apps の料金体系について詳しく取り上げます。"
author: kevinlam1
manager: dwrede
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: klam
translationtype: Human Translation
ms.sourcegitcommit: ce26e2c3de589b6345cabd4394d999e2ba93cd8d
ms.openlocfilehash: 3295cadf31fb0304dee0f17f96756df71904be35

---
# <a name="logic-apps-pricing-model"></a>Logic Apps の料金モデル
Azure Logic Apps を使用すると、クラウドにおける統合ワークフローをスケーリングして実行することができます。  以下、Logic Apps の課金と料金プランについて詳しく取り上げます。
## <a name="consumption-pricing"></a>従量課金
新しく作成された Logic Apps には、従量制の料金プランが使用されます。 Logic Apps の従量課金モデルでは、お客様が使用した分のみが課金の対象となります。  従量制の料金プランを使用している Logic Apps はスロットルされません。
ロジック アプリのインスタンス実行中に行われたすべてのアクションが課金の対象となります。
### <a name="what-are-action-executions"></a>アクションの実行とは
ロジック アプリ定義内のすべてのステップはアクションです。これには、トリガーや制御フローのステップ (条件、スコープ、for each ループ、do until ループなど)、コネクタの呼び出し、ネイティブ アクションの呼び出しが含まれます。
トリガーは、特定のイベントが発生したときにロジック アプリの新しいインスタンスを作成する特殊なアクションと考えることができます。  トリガーには複数の動作が存在し、それによってロジック アプリの課金方法が変わる場合があります。
* **ポーリング トリガー** – ロジック アプリのインスタンスを作成するうえでの基準を満たしたメッセージを受け取るまで絶えずエンドポイントをポーリングするトリガーです。  ポーリング間隔は、Logic Apps デザイナーでトリガーの設定を変えることで調整できます。  ロジック アプリのインスタンスが作成されたかどうかに関係なく、個々のポーリング要求がアクションの実行としてカウントされます。
* **Webhook トリガー** – クライアントから送信される要求を特定のエンドポイントで待機するトリガーです。  Webhook のエンドポイントに送信された個々の要求がアクションの実行としてカウントされます。 Request と HTTP Webhook トリガーはどちらも Webhook トリガーです。
* **定期実行のトリガー** – トリガーに対して構成された繰り返しの間隔に基づいてロジック アプリのインスタンスを作成するトリガーです。  たとえば、3 日に 1 回や 1 分間に 1 回の頻度で実行するように定期実行のトリガーを構成することができます。
トリガーの実行は、Logic Apps のリソース ブレードの [トリガーの履歴] 領域で確認できます。
実行されたすべてのアクションは、成功か失敗かに関係なく、アクションの実行として課金されます。  条件が満たされなかったためにスキップされたアクションや、ロジック アプリが完了前に強制終了されたために実行されなかったアクションは、アクションの実行としてカウントされません。

ループ内で実行されたアクションは、その繰り返しごとにカウントされます。  たとえば、10 個の要素から成るリストを反復処理する for each ループに 1 つのアクションがあるとします。この場合、アクションの実行数は、リストの要素数 (10) にループ内のアクション数 (1) を掛け、ループの開始分に相当する 1 を足した値として計算されます。この例では (10 * 1) + 1 = 11 がアクションの実行数となります。
無効にされた Logic Apps は、新しいインスタンスを作成できないため、無効にされている間は、課金の対象になりません。  ロジック アプリを無効化した後、そのインスタンスが休止状態となって完全に無効になるまでには、少し時間がかかるので注意してください。
### <a name="integration-account-usage"></a>統合アカウントの使用
従量制のプランには、調査、開発、およびテスト目的の[統合アカウント](https://docs.microsoft.com/en-us/azure/app-service-logic/app-service-logic-enterprise-integration-create-integration-account?toc=%2fazure%2flogic-apps%2ftoc.json)が含まれます。このアカウントでは、Logic Apps の[B2B/EDI](https://docs.microsoft.com/en-us/azure/app-service-logic/app-service-logic-enterprise-integration-b2b?toc=%2fazure%2flogic-apps%2ftoc.json) 機能や [XML 処理](https://docs.microsoft.com/en-us/azure/app-service-logic/app-service-logic-enterprise-integration-xml?toc=%2fazure%2flogic-apps%2ftoc.json)機能を追加コストなしで利用できます。 リージョンあたり最大 1 アカウントを作成でき、最大 10 件の契約と 25 個のマップを格納できます。 スキーマ、証明書、およびパートナーには制限がなく、必要な数だけアップロードできます。

従量制プランに含まれている統合アカウントに加えて、標準の統合アカウントを作成することもできます。このアカウントには上記の制限がなく、標準の Logic Apps SLA が適用されます。 詳しくは、[こちら](https://azure.microsoft.com/pricing/details/logic-apps)をご覧ください。
## <a name="app-service-plans"></a>App Service プラン
以前に作成したロジック アプリのうち、App Service プランを参照しているものは、今後もこれまでと同様に動作します。 これらのアプリは、選択したプランに応じて既定された 1 日あたりの実行数を過えると制限されますが、料金はアクション実行メーターを使用して課金されます。
サブスクリプションに App Service プランが含まれている場合 (ロジック アプリに明示的に関連付けられている必要はありません)、その EA カスタマーには使用量特典が提供されます。  たとえば、EA サブスクリプションに Standard App Service プランが含まれていて、同じサブスクリプションにロジック アプリが含まれている場合、1 日あたりのアクション実行が 10,000 回を超えるまでは、料金は課金されません(以下の表をご覧ください)。 

App Service プランと 1 日に許可されているアクションの実行数:
|  | Free/Shared/Basic | 標準 | プレミアム |
| --- | --- | --- | --- |
| 1 日あたりのアクションの実行数 |200 |10,000 |50,000 |
### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>App Service プランから従量課金への変換
App Service プランが関連付けられているロジック アプリを従量制の課金モデルに変更するには、ロジック アプリの定義から App Service プランの参照を削除します。  この変更は PowerShell コマンドレットで実行できます。次のコマンドレットを呼び出してください: `Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`
## <a name="pricing"></a>価格
料金の詳細については、「[Logic Apps の価格](https://azure.microsoft.com/pricing/details/logic-apps/)」を参照してください。
## <a name="next-steps"></a>次のステップ
* [Logic Apps の概要][whatis]
* [Create your first logic app (最初のロジック アプリの作成)][create] [pricing]: https://azure.microsoft.com/pricing/details/logic-apps/ [whatis]: app-service-logic-what-are-logic-apps.md [create]: app-service-logic-create-a-logic-app.md




<!--HONumber=Dec16_HO3-->


