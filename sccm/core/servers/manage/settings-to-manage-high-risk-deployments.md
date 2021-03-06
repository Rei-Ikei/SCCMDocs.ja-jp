---
title: 危険度の高い展開の管理
titleSuffix: Configuration Manager
description: 管理者が危険度の高い展開を作成した場合に管理者に警告するように、Configuration Manager で展開検証サイト設定を構成する方法について説明します。
ms.date: 07/30/2018
ms.prod: configuration-manager
ms.technology: configmgr-other
ms.topic: conceptual
ms.assetid: 8d37b983-a964-402c-819d-2512ed2d463b
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: ab2203b948887a94577826573ccdf3376637eca2
ms.sourcegitcommit: 1826664216c61691292ea2a79e836b11e1e8a118
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2018
ms.locfileid: "39386023"
---
# <a name="settings-to-manage-high-risk-deployments-for-configuration-manager"></a>Configuration Manager の危険度の高い展開を管理するための設定

*適用対象: System Center Configuration Manager (Current Branch)*


Configuration Manager では、展開の検証サイト設定を構成することができます。 これらの設定を使用して、管理者が危険度の高い展開を作成した場合に管理者に警告します。 危険度の高い展開とは次のようなものです。  

-   自動的にインストールされる展開  

-   望ましくない結果を引き起こす可能性がある展開  

たとえば、オペレーティング システムを展開する**必須**の目的を持つタスク シーケンスは、高危険度と見なされます。  

望ましくない危険度の高い展開のリスクを減らすためには、これらの展開検証設定でサイズ制限を構成できます。  

-   **コレクション サイズの制限**: 展開の作成時に、制限より多くのクライアントが含まれているコレクションを非表示にします。  

     -   **既定サイズ**: この設定を使用して、展開の作成時に、制限より多くのクライアントが含まれているコレクションを既定で非表示にします。 展開の作成時にもこれらのコレクションを表示できますが、既定では非表示になっています。 既定値は **100** です。 この設定を無視するには、**0** の値を入力します。  

     -   **最大サイズ**: この設定を使用して、展開の作成時に、制限より多くのクライアントが含まれているコレクションを常に非表示にします。 既定値は **0** であり、この設定は無視されます。 **最大サイズ** の値は、 **既定サイズ** の値より大きい必要があります。  

     たとえば、**既定サイズ**を 100、**最大サイズ**を 1000 に設定します。 危険度の高い展開の作成時に、**[コレクションの選択]** ウィンドウには、含まれるクライアント数が 100 未満のコレクションのみが表示されます。 **[メンバー数がサイトの最小サイズ構成を超えるコレクションを非表示にする]** の設定をオフにすると、含まれるクライアント数が 1000 未満のコレクションがウィンドウに表示されます。  

-   **サイト システム サーバーを含むコレクション**: 対象のコレクションにサイト システムの役割を持つコンピューターが含まれている場合は、展開をブロックするか、展開を作成する前に検証が必要になります。 展開がブロックされている場合は、展開の検証条件を満たす別のコレクションを選択して、展開の作成を続行します。  

> [!NOTE]  
>  危険度の高い展開は、常にカスタム コレクション、作成されたコレクション、および組み込みの **不明なコンピューター** コレクションに制限されています。 危険度の高い展開を作成する際、**すべてのシステム**などの組み込みのコレクションは選択できません。  

### <a name="configure-deployment-verification-for-a-site"></a>サイトの展開検証を構成する  

1.  Configuration Manager コンソールで、**[管理]** ワークスペースに移動し、**[サイトの構成]** を展開して **[サイト]** を選択してから、構成するプライマリ サイトを選びます。  

2.  リボンの **[プロパティ]** をクリックしてから、**[展開の検証]** タブに切り替えます。  

3.  使用する構成を設定した後、**[OK]** をクリックして構成を保存します。  


### <a name="see-also"></a>関連項目  
 [サイトと階層の構成](/sccm/core/servers/deploy/configure/configure-sites-and-hierarchies)
