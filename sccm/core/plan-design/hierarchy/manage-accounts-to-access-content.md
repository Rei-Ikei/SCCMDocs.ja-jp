---
title: コンテンツにアクセスするためのアカウント
titleSuffix: Configuration Manager
description: クライアントが System Center Configuration Manager のコンテンツにアクセスするためのアカウントについて説明します。
ms.date: 2/6/2017
ms.prod: configuration-manager
ms.technology: configmgr-other
ms.topic: conceptual
ms.assetid: a7df9d0f-fbde-47eb-97e7-3d29536424fa
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: c21601246cb0024837256b7cf8d4c1576c00149d
ms.sourcegitcommit: 0b0c2735c4ed822731ae069b4cc1380e89e78933
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
ms.locfileid: "32338688"
---
# <a name="manage-accounts-to-access-content-in-system-center-configuration-manager"></a>System Center Configuration Manager でコンテンツへのアクセスを管理する

*適用対象: System Center Configuration Manager (Current Branch)*

System Center Configuration Manager でコンテンツを展開する前に、配布ポイントのコンテンツにクライアントからどのようにアクセスすることになるかを考えてください。 この記事では、その際に使用されるアカウントについて説明します。

-   **ネットワーク アクセス アカウント**。 クライアントが配布ポイントに接続してコンテンツにアクセスするために使用します。 既定では、クライアントは最初にコンピューター アカウントを使用します。

     このアカウントは、リモート フォレスト内のソース配布ポイントからコンテンツを取得するためにプル配布ポイントによっても使用されます  

-   **パッケージ アクセス アカウント**。 既定では、Configuration Manager は、**ユーザー**および **管理者**という名前の組み込みアカウントに対して配布ポイント上のコンテンツへのアクセスを許可します。 追加のアクセス許可を設定してアクセスを制限することができます。  

-   **マルチキャスト接続アカウント**。 オペレーティング システムの展開に使用します。  

##  <a name="bkmk_NAA"></a> ネットワーク アクセス アカウント  
 ネットワーク アクセス アカウントは、クライアント コンピューターが、そのローカルのコンピューター アカウントで配布ポイントのコンテンツにアクセスできない場合に使用します。 たとえば、このアカウントは、ワークグループ クライアントや信頼されていないドメインのコンピューターに適用されます。 このアカウントは、オペレーティング システムの展開中に、オペレーティング システムをインストールするコンピューターにドメインのコンピューター アカウントがない場合にも使用されます。  

-   クライアントでは、ネットワーク上のリソースにアクセスする場合のみネットワーク アクセス アカウントが使用されます。  

-   プライマリ サイトごとに、ネットワーク アクセス アカウントとして使用する複数のアカウントを設定できます。  

-   クライアントは、最初に *computername*$ アカウントを使用して、配布ポイントのコンテンツへのアクセスを試みます。 このアカウントで失敗した場合、クライアントは、ネットワーク アクセス アカウントの使用を試みます。 以降は、失敗した場合でも、ネットワーク アクセス アカウントでのアクセスが試行されます。  

### <a name="permissions"></a>アクセス許可
このアカウントには、クライアントで必要となるコンテンツのために、ソフトウェアにアクセスする最低限の適切なアクセス許可を付与します。  

-   このアカウントは、配布ポイントで **[ネットワーク経由でコンピューターへアクセス]** の権限を持っている必要があります。   

-   このアカウントは、リソースへの必要なアクセスを可能にする任意のドメインに作成します。 ネットワーク アクセス アカウントには、常にドメイン名が含まれている必要があります。 このアカウントでは、パススルー セキュリティはサポートされません。 複数のドメインに配布ポイントがある場合は、信頼される側のドメインにアカウントを作成します。  

> [!TIP]  
>  アカウントのロックアウトを避けるため、既存のネットワーク アクセス アカウントのパスワードは変更しないでください。 代わりに、新しいアカウントを作成して、Configuration Manager でその新しいアカウントを設定します。 すべてのクライアントが新しいアカウントの詳細情報を受け取ったら、ネットワーク共有フォルダーから古いアカウントを削除して、古いアカウントを削除します。  

> [!IMPORTANT]  
>  このアカウントに対話型サインイン権限を付与しないでください。  
>   
>  コンピューターがドメインに参加する権限をこのアカウントに付与しないでください。 タスク シーケンス中にコンピューターをドメインに参加させる必要がある場合は、タスク シーケンス エディターのドメイン参加アカウントを使用します。  

### <a name="to-configure-the-network-access-account"></a>ネットワーク アクセス アカウントを構成するには  

1.  Configuration Manager コンソールで、**[管理]** >   **[サイトの構成]** >  **[サイト]** の順に選択して、サイトを選択します。  

2.  **[設定]** グループで、**[サイト コンポーネントの構成]** > **[ソフトウェアの配布]** を選択します。  

3.  **[ネットワーク アクセス アカウント]** タブを選択します。1 つまたは複数のアカウントを設定して、**[OK]** を選択します。  

##  <a name="bkmk_Paa"></a> パッケージ アクセス アカウント  
 パッケージ アクセス アカウントを使用すると、NTFS ファイル システムのアクセス許可を設定して、配布ポイントのパッケージ コンテンツにアクセスできるユーザーおよびユーザー グループを指定することができます。 既定では、汎用の**ユーザー** アカウントと**管理者**アカウントにのみ、Configuration Manager によってアクセス権が付与されます。 その他の Windows アカウントや Windows グループを使用してクライアント コンピューターのアクセスを制御することもできます。 パッケージ アクセス アカウントは、モバイル デバイスでは使用されません。モバイル デバイスでは常に、パッケージのコンテンツが匿名で取得されます。  

 既定では、Configuration Manager がパッケージ内のコンテンツ ファイルを配布ポイントにコピーするときに、ローカルの **Users** グループに**読み取り**アクセスを、ローカルの **Administrators** グループに**フル コントロール**を許可します。 実際にどのようなアクセス許可が必要になるかはパッケージによって異なります。 クライアントがワークグループや信頼されていないフォレストに含まれている場合は、そのクライアントはネットワーク アクセス アカウントを使用してパッケージ コンテンツにアクセスします。 定義されたパッケージ アクセス アカウントを使用して、パッケージに対するアクセス許可がネットワーク アクセス アカウントにあることを確認してください。  

 配布ポイントにアクセスできるドメイン内のアカウントを使用します。 パッケージの作成後にアカウントを作成または変更した場合は、パッケージを再配布する必要があります。 パッケージを更新しても、パッケージに関する NTFS ファイル システムのアクセス許可は変更されません。  

 ネットワーク アクセス アカウントは、 **Users** グループのメンバーシップによって自動的に追加されるため、パッケージ アクセス アカウントとして追加する必要はありません。 パッケージ アクセス アカウントをネットワーク アクセス アカウントのみに制限しても、クライアントによるパッケージへのアクセスを妨げることはありません。  

### <a name="to-manage-access-accounts"></a>アクセス アカウントを管理するには  

1.  Configuration Manager コンソールで、**[ソフトウェア ライブラリ]** を選択します。  

2.  **[ソフトウェア ライブラリ]** ワークスペースで、アクセス アカウントの管理対象となるコンテンツの種類を決定し、それぞれに用意されている手順に従います。  

    -   **アプリケーション**: **[アプリケーション管理]** を展開し、**[アプリケーション]** をクリックして、アクセス アカウントを管理するアプリケーションを選択します。  

    -   **パッケージ**: **[アプリケーション管理]** を展開し、**[パッケージ]** を選択して、アクセス アカウントを管理するパッケージを選択します。  

    -   **展開パッケージ**: **[ソフトウェア更新プログラム]** を展開し、**[展開パッケージ]** を選択して、アクセス アカウントを管理する展開パッケージを選択します。  

    -   **ドライバー パッケージ**: **[オペレーティング システム]** を展開し、**[ドライバー パッケージ]** を選択して、アクセス アカウントを管理するドライバー パッケージを選択します。  

    -   **オペレーティング システム イメージ**: **[オペレーティング システム]** を展開し、**[オペレーティング システム イメージ]** を選択して、アクセス アカウントを管理するオペレーティング システム イメージを選択します。  

    -   **オペレーティング システム インストーラー**: **[オペレーティング システム]** を展開し、**[オペレーティング システム インストーラー]** を選択して、アクセス アカウントを管理するオペレーティング システム インストーラーを選択します。  

    -   **ブート イメージ**: **[オペレーティング システム]** を展開し、**[ブート イメージ]** を選択して、アクセス アカウントを管理するブート イメージを選択します。  

3.  選択したオブジェクトを右クリックして、**[アクセス アカウントの管理]** を選択します。  

4.  **[アカウントの追加]** ダイアログ ボックスで、コンテンツに対するアクセス許可が付与されるアカウントの種類を指定して、アカウントに関連付けられているアクセス権を指定します。  

    > [!NOTE]  
    >  アカウントのユーザー名を追加したときに Configuration Manager が同じ名前のローカル ユーザー アカウントとドメイン ユーザー アカウントを検出した場合、Configuration Manager は、ローカル ユーザー アカウントのアクセス権を設定します。  

##  <a name="bkmk_multi"></a> マルチキャスト接続アカウント  
 マルチキャスト接続アカウントは、サイト データベースから情報を読み取るために、マルチキャスト用に設定されている配布ポイントで使用されます。  

-   マルチキャストの Configuration Manager データベース接続を設定するときに使用するアカウントを指定します。  

-   既定では、配布ポイントのコンピューター アカウントが使用されますが、代わりにユーザー アカウントを設定することもできます。  

-   サイト データベースが信頼されていないフォレストにある場合は常に、ユーザー アカウントを指定する必要があります。  

-   アカウントは、サイト データベースに対する**読み取り**アクセス許可を持っている必要があります。  

たとえば、データ センターでは、サイト サーバーとサイト データベースとは別にフォレストに境界ネットワークがある場合、このアカウントを使用してマルチキャスト情報をサイト データベースから読み込むことができます。

このアカウントを作成する場合は、Microsoft SQL Server を実行しているコンピューター上の権限の低いローカル アカウントとして作成します。  

> [!IMPORTANT]  
>  このアカウントに対話型サインイン権限を付与しないでください。  
