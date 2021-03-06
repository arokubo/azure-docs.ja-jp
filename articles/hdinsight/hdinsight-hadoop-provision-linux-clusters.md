---
title: "HDInsight での Hadoop、HBase、Storm、または Spark クラスター (Linux ベース) の作成 | Microsoft Docs"
description: "ブラウザー、Azure CLI、Azure PowerShell、REST を使用するか、SDK 経由で HDInsight に Linux ベースの Hadoop、HBase、Storm、または Spark クラスターを作成する方法について説明します。"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 23a01938-3fe5-4e2e-8e8b-3368e1bbe2ca
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/18/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 8b9a3b853adcbdfd2a5d4113489e8fcad26d9f59


---
# <a name="create-linux-based-hadoop-clusters-in-hdinsight"></a>HDInsight での Linux ベースの Hadoop クラスターの作成
[!INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Hadoop クラスターは、クラスターでのタスクの分散処理に使用される複数の仮想マシン (ノード) で構成されます。 Azure では個々のノードのインストールと構成の実装の詳細を抽象化しているため、提供する必要があるのは一般的な構成情報のみとなります。 この記事では、これらの構成設定について説明します。

## <a name="access-control-requirements"></a>アクセス制御の要件
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="cluster-types"></a>クラスターの種類
現在、Azure HDInsight では、それぞれ特定の機能を提供する一連のコンポーネントを備えた 5 種類のクラスターを提供しています。

| クラスターの種類 | 機能 |
| --- | --- |
| Hadoop は、 |クエリと分析 (バッチ ジョブ) |
| HBase |NoSQL データ ストレージ |
| Storm |リアルタイム イベント処理 |
| Spark |メモリ内処理、対話型クエリ、マイクロバッチ ストリーム処理 |
| [対話型 Hive (プレビュー)](hdinsight-hadoop-use-interactive-hive.md) |対話型で高速な Hive クエリのメモリ内キャッシュ |
| R Server on Spark (プレビュー) |さまざまなビッグ データ統計、予測モデリング、機械学習の機能 |

各クラスターの種類には、クラスター内の独自のノード数、クラスター内のノードを表す用語、およびノードの種類ごとに既定の VM サイズがあります。 次の表では、各ノードの種類のノード数がかっこ内に示されています。

| 型 | Nodes | ダイアグラム |
| --- | --- | --- |
| Hadoop は、 |ヘッド ノード (2)、データ ノード (1 以上) |![HDInsight Hadoop クラスター ノード](./media/hdinsight-provision-clusters/HDInsight.Hadoop.roles.png) |
| HBase |ヘッド サーバー (2)、リージョン サーバー (1 以上)、マスター/Zookeeper ノード (3) |![HDInsight HBase クラスター ノード](./media/hdinsight-provision-clusters/HDInsight.HBase.roles.png) |
| Storm |Nimbus ノード (2)、Supervisor サーバー (1 以上)、Zookeeper ノード (3) |![HDInsight Storm クラスター ノード](./media/hdinsight-provision-clusters/HDInsight.Storm.roles.png) |
| Spark |ヘッド ノード (2)、ワーカー ノード (1 以上)、Zookeeper ノード (3) (A1 Zookeeper VM サイズでは無料) |![HDInsight Spark クラスター ノード](./media/hdinsight-provision-clusters/HDInsight.Spark.roles.png) |

次の表に、HDInsight の既定の VM サイズを示します。

* ブラジル南部と西日本を除くすべてのサポートされているリージョン:
  
  | クラスターの種類 | Hadoop は、 | HBase | Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- |
  | ヘッド - 既定の VM サイズ |D3 v2 |D3 v2 |A3 |D12 v2 |D12 v2 |
  | ヘッド - 推奨される VM サイズ |D3 v2、D4 v2、D12 v2 |D3 v2、D4 v2、D12 v2 |A3、A4、A5 |D12 v2、D13 v2、D14 v2 |D12 v2、D13 v2、D14 v2 |
  | worker – 既定の VM サイズ |D3 v2 |D3 v2 |D3 v2 |Windows: D12 v2、Linux: D4 v2 |Windows: D12 v2、Linux: D4 v2 |
  | worker - 推奨される VM サイズ |D3 v2、D4 v2、D12 v2 |D3 v2、D4 v2、D12 v2 |D3 v2、D4 v2、D12 v2 |Windows: D12 v2、D13 v2、D14 v2、Linux: D4 v2、D12 v2、D13 v2、D14 v2 |Windows: D12 v2、D13 v2、D14 v2、Linux: D4 v2、D12 v2、D13 v2、D14 v2 |
  | Zookeeper - 既定の VM サイズ | |A3 |A2 | | |
  | Zookeeper - 推奨される VM サイズ | |A3、A4、A5 |A2、A3、A4 | | |
  | エッジ - 既定の VM サイズ | | | | |Windows: D12 v2、Linux: D4 v2 |
  | エッジ - 推奨される VM サイズ | | | | |Windows: D12 v2、D13 v2、D14 v2、Linux: D4 v2、D12 v2、D13 v2、D14 v2 |
* ブラジル南部と西日本のみ (v2 サイズはありません):
  
  | クラスターの種類 | Hadoop は、 | HBase | Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- |
  | ヘッド - 既定の VM サイズ |D3 |D3 |A3 |D12 |D12 |
  | ヘッド - 推奨される VM サイズ |D3、D4、D12 |D3、D4、D12 |A3、A4、A5 |D12、D13、D14 |D12、D13、D14 |
  | worker – 既定の VM サイズ |D3 |D3 |D3 |Windows: D12、Linux: D4 |Windows: D12、Linux: D4 |
  | worker - 推奨される VM サイズ |D3、D4、D12 |D3、D4、D12 |D3、D4、D12 |Windows: D12、D13、D14、Linux: D4、D12、D13、D14 |Windows: D12、D13、D14、Linux: D4、D12、D13、D14 |
  | Zookeeper - 既定の VM サイズ | |A2 |A2 | | |
  | Zookeeper - 推奨される VM サイズ | |A2、A3、A4 |A2、A3、A4 | | |
  | エッジ - 既定の VM サイズ | | | | |Windows: D12、Linux: D4 |
  | エッジ - 推推奨される VM サイズ | | | | |Windows: D12、D13、D14、Linux: D4、D12、D13、D14 |

ヘッドは、Storm クラスターでは "Nimbus ** " と呼ばれます。 worker は、HBase クラスターでは *"リージョン"*、Storm クラスターでは *"スーパーバイザー"* と呼ばれます。

> [!IMPORTANT]
> クラスターの作成時または作成後のスケーリングで 32 個を超える worker ノードを使用することを予定している場合は、コア数が 8 個以上、RAM が 14 GB 以上のヘッド ノード サイズを選択する必要があります。
> 
> 

[スクリプト アクション](#customize-clusters-using-script-action)を使用して、Hue や R などの他のコンポーネントをこれらの基本的な種類に追加することができます。

> [!IMPORTANT]
> HDInsight クラスターには、ワークロード、またはクラスターが調整されるテクノロジに対応するさまざまな型があります。 1 つのクラスターの Storm と HBase など、複数の種類を結合するクラスターを作成するメソッドはサポートされていません。 
> 
> 

ソリューションに複数の HDInsight クラスターの種類に分散されたテクノロジが必要な場合は、Azure Virtual Network を作成し、仮想ネットワーク内で必要なクラスターの種類を作成する必要があります。 これにより、クラスターと、それにデプロイするすべてのコードが互いに通信できるようになります。

Azure Virtual Network の HDInsight との併用について詳しくは「[Azure Virtual Network を使用した HDInsight 機能の拡張](hdinsight-extend-hadoop-virtual-network.md)」をご覧ください。

Azure Virtual Network 内で 2 つのクラスターの種類を使用した例については、「[Storm と HBase を使用したセンサー データの分析](hdinsight-storm-sensor-data-analysis.md)」をご覧ください。

## <a name="cluster-tiers"></a>クラスター レベル
Azure HDInsight では、Standard と [Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)の 2 つのカテゴリでビッグ データ クラウド サービスを提供します。 HDInsight Premium には、R とその他の追加コンポーネントが含まれています。 HDInsight Premium は、HDInsight バージョン 3.4 でのみサポートされます。

次の表に、HDInsight クラスターの種類と HDInsight Premium のサポート状況を示します。

| クラスターの種類 | Standard | Premium |
| --- | --- | --- |
| Hadoop は、 |あり |あり |
| Spark |あり |あり |
| HBase |あり |いいえ |
| Storm |あり |いいえ |
| R Server on Spark |いいえ |あり |

HDInsight Premium でサポートされるクラスターの種類が追加されたら、それに応じてこの表も更新されます。 次のスクリーンショットは、クラスターの種類を選択するための Azure ポータルの情報を示しています。

![HDInsight Premium の構成](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)

## <a name="basic-configuration-options"></a>基本的な構成オプション
HDInsight クラスターの作成に使用する基本的な構成オプションを以下に示します。

### <a name="cluster-name"></a>クラスター名
クラスター名は、クラスターの識別に使用します。 クラスター名はグローバルに一意である必要があり、次の名前付けのガイドラインに従う必要があります。

* このフィールドには、3 ～ 63 文字の文字列を指定する必要があります。
* このフィールドには、文字、数字、ハイフンのみを含めることができます。

### <a name="cluster-type"></a>クラスターの種類
「[クラスターの種類](#cluster-types)」と「[クラスター レベル](#cluster-tiers)」をご覧ください。

### <a name="operating-system"></a>オペレーティング システム
次の 2 つのオペレーティング システムのいずれかで HDInsight クラスターを作成できます。

* Linux での HDInsight  HDInsight には、Azure で Linux クラスターを構成するためのオプションが用意されています。 Linux または Unix に詳しい場合や、Linux 向けに構築された Hadoop エコシステム コンポーネントとの簡単な統合が必要な場合は、既存の Linux ベースの Hadoop ソリューションから移行することで Linux クラスターを構成します。 詳細については、「 [Get started with Hadoop on Linux in HDInsight (HDInsight の Linux での Hadoop の使用)](hdinsight-hadoop-linux-tutorial-get-started.md)」をご覧ください。
* Windows 上の HDInsight (Windows Server 2012 R2 Datacenter)。

### <a name="hdinsight-version"></a>HDInsight のバージョン
このクラスターに必要な HDInsight のバージョンを指定します。 詳細については、 [HDInsight での Hadoop クラスターのバージョンとコンポーネント](https://go.microsoft.com/fwLink/?LinkID=320896&clcid=0x409)に関する記事をご覧ください。

### <a name="subscription-name"></a>サブスクリプション名
各 HDInsight クラスターは、1 つの Azure サブスクリプションに関連付けられます。

### <a name="resource-group-name"></a>リソース グループ名
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) を使用すると、アプリケーション内の複数のリソースを、Azure リソース グループと呼ばれる 1 つのグループとして使用できます。 アプリケーションのこれらのすべてのリソースを、1 回の連携した操作でデプロイ、更新、監視、または削除できます。

### <a name="credentials"></a>資格情報
HDInsight クラスターでは、クラスターの作成時に次の 2 つのユーザー アカウントを構成できます。

* HTTP ユーザー。 既定のユーザー名は、Azure ポータルで基本構成を使用する *admin* です。 "クラスター ユーザー" と呼ばれることもあります。
* SSH ユーザー (Linux クラスター)。 SSH を使用してクラスターに接続する際に使用します。 「[Linux、Unix、またはOS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)」または「[HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)」の手順に従ってクラスターを作成した後に、追加の SSH ユーザー アカウントを作成できます。
  
  > [!NOTE]
  > Windows ベースのクラスターでは、RDP を使用してクラスターに接続する RDP ユーザーを作成できます。
  > 
  > 

### <a name="data-source"></a>データ ソース
元の Hadoop 分散ファイル システム (HDFS) では、クラスター上の多数のローカル ディスクを使用します。 HDInsight はデータ ストレージとして Azure BLOB ストレージを使用します。 Azure BLOB ストレージは、堅牢な汎用ストレージ ソリューションであり、HDInsight とシームレスに統合されます。 HDFS インターフェイスを使用して、HDInsight のすべてのコンポーネントで BLOB ストレージの構造化データまたは非構造化データを直接操作できます。 BLOB ストレージにデータを格納すると、ユーザー データを失わずに、計算に使用されている HDInsight クラスターを安全に削除できます。

構成時に、Azure ストレージ アカウントと、その Azure ストレージ アカウントの Azure BLOB ストレージ コンテナーを指定する必要があります。 一部の作成プロセスでは、Azure ストレージ アカウントと BLOB ストレージ コンテナーを事前に作成しておく必要があります。 BLOB ストレージ コンテナーは、既定の保存先としてクラスターで使用されます。 必要に応じて、クラスターがアクセス可能なその他の Azure Storage アカウント (リンクされたストレージ) を指定できます。 また、クラスターは、完全なパブリック読み取りアクセスまたは BLOB だけを対象とするパブリック読み取りアクセスで構成された BLOB ストレージ コンテナーにアクセスすることもできます。  詳細については、 [Azure Storage リソースへのアクセスの管理](../storage/storage-manage-access-to-resources.md)に関するページを参照してください。

![HDInsight ストレージ](./media/hdinsight-provision-clusters/HDInsight.storage.png)

> [!NOTE]
> 次の図に示すように、BLOB ストレージ コンテナーには、一連の BLOB がまとめられています。
> 
> 

![Azure BLOB ストレージ](./media/hdinsight-provision-clusters/Azure.blob.storage.jpg)

ビジネス データの格納には、既定の BLOB ストレージ コンテナーを使用しないことをお勧めします。 ストレージ コストを削減するために、既定の BLOB ストレージ コンテナーの使用後、コンテナーを毎回削除することをお勧めします。 既定のコンテナーには、アプリケーション ログとシステム ログが格納されます。 コンテナーを削除する前に、ログを取り出してください。

> [!WARNING]
> 1 つの BLOB ストレージ コンテナーを複数のクラスターで共有することはサポートされていません。
> 
> 

セカンダリ BLOB ストレージの使用方法の詳細については、 [HDInsight での Azure BLOB ストレージの使用](hdinsight-hadoop-use-blob-storage.md)に関する記事をご覧ください。

Azure Blob ストレージに加え、 [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) も HDInsight での HBase クラスターの既定のストレージ アカウントとして、また、4 つのすべての HDInsight クラスターの種類のリンクされたストレージとして使用できます。 詳細については、「 [Azure ポータルを使用して、Data Lake Store を使用する HDInsight クラスターを作成する](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)」を参照してください。

### <a name="location-region"></a>場所 (リージョン)
HDInsight クラスターとその既定のストレージ アカウントは、同じ Azure の場所に配置されている必要があります。

![Azure Azure リージョン](./media/hdinsight-provision-clusters/Azure.regions.png)

サポートされているリージョンのリストについては、「 **HDInsight の価格** 」の [[リージョン]](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)ドロップダウン リストをクリックしてください。

### <a name="node-pricing-tiers"></a>ノード価格レベル
クラスターの有効期間中、これらのノードの使用量に対して課金されます。 課金はクラスターが作成されると開始され、クラスターが削除されると停止されます。 クラスターを割り当て解除または保留にすることはできません。

クラスターの種類によって、ノードの種類、ノード数、ノード サイズが異なります。 たとえば、Hadoop クラスターの種類には 2 つの*ヘッド ノード*と 4 つの*データ ノード* (既定) がありますが、Storm クラスターの種類には 2 つの *Nimbus ノード*、3 つの *Zookeeper ノード*と 4 つの*Supervisor ノード* (既定) があります。 HDInsight クラスターのコストは、ノード数とノードの仮想マシンのサイズによって決まります。 たとえば、大量のメモリを必要とする操作を実行することがわかっている場合は、より多くのメモリを持つコンピューティング リソースを選択できます。 学習目的の場合、データ ノードを 1 つ使用することをお勧めします。 HDInsight の価格の詳細については、「 [HDInsight 価格](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)」をご覧ください。

> [!NOTE]
> クラスター サイズの制限は、Azure サブスクリプションによって異なります。 制限値を上げるには、課金サポートにお問い合わせください。
> 
> ノードに使用される仮想マシン イメージは、HDInsight サービスの実装の詳細であるため、クラスターで使用されるノードは仮想マシンとしてカウントされません。 ノードで使用されるコンピューティング コアは、サブスクリプションで利用できるコンピューティング コアとしてカウントされ、その合計数に含まれます。 利用可能なコアの数とクラスターで使用されるコアの数は、HDInsight クラスターの作成時に、[ノード価格レベル] ブレードの概要セクションで確認できます。
> 
> 

Azure ポータルを使用してクラスターを構成するときに、 **[ノード価格レベル]** ブレードでノード サイズを利用できます。 また、別のノード サイズに関連するコストを確認することもできます。 次のスクリーンショットは、Linux ベースの Hadoop クラスターの選択肢を示しています。

![HDInsight VM ノードのサイズ](./media/hdinsight-provision-clusters/hdinsight.node.sizes.png)

次の表に、HDInsight クラスターでサポートされているサイズと、それらが提供する容量を示します。

#### <a name="standard-tier-a-series"></a>Standard レベル: A シリーズ
クラシック デプロイ モデルでは、一部の VM サイズが PowerShell と CLI で若干異なります。

* Standard_A3: Large
* Standard_A4: ExtraLarge

| サイズ | CPU コア数 | メモリ | NIC (最大) | 最大 ディスク サイズ | 最大 データ ディスク数 (各ディスク 1,023 GB) | 最大 IOPS (各ディスク 500) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A3\Large |4 |7 GB |2 |一時ディスク = 285 GB |8 |8 x 500 |
| Standard_A4\ExtraLarge |8 |14 GB |4 |一時ディスク = 605 GB |16 |16 x 500 |
| Standard_A6 |4 |28 GB |2 |一時ディスク = 285 GB |8 |8 x 500 |
| Standard_A7 |8 |56 GB |4 |一時ディスク = 605 GB |16 |16 x 500 |

#### <a name="standard-tier-d-series"></a>Standard レベル: D シリーズ
| サイズ | CPU コア数 | メモリ | NIC (最大) | 最大 ディスク サイズ | 最大 データ ディスク数 (各ディスク 1,023 GB) | 最大 IOPS (各ディスク 500) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D3 |4 |14 GB |4 |一時的 (SSD) = 200 GB |8 |8 x 500 |
| Standard_D4 |8 |28 GB |8 |一時的 (SSD) = 400 GB |16 |16 x 500 |
| Standard_D12 |4 |28 GB |4 |一時的 (SSD) = 200 GB |8 |8 x 500 |
| Standard_D13 |8 |56 GB |8 |一時的 (SSD) = 400 GB |16 |16 x 500 |
| Standard_D14 |16 |112 GB |8 |一時的 (SSD) = 800 GB |32 |32 x 500 |

#### <a name="standard-tier-dv2-series"></a>Standard レベル: Dv2 シリーズ
| サイズ | CPU コア数 | メモリ | NIC (最大) | 最大 ディスク サイズ | 最大 データ ディスク数 (各ディスク 1,023 GB) | 最大 IOPS (各ディスク 500) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D3_v2 |4 |14 GB |4 |一時的 (SSD) = 200 GB |8 |8 x 500 |
| Standard_D4_v2 |8 |28 GB |8 |一時的 (SSD) = 400 GB |16 |16 x 500 |
| Standard_D12_v2 |4 |28 GB |4 |一時的 (SSD) = 200 GB |8 |8 x 500 |
| Standard_D13_v2 |8 |56 GB |8 |一時的 (SSD) = 400 GB |16 |16 x 500 |
| Standard_D14_v2 |16 |112 GB |8 |一時的 (SSD) = 800 GB |32 |32 x 500 |

これらのリソースの使用を計画するときに注意する必要のあるデプロイの考慮事項については、 [仮想マシンのサイズ](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)に関する記事をご覧ください。 さまざまなサイズの価格については、「 [HDInsight の価格](https://azure.microsoft.com/pricing/details/hdinsight)」をご覧ください。   

> [!IMPORTANT]
> クラスターの作成時または作成後のスケーリングで 32 個を超える worker ノードを使用することを予定している場合は、コア数が 8 個以上、RAM が 14 GB 以上のヘッド ノード サイズを選択する必要があります。
> 
> 

課金はクラスターが作成されると開始され、クラスターが削除されると停止されます。 価格の詳細については、 [HDInsight の価格詳細](https://azure.microsoft.com/pricing/details/hdinsight/)に関する記述を参照してください。

## <a name="use-additional-storage"></a>追加のストレージの使用
場合によっては、クラスターへのストレージの追加が必要になることがあります。 たとえば、異なる地理的リージョンまたは異なるサービスの複数の Azure ストレージ アカウントがあり、それらをすべて HDInsight で分析する場合などです。

HDInsight クラスターを作成するとき、あるいはクラスターが作成された後に、ストレージ アカウントを追加できます。  「 [スクリプト アクションを使用して Linux ベースの HDInsight クラスターをカスタマイズする](hdinsight-hadoop-customize-cluster-linux.md)」をご覧ください。

セカンダリ BLOB ストレージの詳細については、 [HDInsight での Azure BLOB ストレージの使用](hdinsight-hadoop-use-blob-storage.md)に関する記事をご覧ください。 セカンダリ Data Lake Store の詳細については、「 [Azure ポータルを使用して、Data Lake Store を使用する HDInsight クラスターを作成する](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)」をご覧ください。

## <a name="use-hiveoozie-metastore"></a>Hive/Oozie メタストアの使用
HDInsight クラスターを削除した後も Hive テーブルを保持する場合は、カスタム メタストアを使用することを強くお勧めします。 そのメタストアを別の HDInsight クラスターにアタッチすることもできます。

> [!IMPORTANT]
> あるバージョンの HDInsight クラスター用に作成された HDInsight メタストアは、別の HDInsight クラスター バージョン間で共有できません。 HDInsight のバージョンの一覧は、「[サポートされる HDInsight のバージョン](hdinsight-component-versioning.md#supported-hdinsight-versions)」をご覧ください。
> 
> 

メタストアには、Hive テーブル、パーティション、スキーマ、列などについての Hive と Oozie メタデータが格納されます。 メタストアでは Hive と Oozie メタデータを保持できるため、新しいクラスターを作成するときに、Hive テーブルまたは Oozie ジョブを再作成する必要はありません。 既定では、Hive は組み込みの Azure SQL Database を使用してこの情報を格納します。 組み込みデータベースは、クラスターが削除されるとメタデータを保持できません。 Hive メタストアが構成された HDInsight クラスターで Hive テーブルを作成すると、同じ Hive メタストアを使用してクラスターを再作成したときにそれらのテーブルが保持されます。

HBase のクラスターの種類では、メタストア構成は使用できません。

> [!IMPORTANT]
> カスタム メタストアを作成するときは、データベース名にダッシュやハイフンを使用しないでください。 クラスター作成プロセスが失敗する可能性があります。
> 
> 

## <a name="use-azure-virtual-networks"></a>Azure Virtual Network の使用
[Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/)を使用すると、ソリューションに必要なリソースを含む、セキュリティで保護された永続的なネットワークを作成できます。 Virtual Network では次のことが可能です。

* プライベート ネットワーク (クラウドのみ) 内でのクラウド リソース間の接続
  
    ![クラウドのみの構成の図](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-cloud-only.png)
* 仮想プライベート ネットワーク (VPN) を使用したローカル データセンター ネットワークへのクラウド リソースの接続 (サイト間またはポイント対サイト)。

| サイト間構成 | ポイント対サイト構成 |
| --- | --- |
| サイト間構成では、ハードウェア VPN を使用するか、ルーティングとリモート アクセス サービスを使用して、データセンターの複数のリソースを Azure Virtual Network に接続できます。<br />![サイト間構成の図](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-site-to-site.png) |ポイント対サイト構成では、ソフトウェア VPN を使用して、特定のリソースを Azure Virtual Network に接続できます。<br />![ポイント対サイト構成の図](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-point-to-site.png) |

Windows ベースのクラスターでは、v1 (クラシック) Virtual Network が必要であり、Linux ベースのクラスターでは、v2 (Azure Resource Manager) Virtual Network が必要です。 ネットワークの種類が正しくない場合、クラスターの作成には使用できません。

Virtual Network の具体的な構成要件など、Virtual Network で HDInsight を使用する方法の詳細については、「 [Azure Virtual Network を使用した HDInsight 機能の拡張](hdinsight-extend-hadoop-virtual-network.md)」をご覧ください。

## <a name="customize-clusters-using-hdinsight-cluster-customization-bootstrap"></a>HDInsight クラスターのカスタマイズ (ブートストラップ) を使用したクラスターのカスタマイズ
次の構成ファイルを構成することが必要な場合があります。

* clusterIdentity.xml
* core-site.xml
* gateway.xml
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

クラスターの有効期間中に変更を保持するには、作成プロセスで HDInsight クラスターのカスタマイズを使用するか、Linux ベースのクラスターで Ambari を使用します。 詳細については、「 [ブートストラップを使って HDInsight クラスターをカスタマイズする](hdinsight-hadoop-customize-cluster-bootstrap.md)」をご覧ください。

> [!NOTE]
> 再イメージ化により、Windows ベースのクラスターは変更を保持できません。 詳細については、「 [Role Instance Restarts Due to OS Upgrades (OS のアップグレードに伴うロール インスタンスの再起動)](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)」を参照してください。  クラスターの有効期間中に変更を保持するには、作成プロセスで HDInsight クラスターのカスタマイズを使用する必要があります。
> 
> 

## <a name="customize-clusters-using-script-action"></a>スクリプト アクションを使用したクラスターのカスタマイズ
追加コンポーネントをインストールするか、作成中にスクリプトを使用してクラスターの構成をカスタマイズできます。 このようなスクリプトは、**スクリプト操作**を使用して実行します。これは Azure ポータル、HDInsight Windows PowerShell コマンドレット、HDInsight .NET SDK で使用できる構成オプションです。 詳しくは、「[Script Action を使って HDInsight をカスタマイズする](hdinsight-hadoop-customize-cluster-linux.md)」をご覧ください。

Mahout や Cascading などの一部のネイティブ Java コンポーネントは、Java アーカイブ (JAR) ファイルとしてクラスター上で実行できます。 これらの JAR ファイルは、Azure BLOB ストレージに分配し、Hadoop ジョブ送信メカニズムによって HDInsight クラスターに送信できます。 詳細については、 [プログラムによる Hadoop ジョブの送信](hdinsight-submit-hadoop-jobs-programmatically.md)に関するページを参照してください。

> [!NOTE]
> HDInsight クラスターへの JAR ファイルのデプロイ、または HDInsight クラスターでの JAR ファイルの呼び出しに関する問題がある場合は、 [Microsoft サポート](https://azure.microsoft.com/support/options/)にお問い合わせください。
> 
> Cascading は HDInsight ではサポートされておらず、Microsoft サポートの対象でもありません。 サポートされているコンポーネントの一覧については、 [HDInsight で提供されるクラスター バージョンの新機能](hdinsight-component-versioning.md)
> 
> 

## <a name="use-edge-node"></a>エッジ ノードの使用
 空のエッジ ノードは、ヘッド ノードの場合と同じクライアント ツールがインストールされ、構成された Linux 仮想マシンです。 エッジ ノードは、クラスターへのアクセス、クライアント アプリケーションのテスト、およびクライアント アプリケーションのホストに使用できます。 詳細については、「 [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md)」(HDInsight で空のエッジ ノードを使用する) を参照してください。

## <a name="cluster-creation-methods"></a>クラスターの作成方法
この記事では、Linux ベースの HDInsight クラスターの作成に関する基本的な情報を提供しました。 次の表を使用して、ニーズに最も適した方法を使用したクラスターの作成方法に関する特定の情報を見つけてください。

| クラスターの作成方法 | Web ブラウザー | コマンド ライン | REST API | SDK | Linux、Mac OS X、または Unix | Windows |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| [Azure ポータル](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |✔ |✔ |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |✔ |✔ |
| [Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |✔ |✔ |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |✔ |✔ |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |✔ |✔ |
| [.NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |&nbsp; |&nbsp; |&nbsp; |✔ |✔ |✔ |
| [Azure Resource Manager のテンプレート](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |✔ |✔ |




<!--HONumber=Nov16_HO3-->


