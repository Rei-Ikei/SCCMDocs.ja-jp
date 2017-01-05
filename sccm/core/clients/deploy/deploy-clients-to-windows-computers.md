---
title: "Windows クライアントの展開 | Microsoft Docs"
description: "System Center Configuration Manager でクライアントを Windows コンピューターに展開する方法を説明します。"
ms.custom: na
ms.date: 10/06/2016
ms.prod: configuration-manager
ms.reviewer: na
ms.suite: na
ms.technology:
- configmgr-client
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 341f0d0b-f907-44cf-9e10-e1b41fc15f82
caps.latest.revision: 13
caps.handback.revision: 0
author: nbigman
ms.author: nbigman
manager: angrobe
translationtype: Human Translation
ms.sourcegitcommit: 55c953f312a9fb31e7276dde2fdd59f8183b4e4d
ms.openlocfilehash: daf9ebae82eb5933e211a08d61f0c82a261d4aa7


---
# <a name="how-to-deploy-clients-to-windows-computers-in-system-center-configuration-manager"></a>System Center Configuration Manager でクライアントを Windows コンピューターに展開する方法

*適用対象: System Center Configuration Manager (Current Branch)*

コンピューターに System Center Configuration Manager クライアント ソフトウェアをインストールする際、さまざまなクライアント展開方法を使用できます。 どの展開方法を使用するか決定するうえで、「[System Center Configuration Manager でのクライアントのインストール方法](../../../core/clients/deploy/plan/client-installation-methods.md)」の情報が役立ちます。  

 構成マネージャー クライアントをインストールする前に、すべての前提条件を満たしており、必要なすべての展開構成を完了していることを確認します。 詳細については、「[System Center Configuration Manager で Windows コンピューターにクライアントを展開するための前提条件](../../../core/clients/deploy/prerequisites-for-deploying-clients-to-windows-computers.md)」をご覧ください。  


##  <a name="a-namebkmkclientpusha-how-to-install-clients-with-client-push"></a><a name="BKMK_ClientPush"></a> クライアント プッシュを使用したクライアントのインストール方法  

 クライアント プッシュ インストールは、Configuration Manager によって検出されたコンピューターに構成マネージャー クライアント ソフトウェアをインストールするために使用します。 クライアント プッシュ インストールはサイトに対して構成でき、サイトの境界が境界グループとして構成されている場合、クライアント インストールはこれらの構成済みの境界内で検出されたコンピューターで自動的に実行されます。 特定のコレクションまたはコレクション内のリソースでクライアント プッシュ インストール ウィザードを実行して、クライアント プッシュ インストールを開始することもできます。  

 また、クライアント プッシュ インストール ウィザードを使用して、クエリを実行して得た結果に構成マネージャー クライアントをインストールすることもできます。 このシナリオでインストールが成功するためには、選択したクエリで返される項目のいずれかが、属性クラス [**システム リソース**] の属性 [**ResourceID**] である必要があります。 クエリに関する詳細については、「[System Center Configuration Manager のクエリのテクニカル リファレンス](../../../core/servers/manage/queries-technical-reference.md)」を参照してください。  

 サイト サーバーがクライアント コンピューターにアクセスできない場合やセットアップ プロセスを開始できない場合は、成功するまで自動的に最大 7 日間、1 時間置きにインストールが試みられます。  

 クライアントのインストール プロセスを追跡するには、クライアントをインストールする前に、フォールバック ステータス ポイント サイト システムをインストールします。 インストールしたフォールバック ステータス ポイントは、クライアント プッシュ インストール方法でクライアントをインストールするときに、自動的にクライアントに割り当てられます。 クライアントのインストールの進行状況は、クライアント展開レポートとクライアントの割り当てレポートで確認します。 また、クライアント ログ ファイルにはトラブルシューティングに役立つ詳しい情報が記録されるため、フォールバック ステータス ポイントをインストールする必要はありません。 たとえば、サイト サーバー上の CCM.log ファイルには、サイト サーバーがコンピューターに接続するときに発生したすべての問題が記録され、クライアント上の CCMSetup.log ファイルには、インストール プロセスが記録されます。  

> [!IMPORTANT]  
>  クライアント プッシュが正常に実行されるように、前提条件がすべて満たされていることを確認してください。 その一覧は、「[System Center Configuration Manager で Windows コンピューターにクライアントを展開するための前提条件](../../../core/clients/deploy/prerequisites-for-deploying-clients-to-windows-computers.md)」の「インストール方法の依存関係」というセクションに示されています。  

### <a name="to-configure-the-site-to-automatically-use-client-push-for-discovered-computers"></a>検出されたコンピューターに対して、サイトが自動的にクライアント プッシュ インストールを使用するように構成するには

1.  Configuration Manager コンソールで、[**管理**] をクリックします。  

2.  [**管理**] ワークスペースで [**サイトの構成**] を展開して、[**サイト**] をクリックします。  

3.  [**サイト**] 一覧で、サイト全体の自動クライアント プッシュ インストールを構成するサイトを選択します。  

4.  [**ホーム**] タブの [**設定**] グループで、[**クライアント インストール設定**] をクリックし、[**クライアント プッシュ インストール**] をクリックします。  

5.  [**クライアント プッシュ インストールのプロパティ**] ダイアログ ボックスの [**全般**] タブで、[**サイト全体にクライアントを自動的にプッシュ インストールする**] を選択します。 **[サーバー]**、**[ワークステーション]**、または **[Configuration Manager サイト システム サーバー]** を選択して、Configuration Manager によってクライアント ソフトウェアをプッシュするシステムの種類を選択します。 既定の選択は **[サーバー]** と **[ワークステーション]**です。  

6.  サイト全体の自動クライアント プッシュ インストールによってドメイン コントローラーに構成マネージャー クライアント ソフトウェアをインストールするかどうかを選択します。  

7.  **[アカウント]** タブで、クライアント ソフトウェアをインストールするためにコンピューターに接続するときに Configuration Manager が使用するアカウントを 1 つまたは複数指定します。 [**作成**] アイコンをクリックし、[**ユーザー名**] と [**パスワード**] を入力して、パスワードを確認入力してから、[**OK**] をクリックします。 少なくとも 1 つのクライアント プッシュ インストール アカウントを指定する必要があります。このアカウントは、クライアントをインストールするすべてのコンピューターのローカル管理者権限を持っている必要があります。 クライアント プッシュ インストール アカウントを指定しないと、Configuration Manager はサイト システムのコンピューター アカウントを使おうとします。これにより、ドメイン間のクライアント プッシュが失敗します。  

    > [!IMPORTANT]  
    >  クライアント プッシュ インストール アカウントのパスワードは 38 文字以下に制限されます。  

    > [!NOTE]  
    >  クライアント プッシュ インストール方法をセカンダリ サイトから使用する場合、クライアント プッシュを開始するセカンダリ サイトでアカウントを指定する必要があります。  
    >   
    >  クライアント プッシュ インストール アカウントの詳細については、次の手順「クライアント プッシュ インストール ウィザードを使用するには」をご覧ください。  

8.  **[インストールのプロパティ]** タブで、構成マネージャー クライアントをインストールするときに使用するインストール プロパティを指定します。 Windows インストーラー パッケージ (Client.msi) のインストール プロパティと CCMSetup.exe の次のプロパティを指定できます。  

    -   /forcereboot  

    -   /skipprereq  

    -   /logon  

    -   /BITSPriority  

    -   /downloadtimeout  

    -   /forceinstall  

     スキーマが Configuration Manager 向けに拡張されており、インストール プロパティを指定せずに CCMSetup を実行するクライアント インストールで読み取られる場合は、このタブで指定したクライアント インストール プロパティは、Active Directory Domain Services に発行されます。 クライアント インストールのプロパティの詳細については、「[System Center Configuration Manager のクライアント インストール プロパティについて](../../../core/clients/deploy/about-client-installation-properties.md)」を参照してください。  

    > [!NOTE]  
    >  セカンダリ サイトでクライアント プッシュ インストールを有効にする場合は、SMSSITECODE プロパティがその親プライマリ サイトの Configuration Manager サイト名に設定されていることを確認してください。 Active Directory スキーマが Configuration Manager 用に拡張されている場合は、これを AUTO を設定して、正しいサイトの割り当てを自動的に検索することもできます。  

### <a name="to-use-the-client-push-installation-wizard"></a>クライアント プッシュ インストール ウィザードを使用するには

1.  Configuration Manager コンソールで、[**管理**] をクリックします。  

2.  [**管理**] ワークスペースで [**サイトの構成**] を展開して、[**サイト**] をクリックします。  

3.  [**サイト**] 一覧で、サイト全体の自動クライアント プッシュ インストールを構成するサイトを選択します。  

4.  [**ホーム**] タブの [**設定**] グループで、[**クライアント インストール設定**] をクリックし、[**クライアント プッシュ インストール**] をクリックします。  

5.  **[インストールのプロパティ]** タブで、構成マネージャー クライアントをインストールするときに使用するインストール プロパティを指定します。 Windows インストーラー パッケージ (Client.msi) のインストール プロパティと CCMSetup.exe の次のプロパティを指定できます。  

    -   /forcereboot  

    -   /skipprereq  

    -   /logon  

    -   /BITSPriority  

    -   /downloadtimeout  

    -   /forceinstall  

     スキーマが Configuration Manager 向けに拡張されており、インストール プロパティを指定せずに CCMSetup を実行するクライアント インストールで読み取られる場合は、このタブで指定したクライアント インストール プロパティは、Active Directory Domain Services に発行されます。 クライアント インストールのプロパティの詳細については、「[System Center Configuration Manager のクライアント インストール プロパティについて](../../../core/clients/deploy/about-client-installation-properties.md)」を参照してください。  

6.  Configuration Manager コンソールで、 **[資産とコンプライアンス]**をクリックします。  

7.  [**資産とコンプライアンス**] ワークスペースで、1 つ以上のコンピューター、またはコンピューターのコレクションを選択します。  

8.  [**ホーム**] タブで、次のいずれかを選択します。  

    -   単一のコンピューターまたは複数のコンピューターにクライアントをインストールする場合は、[**デバイス**] グループで [**クライアントのインストール**] をクリックします。  

    -   コンピューターのコレクションにクライアントをインストールする場合は、[**コレクション**] グループで [**クライアントのインストール**] をクリックします。  

9. **クライアントのインストール ウィザード**の [**開始する前に**] ページで情報を確認してから、[**次へ**] をクリックします。  

10. [**インストール オプション**] ページで、クライアントをドメイン コントローラーにインストールできるかどうか、既存のクライアントがインストールされているコンピューターでクライアントの再インストール、アップグレード、または修復を行うかどうか、およびクライアント ソフトウェアをインストールするサイトの名前を構成します。 **[次へ]**をクリックします。  

11. インストール設定を確認してから、ウィザードを閉じます。  

> [!NOTE]  
>  サイトがクライアント プッシュ用に構成されていない場合でも、ウィザードを使用してクライアントをインストールできます。  

##  <a name="a-namebkmkclientsupa-how-to-install-clients-with-software-update-based-installation"></a><a name="BKMK_ClientSUP"></a> ソフトウェアの更新に基づいたインストールを使用したクライアントのインストール方法  
 ソフトウェアの更新に基づいたクライアント インストールでは、構成マネージャー クライアントを追加のソフトウェア更新プログラムとしてソフトウェアの更新ポイントに発行します。 このクライアント インストール方法を使用すると、クライアントがインストールされていないコンピューターに構成マネージャー クライアントをインストールしたり、既存の構成マネージャー クライアントをアップグレードしたりすることができます。  

 コンピューターに構成マネージャー クライアントがインストールされている場合は、Configuration Manager により、ソフトウェア更新プログラムを取得するソフトウェアの更新ポイントのサーバー名とポートがクライアントに提供されます。 この情報は、クライアント ポリシーに含まれています。  

> [!IMPORTANT]  
>  ソフトウェアの更新に基づいたインストールを使用するには、クライアントのインストールとソフトウェア更新プログラムに同じ Windows Server Update Services (WSUS) サーバーを使用する必要があります。 このサーバーは、プライマリ サイトでアクティブなソフトウェアの更新ポイントである必要があります。 詳細については、「[Install a software update point](../../../sum/get-started/install-a-software-update-point.md)」 (ソフトウェアの更新ポイントのインストール) をご覧ください。  

 コンピューターに構成マネージャー クライアントがインストールされていない場合は、ソフトウェアの更新ポイントのサーバー名を指定するように Active Directory Domain Services のグループ ポリシー オブジェクト (GPO) を構成して割り当てて、コンピューターがソフトウェア更新プログラムを取得できるようにする必要があります。  

 ソフトウェアの更新に基づいたクライアント インストールに、コマンド ライン プロパティを追加することはできません。 Configuration Manager 用に Active Directory スキーマを拡張した場合、クライアント コンピューターはインストール時に、自動的に Active Directory Domain Services にインストール プロパティを照会します。  

 Active Directory スキーマを拡張していない場合は、グループ ポリシーを使用して、サイト内のコンピューターにクライアント インストール設定をプロビジョニングできます。 これらの設定は、ソフトウェアの更新に基づいたクライアント インストールに自動的に適用されます。 詳細については、「[クライアント インストールのプロパティの準備方法 (グループ ポリシーおよびソフトウェア更新ベースのクライアント インストール)](#BKMK_Provision)」および「[System Center Configuration Manager でクライアントをサイトに割り当てる方法](../../../core/clients/deploy/assign-clients-to-a-site.md)」を参照してください。  

 構成マネージャー クライアントがインストールされていないコンピューターで、次の手順に従って、ソフトウェアの更新ポイントを使用してクライアントのインストールやソフトウェアの更新を行ったり、構成マネージャー クライアントをソフトウェアの更新ポイントに発行したりするように構成します。  

> [!NOTE]  
>  コンピューターが前のソフトウェアのインストールの後に保留中の再起動の状態になると、ソフトウェア更新ベースのクライアント インストールによってコンピューターが再起動する可能性があります。  

クライアントのインストールおよびソフトウェア更新プログラム用のソフトウェアの更新ポイントを指定するように、Active Directory Domain Services のグループ ポリシー オブジェクトを構成するには  

1.  グループ ポリシー管理コンソールを使用して、新規または既存のグループ ポリシー オブジェクトを開きます。  

2.  コンソールで、[**コンピューターの構成**]、[**管理用テンプレート**]、[**Windows コンポーネント**] の順に展開し、[**Windows Update**] をクリックします。  

3.  設定 [**イントラネットの Microsoft 更新サービスの場所を指定する**] のプロパティを開いてから、[**有効**] をクリックします。  

4.  [**更新を検出するためのイントラネットの更新サービスを設定する**] ボックスで、使用するソフトウェアの更新ポイントのサーバー名とポートを指定します。 このサーバー名とポートは、ソフトウェアの更新ポイントで使用されているサーバー名形式とポートに完全に一致している必要があります。  

    -   Configuration Manager サイト システムが完全修飾ドメイン名 (FQDN) を使用するように構成されている場合は、FQDN 形式を使用してサーバー名を指定します。  

    -   Configuration Manager サイト システムが完全修飾ドメイン名 (FQDN) を使用するように構成されていない場合は、短い名前の形式を使用してサーバー名を指定します。  

    > [!NOTE]  
    >  ソフトウェアの更新ポイントで使用されるポート番号を特定するには、「[System Center Configuration Manager において、WSUS で使用するポート設定を特定する方法](../../../sum/plan-design/plan-for-software-updates.md)」を参照してください。  

     例: **http://server1.contoso.com:8530**  

5.  [**イントラネット統計サーバーの設定**] ボックスで、使用するイントラネット統計サーバーの名前を指定します。 このサーバーの指定に関する特定の要件はありません。 このサーバーは、ソフトウェアの更新ポイント サーバーと同じコンピューターである必要はありません。また、ソフトウェアの更新ポイントと同じサーバーである場合は、形式が一致しなくてもかまいません。  

6.  構成マネージャー クライアントのインストール先にし、ソフトウェア更新プログラムを受け取るコンピューターに、グループ ポリシー オブジェクトを割り当てます。  

### <a name="to-publish-the-configuration-manager-client-to-the-software-update-point"></a>構成マネージャー クライアントをソフトウェアの更新ポイントに発行するには  

1.  Configuration Manager コンソールで、[**管理**] をクリックします。  

2.  [**管理**] ワークスペースで [**サイトの構成**] を展開して、[**サイト**] をクリックします。  

3.  [**サイト**] の一覧で、ソフトウェアの更新に基づいたクライアントのインストールを構成するサイトを選択します。  

4.  [**ホーム**] タブの [**設定**] グループで、[**クライアント インストール設定**] をクリックし、[**ソフトウェアの更新に基づいたクライアントのインストール**] をクリックします。  

5.  [**ソフトウェアの更新に基づいたクライアントのインストールのプロパティ**] ダイアログ ボックスで、[**ソフトウェアの更新に基づいたクライアントのインストールを有効にする**] を選択して、このクライアント インストール方法を有効にします。  

6.  Configuration Manager サイト サーバー上のクライアント ソフトウェアのバージョンが、ソフトウェアの更新ポイントに保存されているクライアントのバージョンよりも新しい場合、**[新しいバージョンのクライアント パッケージが見つかりました]** ダイアログ ボックスが開きます。 **[はい]** をクリックすると、最新バージョンのクライアント ソフトウェアがソフトウェアの更新ポイントに発行されます。  

    > [!NOTE]  
    >  クライアント ソフトウェアがソフトウェアの更新ポイントに発行されたことがない場合、このボックスは空白になります。  

7.  ソフトウェアの更新ポイントのクライアント インストールの構成を完了するには、 **[OK]**をクリックします。  

> [!NOTE]  
>  構成マネージャー クライアント用のソフトウェア更新プログラムは、新しいバージョンが存在する場合に自動的に更新されるわけではありません。 新しいクライアント バージョンが含まれるようにサイトをアップグレードするには、手順 6 でこの手順を繰り返して [**はい**] をクリックする必要があります。  

##  <a name="a-namebkmkclientgpa-how-to-install-clients-with-group-policy"></a><a name="BKMK_ClientGP"></a> グループ ポリシーを使用したクライアントのインストール方法  
 Active Directory Domain Services のグループ ポリシーを使用して、企業内のコンピューターにインストールするように構成マネージャー クライアントを割り当てたり、発行したりできます。 グループ ポリシーを使用して構成マネージャー クライアントをコンピューターに割り当てると、コンピューターの最初の起動時にクライアントがインストールされます。 グループ ポリシーを使用して構成マネージャー クライアントをユーザーに発行すると、インストールするユーザーのコンピューターのコントロール パネルの **[プログラムの追加と削除]** にクライアントが表示されます。  

 グループ ポリシーに基づいたインストールには、Windows インストーラー パッケージ (CCMSetup.msi) を使用します。 このファイルは、Configuration Manager サイト サーバーの **&lt;ConfigMgr インストール ディレクトリ\>\bin\i386** フォルダーにあります。 このファイルにプロパティを追加してインストールの動作を変更することはできません。  

> [!IMPORTANT]  
>  クライアント インストール ファイルにアクセスするためには、このフォルダーの管理者権限を持っている必要があります。  

-   Active Directory スキーマが Configuration Manager 用に拡張されており、**[サイトのプロパティ]** ダイアログ ボックスの **[詳細設定]** タブで **[Active Directory Domain Services 内にこのサイトを発行する]** が選択されている場合、クライアント コンピューターは自動的に Active Directory Domain Services でインストール プロパティを検索します。 発行されるインストール プロパティの詳細については、「[System Center Configuration Manager で Active Directory Domain Services に発行されたクライアント インストールのプロパティについて](../../../core/clients/deploy/about-client-installation-properties-published-to-active-directory-domain-services.md)」を参照してください。  

-   Active Directory スキーマが拡張されていない場合は、このトピックの後述の手順 ( [クライアント インストールのプロパティの準備方法 (グループ ポリシーおよびソフトウェア更新ベースのクライアント インストール)](#BKMK_Provision)) を使用して、コンピューターのレジストリ内にインストール プロパティを格納できます。 これらのインストール プロパティは、構成マネージャー クライアントのインストール時に使用されます。  

 Active Directory Domain Services のグループ ポリシーを使用してソフトウェアをインストールする方法については、Windows Server のドキュメントを参照してください。  

##  <a name="a-namebkmkmanuala-how-to-install-clients-manually"></a><a name="BKMK_Manual"></a> クライアントの手動インストール方法  
 CCMSetup.exe プログラムを使用して、企業内のコンピューターに構成マネージャー クライアント ソフトウェアを手動でインストールできます。 このプログラムとそのサポート ファイルは、サイト サーバー上の Configuration Manager インストール フォルダーの **Client** フォルダーおよびサイトの管理ポイントにあります。 このフォルダーは次のようにネットワークで共有されます。  

 \\\\*&lt;サイト サーバー名\>*\SMS_*&lt;サイト コード\>*\Client\  

 ここで *&lt;サイト サーバー名\>* は管理ポイントをホストするいずれかのサーバーの名前、*&lt;サイト コード\>* はクライアントが属するプライマリ サイトのコードです。  クライアントでコマンド ラインから CCMSetup.exe を実行するには、この場所にネットワーク ドライブをマップしてからコマンドを実行する必要があります。  

> [!IMPORTANT]  
>  クライアント インストール ファイルにアクセスするためには、このフォルダーの管理者権限を持っている必要があります。  

 CCMSetup.exe は、必要なすべてのインストール前提条件をクライアント コンピューターにコピーし、Windows インストーラー パッケージ (Client.msi) を呼び出してクライアント インストールを実行します。  

> [!IMPORTANT]  
>  Client.msi を直接実行することはできません。  

 CCMSetup.exe および Client.msi の両方にコマンド ライン プロパティを指定して、クライアント インストールの動作を変更できます。 Client.msi のプロパティを指定する前に、CCMSetup のプロパティ (先頭が **/** のプロパティ) を指定する必要があります。  

 たとえば、次のコマンドを指定できます。  

```  
CCMSetup.exe /mp:SMSMP01 /logon SMSSITECODE=AUTO FSP=SMSFP01  
```  

 次のプロパティを使用して、クライアント インストールを指定します。  

|プロパティ|説明|  
|--------------|-----------------|  
|**/mp:SMSMP01**|CCMSetup のこのプロパティで、必要なクライアント インストール ファイルをダウンロードする管理ポイント SMSMP01 を指定します。|  
|**/logon**|CCMSetup のこのプロパティで、既存の構成マネージャー クライアントがコンピューターで見つかった場合はインストールを停止するように指定します。|  
|**SMSSITECODE=AUTO**|Client.msi のこのプロパティで、たとえば Active Directory Domain Services を使用して、使用する Configuration Manager サイト コードをクライアントが特定するように指定します。|  
|**FSP=SMSFP01**|Client.msi のこのプロパティで、SMSFP01 というフォールバック ステータス ポイントを使用して、クライアント コンピューターから送信される状態メッセージを受信するように指定します。|  

 すべての CCMSetup.exe プロパティの詳細については、「[System Center Configuration Manager のクライアント インストール プロパティについて](../../../core/clients/deploy/about-client-installation-properties.md)」を参照してください  

### <a name="examples"></a>例
 これらの例は、イントラネット上の Active Directory クライアントを対象としており、次の値を使用してサイトのさまざまな側面を表しています。  

 **MPSERVER** = 管理ポイントをホストするサーバー   
**FSPSERVER** = フォールバック ステータス ポイントをホストするサーバー  
**ABC** = サイト コード  
**contoso.com** = ドメイン名  

 すべてのサイト システム サーバーはイントラネットの FQDN で構成され、サイトはクライアントの Active Directory フォレストに発行されます。  

 クライアント コンピューターで、ローカル管理者としてログオンします。ドライブ (z:) を \\\MPSERVER\SMS_ABC\Client にマップして、コマンド プロンプトを z ドライブに切り替えてから、次のいずれかのコマンドを実行します。  

 **例 1:**  

```  
CCMSetup.exe  
```  

> [!NOTE]  
>  この例では、Active Directory Domain Services に発行されたクライアント インストール プロパティを使ってクライアントが自動的に構成されるように、追加のプロパティを指定せずにクライアントをインストールしています。 たとえば、クライアントにサイト コード (クライアントのネットワークの場所を、クライアントの割り当て用に構成された境界グループ内に含める必要があります)、管理ポイント、およびフォールバック ステータス ポイントが自動的に構成され、クライアントが HTTPS のみを使用して通信する必要があるかどうかも自動的に構成されます。 Active Directory クライアント用に自動構成できるクライアント インストール プロパティの詳細については、「[System Center Configuration Manager で Active Directory Domain Services に発行されたクライアント インストールのプロパティについて](../../../core/clients/deploy/about-client-installation-properties-published-to-active-directory-domain-services.md)」を参照してください。  

 **例 2:**  

```  
CCMSetup.exe /MP:mpserver.contoso.com /UsePKICert SMSSITECODE=ABC CCMHOSTNAME=server05.contoso.com CCMFIRSTCERT=1 FSP=server06.constoso.com  
```  

> [!NOTE]  
>  この例では、Active Directory Domain Services で提供される自動構成を上書きしており、クライアントのネットワークの場所がクライアントの割り当て用に構成された境界グループに含まれている必要はありません。 代わりに、インストールで、サイト、イントラネットの管理ポイントとインターネットベースの管理ポイント、インターネットからの接続を受け入れるフォールバック ステータス ポイント、および有効期限の最も長い、使用するクライアント PKI 証明書 (使用可能な場合) を指定しています。  

##  <a name="a-namebkmkclientlogonscripta-how-to-install-clients-with-logon-scripts"></a><a name="BKMK_ClientLogonScript"></a> ログオン スクリプトを使用したクライアントのインストール方法  
 Configuration Manager では、構成マネージャー クライアント ソフトウェアをインストールするログオン スクリプトをサポートしています。 **CCMSetup.exe** プログラム ファイルをログオン スクリプトで使用して、クライアント インストールを開始できます。  

 ログオン スクリプト インストールの使用方法は、手動のクライアント インストールと同じです。 CCMSsetup.exe に **/logon** インストール プロパティを指定して、既にクライアントがコンピューターに存在する場合は、クライアントをインストールしないようにすることができます。 これにより、ログオン スクリプトが実行されるたびにクライアントの再インストールが発生することはなくなります。  

 **/Source** プロパティを使用しているインストール ソースが指定されておらず、インストールの取得元となる管理ポイントが **/MP** プロパティを使用して指定されていない場合に、Configuration Manager 用にスキーマが拡張されており、サイトが Active Directory Domain Services に発行されていれば、CCMSetup.exe は Active Directory Domain Services を検索して管理ポイントを特定できます。 または、クライアントが DNS または WINS を使用して管理ポイントを特定することもできます。  

##  <a name="a-namebkmkclientappa-how-to-install-clients-with-a-package-and-program"></a><a name="BKMK_ClientApp"></a> パッケージとプログラムを使用したクライアントのインストール方法  
 Configuration Manager を使用し、階層内の選択したコンピューターのクライアント ソフトウェアをアップグレードするパッケージとプログラムを作成して展開できます。 Configuration Manager には、通常使用される値をパッケージ プロパティに適用する、パッケージ定義ファイルが付属しています。 追加のコマンド ライン プロパティを指定することで、クライアント インストールの動作をカスタマイズできます。  

> [!NOTE]  
>  この方法では、Configuration Manager 2007 クライアントをアップグレードすることはできません。 代わりに、最新バージョンのクライアントが含まれたパッケージを自動的に作成して展開する自動クライアント アップグレードを使用します。 詳細については、「[System Center Configuration Manager でのクライアントのアップグレード](../../../core/clients/manage/upgrade/upgrade-clients.md)」を参照してください。  
>   
>  旧バージョンの構成マネージャー クライアントから移行する方法について、詳しくは「[System Center Configuration Manager でのクライアント移行戦略の計画](../../../core/migration/planning-a-client-migration-strategy.md)」をご覧ください。  

 次の手順に従って、クライアント ソフトウェアをアップグレードするために構成マネージャー クライアント コンピューターに展開できる、Configuration Manager パッケージとプログラムを作成します。  

クライアント ソフトウェア用のパッケージとプログラムを作成するには  

1.  Configuration Manager コンソールで、**[ソフトウェア ライブラリ]**をクリックします。  

2.  **[ソフトウェア ライブラリ]** ワークスペースで **[アプリケーション管理]** を展開して、**[パッケージ]** をクリックします。  

3.  [**ホーム**] タブの [**作成**] グループで、[**定義に基づいたパッケージの作成**] をクリックします。  

4.  **定義に基づくパッケージの作成ウィザード** の **[パッケージの定義]**ページで、 **[発行元]** ドロップダウン リストから **[Microsoft]** を選択し、 **[パッケージ定義]** の一覧から **[構成マネージャー クライアントのアップグレード]** を選択してから、 **[次へ]**をクリックします。  

5.  ウィザードの [**ソース ファイル**] ページで、[**常にソース フォルダーからファイルを取得する**] を選択し、[**次へ**] をクリックします。  

6.  **定義に基づいたパッケージの作成ウィザード**の **[ソース フォルダー]** ページで、**[ネットワーク パス (UNC 名)]** を選択し、構成マネージャー クライアントのインストール ファイルが含まれているコンピューターとフォルダーへのネットワーク パスを入力します。  

    > [!NOTE]  
    >  Configuration Manager の展開を実行するコンピューターが、指定したネットワーク フォルダーへのアクセス権を持っている必要があります。 コンピューターがアクセス権を持っていない場合、インストールは失敗します。  

7.  **[次へ]** をクリックして、ウィザードを完了します。  

8.  クライアント インストールのプロパティを変更する場合は、[**Configuration Manager エージェントのサイレント アップグレードのプロパティ**] プログラム ダイアログ ボックスの [**全般**] タブで CCMSetup.exe コマンド ライン パラメーターを変更できます。 既定のインストール プロパティは、 **/noservice SMSSITECODE=AUTO**です。  

9. クライアント アップグレード パッケージをホストする必要があるすべての配布ポイントにパッケージを配布します。 次に、アップグレードが必要な構成マネージャー クライアントが含まれているコンピューターのコレクションにパッケージを展開できます。  

## <a name="how-to-install-clients-to-mdm-managed-windows-devices-with-intune"></a>Intune を使用した MDM 管理対象 Windows デバイスへのクライアントのインストール方法

Microsoft Intune を使用して、登録されているコンピューターに構成マネージャー クライアントのインストール ファイルを展開できます。 この操作を行うには、Intune ソフトウェア パブリッシャーを使用して、クライアントのインストール ファイル **ccmsetup.msi** を含む、Windows インストーラー (\*.msi) アプリを作成します。 次に、アプリを登録済みデバイスに展開します。これで、クライアント ソフトウェアがインストールされます。

クライアント ソフトウェアをインストールした後に、デバイスが管理状態にあることを確認するには、デバイスが企業ネットワーク上にあり、Configuration Manager サイトの境界内にある必要があります。 

> [!NOTE]
> クライアント ソフトウェアがインストールされると、デバイスが Intune から登録されます。

### <a name="to-install-clients-with-intune"></a>Intune を使用してクライアントをインストールするには

1. Intune で、構成マネージャー クライアントのインストール ファイル **ccmsetup.msi** を含む、[アプリを作成](/intune/deploy-use/add-apps-for-mobile-devices-in-microsoft-intune)します。

2. ソフトウェア パブリッシャーでは、次のコマンド ラインのパラメーターを使用します。

  **CCMSETUPCMD="/MP:&lt;管理ポイントの FQDN> SMSMP=&lt;管理ポイントの FQDN> SMSSITECODE=&lt;サイト コード> DNSSUFFIX=&lt;管理ポイントの DNS サフィックス>"**

3. 登録済みの Windows コンピューターに[アプリを展開](/intune/deploy-use/deploy-apps-in-microsoft-intune)します。

##  <a name="a-namebkmkclientimagea-how-to-install-clients-with-a-computer-image"></a><a name="BKMK_ClientImage"></a> コンピューター イメージングを使用したクライアントのインストール方法  
 企業内でコンピューターを構築するために使用するマスター イメージ コンピューターに、構成マネージャー クライアント ソフトウェアをプレインストールできます。 クライアントをマスター コンピューターにインストールするときに、クライアント用のサイト コードを指定しないでください。 このマスター イメージからイメージングされたコンピューターには構成マネージャー クライアントが含まれ、インストールが完了したときにサイトの割り当てを完了する必要があります。  

> [!IMPORTANT]  
>  構成マネージャー クライアントが Configuration Manager サイトに割り当てられるまでは、イメージングされたコンピューターは構成マネージャー クライアントとして機能できません。  

 マスター イメージ コンピューターにインストールされているコンピューター固有の証明書は削除する必要があります。 たとえば、公開キー基盤 (PKI) 証明書を使用する場合、コンピューターをイメージ化する前に、[**コンピューター**] と [**ユーザー**] の [**個人用**] ストアの証明書を削除する必要があります。  

 クライアントは、Active Directory Domain Services に管理ポイントを照会できない場合、信頼されたルート キーを使用して信頼された管理ポイントを特定します。 イメージングされたすべてのクライアントをマスター コンピューターと同じ階層に展開する場合は、信頼されたルート キーを所定の場所に残しておきます。 クライアントを別の階層に展開する場合、信頼されたルート キーを削除し、ベスト プラクティスとして、これらのクライアントに信頼されたルート キーを事前準備することをお勧めします。 詳細については、「[Planning for the Trusted Root Key](../../../core/plan-design/security/plan-for-security.md#BKMK_PlanningForRTK)」を参照してください。  

### <a name="to-prepare-the-client-computer-for-imaging"></a>クライアント コンピューターのイメージングを準備するには  

1.  マスター イメージ コンピューターに構成マネージャー クライアント ソフトウェアを手動でインストールします。 詳細については、「[Configuration Manager クライアントの手動インストール方法](#BKMK_Manual)」をご覧ください。  

    > [!IMPORTANT]  
    >  CCMSetup.exe コマンド ライン プロパティでクライアントに Configuration Manager サイト コードを指定しないでください。  

2.  コマンド プロンプトで「**net stop ccmexec**」と入力して、 **SMS Agent Host** サービス (Ccmexec.exe) がマスター イメージ コンピューターで実行されていないことを確認します。  

3.  マスター イメージ コンピューターのローカル コンピューター ストアに保存されている証明書を削除します。  

4.  クライアントをマスター イメージ コンピューターとは別の Configuration Manager 階層にインストールする場合は、信頼されたルート キーをマスター イメージ コンピューターから削除します。  

5.  イメージング ソフトウェアを使用して、マスター コンピューターのイメージをキャプチャします。  

6.  イメージを対象コンピューターに展開します。  

##  <a name="a-namebkmkclientworkgroupa-how-to-install-clients-on-workgroup-computers"></a><a name="BKMK_ClientWorkgroup"></a> ワークグループ コンピューターへのクライアントのインストール方法  
 Configuration Manager は、ワークグループ内のコンピューターへのクライアント インストールをサポートします。 「[Configuration Manager クライアントの手動インストール方法](#BKMK_Manual)」で指定された方法を使用して、ワークグループ コンピューターにクライアントをインストールします。  

 ワークグループ コンピューターに構成マネージャー クライアントをインストールするには、次の前提条件を満たす必要があります。  

-   クライアントは、各ワークグループ コンピューターに手動でインストールする必要があります。 インストール時は、ログオン ユーザーにワークグループ コンピューターに対するローカル管理者権限が必要です。  

-   Configuration Manager サイト サーバー ドメイン内のリソースにアクセスするには、このサイト用にネットワーク アクセス アカウントを構成する必要があります。 このアカウントをソフトウェアの配布コンポーネントのプロパティとして指定します。 詳細については、「[Site components for System Center Configuration Manager](../../../core/servers/deploy/configure/site-components.md)」をご覧ください。  

 ワークグループ コンピューターのサポートには次のようないくつかの制限があります。  

-   ワークグループ クライアントは、Active Directory Domain Services から管理ポイントを特定できません。代わりに、DNS、WINS、または別の管理ポイントを使用する必要があります。  

-   クライアントは、Active Directory Domain Services にサイト情報を照会できないので、グローバル ローミングはサポートされません。  

-   Active Directory 探索方法では、ワークグループ内のコンピューターを探索できません。  

-   ワークグループ コンピューターのユーザーにソフトウェアを展開することはできません。  

-   クライアント プッシュ インストール方法を使用して、ワークグループ コンピューターにクライアントをインストールすることはできません。  

-   ワークグループ クライアントでは認証に Kerberos を使用できません。そのため、手動の承認が必要です。  

-   ワークグループ クライアントは、配布ポイントとして構成できません。 Configuration Manager では、配布ポイントであるコンピューターがドメインのメンバーに指定されている必要があります。  

### <a name="to-install-the-client-on-workgroup-computers"></a>ワークグループ コンピューターにクライアントインストールするには  

1.  クライアントをインストールするコンピューターが上記の条件を満たしていることを確認します。  

2.  セクション「[Configuration Manager クライアントの手動インストール方法](#BKMK_Manual)」の指示に従います。  

     例 1: **CCMSetup.exe SMSSITECODE=ABC DNSSUFFIX=constoso.com**  

    > [!NOTE]  
    >  この例では、イントラネットのクライアント管理用のクライアントをインストールし、サイト コードと DNS サフィックスを指定して管理ポイントを検索しています。  

     例 2: **CCMSetup.exe FSP=fspserver.constoso.com**  

    > [!NOTE]  
    >  この例では、サイトの自動割り当てが成功するように、境界グループ内に構成されたネットワークの場所にクライアントを配置する必要があります。 クライアントの展開を追跡すると共に、クライアントの通信の問題を特定できるように、コマンドにサーバー FSPSERVER のフォールバック ステータス ポイントを含めています。  

##  <a name="a-namebkmkclientinterneta-how-to-install-clients-for-internet-based-client-management"></a><a name="BKMK_ClientInternet"></a> インターネット ベースのクライアント管理用クライアントのインストール方法  
 Configuration Manager サイトが、イントラネット上にある場合とインターネット上にある場合があるクライアントに対してインターネットベースのクライアント管理をサポートするときは、次の 2 つの方法のいずれかを使用して、イントラネット上のクライアントをインストールできます。  

-   たとえば、手動インストールやクライアント プッシュを使ってクライアントをインストールするときには、CCMHOSTNAME=*&lt;インターネットベースの管理ポイントのインターネット FQDN\>* という Client.msi プロパティを含めることができます。 この方法を使用するときには、クライアントをサイトに直接割り当てる必要があります。また、サイトの自動割り当ては使用できません。 このトピックの「[Configuration Manager クライアントの手動インストール方法](#BKMK_Manual)」セクションに、この構成方法の例を示します。  

-   イントラネットのクライアント管理用にクライアントをインストールしてから、コントロール パネルで構成マネージャー クライアントのプロパティを使用するかスクリプトを使用して、インターネット ベースのクライアント管理ポイントをクライアントに割り当てることができます。 この方法を使用するときに、自動クライアント割り当てを使用できます。 詳細については、このトピックの「[クライアント インストール後のインターネットベースのクライアント管理用のクライアント構成方法](#BKMK_ConfigureIBCM_MP)」セクションを参照してください。  

 クライアントがインターネットのみのクライアントであるか、またはイントラネットに戻る前にクライアントをインストールする必要があるために、インターネット上のクライアントをインストールする必要がある場合は、次のサポートされる方法のいずれかを選択します。  

-   仮想プライベート ネットワーク (VPN) を使用してこれらのクライアントが一時的にイントラネットに接続し、適切なクライアント インストール方法を使用してクライアントをインストールするメカニズムを提供します。  

-   リムーバブル メディアにクライアント インストール ソース ファイルをパッケージ化し、インストールするユーザーに指示と共に送るなど、Configuration Manager に依存しないインストール方法を使用します。 クライアント インストール ソース ファイルは、Configuration Manager サイト サーバーおよび管理ポイントの*&lt;インストール パス\>*\Client フォルダーにあります。 クライアント フォルダー経由とこのフォルダーから手動でコピーし、CCMSetup.exe と適切なすべての CCMSetup コマンド ライン プロパティを使用して、クライアントを手動でインストールするためのスクリプトをメディアに含めます。  

> [!NOTE]  
>  Configuration Manager では、インターネット ベースの管理ポイントまたはインターネット ベースのソフトウェア更新ポイントからクライアントを直接インストールすることはサポートしていません。  

 インターネット経由で管理されるクライアントはインターネット ベースのサイト システムと通信する必要があるため、インストールする前に、これらのクライアントに公開キー基盤 (PKI) 証明書がインストールされていることを確認します。 これらの証明書は Configuration Manager とは別にインストールする必要があります。 証明書の要件の詳細については、「[System Center Configuration Manager での PKI 証明書の要件](../../../core/plan-design/network/pki-certificate-requirements.md)」を参照してください。  

### <a name="to-install-clients-on-the-internet-by-specifying-ccmsetup-command-line-properties"></a>CCMSetup コマンドライン プロパティを指定してインターネット上のクライアントをインストールするには  

1.  セクション「[Configuration Manager クライアントの手動インストール方法](#BKMK_Manual)」の指示に従い、常に以下を含めます。  

    -   CCMSetup のコマンド ライン プロパティ **/source:***&lt;コピーされた Client フォルダーへのローカル パス\>*  

    -   CCMSetup のコマンド ライン プロパティ **/UsePKICert**  

    -   Client.msi のプロパティ **CCMHOSTNAME=***&lt;インターネット ベースの管理ポイントの FQDN\>*  

    -   Client.msi のプロパティ **SMSSIGNCERT=***&lt;エクスポートされたサイト サーバー署名証明書へのローカル パス\>*  

    -   Client.msi のプロパティ **SMSSITECODE=***&lt;インターネット ベースの管理ポイントのサイト コード\>*  

    > [!NOTE]  
    >  サイトに複数のインターネット ベースの管理ポイントがある場合、どのインターネット ベースの管理ポイントを CCMHOSTNAME プロパティに指定してもかまいません。 構成マネージャー クライアントが指定したインターネット ベースの管理ポイントに接続するときに、サイト内で使用できるインターネット ベースの管理ポイントの一覧が管理ポイントからクライアントに送信され、クライアントは一覧から 1 つを選択します。 選択は非決定的です。  

2.  クライアントで証明書失効リスト (CRL) を確認しないようにする場合は、CCMSetup コマンドライン プロパティ「**/NoCRLCheck**」を指定します。  

3.  インターネット ベースのフォールバック ステータス ポイントを使用する場合は、Client.msi プロパティ **FSP=***&lt;インターネット ベースのフォールバック ステータス ポイントのインターネット FQDN\>* を指定します。  

4.  インターネットのみのクライアント管理用にクライアントをインストールする場合、Client.msi プロパティ「**CCMALWAYSINF=1**」を指定します。  

5.  その他の CCMSetup コマンドライン プロパティを指定する必要があるかどうかを確認します。 たとえば、クライアントに複数の有効な PKI 証明書がある場合は、場合によって証明書選択条件を指定する必要があります。 使用可能なプロパティの一覧については、「[System Center Configuration Manager のクライアント インストール プロパティについて](../../../core/clients/deploy/about-client-installation-properties.md)」を参照してください。  

     例: **CCMSetup.exe /source:D:\Clients /UsePKICert CCMHOSTNAME=server1.contoso.com SMSSIGNCERT=siteserver.cer SMSSITECODE=ABC FSP=server2.contoso.com CCMALWAYSINF=1 CCMFIRSTCERT=1**  

    > [!NOTE]  
    >  この例では、クライアント PKI 証明書を使用するように設定された D ドライブのフォルダーからクライアント ソース ファイルをインストールし、インターネットのみのクライアント管理用の有効期限の最も長い証明書を選択し、SERVER1 というインターネットベースの管理ポイントおよび contoso.com domain 内のインターネットベースのフォールバック ステータス ポイントを使用するようにクライアントを割り当てて、ABC サイトにクライアントを割り当てています。  

###  <a name="a-namebkmkconfigureibcmmpato-configure-clients-for-internet-based-client-management-after-client-installation"></a><a name="BKMK_ConfigureIBCM_MP"></a> クライアント インストール後のインターネット ベースのクライアント管理用のクライアントを構成するには  
 クライアントのインストール後にインターネットベースの管理ポイントを割り当てるには、次のいずれかの手順に従います。 1 つ目の手順は手動で構成する必要があるため、クライアント数が少ない場合に適しています。2 つ目の手順は、構成するクライアント数が多い場合に適しています。  

#### <a name="to-configure-clients-for-internet-based-client-management-after-client-installation-by-assigning-the-internet-based-management-point-in-configuration-manager-properties"></a>クライアントのインストール後に、Configuration Manager のプロパティでインターネットベースの管理ポイントを割り当てて、インターネットベースのクライアント管理用のクライアントを構成するには  

1.  クライアント コンピューターのコントロール パネルで [**Configuration Manager**] に移動してから、ダブルクリックしてプロパティを開きます。  

2.  [**インターネット**] タブで、[インターネット FQDN] ボックスにインターネットベースの管理ポイントの完全修飾ドメイン名を入力します。  

    > [!NOTE]  
    >  [**インターネット**] タブは、クライアントにクライアント PKI 証明書がある場合のみ使用できます。  

3.  プロキシ サーバーを使ってクライアントがインターネットにアクセスする場合は、プロキシ サーバー設定を入力します。  

4.  **[OK]**をクリックします。  

#### <a name="to-configure-clients-for-internet-based-client-management-after-client-installation-by-using-a-script"></a>クライアントのインストール後に、スクリプトを使用して、インターネットベースのクライアント管理用のクライアントを構成するには  

1.  メモ帳などのテキスト エディターを開きます。  

2.  次をコピーし、ファイルに挿入します。  

    ```  
    on error resume next  

    ' Create variables.  
    Dim newInternetBasedManagementPointFQDN  
    Dim client  

    newInternetBasedManagementPointFQDN = "mp.contoso.com"  

    ' Create the client COM object.  
    Set client = CreateObject ("Microsoft.SMS.Client")  

    ' Set the Internet-Based Management Point FQDN by calling the SetCurrentManagementPoint method.  
    client.SetInternetManagementPointFQDN newInternetBasedManagementPointFQDN  

    ' Clear variables.  
    Set client = Nothing  
    Set internetBasedManagementPointFQDN = Nothing  

    ```  

3.  *mp.contoso.com* を、使用するインターネットベースの管理ポイントのインターネット FQDN に置き換えます。  

    > [!NOTE]  
    >  インターネットベースの管理ポイントを使用するようにはクライアントが構成されないように、指定のインターネットベースの管理ポイントを削除する必要がある場合は、引用符の内側の値を削除して、この行が **newInternetBasedManagementPointFQDN = ""**になるようにします。  

4.  ファイルを .vbs 拡張子で保存します。  

5.  スクリプトを使って、次のいずれかの方法でクライアント コンピューターでスクリプトを実行します。  

    -   パッケージとプログラムを使用して、既存の Configuration Manager クライアントにファイルを展開する  

    -   Windows エクスプローラーでスクリプトをダブルクリックして、既存の Configuration Manager クライアントでファイルをローカルに実行する  

 このスクリプトの新しい設定を適用するには、クライアントの再起動が必要になることがあります。  

##  <a name="a-namebkmkprovisiona-how-to-provision-client-installation-properties-group-policy-and-software-update-based-client-installation"></a><a name="BKMK_Provision"></a> クライアント インストールのプロパティの準備方法 (グループ ポリシーおよびソフトウェア更新ベースのクライアント インストール)  
 Windows グループ ポリシーを使用して、企業のコンピューターに構成マネージャー クライアントのインストール プロパティを事前に準備しておくことができます。 これらのプロパティは、コンピューターのレジストリに格納され、クライアント ソフトウェアがインストールされるときに読み込まれます。 通常、この処理は Configuration Manager では必須ではありません。 ただし、次のような場合には必須になることがあります。  

-   グループ ポリシー設定またはソフトウェア更新ベースのクライアント インストール方法を使用し、Active Directory スキーマを Configuration Manager 用に拡張していない場合。  

-   特定のコンピューターのクライアント インストール プロパティを上書きする場合。  

> [!NOTE]  
>  CCMSetup.exe コマンド ラインで何らかのインストール プロパティを指定した場合、コンピューターに準備されたインストール プロパティは使用されません。  

 Configuration Manager のインストール メディアには、ConfigMgrInstallation.adm というグループ ポリシー管理テンプレートが用意されており、クライアント コンピューターにインストール プロパティを準備するために使用できます。 このテンプレートを構成して、組織のコンピューターに割り当てるには、次の手順に従います。  

### <a name="to-configure-and-assign-client-installation-properties-by-using-a-group-policy-object"></a>グループ ポリシー オブジェクトを使用してクライアント インストール プロパティの構成と割り当てを行うには  

1.  Windows グループ ポリシー オブジェクト エディターなどのエディターを使用し、新しいグループ ポリシー オブジェクトまたは既存のグループ ポリシー オブジェクトに、管理テンプレート ConfigMgrInstallation.adm をインポートします。  

    > [!NOTE]  
    >  このファイルは、Configuration Manager インストール メディアの **TOOLS\ConfigMgrADMTemplates** フォルダーにあります。  

2.  インポートした設定 **[クライアント展開の設定を構成する]**のプロパティを開きます。  

3.  **[有効]**をクリックします。  

4.  **[CCMSetup]** ボックスに、必要な CCMSetup コマンドライン プロパティを入力します。 すべての CCMSetup コマンド ライン プロパティのリストと使用例については、「[System Center Configuration Manager のクライアント インストール プロパティについて](../../../core/clients/deploy/about-client-installation-properties.md)」を参照してください。  

5.  構成マネージャー クライアントのインストール プロパティを準備するコンピューターにグループ ポリシー オブジェクトを割り当てます。  

 Windows グループ ポリシーの詳細については、Windows Server のマニュアルを参照してください。  



<!--HONumber=Dec16_HO3-->

