---
title: Microsoft Intune でのハイブリッド MDM
titleSuffix: Configuration Manager
description: Configuration Manager と Microsoft Intune を使用するハイブリッド モバイル デバイス管理 (MDM) について説明します。
ms.date: 08/14/2018
ms.prod: configuration-manager
ms.technology: configmgr-hybrid
ms.topic: conceptual
ms.assetid: bb95154b-f63e-4491-896e-41d732c802f8
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: 9f65c7665d5c055fc2a1636a1c9556b4ff75a4f7
ms.sourcegitcommit: 98c3f7848dc9014de05541aefa09f36d49174784
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2018
ms.locfileid: "42584483"
---
# <a name="hybrid-mdm-with-configuration-manager-and-microsoft-intune"></a>Configuration Manager と Microsoft Intune を使用するハイブリッド MDM

*適用対象: System Center Configuration Manager (Current Branch)*

> [!Important]  
> 2018 年 8 月 14 日の時点では、ハイブリッド モバイル デバイス管理は[非推奨の機能](/sccm/core/plan-design/changes/deprecated/removed-and-deprecated-cmfeatures)です。
> <!--Intune feature 2683117--> 1 年以上前に Azure で利用できるようになってから、Intune には、お客様からの要望や市場の発展に従って何百もの新しいサービス機能が追加されています。 ハイブリッド モバイル デバイス管理 (MDM) で提供されるものよりはるかに多くの機能が用意されています。 Azure での Intune では、エンタープライズ モビリティのニーズに対していっそう統合された効率的な管理エクスペリエンスが提供されています。
> 
> その結果、ほとんどのお客様は、ハイブリッド MDM ではなく Azure での Intune を選択されています。 クラウドに移行するお客様の増加により、ハイブリッド MDM を使用するお客様の数は減り続けています。 そのため、2019 年 9 月 1 日をもって、Microsoft はハイブリッド MDM サービスの提供を終了します。 MDM のニーズを [Azure での Intune に移行する](/sccm/mdm/deploy-use/migrate-hybridmdm-to-intunesa)ことを計画してください。 
> 
> この変更は、オンプレミスの Configuration Manager または [Windows 10 デバイスの共同管理](/sccm/core/clients/manage/co-management-overview)には影響しません。 ハイブリッド MDM を使用しているかどうかわからない場合は、Configuration Manager コンソールの **[管理]** ワークスペースに移動し、**[クラウド サービス]** を展開して、**[Microsoft Intune サブスクリプション]** をクリックします。 Microsoft Intune のサブスクリプションが設定されている場合、そのテナントはハイブリッド MDM 用に構成されています。
> 
> **ユーザーへの影響**
> 
> - ハイブリッド MDM の使用は来年はサポートされます。 機能の主要なバグの修正プログラムは引き続き受け取ります。 iOS 12 への登録など、新しい OS バージョンでの既存機能はサポートされます。 ハイブリッド MDM には新機能は追加されません。  
> 
> - ハイブリッド MDM 提供終了前に Azure での Intune に移行する場合、エンドユーザーに対する影響はありません。  
> 
> - ライセンスは同じままです。 Azure での Intune のライセンスは、ハイブリッド MDM に含まれています。  
> 
> - 2018 年 11 月以降、Microsoft はハイブリッド MDM への新規顧客のオンボードのブロックを開始します。  
> 
> - 2019 年 9 月 1 日をもって、残っているハイブリッド MDM のデバイスは、ポリシー、アプリ、またはセキュリティの更新プログラムを受け取らなくなります。  
> 
> **この変更に対して必要な準備**
> 
> - ConfigMgr コンソールから Azure への MDM の移行の計画を開始してください。 Microsoft IT を含む多くのお客様は、このプロセスを既に行っています。 詳しくは、[Microsoft のケース スタディ](https://aka.ms/Intune_MSFT)に関するページをご覧ください。  
> 
> - ハイブリッド MDM から Azure での Intune への移行プロセスを簡略化するには、次のツールとドキュメントを確認してください。  
>     - [Configuration Manager のデータの Microsoft Intune へのインポート](/sccm/mdm/deploy-use/migrate-import-data)  
>     - [ハイブリッド MDM のユーザーとデバイスを Intune スタンドアロンに移行する](/sccm/mdm/deploy-use/migrate-hybridmdm-to-intunesa)  
> 
> - 詳しくは、レコードまたは FastTrack のパートナーにお問い合わせください。 [FastTrack for Microsoft 365](https://aka.ms/hybrid_fasttrack) は、ハイブリッド MDM から Azure での Intune への移行に役立ちます。 
> 
> 詳しくは、[Intune サポートのブログ投稿](https://aka.ms/hybrid_notification)をご覧ください。



Configuration Manager のハイブリッド モバイル デバイス管理 (MDM) 機能を使用して、iOS、Windows、および Android デバイスを管理します。 すべての管理タスクは Configuration Manager コンソールから処理されます。このコンソールでは、インターネット経由で Microsoft Intune のオンライン サービスとシームレスに統合された残りの管理タスクを実行します。 Configuration Manager を利用し、ユーザーが自分のデバイスで安全かつ管理された方法で会社のリソースにアクセスできるように設定します。 つまり、会社のデータを守ると共に、ユーザー個人のデバイスや会社支給のデバイスから会社のデータにアクセスできるようにします。 

この記事では、お客様は Configuration Manager を使用してコンピューターを管理しているものとします。 また、モバイル デバイスを管理するために Intune で Configuration Manager コンソールを拡張することに関心を持っているものとします。 



## <a name="capabilities"></a>機能

ハイブリッド MDM では、デバイスでの次の管理機能がサポートされています。

-   デバイスの削除とワイプ  

-   パスワード、セキュリティ、ローミング、暗号化、ワイヤレス通信などのコンプライアンス設定の構成  

-   デバイスへの基幹業務 (LOB) アプリの展開  

-   Windows ストア、Windows Phone ストア、App Store、または Google Play に接続するデバイスへのアプリの展開  

-   ハードウェア インベントリの収集  

-   組み込みレポートを使用したソフトウェア インベントリの収集  

ハイブリッド MDM で使用できる新機能については、「[What's new in hybrid mobile device management](/sccm/mdm/understand/whats-new-in-hybrid-mobile-device-management)」 (ハイブリッド モバイル デバイス管理の新機能) を参照してください。



## <a name="hybrid-mdm-enrollment"></a>ハイブリッド MDM の登録

デバイスをハイブリッド管理するには、サービスに登録する必要があります。 デバイスの登録方法は、デバイスの種類、所有権、必要な管理のレベルによって異なります。

- **"Bring your own device" (BYOD)**: ユーザーは個人のスマートフォン、タブレット、PC を登録します  

- **企業所有デバイス (COD)**: リモート ワイプ、共有デバイス、デバイスのユーザー アフィニティなどの管理シナリオを有効にします  

- オンプレミスの、あるいはクラウドでホストされている [Exchange ActiveSync](/sccm/mdm/plan-design/device-enrollment-methods#mobile-device-management-with-exchange-activesync-and-configuration-manager) を使用する場合、登録なしの簡単な Intune 管理を有効にできます。 Windows PC は [Intune クライアント ソフトウェア](/intune/deploy-use/manage-windows-pcs-with-microsoft-intune)でも管理できます。
