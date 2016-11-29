---
title: "コンテンツの移行 | System Center Configuration Manager"
description: "配布ポイントを使用すると、System Center Configuration Manager の移行先階層にデータを移行している間にコンテンツを管理できます。"
ms.custom: na
ms.date: 10/06/2016
ms.prod: configuration-manager
ms.reviewer: na
ms.suite: na
ms.technology:
- configmgr-other
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66f7759c-6272-4116-aad7-0d05db1d46cd
caps.latest.revision: 8
caps.handback.revision: 0
author: Brenduns
ms.author: brenduns
manager: angrobe
translationtype: Human Translation
ms.sourcegitcommit: 1134bb2f04152288e72d40b1b1083f415cb4e900
ms.openlocfilehash: aa88f18247b49995bff4a4a6f5fd1e1ed70ca214


---
# <a name="planning-a-content-deployment-migration-strategy-in-system-center-configuration-manager"></a>System Center Configuration Manager のコンテンツ展開移行戦略の計画

*適用対象: System Center Configuration Manager (Current Branch)*

System Center Configuration Manager の移行先階層にデータを移行している間、ソースと移行先の両方の階層の Configuration Manager クライアントは、ソース階層に展開されたコンテンツに引き続きアクセスすることができます。 また、移行を使用すると、ソース階層の配布ポイントをアップグレードまたは再割り当てして移行先階層の配布ポイントにすることができます。 配布ポイントの共有およびアップグレードまたは再割り当てを行うときに、この戦略を使用すると、移行するクライアント用に、移行先階層の新しいサーバーにコンテンツを再展開する必要がなくなります。  

コンテンツを再作成して移行先階層に配布することはできますが、次のオプションを使用してこのコンテンツを管理することもできます。  

-   ソース階層の配布ポイントを移行先階層のクライアントと共有する  

-   スタンドアロンの Configuration Manager 2007 配布ポイントまたはソース階層の Configuration Manager 2007 セカンダリ サイトをアップグレードして移行先階層の配布ポイントにする  

-   System Center Configuration Manager のソース階層の配布ポイントを移行先階層のサイトに再割り当てします。  

次のセクションを使用して、移行時のコンテンツの展開を計画します。  

-   [ソース階層と移行先階層で配布ポイントを共有します。](#About_Shared_DPs_in_Migration)  

-   [Configuration Manager 2007 共有配布ポイントのアップグレードの計画](#Planning_to_Upgrade_DPs)  

    -   [配布ポイントのアップグレード プロセス](#BKIMK_UpgradeProcess)  

    -   [Planning to upgrade Configuration Manager 2007 secondary sites](#BKMK_UpgradeSS)  

-   [System Center Configuration Manager 配布ポイントの再割り当ての計画](#BKMK_ReassignDistPoint)  

    -   [配布ポイントの再割り当てプロセス](#BKMK_ReassignProcess)  

-   [コンテンツを移行するときのコンテンツの所有権](#About_Migrating_Content)  

##  <a name="a-nameaboutshareddpsinmigrationa-share-distribution-points-between-source-and-destination-hierarchies"></a><a name="About_Shared_DPs_in_Migration"></a> ソース階層と移行先階層で配布ポイントを共有します。  
移行時に、ソース階層の配布ポイントを移行先階層と共有することができます。 共有の配布ポイントを使用すると、ソース階層から移行したコンテンツを移行先階層のクライアントですぐに利用可能にすることができます。コンテンツを再作成し、移行先階層の新しい配布ポイントに配布する必要はありません。 移行先階層のクライアントから、共有の配布ポイントに展開されたコンテンツが要求された場合は、この共有の配布ポイントを有効なコンテンツの場所としてクライアントに提供することができます。  

 ソース階層からの移行が引き続きアクティブになっている間は、移行先階層のクライアントの有効なコンテンツの場所にできるだけでなく、配布ポイントをアップグレードしたり移行先階層に再割り当てしたりすることもできます。 Configuration Manager 2007 共有配布ポイントをアップグレードし、System Center 2012 Configuration Manager の共有配布ポイントを再割り当てできます。 共有配布ポイントをアップグレードまたは再割り当てすると、その配布ポイントがソース階層から削除されて、移行先階層の配布ポイントになります。 共有配布ポイントをアップグレードまたは再割り当てし、ソース階層からの移行が終了したら、移行先階層の配布ポイントを引き続き使用することができます。 共有配布ポイントのアップグレード方法の詳細については、「 [Planning to upgrade Configuration Manager 2007 shared distribution points](#Planning_to_Upgrade_DPs)」を参照してください。 共有配布ポイントを再割り当てする方法については、「 [System Center Configuration Manager 配布ポイントの再割り当ての計画](#BKMK_ReassignDistPoint)」を参照してください。  

 階層内の任意のソース サイトの配布ポイントを共有できます。 ソース サイトの配布ポイントを共有すると、そのプライマリ サイトおよびプライマリ サイトの各子セカンダリ サイトの適合する各配布ポイントが共有されます。 共有配布ポイントとして適合するためには、配布ポイントをホストするサイト システム サーバーを完全修飾ドメイン名 (FQDN) を使って構成する必要があります。 NetBIOS 名を使って構成した配布ポイントは無視されます。  

> [!TIP]  
>  Configuration Manager 2007 ではサイト システム サーバーに FQDN を構成する必要はありません。  

次の情報を使用して、共有配布ポイントを計画します。  

-   共有する配布ポイントが、共有配布ポイントの前提条件を満たしている必要があります。 これらの前提条件の詳細については、「[移行に必要な構成](../../core/migration/prerequisites-for-migration.md#BKMK_Required_Configurations)」の「[Prerequisites for migration in System Center Configuration Manager](../../core/migration/prerequisites-for-migration.md)」(System Center Configuration Manager での移行の前提条件) セクションを参照してください。  

-   共有配布ポイントの設定はサイト全体の設定で、ソース サイトおよび直接の子セカンダリ サイトの適合するすべての配布ポイントが共有されます。 配布ポイントの共有を有効にする場合、個々の配布ポイントを選択して共有することはできません。  

-   移行先階層のクライアントは、ソース階層の共有配布ポイントに配布されているパッケージのコンテンツの場所の情報を取得できます。 Configuration Manager 2007 ソース階層の配布ポイントでは、これにはブランチ配布ポイント、サーバー共有上の配布ポイント、および標準配布ポイントが含まれます。  

    > [!WARNING]  
    >  ソース階層を変更すると、元のソース階層の共有配布ポイントは利用できなくなり、移行先階層のクライアントにコンテンツの場所として提供できなくなります。 元のソース階層を使用するように移行を再構成すると、以前に共有されていた配布ポイントが有効なコンテンツの場所のサーバーとして復元されます。  

-   共有配布ポイントにホストされているパッケージを移行する場合、ソース階層と移行先階層でパッケージのバージョンが同じである必要があります。 ソース階層と移行先階層でパッケージのバージョンが同じでないと、移行先階層のクライアントは共有配布ポイントからコンテンツを取得できません。 そのため、ソース階層のパッケージを更新する場合、移行先階層のクライアントが共有配布ポイントからコンテンツを取得できるようにするには、パッケージ データを再度移行する必要があります。  

    > [!NOTE]  
    >  共有配布ポイントでホストされているパッケージの詳細を表示する際に、ソース サイトの [ **共有配布ポイント** ] タブに [ **ホストされた移行済みパッケージ** ] として表示されるパッケージの数は、次のデータ収集サイクルが完了するまで更新されません。  

-   共有配布ポイントとそのプロパティは、Configuration Manager コンソールの [**管理**] ワークスペースで、移行先階層に接続する [**ソース階層**] ノードに表示できます。  

-   Configuration Manager 2007 ソース階層の共有配布ポイントを使用して、Microsoft Application Virtualization (App-V) のパッケージをホストすることはできません。 App-V パッケージは、移行先階層のクライアントで使用できるように移行および変換する必要があります。 ただし、System Center 2012 Configuration Manager または System Center Configuration Manager ソース階層の共有配布ポイントを使用して、移行先階層のクライアント用に App-V パッケージをホストすることができます。  

-   Configuration Manager 2007 ソース階層の保護された配布ポイントを共有すると、移行先階層では、その配布ポイントの保護されたネットワークの場所が含まれた境界グループが作成されます。 移行先階層のこの境界グループを変更することはできません。 ただし、Configuration Manager 2007 ソース階層の配布ポイントの保護された境界情報を変更すると、この変更は、次回のデータ収集サイクルが終了した後で移行先階層に反映されます。  

    > [!NOTE]  
    >  System Center 2012 Configuration Manager サイトおよび System Center Configuration Manager サイトでは、保護された配布ポイントではなく優先配布ポイントの概念が使用されるため、この条件は、Configuration Manager 2007 ソース サイトと共有されている配布ポイントにのみ適用されます。  

ソース サイトの配布ポイントを共有する前は、適合する配布ポイントは Configuration Manager コンソールに表示されません。 配布ポイントを共有した後で、正常に共有された配布ポイントのみ表示されます。  

配布ポイントを共有した後で、ソース階層の共有配布ポイントの構成を変更できます。 配布ポイントの構成に加えた変更は、次回のデータ収集サイクルの後で移行先階層に反映されます。 共有に適合するように更新した配布ポイントは自動的に共有され、適合しなくなった配布ポイントは共有が停止されます。 たとえば、イントラネットの FQDN を使って構成しておらず、最初に移行先階層と共有しなかった配布ポイントがあるとします。 その配布ポイントに FQDN を構成すると、次回のデータ収集サイクルでこの構成が識別され、その配布ポイントが移行先階層と共有されます。  

##  <a name="a-nameplanningtoupgradedpsa-planning-to-upgrade-configuration-manager-2007-shared-distribution-points"></a><a name="Planning_to_Upgrade_DPs"></a> Configuration Manager 2007 共有配布ポイントのアップグレードの計画  
Configuration Manager 2007 ソース階層から移行するときに、共有配布ポイントをアップグレードして System Center Configuration Manager 配布ポイントにすることができます。 プライマリ サイトとセカンダリ サイトの両方で、配布ポイントをアップグレードできます。 アップグレード プロセスでは、Configuration Manager 2007 階層から配布ポイントが削除されて、移行先階層のサイト システム サーバーになります。 また、このプロセスで、配布ポイントの既存のコンテンツが配布ポイントのコンピューターの新しい場所にコピーされます。 次に、コンテンツのコピーが変更され、移行先階層のコンテンツ展開に使用するの単一インスタンス ストアが作成されます。 このため、配布ポイントをアップグレードする場合、Configuration Manager 2007 配布ポイントでホストされていた移行されたコンテンツを再配布する必要はありません。  

Configuration Manager によってコンテンツが単一インスタンス ストアに変換された後、Configuration Manager によって配布ポイントのコンピューターの元のソース コンテンツは削除されて、ディスク領域が解放されます。元のソース コンテンツの場所は使用されません。  

共有できるすべての Configuration Manager 2007 の配布ポイントが、System Center Configuration Manager のアップグレードに適合するわけではありません。 アップグレードに適合するためには、配布ポイントがインストールされているサイト システム サーバーや、インストールされている Configuration Manager 2007 配布ポイントの種類を含め、Configuration Manager 2007 配布ポイントがアップグレードの条件を満たしている必要があります。 たとえば、プライマリ サイトのサイト サーバー コンピューターにインストールされている配布ポイントはどの種類もアップグレードできませんが、セカンダリ サイトのサイト サーバー コンピューターにインストールされている標準配布ポイントはアップグレードすることができます。  

> [!NOTE]  
>  移行先階層の配布ポイントでサポートされているオペレーティング システム バージョンを実行するコンピューターの Configuration Manager 2007 共有配布ポイントのみ、アップグレードできます。 たとえば、Windows Vista を実行するコンピューターの Configuration Manager 2007 配布ポイントは共有できますが、System Center Configuration Manager では、配布ポイントとしての使用にはこのオペレーティング システムはサポートされていないため、この共有配布ポイントをアップグレードすることはできません。  

次の表に、アップグレードすることができる Configuration Manager 2007 配布ポイントの種類ごとに、サポートされる場所を示します。  

|配布ポイントの種類|サイト サーバー以外のサイト システム コンピューターの配布ポイント|他のサイト システムの役割をホストする、サイト サーバー以外のサイト システム コンピューターの配布ポイント|セカンダリ サイト サーバーの配布ポイント|  
|--------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|---------------------------------------------------|  
|標準配布ポイント|○|×|○|  
|サーバー共有上の配布ポイント<sup>1</sup>|○|いいえ|×|  
|ブランチ配布ポイント|○|いいえ|いいえ|  

 <sup>1</sup> System Center Configuration Manager では、サイト システムでのサーバー共有をサポートしていませんが、サーバー共有上の Configuration Manager 2007 配布ポイントのアップグレードをサポートしています。 サーバー共有上の Configuration Manager 2007 配布ポイントをアップグレードするときに、配布ポイントの種類が自動的にサーバーに変換されるため、単一インスタンスのコンテンツ ストアを格納する配布ポイント コンピューターのドライブを選択する必要があります。  

> [!WARNING]  
>  ブランチ配布ポイントをアップグレードする前に、Configuration Manager 2007 クライアント ソフトウェアをアンインストールしてください。 Configuration Manager 2007 クライアント ソフトウェアがインストールされているブランチ配布ポイントをアップグレードすると、コンピューターに以前に展開されたコンテンツがそのコンピューターから削除されて、配布ポイントのアップグレードは失敗します。  

Configuration Manager コンソールの [**ソース階層**] ノードでアップグレード対象の配布ポイントを識別するには、ソース サイトを選択してから、[**共有配布ポイント**] タブを選択します。 適合する配布ポイントは、[アップグレードの条件に適合 **** ] 列に [はい **** ] が表示されます。  

Configuration Manager 2007 のセカンダリ サイト サーバーにインストールされている配布ポイントをアップグレードすると、そのセカンダリ サイトはソース階層からアンインストールされます。 このシナリオはセカンダリ サイトのアップグレードと呼ばれますが、これは配布ポイントのサイト システムの役割にのみ適用されます。 結果として、セカンダリ サイトはアップグレードされず、代わりにアンインストールされます。 アンインストール後は、セカンダリ サイト サーバーだったコンピューターに移行先階層の配布ポイントのみが残ります。 セカンダリ サイトがソース階層から削除されるため、セカンダリ サイトの配布ポイントのアップグレードを計画している場合は、このトピックの「 [Planning to upgrade Configuration Manager 2007 secondary sites](#BKMK_UpgradeSS) 」セクションを参照してください。  

###  <a name="a-namebkimkupgradeprocessa-distribution-point-upgrade-process"></a><a name="BKIMK_UpgradeProcess"></a> 配布ポイントのアップグレード プロセス  
Configuration Manager コンソールを使用して、移行先階層と共有している Configuration Manager 2007 配布ポイントをアップグレードできます。 共有配布ポイントをアップグレードすると、その配布ポイントは Configuration Manager 2007 サイトからアンインストールされて、移行先階層に指定したプライマリ サイトまたはセカンダリ サイトに接続される配布ポイントとしてインストールされます。 アップグレード プロセスでは、配布ポイントに格納されている移行されたコンテンツをコピーしてから、このコピーを単一インスタンスのコンテンツ ストアに変換します。 Configuration Manager は、パッケージを単一インスタンスのコンテンツ ストアに変換すると、そのパッケージに [**配布ポイントからプログラムを実行する **] として構成されている提供情報が 1 つ以上含まれていない限り、そのパッケージを配布ポイント コンピューター上の SMSPKG 共有から削除します。  

配布ポイントをアップグレードするため、Configuration Manage は、ソース サイトの SMS プロバイダーからデータを収集するように構成された **ソース サイトのアクセス アカウント** を使用します。 ソース サイトからデータを収集するには、このアカウントにサイト オブジェクトの [**読み取り**] アクセス許可があれば十分ですが、アップグレード中に配布ポイントを Configuration Manager 2007 サイトから正常に削除するためには、[**サイト**] クラスに対する [**削除**] および [**変更**] アクセス許可も必要です。  

> [!NOTE]  
>  Configuration Manager は、一度に 1 つの配布ポイントの単一インスタンス ストアにのみコンテンツを変換できます。 複数の配布ポイントのアップグレードを構成すると、一度に 1 つの配布ポイントがアップグレードのためにキューに入れられて処理されます。  

共有配布ポイントをアップグレードする前に、配布ポイントに展開したすべてのコンテンツが移行されていることを確認します。 配布ポイントをアップグレードする前に移行していないコンテンツは、アップグレード後に移行先階層で利用できません。 配布ポイントをアップグレードすると、移行されたパッケージ内のコンテンツは移行先階層の単一インスタンス ストアと互換性がある形式に変換されます。  

Configuration Manager コンソール内から配布ポイントをアップグレードするには、Configuration Manager 2007 サイト システム サーバーが次の条件を満たしている必要があります。  

-   配布ポイントの構成と場所がアップグレードに適合している必要があります。  

-   配布ポイントのコンピューターに、コンテンツを Configuration Manager 2007 コンテンツの格納形式から単一インスタンス ストア形式に変換するための十分なディスク領域がある必要があります。 この変換には、配布ポイントに格納されている最も大きいサイズのパッケージと同じサイズの空きディスク領域が必要です。  

-   配布ポイント コンピューターは、対象階層の配布ポイントとしてサポートされているオペレーティング システム バージョンを実行する必要があります。  

    > [!NOTE]  
    >  Configuration Manager は、配布ポイントをアップグレードできるかどうかを確認するときに、配布ポイント コンピューターのオペレーティング システム バージョンを検証しません。  

配布ポイントをアップグレードするには、[管理 **** ] ワークスペースで [移行 ****] を展開して、[ソース階層 **** ] ノードを展開し、アップグレードする配布ポイントが含まれているサイトを選択します。 次に、詳細ウィンドウの [共有配布ポイント **** ] タブで、アップグレードする配布ポイントを選択します。  

配布ポイントのアップグレードの準備ができていることを確認するには、 **[再割り当ての条件に適合]** 列のステータスを確認します。  次に、Configuration Manager コンソールのリボンの [**配布ポイント**] タブの [**配布ポイント**] グループで、[**再割り当て**] を選びます。 配布ポイントのアップグレードを完了するために使用するウィザードが開きます。  

共有配布ポイントをアップグレードする場合は、移行先階層で選択したプライマリ サイトまたはセカンダリ サイトに配布ポイントを割り当てる必要があります。 配布ポイントをアップグレードした後は、他の配布ポイントと同様に、配布ポイントを対象階層の配布ポイントとして管理します。  

Configuration Manager コンソールで配布ポイントのアップグレードの進行状況を監視するには、[**管理**] ワークスペースの [**移行**] ノードの下で、[**配布ポイントの移行**] ノードを選びます。 対象階層の中央管理サイト サーバー、またはアップグレードされた配布ポイントを管理する対象階層のサイト サーバーの **Migmctrl.log** で、 **distmgr.log** の情報を確認することもできます。  

> [!NOTE]  
>  配布ポイントを移行先階層にアップグレードすると、配布ポイントのサイト システムの役割が Configuration Manager 2007 ソース サイトから削除されます。ただし、その配布ポイントに送信されていたパッケージは Configuration Manager 2007 階層で更新されません。 Configuration Manager 2007 コンソールでは、配布ポイントに送信されたパッケージがサイト システム コンピューターを [**種類**] が [**不明**] として引き続き一覧表示します。 Configuration Manager 2007 のパッケージへの後続の更新では、その不明なサイト システムのパッケージを更新しようとしたときに、配布マネージャーがそのサイトの distmgr.log にエラーを報告します。  

共有配布ポイントをアップグレードしないことにした場合でも、以前の Configuration Manager 2007 配布ポイントに移行先階層の配布ポイントをインストールできます。 新しい配布ポイントをインストールする前に、まず、配布ポイントのコンピューターからすべての Configuration Manager 2007 サイト システムの役割をアンインストールする必要があります。 その際、Configuration Manager 2007 サイトがサイト サーバー コンピューターである場合は、これも含まれます。 Configuration Manager 2007 配布ポイントをアンインストールしても、配布ポイントに展開したコンテンツはコンピューターから削除されません。  

###  <a name="a-namebkmkupgradessa-planning-to-upgrade-configuration-manager-2007-secondary-sites"></a><a name="BKMK_UpgradeSS"></a> Configuration Manager 2007 セカンダリ サイトへのアップグレードの計画  
 移行を使用して、Configuration Manager 2007 セカンダリ サイト サーバーでホストされている共有配布ポイントをアップグレードすると、Configuration Manager は配布ポイント サイト システムの役割を移行先階層の配布ポイントにアップグレードするだけでなく、ソース階層からセカンダリ サイトをアンインストールします。 結果は、セカンダリ サイトではなく、System Center Configuration Manager 配布ポイントになります。  

 サイト サーバーのコンピューター上の配布ポイントがアップグレードに適合するためには、Configuration Manager が、そのコンピューターの各サイト システムの役割を含むセカンダリ サイトをアンインストールできる必要があります。 通常、Configuration Manager 2007 サーバー共有の共有配布ポイントはアップグレードすることができます。 ただし、セカンダリ サイト サーバー上にサーバー共有が存在する場合、そのコンピューター上のセカンダリ サイトおよび共有配布ポイントはアップグレードに適合していません。 これは、プロセスがセカンダリ サイトをアンインストールしようとすると、サーバー共有が追加のサイト システム オブジェクトとして扱われ、このオブジェクトをアンインストールできなくなるためです。 このシナリオでは、セカンダリ サイト サーバーで標準の配布ポイントを有効にしてから、その標準の配布ポイントにコンテンツを再配布する方法があります。 この方法ではネットワーク帯域幅を使用しません。また、完了したら、サーバー共有の配布ポイントをアンインストールし、サーバー共有を削除してから、配布ポイントとセカンダリ サイトをアップグレードすることができます。  

 共有配布ポイントをアップグレードする前に、Configuration Manager 2007 の配布ポイントの構成を確認し、Configuration Manager 2007 で引き続き使用するセカンダリ サイトの配布ポイントをアップグレードしないようにします。 セカンダリ サイト サーバーの共有配布ポイントをアップグレードすると、サイト システム サーバーが Configuration Manager 2007 階層から削除され、その階層で使用できなくなるためです。 セカンダリ サイトが削除されると、そのセカンダリ サイトの残りの配布ポイントは孤立します。 つまり、Configuration Manager 2007 によって管理されなくなり、共有されなくなったり、アップグレードに適合しなくなります。  

> [! 警告]  
>  Configuration Manager コンソールで共有配布ポイントを表示しても、共有配布ポイントがリモート サイト システム サーバーに配置されていることや、セカンダリ サイト サーバーに配置されているかどうかは、表示されません。  

 リモート ネットワークの場所へのコンテンツの展開を制御するために主に使用されるセカンダリ サイトがリモート ネットワークの場所にある場合は、共有配布ポイントがあるセカンダリ サイトをアップグレードすることを検討してください。 System Center Configuration Manager 配布ポイントにコンテンツを配布するときは帯域幅の制御を構成できるため、多くの場合、セカンダリ サイトを配布ポイントにアップグレードして、その配布ポイントの帯域幅の制御を構成することにより、移行先階層のそのネットワークの場所にセカンダリ サイトをインストールすることを回避できます。  

 セカンダリ サイト サーバー上の共有配布ポイントをアップグレードする処理は、その他の共有配布ポイントのアップグレードと同様に動作します。 コンテンツは、移行先階層で使用される単一インスタンス ストアにコピーおよび変換されます。 ただし、セカンダリ サイト サーバーの共有配布ポイントをアップグレードする場合、アップグレード プロセスでは、管理ポイントがアンインストールされ (存在する場合)、そのサーバーからセカンダリ サイトもアンインストールされます。 その結果、セカンダリ サイトが Configuration Manager 2007 階層から削除されます。 セカンダリ サイトをアンインストールするために、Configuration Manager はソース サイトからデータを収集するために構成されているアカウントを使用します。  

 アップグレード時に、Configuration Manager 2007 セカンダリ サイトがアンインストールされてから、移行先階層の配布ポイントのインストールが開始されるまでには遅延があります。 データ収集サイクルに応じて、最大 4 時間の遅延が発生します。 この遅延は、新しい配布ポイントのインストールが開始される前に、セカンダリ サイトでアンインストールを行う時間を用意するために発生します。  

 共有配布ポイントのアップグレード方法の詳細については、「 [Planning to upgrade Configuration Manager 2007 shared distribution points](#Planning_to_Upgrade_DPs)」を参照してください。  

##  <a name="a-namebkmkreassigndistpointa-planning-to-reassign-system-center-configuration-manager-distribution-points"></a><a name="BKMK_ReassignDistPoint"></a> System Center Configuration Manager 配布ポイントの再割り当ての計画  
 サポートされるバージョンの System Center 2012 Configuration Manager から同じバージョンの階層に移行する場合、ソース階層の共有配布ポイントを移行先階層のサイトに再割り当てすることができます。 これは、Configuration Manager 2007 配布ポイントを移行先階層の配布ポイントにアップグレードする場合の概念と似ています。 プライマリ サイトとセカンダリ サイトの両方の配布ポイントを再割り当てできます。 この配布ポイントを再割り当てする処理によって、配布ポイントはソース階層から削除され、コンピューターとその配布ポイントが、移行先階層で選択したサイトのサイト システム サーバーに作成されます。  

 配布ポイントを再割り当てする場合、ソース サイトの配布ポイントでホストされていた移行済みコンテンツを再配布する必要はありません。 また、Configuration Manager 2007 配布ポイントのアップグレードとは異なり、配布ポイントの再割り当てのために、配布ポイントのコンピューターに追加のディスク領域は必要ありません。 System Center 2012 Configuration Manager 以降では、配布ポイントは、コンテンツに単一インスタンス ストア形式を使用しており、階層間で配布ポイントを再割り当てするときに、配布ポイントのコンピューターのコンテンツを変換する必要がないためです。  

 System Center 2012 Configuration Manager 配布ポイントが再割り当てに適合するためには、次の条件を満たしている必要があります。  

-   共有配布ポイントがサイト サーバーとは異なるコンピューターにインストールされている。  

-   共有配布ポイントが追加のサイト システムの役割と併置されていない。  

Configuration Manager コンソールの [**ソース階層**] ノードで再割り当て対象の配布ポイントを識別するには、ソース サイトを選択してから、[**共有配布ポイント**] タブを選択します。 適合する配布ポイントの [**再割り当ての条件に適合**] 列には、[**はい**] が表示されます (System Center 2012 R2 Configuration Manager より前のバージョンでは、この列の名前は [**アップグレードの条件に適合**] となっています)。  

###  <a name="a-namebkmkreassignprocessa-distribution-point-reassignment-process"></a><a name="BKMK_ReassignProcess"></a> 配布ポイントの再割り当てプロセス  
 Configuration Manager コンソールを使用して、アクティブなソース階層から共有した配布ポイントを再割り当てすることができます。 共有配布ポイントを再割り当てすると、その配布ポイントがソース サイトからアンインストールされ、移行先階層で指定したプライマリ サイトまたはセカンダリ サイトにアタッチされた配布ポイントとしてインストールされます。  

 配布ポイントを再割り当てするために、移行先階層は構成されている [ソース サイトのアクセス アカウント] を使用して、ソース サイトの SMS プロバイダーからデータを収集します。 必要なアクセス許可と追加の前提条件については、「[Prerequisites for migration in System Center Configuration Manager](../../core/migration/prerequisites-for-migration.md)」(System Center Configuration Manager の移行の前提条件) を参照してください。  

##  <a name="a-nameaboutmigratingcontenta-content-ownership-when-migrating-content"></a><a name="About_Migrating_Content"></a> コンテンツを移行するときのコンテンツの所有権  
 展開のためにコンテンツを移行する場合は、コンテンツ オブジェクトを移行先階層内のサイトに割り当てる必要があります。 このサイトが移行先階層内のコンテンツの所有者になります。 移行先階層の最上位サイトは、コンテンツのメタデータを実際に移行するサイトですが、ネットワーク経由でコンテンツの元のソース ファイルにアクセスするのは、割り当てられているサイトです。  

 コンテンツを移行するときに使用されるネットワーク帯域幅を最小限に抑えるために、コンテンツの所有権をソース階層のコンテンツの場所にネットワーク上で近い移行先階層のサイトに移動することを検討します。 移行先階層のコンテンツに関する情報はグローバルに共有されるため、すべてのサイトで利用できます。  

 コンテンツに関する情報はデータベース レプリケーションを使用してすべてのサイトで共有されますが、プラマリ サイトに割り当てられてから、他のプライマリ サイトの配布ポイントに展開されたコンテンツは、ファイル ベースのレプリケーションを使用して転送されます。 これは、中央管理サイトを経由して、その他のプライマリ サイトに転送されます。 コンテンツの所有者としてサイトを割り当てる場合、移行の前または移行中に複数のプライマリ サイトへの配布を計画しているパッケージを中央に集めると、帯域幅の狭いネットワークを使用したデータの転送を減らすことができます。  



<!--HONumber=Nov16_HO1-->

