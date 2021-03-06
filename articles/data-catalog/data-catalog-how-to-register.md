---
title: "データ ソースの登録方法 | Microsoft Docs"
description: "この記事では、Azure Data Catalog にデータ ソースを登録する方法について、登録中に抽出されるメタデータ フィールドを含め重点的に説明しています。"
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: bab89906-186f-4d35-9ffd-61b1d903905d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/23/2017
ms.author: maroche
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 25c2b279487f099a0e688621e63faaa4ed265e6e


---
# <a name="how-to-register-data-sources"></a>データ ソースの登録方法
## <a name="introduction"></a>はじめに
**Microsoft Azure Data Catalog** は、完全に管理されたクラウド サービスであり、エンタープライズ データ ソースの登録のシステムと検出のシステムとして機能します。 つまり、 **Azure Data Catalog** を使用すると、ユーザーはデータ ソースを検出、理解、使用でき、組織は既存のデータからより多くの価値を引き出すことができます。 **Azure Data Catalog** でデータ ソースを検出できるようにするための最初のステップは、そのデータ ソースを登録することです。

## <a name="registering-data-sources"></a>データ ソースの登録
登録は、メタデータをデータ ソースから抽出し、そのデータを **Azure Data Catalog** サービスにコピーするプロセスです。 データは現在存在する場所に残り、現在のシステムの管理者とポリシーの制御下に留まります。

データ ソースを登録するには、**Azure Data Catalog** ポータルから **Azure Data Catalog** データ ソース登録ツールを起動するだけです。 職場または学校アカウント (ポータルへのログインに使用するのと同じ Azure Active Directory 資格情報) でログインし、登録するデータ ソースを選択します。
詳細な手順については、「 [Azure Data Catalog の概要](data-catalog-get-started.md) 」チュートリアルをご覧ください。

データ ソースが登録されると、Catalog はその場所を追跡し、メタデータのインデックスを作成して、ユーザーがデータ ソースを検索、参照、検出し、その場所を使用して任意のアプリケーションまたはツールで接続できるようにします。

## <a name="sources-supported"></a>サポートされているソース
現在サポートされているデータ ソースの一覧については、「 [データ カタログ DSR](data-catalog-dsr.md) 」を参照してください。
<br/>

## <a name="structural-metadata"></a>構造メタデータ
データ ソースを登録するとき、登録ツールは選択されたオブジェクトの構造に関する情報を抽出します。これは、構造メタデータと呼ばれます。

すべてのオブジェクトについて、この構造メタデータには、データを検出するユーザーがその情報を使用して、任意のクライアント ツールのオブジェクトに接続できるように、オブジェクトの場所が含まれます。 他の構造メタデータとしては、オブジェクトの名前と型、属性/列の名前、データの型などがあります。

## <a name="descriptive-metadata"></a>記述メタデータ
データ ソースから抽出されるコア構造メタデータに加えて、データ ソース登録ツールは記述メタデータも抽出します。 SQL Server Analysis Services および SQL Server Reporting Services の場合、これはこれらのサービスによって公開される Description プロパティから取得され+ます。 SQL Server の場合は、ms_description 拡張プロパティを使用して提供される値が抽出されます。 Oracle データベースでは、データ ソース登録ツールは ALL_TAB_COMMENTS ビューから COMMENTS 列を抽出します。

データ ソースから抽出される記述メタデータに加えて、ユーザーはデータ ソース登録ツールを使用して記述メタデータを入力することもできます。 ユーザーはタグを追加でき、登録されているオブジェクトの専門家を識別できます。 この記述メタデータはすべて、構造メタデータと共に **Azure Data Catalog** サービスにコピーされます。

## <a name="including-previews"></a>プレビューの組み込み
既定では、メタデータのみがデータ ソースから抽出されて **Azure Data Catalog** サービスにコピーされますが、含まれるデータのサンプルを見る方がデータ ソースを簡単に理解できることがよくあります。

**Azure Data Catalog** データ ソース登録ツールでは、ユーザーは登録される各テーブルおよびビューにデータのスナップショット プレビューを組み込むことができます。 ユーザーが登録の間にプレビューの組み込みを選択した場合、登録ツールは各テーブルおよびビューから最大 20 個のレコードを組み込みます。 このスナップショットは、構造メタデータおよび記述メタデータと共に Catalog にコピーされます。

> [!NOTE]
> 多くの列を含む幅の広いテーブルでは、プレビューに組み込まれるレコードが 20 より少ない場合があります。
>
>

## <a name="including-data-profiles"></a>データ プロファイルの組み込み
プレビューを組み込むことで、 **Azure Data Catalog**内のデータ ソースを検索するユーザーに貴重なコンテキストを提供できるのと同じように、データ プロファイルを取り込むことで、検出されたデータ ソースをより簡単に理解できるようにすることができます。

**Azure Data Catalog** データ ソース登録ツールでは、ユーザーは登録されたテーブルおよびビューごとにデータ プロファイルを組み込むことができます。 ユーザーが登録時にデータ プロファイルの取り込みを選択すると、登録ツールは各テーブルとビューのデータに関する次のような集計情報を組み込みます。

* オブジェクト内の行数とデータのサイズ
* データとオブジェクト スキーマに対して最新の更新が行われた日付
* 列における null レコードの数と重複しない値の数
* 列の最小値、最大値、平均値、および標準偏差値

これらの統計は、構造メタデータおよび記述メタデータと共に Catalog にコピーされます。

> [!NOTE]
> テキスト列と日付列には、該当するデータ プロファイル内の平均または標準偏差の統計情報は取り込まれません。
>
>

## <a name="updating-registrations"></a>登録の更新
データ ソースを登録すると、登録の間に抽出されるメタデータおよびオプションのプレビューを使用して、 **Azure Data Catalog** でデータ ソースを検出できるようになります。 Catalog のデータ ソースを更新する必要がある場合は (たとえば、オブジェクトのスキーマが変更された場合、もともと除外されていたテーブルを含める必要がある場合、ユーザーがプレビューに含まれるデータを更新したい場合など)、データ ソース登録ツールを再実行できます。

既に登録されているデータ ソースを再登録すると、マージ "upsert" 操作が実行されます。既存のオブジェクトは更新され、新しいオブジェクトは作成されます。 ユーザーが **Azure Data Catalog** ポータルで提供したメタデータはすべて維持されます。

## <a name="summary"></a>概要
**Azure Data Catalog** でデータ ソースを登録すると、構造メタデータと記述メタデータがデータ ソースから Catalog サービスにコピーされることによって、データ ソースの検出と理解が容易になります。 登録されたデータ ソースは、 **Azure Data Catalog** ポータルを使用して注釈付け、管理、および検出することができます。

## <a name="see-also"></a>関連項目
* [Azure Data Catalog の概要](data-catalog-get-started.md) 」チュートリアルをご覧ください。



<!--HONumber=Nov16_HO3-->


