---
title: "Azure API Management ポリシー リファレンス"
description: "API Management の構成に使用できるポリシーについて説明します。"
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 9d4ac609-b221-4032-93ae-e27a1254d43d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 97a6f253eda39d0573a2384691a6b294c3894c25


---
# <a name="azure-api-management-policy-reference"></a>Azure API Management ポリシー リファレンス
このセクションでは、「[API Management ポリシー リファレンス][API Management ポリシー リファレンス]」で説明されているポリシーのインデックスを示します。 ポリシーを追加および構成する方法については、「[Azure API Management のポリシー][Azure API Management のポリシー]」を参照してください。

ポリシーの式は、ポリシーで特に指定されていない限り、任意の API Management ポリシーで属性値またはテキスト値として使用できます。 [制御フロー][制御フロー]ポリシーや[変数の設定][変数の設定]ポリシーなど、一部のポリシーはポリシーの式に基づいています。 詳細については、[高度なポリシー][高度なポリシー]、および[ポリシーの式][ポリシーの式]に関する記事をご覧ください。

## <a name="policy-reference-index"></a>ポリシー リファレンスのインデックス
* [アクセス制限ポリシー][アクセス制限ポリシー]
  * [HTTP ヘッダーを確認する][HTTP ヘッダーを確認する] - HTTP ヘッダーの存在と値の両方、またはそのどちらかを適用します。
  * [呼び出しレートをサブスクリプション別に制限する][呼び出しレートをサブスクリプション別に制限する] - 呼び出しレートをサブスクリプションに基づいて制限することで、API の使用量の急増を防ぎます。
  * [呼び出しレートをキー別に制限する](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - 呼び出しレートをキーに基づいて制限することで、API の使用量の急増を防ぎます。
  * [呼び出し元 IP を制限する][呼び出し元 IP を制限する] - 特定の IP アドレスとアドレス範囲の両方、またはそのどちらかからの呼び出しをフィルター処理 (許可/拒否) します。
  * [使用量のクォータをサブスクリプション別に設定する][使用量のクォータをサブスクリプション別に設定する] - 更新可能な呼び出しまたは有効期間中の呼び出しのボリュームと帯域幅クォータの両方またはそのどちらかをサブスクリプションに基づいて適用できます。
  * [使用量のクォータをキー別に設定する](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - 更新可能な呼び出しまたは有効期間中の呼び出しのボリュームと帯域幅クォータの両方またはそのどちらかをキーに基づいて適用できます。
  * [JWT を検証する][JWT を検証する] - 指定された HTTP ヘッダーまたは指定されたクエリ パラメーターから抽出した JWT の存在と有効性を適用します。
* [高度なポリシー][高度なポリシー]
  * [制御フロー][制御フロー] - ブール[式][式]の評価の結果に基づいてポリシー ステートメントを条件付きで適用します。
  * [要求を転送する][要求を転送する] - バックエンド サービスに要求を転送します。
  * [Event Hub にログを記録する][Event Hub にログを記録する] - [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) エンティティによって定義されたメッセージ ターゲットに、指定された形式でメッセージを送信します。
  * [再試行](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - 条件が満たされるまで、囲まれたポリシー ステートメントの実行を再試行します。 実行は、指定された間隔で、指定された最大試行回数まで繰り返されます。
  * [応答を返す](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - パイプラインの実行を中止し、指定された応答を呼び出し元に直接返します。
  * [1 方向の要求を送信する](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - 指定された URL に要求を送信します。応答は待機しません。
  * [要求を送信する](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - 指定された URL に要求を送信します。
  * [要求メソッドを設定する](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - 要求の HTTP メソッドを変更できます。
  * [状態コードを設定する](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - HTTP 状態コードを指定された値に変更します。
  * [変数の設定][変数の設定] - 名前付き[コンテキスト][コンテキスト]変数の値を、後でアクセスできるように保持します。
  * [トレース](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - [API Inspector](api-management-howto-api-inspector.md) の出力に文字列を追加します。
  * [待機](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - 含まれている要求の送信、キャッシュからの値の取得、または制御フロー ポリシーが完了するまで待機してから次に進みます。
* [認証ポリシー][認証ポリシー]
  * [基本認証][基本認証] -基本認証を使用してバックエンド サービスで認証します。
  * [クライアント証明書による認証][クライアント証明書による認証] -クライアント証明書を使用してバックエンド サービスで認証します。
* [キャッシュ ポリシー][キャッシュ ポリシー] 
  * [キャッシュから取得][キャッシュから取得] - キャッシュを検索して、キャッシュに格納された有効な応答があればそれを返します。
  * [キャッシュに格納][キャッシュに格納] - 指定されたキャッシュ制御の構成に従って応答をキャッシュに格納します。
  * [キャッシュから値を取得](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - キャッシュされたキー別の項目を取得します。
  * [値をキャッシュに格納](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - 項目をキー別にキャッシュに格納します。
  * [キャッシュから値を削除](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - キー別にキャッシュ内の項目を削除します。
* [クロス ドメイン ポリシー][クロス ドメイン ポリシー] 
  * [クロスドメイン呼び出しを許可][クロスドメイン呼び出しを許可] - Adobe Flash や Microsoft Silverlight ブラウザーベースのクライアントから API を利用できるようにします。
  * [CORS][CORS] - クロス オリジン リソース共有 (CORS) のサポートを操作または API に追加して、ブラウザーベースのクライアントからのクロス ドメイン呼び出しを可能にします。
  * [JSONP][JSONP] - JSON with padding (JSONP) のサポートを操作または API に追加して、JavaScript ブラウザーベースのクライアントからのクロス ドメイン呼び出しを可能にします。
* [変換ポリシー][変換ポリシー] 
  * [JSON から XML への変換][JSON から XML への変換] - 要求本文または応答本文を JSON から XML に変換します。
  * [XML から JSON への変換][XML から JSON への変換] - 要求本文または応答本文を XML から JSON に変換します。
  * [本文内の文字列の検索および置換][本文内の文字列の検索および置換] - 要求または応答の部分文字列を検索して、別の部分文字列に置き換えます。
  * [コンテンツ内の URL のマスク][コンテンツ内の URL のマスク] - ゲートウェイを経由して同じリンクをポイントするよう、応答本文のリンクを書き換えます (マスクします)。
  * [バックエンド サービスの設定][バックエンド サービスの設定] -受信要求のバックエンド サービスを変更します。
  * [本文の設定][本文の設定] - 着信要求と発信要求のメッセージ本文を設定します。
  * [HTTP ヘッダーの設定][HTTP ヘッダーの設定] - 既存の応答および/または要求ヘッダーに値を割り当てるか、または新しい応答および/または要求ヘッダーを追加します。
  * [クエリ文字列パラメーターの設定][クエリ文字列パラメーターの設定] - 要求クエリ文字列パラメーターの追加、値の置換、または削除を行います。
  * [URL の書き換え][URL の書き換え] - 要求 URL をパブリックな形式から Web サービスで想定されている形式に変換します。

## <a name="next-steps"></a>次のステップ
ポリシー式に関する詳細については、次のビデオをご覧ください。

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[アクセス制限ポリシー]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[HTTP ヘッダーを確認する]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[呼び出しレートをサブスクリプション別に制限する]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[呼び出し元 IP を制限する]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[使用量のクォータをサブスクリプション別に設定する]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[JWT を検証する]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[高度なポリシー]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[制御フロー]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[変数の設定]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[式]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[コンテキスト]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[要求を転送する]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Event Hub にログを記録する]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[認証ポリシー]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[基本認証]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[クライアント証明書による認証]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[キャッシュ ポリシー]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[キャッシュから取得]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[キャッシュに格納]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[クロス ドメイン ポリシー]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[クロスドメイン呼び出しを許可]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[変換ポリシー]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[JSON から XML への変換]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[XML から JSON への変換]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[本文内の文字列の検索および置換]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[コンテンツ内の URL のマスク]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[バックエンド サービスの設定]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[本文の設定]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[HTTP ヘッダーの設定]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[クエリ文字列パラメーターの設定]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[URL の書き換え]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Azure API Management のポリシー]: api-management-howto-policies.md
[API Management ポリシー リファレンス]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[ポリシーの式]: https://msdn.microsoft.com/library/azure/dn910913.aspx





<!--HONumber=Nov16_HO3-->


