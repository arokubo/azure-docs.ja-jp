---
title: "Web サイトのログ分析のための Hadoop での Hive の使用| Microsoft Docs"
description: "Web サイトのログを分析するために HDInsight で Hive を使用する方法を説明します。 HDInsight テーブルへの入力にログ ファイルを使用し、HiveQL を使用してデータを照会します。"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6fb7b5c2-8df4-40b1-a9e2-6815080004f9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7038ba5e4229f65125efffb1d403364fc36a3783


---
# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>Web サイトのログを分析するための HDInsight での Hive の使用
Web サイトのログを分析するために HDInsight で HiveQL を使用する方法を説明します。 Web サイトのログ分析は、類似するアクティビティに基づく対象ユーザーの区分、人口統計によるサイト訪問者の分類、参照されたコンテンツや訪問元の Web サイトの確認などのために使用できます。

このサンプルでは、HDInsight クラスターを使用して Web サイトのログ ファイルを分析することにより、1 日の間に発生した外部 Web サイトからの Web サイトへのアクセス数を調べます。 また、発生した Web サイト エラーの概要を生成します。 学習内容:

* Web サイトのログ ファイルが含まれている Azure Blob ストレージへの接続
* これらのログを照会するための HIVE テーブルの作成
* データを分析するための HIVE クエリの作成
* 分析したデータを取得するための、Microsoft Excel を使用した HDInsight への接続 (Open Database Connectivity (ODBC) を使用)

![HDI.Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a>前提条件
* Azure HDInsight 上で Hadoop クラスターをプロビジョニングしておく必要があります。 手順については、「[HDInsight での Hadoop クラスターのプロビジョニング][hdinsight-provision]」をご覧ください。 
* Microsoft Excel 2013 または Microsoft Excel 2010 がインストールされていること。
* Hive から Excel にデータをインポートするための [Microsoft Hive ODBC ドライバー](http://www.microsoft.com/download/details.aspx?id=40886) があること。

## <a name="to-run-the-sample"></a>サンプルを実行するには
1. [Azure ポータル](https://portal.azure.com/)のスタート画面 (ここにクラスターをピン留めした場合) で、サンプルを実行するクラスターのタイルをクリックします。
2. クラスターのブレードの **[クイック リンク]** で **[クラスター ダッシュボード]** をクリックし、**[クラスター ダッシュボード]** ブレードで **[HDInsight クラスター ダッシュボード]** をクリックします。 または、次に示す URL を使用してダッシュボードを直接開くこともできます。
   
         https://<clustername>.azurehdinsight.net
   
    入力を要求されたら、クラスターをプロビジョニングするときに使用した管理者名とパスワードを使用して認証します。
3. 表示された Web ページの **[概要ギャラリー]** タブをクリックし、**[サンプル データを使用したソリューション]** カテゴリにある **[Web サイトのログの分析]** サンプルをクリックします。
4. Web ページに記載されている手順に従って、サンプルを完了します。

## <a name="next-steps"></a>次のステップ
「 [Hive を HDInsight と共に使用したセンサー データの分析](hdinsight-hive-analyze-sensor-data.md)」のサンプル手順に従ってください。

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png




<!--HONumber=Nov16_HO3-->


