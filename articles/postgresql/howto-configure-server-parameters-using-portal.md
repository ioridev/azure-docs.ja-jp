---
title: Azure Portal を使用した Azure Database for PostgreSQL のサーバー パラメーターの構成
description: この記事では、Azure Portal を使用して Azure Database for PostgreSQL のサーバー パラメーターを構成する方法について説明します。
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: ed19083c6a4245a1b4bf7af166ae965d956c9e37
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067140"
---
# <a name="configure-server-parameters-in-azure-database-for-postgresql---single-server-via-the-azure-portal"></a>Azure portal を使用した Azure Database for PostgreSQL - Single Server でのサーバー パラメーターの構成 
Azure Portal を使用して、Azure Database for PostgreSQL サーバーの構成パラメーターを一覧表示、表示、更新できます。

## <a name="prerequisites"></a>前提条件
このハウツー ガイドの手順を実行するには、以下が必要です。
- [Azure Database for PostgreSQL サーバー](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>パラメーターの編集と表示
1. [Azure Portal](https://portal.azure.com)を開きます。

2. Azure Database for PostgreSQL サーバーを選択します。

3. **[設定]** セクションで、 **[サーバー パラメーター]** を選択します。 ページにパラメーター、その値、説明の一覧が表示されます。
![パラメーターの [概要] ページ](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. **ドロップ ダウン** ボタンをクリックして、client_min_messages のような列挙型のパラメーターに指定できる値を表示します。
![列挙ドロップ ダウン](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png)

5. **[i]** (情報) ボタンを選択するかマウス ポインターを置いて、cpu_index_tuple_cost のような数値パラメーターに指定できる値の範囲を表示します。
![情報ボタン](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. 必要に応じて、**検索ボックス**を使用して、特定のパラメーターを絞り込みます。 検索は、パラメーターの名前と説明に対して実行されます。
![検索結果](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. 調整するパラメーターの値を変更します。 セッションで行ったすべての変更が紫色で強調表示されます。 値を変更したら、 **[保存]** を選択できます。 または変更を **[破棄]** することができます。
![変更の保存または破棄](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. パラメーターの新しい値を保存した場合は、 **[すべて既定値にリセット]** を選択していつでもすべてを既定値に戻すことができます。
![すべて既定値にリセット](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>次の手順
内容は次のとおりです。
- [Azure Database for PostgreSQL のサーバー パラメーターの概要](concepts-servers.md)
- [Azure CLI を使用したパラメーターの構成](howto-configure-server-parameters-using-cli.md)
