---
title: CMG 証明書
description: クラウド管理ゲートウェイと共に使用するさまざまなデジタル証明書について説明します。
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.date: 03/22/2018
ms.topic: conceptual
ms.prod: configuration-manager
ms.technology: configmgr-client
ms.assetid: 71eaa409-b955-45d6-8309-26bf3b3b0911
ms.openlocfilehash: 02a830d10263164e26902247856f999523092c76
ms.sourcegitcommit: a849dab9333ebac799812624d6155f2a96b523ca
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/10/2018
ms.locfileid: "42584513"
---
# <a name="certificates-for-the-cloud-management-gateway"></a>クラウド管理ゲートウェイの証明書

*適用対象: System Center Configuration Manager (Current Branch)*

クラウド管理ゲートウェイ (CMG) でインターネット上のクライアントを管理するためのシナリオによっては、1 つまたは複数のデジタル証明書が必要になります。 さまざまなシナリオに関する詳細については、「[クラウド管理ゲートウェイの計画](/sccm/core/clients/manage/cmg/plan-cloud-management-gateway)」を参照してください。


## <a name="cmg-server-authentication-certificate"></a>CMG サーバー認証証明書

*この証明書はすべてのシナリオで必要です。*

Configuration Manager コンソールで CMG を作成するとき、この証明書を提供します。

CMG によって、インターネットベースのクライアントが接続する HTTPS が作成されます。 サーバーには、安全なチャネルを構築するためにサーバー認証証明書が必要になります。 パブリック プロバイダーからこの目的で証明書を購入するか、公開キー基盤 (PKI) から証明書を発行します。 詳細については、[クライアントが信頼する CMG のルート証明書](#cmg-trusted-root-certificate-to-clients)に関するページを参照してください。

 > [!TIP]
 > この証明書には、Azure のサービスを識別するために、一意の名前が必要になります。 証明書を要求する前に、希望する Azure ドメイン名が一意であることを確認します。 たとえば、*GraniteFalls.CloudApp.Net* にします。 [Microsoft Azure ポータル](https://portal.azure.com)にログオンします。 **[リソースの作成]** をクリックし、**[コンピューティング]** カテゴリを選択し、**[クラウド サービス]** をクリックします。 **[DNS 名]** フィールドに任意のプレフィックスを入力します。たとえば、*GraniteFalls* にします。 ドメイン名が使用できるか、既に別のサービスで使用されているかがインターフェイスに表示されます。 ポータルではサービスを作成しないでください。名前を利用できるかどうかはこのプロセスを利用して確認してください。 
  
 > [!NOTE]
 > バージョン 1802 以降、CMG サーバー認証証明書はワイルドカードに対応しています。 一部の証明書機関は、ホスト名にワイルドカード文字を使用して証明書を発行します。 たとえば、**\*.contoso.com** です。 ワイルドカード証明書を利用して PKI を簡素化し、保守管理コストを下げる組織もあります。
 <!--491233-->


### <a name="cmg-trusted-root-certificate-to-clients"></a>クライアントが信頼する CMG のルート証明書

クライアントは、CMG サーバー認証証明書を信頼する必要があります。 この信頼は、次の 2 つの方法で実現できます。
- グローバルで信頼されているパブリック証明書プロバイダーの証明書を利用します。 たとえば、DigiCert、Thawte、VeriSign です。この 3 つに限らず他にも存在します。 Windows クライアントには、このようなプロバイダーからの信頼されたルート CA (証明書機関) が含まれています。 このようなプロバイダーのいずれかが発行したサーバー認証証明書を利用することで、クライアントは自動的にそれを信頼します。 
- 公開キー基盤 (PKI) からのエンタープライズ CA によって発行された証明書を利用します。 ほとんどのエンタープライズ PKI 実装によって、Windows クライアントに信頼されたルート CA が追加されます。 たとえば、Active Directory 証明書サービスとグループ ポリシーを使用します。 クライアントが自動的に信頼しない CA から CMG サーバー認証証明書を発行する場合、CA の信頼されたルート証明書をインターネットベースのクライアントに追加する必要があります。
    - Configuration Manager 証明書プロファイルを使用し、クライアントで証明書をプロビジョニングすることもできます。 詳細については、「[証明書プロファイルの概要](/sccm/protect/deploy-use/introduction-to-certificate-profiles)」をご覧ください。

### <a name="server-authentication-certificate-issued-by-public-provider"></a>パブリック プロバイダーが発行したサーバー認証証明書

この方式を使用する場合、クライアントは証明書を自動的に信頼することになり、管理者が自分でカスタム証明書を作成する必要はありません。 Configuration Manager によって、cloudapp.net ドメインで Azure にサービスが作成されます。 パブリック証明書プロバイダーは、この名前で証明書を発行できません。 次のプロセスで DNS エイリアスを作成します。

1. 組織のパブリック DNS で正規名レコード (CNAME) を作成します。 このレコードによって、パブリック証明書で使用するフレンドリ名に CMG のエイリアスが作成されます。
たとえば、Contoso がその CMG に **GraniteFalls** という名前を付けると、Azure では **GraniteFalls.CloudApp.Net** になります。 Contoso 社のパブリック DNS である contoso.com 名前空間内に DNS 管理者が、実際のホスト名 **GraniteFalls.CloudApp.net** に対する **GraniteFalls.Contoso.com** 用の新しい CNAME レコードを作成します。
2. CNAME エイリアスの共通名 (CN) を使用してパブリック プロバイダーからのサーバー認証証明書を要求します。
たとえば、Contoso 社では、証明書の CN 用に **GraniteFalls.Contoso.com** を使用しています。
3. この証明書を利用し、Configuration Manager コンソールで CMG を作成します。 クラウド管理ゲートウェイの作成ウィザードの **[設定]** ページで: 
    - このクラウド サービスのサーバー証明書を (**証明書ファイル**から) 追加すると、ウィザードによって、サービス名として証明書 CN からホスト名が抽出されます。 
    - その後、**cloudapp.net** または Azure 米国政府クラウドの **usgovcloudapp.net** にサービス FQDN としてそのホスト名が追加され、Azure でサービスが作成されます。
    - たとえば、Contoso が CMG を作成すると、Configuration Manager によって証明書 CN からホスト名 **GraniteFalls** が抽出されます。 Azure によって、**GraniteFalls.CloudApp.net** として実際のサービスが作成されます。

### <a name="server-authentication-certificate-issued-from-enterprise-pki"></a>エンタープライズ PKI から発行されたサーバー認証証明書

クラウド配布ポイントの場合と同じように、CMG のカスタム SSL 証明書を作成します。 次の操作以外は、「[クラウドベースの配布ポイント用のサービス証明書の展開](/sccm/core/plan-design/network/example-deployment-of-pki-certificates#BKMK_clouddp2008_cm2012)」の指示に従います。

- カスタム Web サーバー証明書を要求するとき、証明書の共通名の FQDN を提供します。 これは、自分が所有するパブリック ドメイン名でも、cloudapp.net ドメインでもかまいません。 独自のパブリック ドメインを使用する場合は、組織のパブリック DNS に DNS エイリアスを作成するための上記のプロセスを参照してください。
- Azure パブリック クラウドで CMG Web サーバー証明書用の cloudapp.net パブリック ドメインを使用する場合は、**cloudapp.net** または Azure 米国政府クラウドの **usgovcloudapp.net** で終わる名前を使用します。



## <a name="azure-management-certificate"></a>Azure 管理証明書

*この証明書は従来のサービス展開に必要です。Azure Resource Manager の展開には必要ありません。*

この証明書は Azure Portal で、また、Configuration Manager コンソールで CMG を作成するときに提供します。

Azure で CMG を作成するには、Configuration Manager サービス接続ポイントは最初に Azure サブスクリプションに認証する必要があります。 従来のサービス展開を使用すると、この認証に Azure 管理証明書が使用されます。 Azure 管理者はこの証明書をサブスクリプションにアップロードします。 Configuration Manager コンソールで CMG を作成するとき、この証明書を提供します。

管理証明書をアップロードする方法の詳細と手順については、Azure のドキュメントの次の記事を参照してください。

- [クラウド サービスと管理証明書](/azure/cloud-services/cloud-services-certs-create#what-are-management-certificates)

- [Azure サービス管理証明書をアップロードする](/azure/azure-api-management-certs)

> [!IMPORTANT]
> 管理証明書に関連付けられているサブスクリプション ID を必ずコピーしてください。 Configuration Manager コンソールで CMG を作成するために使用します。



## <a name="client-authentication-certificate"></a>クライアント認証証明書

*この証明書は、Windows 7 デバイス、Windows 8.1 デバイス、Azure Active Directory (Azure AD) に参加していない Windows 10 デバイスを実行しているインターネットベースのクライアントに必要です。CMG 接続ポイントにも必要です。Azure AD に参加している Windows 10 クライアントには必要ありません。*

クライアントはこの証明書を利用し、CMG で認証します。 ハイブリッドであるか、クラウド ドメインに参加している Windows 10 デバイスは、Azure AD を利用して認証するため、この証明書を必要としません。

この証明書は Configuration Manager と前後関係のないところでプロビジョニングします。 たとえば、Active Directory 証明書サービスとグループ ポリシーを使用し、クライアント認証証明書を発行します。 詳細については、「[Windows コンピューター用のクライアント証明書の展開](/sccm/core/plan-design/network/example-deployment-of-pki-certificates#BKMK_client2008_cm2012)」を参照してください。


### <a name="client-trusted-root-certificate-to-cmg"></a>CMG が信頼するクライアントのルート証明書

*この証明書は、クライアント認証証明書を利用するときに必要です。すべてのクライアントで認証に Azure AD を使用するとき、この証明書は必要ありません。* 

Configuration Manager コンソールで CMG を作成するとき、この証明書を提供します。

CMG はクライアント認証証明書を信頼する必要があります。 この信頼を実現するには、信頼されたルート証明書チェーンを提供します。 信頼されたルート CA を 2 つ、中間 (下位) CA を 4 つ指定できます。 


#### <a name="export-the-client-certificates-trusted-root"></a>クライアント証明書の信頼されたルートをエクスポートする

クライアント認証証明書をコンピューターに発行したら、そのコンピューターでこのプロセスを使用し、信頼されたルートをエクスポートします。

1.  [スタート] メニューを開きます。 [ファイル名を指定して実行] ウィンドウに "run" と入力します。 **mmc** を開きます。 

2.  [ファイル] メニューで **[スナップインの追加と削除]** を選択します。

3.  [スナップインの追加と削除] ダイアログ ボックスで、**[証明書]** を選択し、**[追加]** をクリックします。 
    a. [証明書スナップイン] ダイアログ ボックスで、**[コンピューター アカウント]** を選択してから **[次へ]** をクリックします。 
    b. [コンピューターの選択] ダイアログ ボックスで **[ローカル コンピューター]** を選択し、**[完了]** をクリックします。 
    c. [スナップインの追加と削除] ダイアログ ボックスで、**[OK]** をクリックします。

4.  **[証明書]** を展開し、**[個人用]** を展開し、**[証明書]** を選択します。

5.  その使用目的が **[クライアント認証]** の証明書を選択します。 
    a. [操作] メニューから **[開く]** を選択します。 
    b. **[証明のパス]** タブに切り替えます。 c. チェーンの 1 つ上の証明書を選択し、**[証明書の表示]** をクリックします。

6.  この新しい証明書ダイアログ ボックスで、**[詳細]** タブに切り替えます。**[ファイルにコピー]** をクリックします。

7.  既定の証明書形式である **DER encoded binary X.509 (.CER)** を使用して証明書のエクスポート ウィザードを完了します。 エクスポートした証明書の名前と場所をメモします。

8. 元のクライアント認証証明書の証明パスですべての証明書をエクスポートします。 エクスポートされた証明書のうち、中間 CA である証明書と信頼されたルート CA である証明書をメモします。



## <a name="enable-management-point-for-https"></a>HTTPS 用の管理ポイントを有効にする

*証明書の要件*
- バージョン 1706 または 1710 で、クライアント認証証明書を利用してオンプレミス ID で従来のクライアントを管理するとき、この証明書が推奨されますが、必須ではありません。
- バージョン 1710 では、Azure AD に参加している Windows 10 クライアントを管理するとき、管理ポイントにこの証明書が必要になります。 
- バージョン 1802 より、この証明書はすべてのシナリオで必要です。 CMG に対して有効にする管理ポイントのみが HTTPS である必要があります。 この動作の変更により、Azure AD トークン ベースの認証のサポートが強化されます。 

この証明書は Configuration Manager と前後関係のないところでプロビジョニングします。 たとえば、Active Directory 証明書サービスとグループ ポリシーを使用し、Web サーバー証明書を発行します。 詳細については、「[PKI 証明書の要件](/sccm/core/plan-design/network/pki-certificate-requirements)」と「[IIS を実行するサイト システム用の Web サーバー証明書の展開](/sccm/core/plan-design/network/example-deployment-of-pki-certificates#BKMK_webserver2008_cm2012)」を参照してください。



## <a name="next-steps"></a>次のステップ

- [Set up cloud management gateway](/sccm/core/clients/manage/cmg/setup-cloud-management-gateway) (クラウド管理ゲートウェイの設定)
- [クラウド管理ゲートウェイについてよくあるご質問](/sccm/core/clients/manage/cmg/cloud-management-gateway-faq)
- [クラウド管理ゲートウェイのセキュリティとプライバシー](/sccm/core/clients/manage/cmg/security-and-privacy-for-cloud-management-gateway)
