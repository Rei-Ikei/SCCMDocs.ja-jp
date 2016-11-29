---
title: "エンドポイント間の通信 | System Center Configuration Manager での"
description: "ネットワーク経由で System Center Configuration Manager サイト システムとコンポーネントがどのように通信するかを説明します。"
ms.custom: na
ms.date: 10/06/2016
ms.prod: configuration-manager
ms.reviewer: na
ms.suite: na
ms.technology:
- configmgr-other
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68fe0e7e-351e-4222-853a-877475adb589
caps.latest.revision: 10
author: Brenduns
ms.author: brenduns
manager: angrobe
translationtype: Human Translation
ms.sourcegitcommit: 1134bb2f04152288e72d40b1b1083f415cb4e900
ms.openlocfilehash: bf485456b4d8f0bffe956006b2a8b3dd8c17a5db


---
# <a name="communications-between-endpoints-in-system-center-configuration-manager"></a>System Center Configuration Manager でのエンドポイント間の通信

*適用対象: System Center Configuration Manager (Current Branch)*


##  <a name="a-nameplanningintra-sitecoma-communications-between-site-systems-in-a-site"></a><a name="Planning_Intra-site_Com"></a> サイト内のサイト システム間の通信  
 Configuration Manager サイト システムまたはコンポーネントがサイト内の他のサイト システムまたは Configuration Manager コンポーネントとネットワークを介して通信する場合、サイトの構成方法に応じて次のいずれかが使用されます。  

-   サーバー メッセージ ブロック (SMB)  

-   HTTP  

-   HTTPS  

サイト サーバーから配布ポイントへの通信を除き、サイト内のサーバー同士がいつ通信するかは決まっておらず、ネットワーク帯域幅も制御できません。 サイト システム間の通信は制御できないので、ネットワーク接続が良好で高速な場所にサイト システム サーバーを設置してください。  

サイト サーバーから配布ポイントへのコンテンツの転送を管理する方法は、次のとおりです。  

-   配布ポイントで使用するネットワーク帯域幅とスケジュールを構成します。 この構成は、サイト間通信のアドレスの構成とよく似ています。通常、リモート ネットワークへのコンテンツの転送が、使用可能な帯域幅に大きく影響する場合に、別の Configuration Manager サイトをインストールする代わりに、この構成を行います。  

-   配布ポイントを、事前設定済み配布ポイントにすることができます。 配布ポイント サーバーに手動で配置したコンテンツを使用できるので、ネットワークを介してコンテンツ ファイルを転送する必要がなくなります。  

詳細については、「[コンテンツ管理でのネットワーク帯域幅の管理](manage-network-bandwidth.md)」を参照してください。


##  <a name="a-nameplanningclienttositesystema-communications-from-clients-to-site-systems-and-services"></a><a name="Planning_Client_to_Site_System"></a> クライアントからサイト システムとサービスへの通信  
クライアントはサイト システムの役割、Active Directory ドメイン サービス、およびオンライン サービスへの通信を開始します。 これらの通信を有効にするには、ファイアウォールがクライアントと通信のエンドポイント間のネットワーク トラフィックを許可する必要があります。 エンドポイントは次のとおりです。  

-   **アプリケーション カタログ Web サイト ポイント** -(HTTP 通信と HTTPS 通信をサポートします)  

-   **クラウド ベースのリソース** (Microsoft Azure および Microsoft Intune)  

-   **Configuration Manager ポリシー モジュール (NDES)** - (HTTP 通信と HTTPS 通信をサポートします)  

-   **配布ポイント** -(HTTP 通信と HTTPS 通信をサポートします。クラウド ベースの配布ポイントでは HTTPS は必須です)  

-   **フォールバック ステータス ポイント** -(HTTP 通信 をサポートします)  

-   **管理ポイント** -(HTTP 通信と HTTPS 通信をサポートします)  

-   **Microsoft Update**  

-   **ソフトウェアの更新ポイント** -(HTTP 通信と HTTPS 通信をサポートします)  

-   **状態移行ポイント** -(HTTP 通信と HTTPS 通信をサポートします)  

-   **さまざまなドメイン サービス**  

クライアントは、サイト システムの役割と通信する前に、サービスの場所を使用して、クライアントのプロトコル (HTTP または HTTPS) をサポートするサイト システムの役割を検索します。 既定では、クライアントは次のように使用可能な最も安全な方法を使用します。  

-   HTTPS を使用するには、公開キー基盤 (PKI) を導入し、クライアントとサーバーに PKI 証明書をインストールする必要があります。 証明書の使用方法については、「[System Center Configuration Manager での PKI 証明書の要件](../../../core/plan-design/network/pki-certificate-requirements.md)」を参照してください。  

-   インターネット インフォメーション サービス (IIS) を使用し、クライアントからの通信をサポートするサイト システムの役割を展開する場合は、そのサイト システムにクライアントが接続するときに HTTP と HTTPS のどちらを使用するかを指定する必要があります HTTP を使用する場合は、署名と暗号化のオプションについても検討してください。 詳細については、「 [Plan for Security 」の「 System Center Configuration Manager](../../../core/plan-design/security/plan-for-security.md#BKMK_PlanningForSigningEncryption) 」の「 [Plan for security 」の「 System Center Configuration Manager](../../../core/plan-design/security/plan-for-security.md)」をご覧ください。  

クライアントが使用するサービスの場所については、「[クライアントが System Center Configuration Manager のサイト リソースやサービスを検索する方法を理解する](../../../core/plan-design/hierarchy/understand-how-clients-find-site-resources-and-services.md)」を参照してください。  

これらのエンドポイントへの通信時に、クライアントで使用されるポートとプロトコルの詳細については、「[System Center Configuration Manager で使用されるポート](../../../core/plan-design/hierarchy/ports.md)」を参照してください。  

###  <a name="a-namebkmkclientspana-considerations-for-client-communications-from-the-internet-or-an-untrusted-forest"></a><a name="BKMK_clientspan"></a> インターネットや信頼されていないフォレストからのクライアント通信に関する考慮事項  
プライマリ サイトにインストールされた次のサイト システムの役割では、インターネットや信頼されていないフォレストなどの信頼されていない場所にあるクライアントからの接続がサポートされます (セカンダリ サイトでは信頼されていない場所からのクライアント接続はサポートされません)。  

-   アプリケーション カタログ Web サイト ポイント  

-   Configuration Manager ポリシー モジュール  

-   配布ポイント (クラウドベースの配布ポイントでは HTTPS が必要)  

-   登録プロキシ ポイント  

-   フォールバック ステータス ポイント  

-   管理ポイント  

-   ソフトウェアの更新ポイント  

**インターネットに接続するサイト システムについて:**   
クライアントのフォレストと、サイト システム サーバーのフォレスト間の信頼を確立する必要はありませんが、インターネットに接続するサイト システムを含むフォレストがユーザー アカウントを含むフォレストを信頼している状態で、**[クライアント ポリシー]** のクライアント設定 **[インターネット クライアントからのユーザー ポリシー要求を有効にする]** を有効にした場合は、この構成でインターネット上のデバイスのユーザーベース ポリシーがサポートされます。  

たとえば、次のような場合に、インターネット ベースのクライアント管理がインターネット上のデバイス用のユーザー ポリシーを使用します。  

-   インターネット ベースの管理ポイントが、ユーザーを認証する読み取り専用ドメイン コントローラーが存在する境界ネットワークにあり、介在するファイアウォールで Active Directory パケットを許可する。  

-   ユーザー アカウントがフォレスト A (イントラネット) にあり、インターネット ベースの管理ポイントがフォレスト B (境界ネットワーク) にある。 フォレスト B がフォレスト A を信頼しており、介在するファイアウォールが認証パケットを許可する。  

-   ユーザー アカウントとインターネット ベースの管理ポイントがフォレスト A (イントラネット) にある。 管理ポイントは、Web プロキシ サーバー (Forefront Threat Management Gateway など) を使用してインターネットに発行される。  

> [!NOTE]  
>  Kerberos 認証に失敗すると、自動的に NTLM 認証が試みられます。  

上の例に示すように、インターネット ベースのサイト システムを Web プロキシ サーバー (ISA Server や Forefront Threat Management Gateway など) を使用してインターネットに発行する場合は、それらのサイト システムをイントラネットに配置することができます。 これらのサイト システムは、インターネットからのクライアント接続のみ、またはインターネットとイントラネットの両方を受け付けるように構成できます。 Web プロキシ サーバーを使用する場合は、SSL (Secure Sockets Layer) から SSL へのブリッジングまたは SSL トンネリングを構成できます。ブリッジングの方が安全です。  

-   **SSL から SSL へのブリッジング:**   
    インターネットベースのクライアント管理でプロキシ Web サーバーを使用する場合は、認証と SSL 終了を使用する SSL から SSL へのブリッジングを構成することをお勧めします。 クライアント コンピューターは、コンピューター認証によって、モバイル デバイス レガシ クライアントは、ユーザー認証によって認証されなければなりません。 Configuration Manager によって登録されるモバイル デバイスは、SSL ブリッジングをサポートしていません。  

     プロキシ Web サーバーで SSL 終了を使用する利点は、インターネットからのパケットが内部ネットワークに転送される前に検査されることです。 プロキシ Web サーバーはクライアントからの接続を認証してから終了します。次に、インターネット ベースのサイト システムに認証済みの新しい接続を開きます。 Configuration Manager クライアントでプロキシ Web サーバーが使用される場合、クライアント ID (クライアント GUID) はパケット ペイロード内に安全に格納されるので、管理ポイントではプロキシ Web サーバーはクライアントとは見なされません。 HTTP から HTTPS、または HTTPS から HTTP へのブリッジングは Configuration Manager ではサポートされていません。  

-   **トンネリング**:   
    プロキシ Web サーバーが SSL ブリッジングの要件をサポートできない場合や、Configuration Manager によって登録されたモバイル デバイスに対してインターネット サポートを構成する場合は、SSL トンネリングもサポートされます。 ただし、この方法は、安全性が低くなります。これは、インターネットからの SSL パケットが SSL 終了なしにサイト システムに転送されるので、悪意のあるコンテンツがないかどうかを検査できないためです。 SSL トンネリングを使用する場合は、プロキシ Web サーバーに証明書は必要ありません。  

##  <a name="a-nameplancomx-foresta-communications-across-active-directory-forests"></a><a name="Plan_Com_X-Forest"></a> 複数の Active Directory フォレスト間での通信  
System Center Configuration Manager では、複数の Active Directory フォレストにまたがるサイトや階層がサポートされます。  

Configuration Manager では、サイト サーバーと同じ Active Directory フォレストにはないドメイン コンピューターや、ワークグループのコンピューターもサポートされます。  

-   **ご使用のサイト サーバーのフォレストによって信頼されていないフォレスト内のドメイン コンピューターをサポートするために**、以下の操作を実行できます。  

    -   サイト情報をその Active Directory フォレストに発行するオプションを使用して、信頼されていないフォレストにサイト システムの役割をインストールします。  

    -   これらのコンピューターを、ワークグループ コンピューターであるかのように管理します。  

  信頼されていない Active Directory フォレストにサイト システム サーバーをインストールすると、そのフォレストのクライアントからのクライアントとサーバー間の通信はフォレスト内で保たれ、Configuration Manager が Kerberos 方式でコンピューターを認証できるようになります。 クライアントのフォレストにサイト情報を発行した場合は、クライアントは、利用可能な管理ポイントのリストなどのサイト情報を、割り当て済み管理ポイントからダウンロードする代わりに、Active Directory フォレストから取得できるという利点があります。  

  > [!NOTE]  
  >  インターネット上のデバイスを管理したい場合は、サイト システム サーバーが Active Directory フォレストにあるときは、インターネット ベースのサイト システムの役割を境界ネットワークにインストールできます。 この場合は、境界ネットワークとサイト サーバーのフォレスト間に双方向の信頼関係は必要ありません。  

-   **ワークグループ内のコンピューターをサポートするには**、以下の操作を実行する必要があります。  

    -   サイト システムの役割への HTTP クライアント接続を使用するときに、ワークグループのコンピューターを手動で承認します。 これは、Configuration Manager が Kerberos を使用してこれらのコンピューターを認証できないためです。  

    -   これらのコンピューターが配布ポイントからコンテンツを取得できるように、ネットワーク アクセス アカウントを使用するようにワークグループ クライアントを構成します。  

    -   ワークグループ クライアントが管理ポイントを検索するための代替手段を提供します。 DNS または WINS にサイト データを発行する方法と、管理ポイントを直接割り当てる方法があります。 これらのクライアントが Active Directory ドメイン サービスから情報を取得できないためです。  

    このコンテンツ ライブラリ内の関連リソース:  

    -   [Configuration Manager クライアントの競合しているレコードを管理する](../../../core/clients/manage/manage-clients.md#BKMK_ConflictingRecords)  

    -   [ネットワーク アクセス アカウント](../../../core/plan-design/hierarchy/fundamental-concepts-for-content-management.md#bkmk_NAA)  

    -   [ワークグループ コンピューターへの Configuration Manager クライアントのインストール方法](../../../core/clients/deploy/deploy-clients-to-windows-computers.md#BKMK_ClientWorkgroup)  

###  <a name="a-namebkmkspana-scenarios-to-support-a-site-or-hierarchy-that-spans-multiple-domains-and-forests"></a><a name="bkmk_span"></a> 複数のドメインとフォレストにまたがるサイトまたは階層をサポートするシナリオ  

#### <a name="communication-between-sites-in-a-hierarchy-that-spans-forests"></a>複数のフォレストにまたがっている階層のサイト間の通信  
このシナリオでは、Kerberos 認証をサポートするように、フォレスト間に双方向の信頼関係が成立している必要があります。  Kerberos 認証をサポートする双方向のフォレスト信頼関係がない場合、Configuration Manager ではリモート フォレスト内の子サイトがサポートされません。  

 **Configuration Manager では、親サイトのフォレストとの必要な双方向の信頼関係があるリモート フォレストへの子サイトのインストールがサポートされます**  

-   たとえば、必要な信頼関係が成立している限り、プライマリ親サイトとは異なるフォレストにセカンダリ サイトを配置できます。  

> [!NOTE]  
>  子サイトは、プライマリ サイト (中央管理サイトが親サイトになります) とセカンダリ サイトのどちらでもかまいません。  

Configuration Manager のサイト間通信には、データベースのレプリケーションとファイルベースの転送を使用します。 サイトをインストールするときに、目的のサーバーにサイトをインストールするアカウントを指定する必要があります。 このアカウントは、サイト間の通信を確立して管理するときにも使います。  

サイトのインストールが完了し、ファイルベースの転送とデータベースのレプリケーションが開始されたら、サイト間の通信で他に必要な構成はありません。  

**フォレストに双方向の信頼関係がある場合は、Configuration Manager で他に何も構成する必要はありません。**  

既定では、新しいサイトを別のサイトの子としてインストールすると、Configuration Manager によって、次の構成が行われます。  

-   サイト サーバーのコンピューター アカウントを使用する各サイトのファイルベースのサイト間レプリケーション ルート。 Configuration Manager は、各コンピューターのコンピューター アカウントを、対象コンピューターの **SMS_SiteToSiteConnection_&lt;サイト コード\>** グループに追加します。  

-   各サイトの SQL Server 間のデータベースのレプリケーション。  

次の設定も行う必要があります。  

-   介在するファイアウォールとネットワーク デバイスが、Configuration Manager で必要なネットワーク パケットを許可するように設定します。  

-   フォレスト間で名前が正しく解決されるようにします。  

-   サイトまたはサイト システムの役割をインストールするには、目的のコンピューターのローカル管理者権限のあるアカウントを指定する必要があります。  

#### <a name="communication-in-a-site-that-spans-forests"></a>複数のフォレストにまたがっているサイト内での通信  
このシナリオでは、フォレスト間の双方向の信頼関係は必要ありません。  

**プライマリ サイトでは、リモート フォレスト内のコンピューターへのサイト システムの役割のインストールがサポートされます**。  

-   アプリケーション カタログ Web サービス ポイントは、唯一の例外です。  この場合、サイト サーバーと同じフォレストでのみサポートされます。  

サイト システムの役割がインターネットからの接続を受け入れる場合は、セキュリティのベスト プラクティスとして、このサイト システムの役割を、フォレストの境界によってサイト サーバーが保護される場所 (境界ネットワークなど) にインストールすることをお勧めします。  

**信頼されていないフォレスト内のコンピューターにサイト システムの役割をインストールするには:**  

-   サイト システムの役割のインストールに使用される **サイト システムのインストール アカウント** を指定する必要があります。 このアカウントには、指定したコンピューターに接続してサイト システムの役割をインストールするローカル管理者の資格情報が必要です。  

-   サイト システムのオプションの [ **サイト サーバーがこのサイト システムへの接続を開始する必要がある**] を選択する必要があります。 この場合、サイト サーバーがサイト システム サーバーへの接続を確立してデータを転送する必要があります。 これで、信頼されていない場所にあるコンピューターが、信頼されたネットワーク内にあるサイト サーバーへの接続を開始するのを防げます。 これらの接続では **サイト システムのインストール アカウント**を使用します。  

**信頼されていないフォレストにインストールされたサイト システムの役割を使用するには** 、サイト サーバーがデータの転送を開始する場合でも、ファイアウォールがネットワーク トラフィックを許可する必要があります。  

さらに、次のサイト システムの役割ではサイト データベースに直接アクセスする必要があります。 そのため、ファイアウォールは、信頼されていないフォレストからサイトの SQL Server への適用可能なトラフィックを許可する必要があります。  

-   資産インテリジェンス同期ポイント  

-   Endpoint Protection ポイント  

-   登録ポイント  

-   管理ポイント  

-   レポート サービス ポイント  

-   状態移行ポイント  

詳細については、「[System Center Configuration Manager で使用されるポート](../../../core/plan-design/hierarchy/ports.md)」を参照してください。  

**このサイト データベースにアクセスするようにサイト システムの役割を構成しなければならないことがあります。**  

管理ポイントと登録ポイントの両方のサイト システムの役割が、サイト データベースに接続します。  

-   既定では、これらのサイト システムの役割をインストールするときに、Configuration Manager が新しいサイト システム サーバーのコンピューター アカウントをサイト システムの役割の接続アカウントとして構成し、適切な SQL Server データベースの役割にアカウントを追加します。  

-   これらのサイト システムの役割を信頼されていないドメインにインストールする場合は、サイト システムの役割の接続アカウントを構成し、サイト システムの役割がデータベースから情報を取得できるようにする必要があります。  

これらのサイト システムの役割の接続アカウントにドメイン ユーザー アカウントを構成する場合は、そのドメイン ユーザー アカウントに、対象サイトの SQL Server データベースへの適切なアクセス権があることをご確認ください。  

-   管理ポイント: **管理ポイント データベースの接続アカウント**  

-   登録ポイント: **登録ポイントの接続アカウント**  

サイト サーバーとは別のフォレストにサイト システムの役割を配置する場合は、次のことも検討してください。  

-   Windows ファイアウォールを実行する場合は、ファイアウォールの適切なプロファイルで、リモートのサイト システムの役割をインストールするコンピューターとサイト データベース サーバー間の通信を許可するように構成します。 ファイアウォールのプロファイルの詳細については、「 [ファイアウォールのプロファイルについて](http://go.microsoft.com/fwlink/p/?LinkId=233629)」をご覧ください。  

-   インターネット ベースの管理ポイントが、ユーザー アカウントを含むフォレストを信頼している場合は、ユーザー ポリシーを使用することができます。 信頼していない場合は、コンピューター ポリシーだけを使用できます。  

#### <a name="communication-between-clients-and-site-system-roles-when-the-clients-are-not-in-the-same-active-directory-forest-as-their-site-server"></a>クライアントがそのサイト サーバーと同じ Active Directory フォレストにない場合のクライアントとサイト システムの役割間の通信  
Configuration Manager では、サイト サーバーと同じフォレストにないクライアントに対する次のシナリオがサポートされます。  

-   クライアントのフォレストとサイト サーバーのフォレストとの間に双方向の信頼関係がある。  

-   サイト システムの役割サーバーがクライアントと同じフォレストにある。  

-   サイト サーバーのフォレストと双方向の信頼関係がないドメイン コンピューターにクライアントがあり、サイト システムの役割はクライアントのフォレストにインストールされていない。  

-   クライアントがワークグループのコンピューターにある。  

ドメインに参加しているコンピューターのクライアントは、そのサイト情報が Active Directory フォレストに発行されていると、Active Directory ドメイン サーバーをサービスの場所として使用することができます。  

サイト情報を別の Active Directory フォレストに発行できるようにするには、次の操作を実行する必要があります。  

-   フォレストを指定してから、 **[管理]** ワークスペースの **[Active Directory フォレスト]** ノードでそのフォレストへの発行を有効にする必要があります。  

-   各サイトを、Active Directory ドメイン サービスにデータを発行するように構成します。 このように構成すると、発行先フォレストにあるクライアントが、サイト情報を取得して管理ポイントを見つけられるようになります。 Active Directory Domain Services をサービスの場所として使用できないクライアントでは、DNS、WINS、またはクライアントの割り当て済み管理ポイントを使用できます。  

###  <a name="a-namebkmkxchangea-put-the-exchange-server-connector-in-a-remote-forest"></a><a name="bkmk_xchange"></a> リモート フォレストへの Exchange Server コネクタの配置  
このシナリオに対応するには、フォレスト間で正しく名前が解決されるようにして (たとえば、DNS 転送を構成します)、Exchange Server コネクタを構成するときに Exchange Server のイントラネット FQDN を指定します。 詳細については、「[System Center Configuration Manager と Exchange によるモバイル デバイスの管理](../../../mdm/deploy-use/manage-mobile-devices-with-exchange-activesync.md)」を参照してください。  



<!--HONumber=Nov16_HO1-->

