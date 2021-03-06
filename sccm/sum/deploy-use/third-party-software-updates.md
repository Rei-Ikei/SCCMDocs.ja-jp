---
title: サード パーティの更新プログラムを有効にする
titleSuffix: Configuration Manager
description: Configuration Manager でサード パーティの更新プログラムを有効にする
ms.date: 07/30/2018
ms.prod: configuration-manager
ms.technology: configmgr-sum
ms.topic: conceptual
ms.assetid: 946b0f74-0794-4e8f-a6af-9737d877179b
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: 3c31b950ef59147f6f3f46c1cba7780b7789948c
ms.sourcegitcommit: 4b7812b505e80f79fc90dfa8a6db06eea79a3550
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2018
ms.locfileid: "42584418"
---
# <a name="enable-third-party-updates"></a>サード パーティの更新プログラムを有効にする 

*適用対象: System Center Configuration Manager バージョン 1806*

バージョン 1806 以降では、Configuration Manager コンソールの **[サード パーティのソフトウェア更新プログラムのカタログ]** ノードで、サード パーティのカタログをサブスクライブし、その更新プログラムをソフトウェア更新ポイント (SUP) に発行してからクライアントに展開できます。  <!--1357605, 1352101, 1358714-->



## <a name="prerequisites"></a>[前提条件] 
- 最上位のソフトウェア更新ポイントの WSUSContent フォルダーに、サード パーティのソフトウェア更新プログラム用のソース バイナリ コンテンツを格納するのに十分なディスク領域がある必要があります。
    - 必要な記憶域の容量は、ベンダー、更新プログラムの種類、および展開のために発行する特定の更新プログラムによって異なります。
    - 空き領域がより多い別のドライブに WSUSContent フォルダーを移動する必要がある場合は、「[How to change the location where WSUS stores updates locally](https://blogs.technet.microsoft.com/sus/2008/05/19/wsus-how-to-change-the-location-where-wsus-stores-updates-locally/)」 (WSUS で更新プログラムをローカルに格納する場所を変更する方法) のブログ記事を参照してください。
- サード パーティのソフトウェア更新プログラムの同期サービスにはインターネット アクセスが必要です。
    - パートナー カタログ一覧については、HTTPS ポート 443 経由での download.microsoft.com が必要です。 
    -  サード パーティのカタログおよび更新プログラムのコンテンツ ファイルへのインターネット アクセスを行います。 443 以外の追加のポートが必要な場合があります。
    - サード パーティの更新プログラムでは、SUP と同じプロキシ設定が使用されます。

## <a name="additional-requirements-when-the-sup-is-remote-from-the-top-level-site-server"></a>SUP が最上位サイト サーバーから離れている場合の追加の要件 

1. SUP が離れている場合、SUP で SSL を有効にする必要があります。 
    - [WSUS で SSL を構成する](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/2-configure-wsus#bkmk_2.5.ConfigSSL)
        - WSUS で SSL を構成する場合、一部の Web サービスおよび仮想ディレクトリは、HTTPS ではなく、常に HTTP であることに注意してください。 
        - Configuration Manager では、HTTP 経由で WSUS コンテンツ ディレクトリからソフトウェア更新プログラム パッケージ用のサード パーティのコンテンツがダウンロードされます。   
    - [SUP で SSL を構成する](../get-started/install-a-software-update-point.md#configure-ssl-communications-to-wsus)

2. 自己署名入りの WSUS 証明書の作成を許可するには: 
   - SUP サーバー上でリモート レジストリを有効にする必要があります。
   -  **WSUS サーバー接続アカウント**に、SUP/WSUS サーバーに対するリモート レジストリ アクセス許可が必要です。 


3. Configuration Manager サイト サーバーで次のレジストリ キーを作成します。 
    - `HKLM\Software\Microsoft\Update Services\Server\Setup`。値 `1` が設定された **EnableSelfSignedCertificates** という新しい DWORD を作成します。 

4. リモート SUP サーバー上で信頼された発行元ストアと信頼されたルート ストアへの証明書のインストールを有効にするには:
   - **WSUS サーバー接続アカウント**に、SUP サーバーに対するリモート管理アクセス許可が必要です。

    この要件を満たせない場合は、証明書をローカル コンピューターの WSUS ストアから信頼された発行元ストアと信頼されたルート ストアにエクスポートします。 

> [!NOTE] 
>SUP のサイト システムの役割プロパティにある **[プロキシとアカウントの設定]** タブを表示することで、**WSUS サーバー接続アカウント**を識別できます。 アカウントが指定されていない場合は、サイト サーバーのコンピューター アカウントが使用されます。

## <a name="enable-third-party-updates-on-the-sup"></a>SUP でサード パーティの更新プログラムを有効にする
このオプションを有効にした場合は、Configuration Manager コンソールでサード パーティの更新プログラム カタログをサブスクライブできます。 その後、これらの更新プログラムを WSUS に発行し、クライアントに展開できます。 階層ごとに 1 回、次の手順を行って、機能を使用できるように設定する必要があります。 最上位の SUP の WSUS サーバーを交換したことがある場合は、手順をもう一度行う必要がある可能性があります。 

1. Configuration Manager コンソールで、**[管理]** ワークスペースに移動します。 **[サイトの構成]** を展開し、**[サイト]** ノードを選択します。
2. 階層の最上位サイトを選択します。 リボンの **[サイト コンポーネントの構成]** をクリックし、**[ソフトウェアの更新ポイント]** を選択します。
3. **[サード パーティ製の更新プログラム]** タブに切り替えます。**[サード パーティ製のソフトウェア更新プログラムを有効にする]** オプションを選択します。

    ![サード パーティ製の更新プログラムの SUP プロパティのスクリーンショット](media/third-party-sup-properties.PNG)


## <a name="configure-the-wsus-signing-certificate"></a>WSUS 署名証明書を構成する
Configuration Manager でサード パーティの WSUS 署名証明書を自動的に管理するか、証明書を手動で構成する必要があるかを判断する必要があります。 

### <a name="automatically-manage-the-wsus-signing-certificate"></a>WSUS 署名証明書を自動的に管理する
PKI 証明書を使用する必要がない場合は、サード パーティの更新プログラム用の署名証明書を自動的に管理するように選択できます。 WSUS 証明書の管理は同期サイクルの一部として行われ、ログは `wsyncmgr.log` に記録されます。 

1. Configuration Manager コンソールで、**[管理]** ワークスペースに移動します。 **[サイトの構成]** を展開し、**[サイト]** ノードを選択します。
2. 階層の最上位サイトを選択します。 リボンの **[サイト コンポーネントの構成]** をクリックし、**[ソフトウェアの更新ポイント]** を選択します。
3. **[サード パーティ製の更新プログラム]** タブに切り替えます。**[Configuration Manager によって証明書を管理する]** オプションを選択します。 
4. **[管理]** ワークスペースの **[セキュリティ]** の下にある **[証明書]** ノードに、**サード パーティの WSUS 署名**という種類の新しい証明書が作成されます。  

### <a name="manually-manage-the-wsus-signing-certificate"></a>WSUS 署名証明書を手動で管理する
証明書を手動で構成する必要がある (PKI 証明書を使用する必要があるなど) 場合は、[System Center Updates Publisher](../tools/updates-publisher-options.md#update-server) または別のツールを使用して行う必要があります。  

1. [System Center Updates Publisher](../tools/updates-publisher-options.md#update-server) を使用して署名証明書を構成します。
2. Configuration Manager コンソールで、**[管理]** ワークスペースに移動します。 **[サイトの構成]** を展開し、**[サイト]** ノードを選択します。
3. 階層の最上位サイトを選択します。 リボンの **[サイト コンポーネントの構成]** をクリックし、**[ソフトウェアの更新ポイント]** を選択します。
4. **[サード パーティ製の更新プログラム]** タブに切り替えます。**証明書を手動で管理する**ためのオプションを選択します。


## <a name="enable-third-party-updates-on-the-clients"></a>クライアントでサード パーティの更新プログラムを有効にする
クライアント設定で、クライアントのサード パーティの更新プログラムを有効にします。 この設定により、[[イントラネットの Microsoft 更新サービスの保存場所にある署名済み更新を許可する]](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#BKMK_comp3) の Windows Update エージェント ポリシーが設定されます。 また、このクライアント設定により、クライアントの信頼された発行元ストアに WSUS 署名証明書がインストールされます。 証明書の管理ログはクライアントの `updatesdeployment.log` に示されます。  サード パーティの更新プログラムに使用するカスタム クライアント設定ごとに、これらの手順を行います。 詳細については、「[クライアント設定について](/sccm/core/clients/deploy/about-client-settings#Enable-third-party-software-updates)」の記事を参照してください。

1. Configuration Manager コンソールで、**[管理]** ワークスペースに移動して、**[クライアント設定]** ノードを選択します。
2. 既存のカスタム クライアント設定を選択するか、新しく作成します。 
3. 左側にある **[ソフトウェア更新プログラム]** タブを選択します。 このタブがない場合は、**[ソフトウェア更新プログラム]** ボックスが有効になっていることを確認します。
4. **[サード パーティ製のソフトウェア更新プログラムを有効にする]** を **[はい]** に設定します。 


## <a name="add-a-custom-catalog"></a>カスタム カタログを追加する
*パートナー カタログ*はソフトウェア ベンダーのカタログであり、Microsoft に既に登録されている情報が含まれています。 パートナー カタログには、追加情報を指定することなく、サブスクライブできます。 追加するカタログは*カスタム カタログ*と呼ばれます。 サード パーティの更新プログラム ベンダーから Configuration Manager にカスタム カタログを追加することができます。 カスタム カタログでは https を使用する必要があり、更新プログラムにはデジタル署名が必要です。 

1. **[Software Updates Library]\(ソフトウェア更新プログラム ライブラリ\)** ワークスペースに移動し、**[ソフトウェア更新プログラム]** を展開して、**[サード パーティのソフトウェア更新プログラムのカタログ]** ノードを選択します。 
   
     ![サード パーティの更新プログラム ノードのスクリーンショット](media/third-party-updates-node.PNG)
2. リボンで **[Add Custom Catalog]\(カスタム カタログの追加\)** をクリックします。 

     ![サード パーティの更新プログラムのカスタム カタログの追加](media/third-party-updates-custom-catalog.png)
1. **[全般]** ページで、次の項目を指定します。 
    - **[ダウンロード URL]**: カスタム カタログの有効な HTTPS アドレス。
    - **[発行者]**: カタログを発行している組織の名前。 
    - **[名前]**: Configuration Manager コンソールに表示されるカタログの名前。 
    - **[説明]**: カタログの説明。 
    - **[サポート URL]** (省略可能): カタログのヘルプを取得するための Web サイトの有効な HTTPS アドレス。 
    - **[サポートの連絡先]** (省略可能): カタログのヘルプを得るための連絡先情報。 
2. **[次へ]** をクリックして、カタログの概要を確認し、操作を続行して**サード パーティのソフトウェア更新プログラムのカスタム カタログ ウィザード**を完了します。


## <a name="subscribe-to-a-third-party-catalog-and-sync-updates"></a>サード パーティのカタログをサブスクライブして更新プログラムを同期する
Configuration Manager コンソールでサード パーティのカタログをサブスクライブするときに、カタログ内のすべての更新プログラムのメタデータが SUP の WSUS サーバーに同期されます。 メタデータの同期により、更新プログラムのいずれかが適用可能かどうかをクライアントで判断できるようになります。 サブスクライブするサード パーティのカタログごとに、次の手順を行います。  

1. Configuration Manager コンソールで、**[ソフトウェア ライブラリ]** ワークスペースに移動します。 **[ソフトウェア更新プログラム]** を展開し、**[サード パーティのソフトウェア更新プログラムのカタログ]** ノードを選択します。  
2. サブスクライブするカタログを選択し、リボンの **[カタログのサブスクライブ]** をクリックします。 
    ![サード パーティの更新プログラムのカスタム カタログの追加](media/third-party-updates-subscribe.png)
1. カタログの証明書を確認して承認します。  
    >[!NOTE]
    
    > サード パーティのソフトウェア更新プログラム カタログをサブスクライブすると、ウィザードで確認して承認した証明書がサイトに追加されます。 この証明書の種類は、**サード パーティ製のソフトウェア更新プログラム カタログ**です。 これは、**[管理]** ワークスペースの **[セキュリティ]** の下にある **[証明書]** ノードから管理できます。  
2. ウィザードを完了します。 初期サブスクリプションの後、数分でカタログのダウンロードが開始されます。 
    - カタログは 7 日おきに自動的に同期されます。
    - 強制的に同期するには、リボンの **[今すぐ同期]** をクリックします。
3. カタログがダウンロードされた後、WSUS データベースから Configuration Manager データベースに製品メタデータを同期する必要があります。 製品情報を同期するには、[ソフトウェア更新プログラムの同期を手動で開始](../get-started/synchronize-software-updates.md#manually-start-software-updates-synchronization)します。
4. 製品情報が同期されたら、Configuration Manager に[必要な製品を同期するように SUP を構成](../get-started/configure-classifications-and-products.md#to-configure-classifications-and-products-to-synchronize)します。  
5. Configuration Manager に新しい製品の更新プログラムを同期するには、[ソフトウェア更新プログラムの同期を手動で開始](../get-started/synchronize-software-updates.md#manually-start-software-updates-synchronization)します。  
6. 同期が完了したら、**[すべての更新プログラム]** ノードでサード パーティの更新プログラムを確認できます。 これらの更新プログラムは、発行するように選択するまで、**メタデータのみ**の更新プログラムとして発行されます。 
     - 青い矢印の付いたアイコンは、メタデータのみのソフトウェア更新プログラムを表します。 ![メタデータのみのソフトウェア更新プログラムのアイコン](media/MetadataOnly.png)


## <a name="publish-and-deploy-third-party-software-updates"></a>サード パーティのソフトウェア更新プログラムを発行して展開する 
サード パーティの更新プログラムが **[すべての更新プログラム]** ノードに表示されたら、展開のために発行する更新プログラムを選択できます。 更新プログラムを発行するときに、ベンダーからバイナリ ファイルがダウンロードされ、最上位の SUP の WSUSContent ディレクトリに配置されます。 

1. Configuration Manager コンソールで、**[ソフトウェア ライブラリ]** ワークスペースに移動します。 **[ソフトウェア更新プログラム]** を展開し、**[すべてのソフトウェア更新プログラム]** ノードを選択します。
2. **[条件の追加]** をクリックして、更新プログラムのリストをフィルター処理します。 たとえば、**HP** の**ベンダー**を追加し、 HP からのすべての更新プログラムを表示します。  
3. 組織に必要な更新プログラムを選択します。 **[サード パーティ製のソフトウェア更新プログラムのコンテンツを発行する]** をクリックします。
    - この操作で、ベンダーからの更新プログラム バイナリがダウンロードされ、最上位のソフトウェア更新ポイントの WSUSContent フォルダーに格納されます。 
4. 発行される更新プログラムの状態を、メタデータのみからコンテンツを含む展開可能な更新プログラムに変更するには、[ソフトウェア更新プログラムの同期を手動で開始](../get-started/synchronize-software-updates.md#manually-start-software-updates-synchronization)します。 
    >[!NOTE]
    >サード パーティ製のソフトウェア更新プログラムのコンテンツを発行するときに、コンテンツの署名に使用されるすべての証明書がサイトに追加されます。 これらの証明書の種類は、**サード パーティ製のソフトウェア更新プログラムのコンテンツ**です。 これらは、**[管理]** ワークスペースの **[セキュリティ]** の下にある **[証明書]** ノードから管理できます。  

5. SMS_ISVUPDATES_SYNCAGENT.log で進行状況を確認します。 ログは、最上位のソフトウェア更新ポイントのサイト システムの Logs フォルダーにあります。
6. [ソフトウェア更新プログラムの展開](../deploy-use/deploy-software-updates.md)プロセスを使用して、更新プログラムを展開します。 
7. **ソフトウェア更新プログラムの展開ウィザード**の **[ダウンロード場所]** ページで、**[インターネットからソフトウェア更新プログラムをダウンロードする]** という既定のオプションを選択します。 このシナリオでは、展開パッケージのコンテンツのダウンロードに使用される、ソフトウェア更新ポイントにコンテンツが既に発行されています。
8. コンプライアンス結果を表示するには、クライアントで更新プログラムをスキャンして評価する必要があります。  **ソフトウェア更新プログラムのスキャン サイクル**操作を実行することで、クライアントの Configuration Manager コンソール パネルからこのサイクルを手動でトリガーできます。


## <a name="monitoring-progress-of-third-party-software-updates"></a>サード パーティのソフトウェア更新プログラムの進行状況の監視 

サード パーティのソフトウェア更新プログラムの同期は、最上位の既定のソフトウェア更新ポイントにある SMS_ISVUPDATES_SYNCAGENT コンポーネントによって処理されます。 このコンポーネントからステータス メッセージを表示したり、SMS_ISVUPDATES_SYNCAGENT.log でより詳細な状態を確認したりすることができます。 このログは、最上位のソフトウェア更新ポイントのサイト システムの Logs フォルダーにあります。 既定では、このパスは C:\Program Files\Microsoft Configuration Manager\Logs です。 一般的なソフトウェア更新プログラム管理プロセスの監視の詳細については、[ソフトウェア更新プログラムの監視](../deploy-use/monitor-software-updates.md)に関するページを参照してください。 

## <a name="known-issues"></a>既知の問題

- コンソールが実行されているコンピューターは、WSUS から更新プログラムをダウンロードして、更新プログラム パッケージに追加するために使用されます。 WSUS 署名証明書は、コンソール コンピューターで信頼されている必要があります。 信頼されていない場合は、サード パーティの更新プログラムのダウンロード中に署名確認に関する問題が発生する可能性があります。 
- サード パーティのソフトウェア更新プログラムの同期サービスでは、SCUP などの別のアプリケーション、ツール、またはスクリプトで WSUS に追加されたメタデータのみの更新プログラムにコンテンツを発行することはできません。 これらの更新プログラムに対する**サード パーティ製のソフトウェア更新プログラムのコンテンツを発行する**操作は失敗します。 この機能でまだサポートされていないサード パーティの更新プログラムを展開する必要がある場合は、完全に既存のプロセスを使用して更新プログラムを展開してください。  
-  Configuration Manager には、カタログの cab ファイル形式用の新しいバージョンがあります。 新しいバージョンには、ベンダーのバイナリ ファイル用の証明書が含まれています。 カタログを承認して信頼すると、これらの証明書は **[管理]** ワークスペースの **[セキュリティ]** の下にある **[証明書]** ノードに追加されます。  
     - ダウンロード URL が https であり、更新プログラムが署名されている限り、古いカタログの cab ファイル バージョンを引き続き使用できます。 バイナリの証明書が cab ファイル内になく、まだ承認されていないため、コンテンツの発行は失敗します。 **[証明書]** ノードで証明書を見つけてブロックを解除し、更新プログラムをもう一度発行することで、この問題を回避できます。 別の証明書で署名された複数の更新プログラムを発行する場合は、使用される各証明書のブロックを解除する必要があります。
     - 詳細については、以下のステータス メッセージ表の 11523 と 11524 のステータス メッセージを参照してください。
-  最上位のソフトウェア更新ポイントにあるサード パーティ製ソフトウェア更新同期サービスで、インターネット アクセスのためにプロキシ サーバーが必要な場合は、デジタル署名のチェックが失敗する可能性があります。 この問題を緩和するには、サイト システムで WinHTTP プロキシ設定を構成します。 詳しくは、「[Netsh commands for WinHTTP](https://go.microsoft.com/fwlink/p/?linkid=199086)」(WinHTTP 用の Netsh コマンド) をご覧ください。

## <a name="status-messages"></a>ステータス メッセージ

| MessageID       | 重大度           | 説明 | 考えられる原因| 解決方法
| ------------- |-------------| -----|----|----|
| 11516     | エラー |コンテンツが署名されていないため、更新プログラム "更新プログラム ID" のコンテンツを発行できませんでした。  発行できるのは、署名が有効なコンテンツのみです。  |Configuration Manager では、署名されていない更新プログラムの発行は許可されません。| 別の方法で更新プログラムを発行します。 </br></br>署名された更新プログラムがベンダーから入手可能かどうかを確認してください。|
| 11523  | 警告 |  カタログ "X" にはコンテンツ署名証明書が含まれていません。コンテンツ署名証明書が追加および承認されるまでは、このカタログの更新プログラム用の更新プログラム コンテンツを発行しようとしても失敗する可能性があります。 | このメッセージは、古いバージョンの cab ファイル形式を使用しているカタログをインポートするときに表示される可能性があります。|カタログのプロバイダーに連絡して、コンテンツ署名証明書を含む更新されたカタログを入手してください。 </br> </br> バイナリの証明書が cab ファイルに含まれていないため、コンテンツの発行は失敗します。 **[証明書]** ノードで証明書を見つけてブロックを解除し、更新プログラムをもう一度発行することで、この問題を回避できます。 別の証明書で署名された複数の更新プログラムを発行する場合は、使用される各証明書のブロックを解除する必要があります。|
| 11524| エラー  | 更新プログラムのメタデータがないため、更新プログラム "ID" を発行できませんでした。 | 更新プログラムが Configuration Manager の外部の WSUS に同期されている可能性があります。| 更新プログラムのコンテンツの発行を試みる前に、更新プログラムを Configuration Manager に同期します。  </br> </br>**メタデータのみ**として更新プログラムを発行するために外部ツールが使用された場合は、それと同じツールを使用して、更新プログラムのコンテンツを発行します。|



## <a name="working-with-third-party-updates-video"></a>サード パーティの更新プログラムの操作に関するビデオ
<iframe width="560" height="315" src="https://www.youtube.com/embed/ai8rLCLtuTI?rel=0" frameborder="0" allowfullscreen></iframe>



## <a name="next-step"></a>次のステップ
> [!div class="nextstepaction"]
> [ソフトウェア更新プログラムの展開](./deploy-software-updates.md)
