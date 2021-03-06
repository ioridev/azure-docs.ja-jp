---
title: 需要の変化に対応するために Azure Data Explorer のクラスターの水平スケーリング (スケールアウト) を管理する
description: この記事では、変化する需要に基づいて、Azure Data Explorer クラスターをスケールアウトおよびスケールインする手順について説明します。
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: 70e6bdfcf9718244632ad02e09d3ddadee71a617
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311565"
---
# <a name="manage-cluster-horizontal-scaling-scale-out-in-azure-data-explorer-to-accommodate-changing-demand"></a>需要の変化に対応するために Azure Data Explorer のクラスターの水平スケーリング (スケールアウト) を管理する

クラスターのサイズを適切に設定することは、Azure データ エクスプローラーのパフォーマンスにとって重要なことです。 静的クラスターのサイズが使用率の低下や超過の原因となる可能性があり、どちらも理想的な状態ではありません。

クラスターに対する需要を高い精度で予測することはできないため、変化する需要に合わせて容量と CPU リソースを追加および削除して、クラスターを "*スケーリング*" することをお勧めします。 

Azure Data Explorer クラスターのスケーリングには、2 つのワークフローがあります。 

* 水平スケーリング (スケールアウトおよびスケールインともいいます)。
* [垂直スケーリング](manage-cluster-vertical-scaling.md) (スケールアップおよびスケールダウンともいいます)。

この記事では、水平スケーリングのワークフローについて説明します。

## <a name="configure-horizontal-scaling"></a>水平スケーリングを構成する

水平スケーリングを使用して、事前に定義されたルールとスケジュールに基づいて、インスタンス数を自動的にスケーリングすることができます。 クラスターの自動スケーリングの設定を指定するには:

1. Azure portal で、Azure Data Explorer クラスター リソースに移動します。 **[設定]** で、 **[スケールアウト]** を選択します。 

2. **[スケールアウト]** ウィンドウで、必要な自動スケーリング方法を次から選択します: **[手動スケール]** 、 **[Optimized autoscale]\(最適化された自動スケーリング\)** 、 **[Custom autoscale]\(カスタム自動スケーリング\)** 。

### <a name="manual-scale"></a>手動でのスケーリング

手動スケーリングは、クラスターを作成するときの既定の設定です。 クラスターには、自動的に変更されない固定の容量があります。 固定容量を選択するには、 **[インスタンス数]** バーを使います。 別の変更を行うまで、クラスターのスケーリングはその設定のままになります。

   ![手動スケール方法](media/manage-cluster-horizontal-scaling/manual-scale-method.png)

### <a name="optimized-autoscale"></a>最適化された自動スケーリング

最適化された自動スケーリングは、推奨される自動スケーリング方法です。 この方法では、クラスターのパフォーマンスとコストが最適化されます。 クラスターが低使用率状態に近づいた場合は、スケールインされます。 この操作により、コストは削減されますが、パフォーマンス レベルは維持されます。 クラスターが高使用率状態に近づいた場合は、最適なパフォーマンスを維持するためにスケールアウトされます。 最適化された自動スケーリングの構成するには:

1. **[Optimized autoscale]\(最適化された自動スケーリング\)** を選択します。 

1. 最小インスタンス数と最大インスタンス数を選択します。 クラスターは、負荷に基づいてそれら 2 つの値の範囲で自動スケーリングされます。

1. **[保存]** を選択します。

   ![最適化された自動スケーリング方法](media/manage-cluster-horizontal-scaling/optimized-autoscale-method.png)

最適化された自動スケーリングの動作が始まります。 アクションが、クラスターの Azure アクティビティ ログに表示されます。

### <a name="custom-autoscale"></a>カスタム自動スケーリング

カスタム自動スケーリングを使うと、指定したメトリックに基づいて、クラスターを動的にスケーリングできます。 次の図では、カスタム自動スケーリングを構成するフローと手順を示します。 図の下に詳細情報があります。

1. **[自動スケーリング設定の名前]** ボックスに、名前を入力します (例: "*スケールアウト: キャッシュ使用率*")。 

   ![スケール ルール](media/manage-cluster-horizontal-scaling/custom-autoscale-method.png)

2. **[スケール モード]** には、 **[メトリックに基づいてスケーリングする]** を選択します。 このモードは、動的スケーリングを提供します。 また、 **[特定のインスタンス数にスケーリングする]** も選択できます。

3. **[+ ルールの追加]** を選択します。

4. 右側の **[スケール ルール]** セクションで、各設定の値を入力します。

    **条件**

    | Setting | 説明と値 |
    | --- | --- |
    | **時間の集計** | **[平均]** など、集計条件を選択します。 |
    | **メトリック名** | **[キャッシュ使用率]** など、スケール操作のベースとするメトリックを選択します。 |
    | **時間グレインの統計** | **[平均]** 、 **[最小]** 、 **[最大]** 、 **[合計]** から選択します。 |
    | **演算子** | **[次の値以上]** など、適切なオプションを選択します。 |
    | **しきい値** | 適切な値を選択します。 たとえば、キャッシュ使用率の場合、80% が適切な開始点です。 |
    | **期間 (分)** | システムでメトリックを計算する際にさかのぼって確認する適切な期間を選択します。 既定値の 10 分から始めます。 |
    |  |  |

    **アクション**

    | Setting | 説明と値 |
    | --- | --- |
    | **操作** | スケールインまたはスケール アウトする適切なオプションを選択します。 |
    | **インスタンス数** | メトリック条件が満たされたときに追加または削除するノードまたはインスタンスの数を選択します。 |
    | **クール ダウン (分)** | スケール操作の間の待機する適切な時間間隔を選択します。 既定値の 5 分から始めます。 |
    |  |  |

5. **[追加]** を選択します。

6. 左側の **[インスタンスの制限]** セクションで、各設定の値を入力します。

    | Setting | 説明と値 |
    | --- | --- |
    | **最小** | インスタンス数の下限。使用率に関係なく、クラスターはこの数未満にスケールしません。 |
    | **[最大]** | インスタンス数の上限。使用率に関係なく、クラスターはこの数を超えてスケールしません。 |
    | **既定値** | 既定のインスタンス数。 この設定は、リソース メトリックの読み取りで問題がある場合に使用されます。 |
    |  |  |

7. **[保存]** を選択します。

これで、Azure Data Explorer クラスターの水平スケーリングが構成されました。 垂直スケーリング用に別のルールを追加します。 クラスターのスケーリングに関する問題が発生したときにサポートが必要な場合は、Azure portal で[サポート要求を開いてください](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)。

## <a name="next-steps"></a>次の手順

* [メトリックを使用した Azure Data Explorer のパフォーマンス、正常性、および使用状況の監視](using-metrics.md)

* クラスターの適切なサイズ設定を行うために、[クラスターの垂直スケーリングを管理](manage-cluster-vertical-scaling.md)する。
