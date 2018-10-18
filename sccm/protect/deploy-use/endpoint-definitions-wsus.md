---
title: WSUS からの Endpoint Protection のマルウェア定義
titleSuffix: Configuration Manager
ms.date: 02/14/2017
ms.prod: configuration-manager
ms.technology: configmgr-protect
ms.topic: conceptual
ms.assetid: a34d9401-83e4-471d-8e23-b8042fc11c90
author: aczechowski
description: 定義ファイルの更新を自動的に承認するように Windows Server Update Services を構成する方法を説明します。
manager: dougeby
ms.author: aaroncz
ms.openlocfilehash: b1c79a839ad1c83c96d7fe4583b46f6a3edbffba
ms.sourcegitcommit: 0b0c2735c4ed822731ae069b4cc1380e89e78933
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
ms.locfileid: "32349859"
---
# <a name="enable-endpoint-protection-malware-definitions-to-download-from-windows-server-update-services-wsus-for-configuration-manager"></a>Configuration Manager で Endpoint Protection のマルウェア定義を Windows Server Update Services (WSUS) からダウンロードできるようにする

*適用対象: System Center Configuration Manager (Current Branch)*

 マルウェア対策の定義を最新に保つために WSUS を使用する場合は、定義ファイルの更新を自動的に承認するように構成できます。 定義ファイルを最新に保つ方法として Configuration Manager ソフトウェア更新プログラムの使用をお勧めしますが、ユーザーが手動で定義ファイルの更新を開始できるようにする方法として、WSUS を構成することもできます。 次の手順を使用して、WSUS を定義ファイルの更新ソースとして構成します。

## <a name="to-synchronize-endpoint-protection-definition-updates-in-configuration-manager-software-updates"></a>Configuration Manager ソフトウェア更新プログラムで Endpoint Protection の定義ファイルの更新を同期するには

1.  Configuration Manager コンソールで、**[管理]** をクリックします。

2.  **[管理]** ワークスペースで **[サイトの構成]** を展開して、**[サイト]** をクリックします。

3.  ソフトウェアの更新ポイントが含まれているサイトを選びます。 **[設定]** グループで **[サイト コンポーネントの構成]** をクリックし、 **[ソフトウェアの更新ポイント]** をクリックします。

4.  **[ソフトウェアの更新ポイント コンポーネントのプロパティ]** ダイアログ ボックスの **[分類]** タブで、 **[定義ファイルの更新]** チェック ボックスをオンにします。

5.  WSUS で更新される **[製品]** を指定します。

    -   Windows 8.1 以前では、 **[ソフトウェアの更新ポイント コンポーネントのプロパティ]** ダイアログ ボックスの **[製品]** タブで、 **[Forefront Endpoint Protection 2010]** チェック ボックスをオンにします。

    -   Windows 10 以降では、 **[ソフトウェアの更新ポイント コンポーネントのプロパティ]** ダイアログ ボックスの **[製品]** タブで、 **[Windows Defender]** チェック ボックスをオンにします。

6.  **[OK]** をクリックして、[ **[分類]** ] ダイアログ ボックスを閉じます。

 次の手順を使用して、WSUS サーバーが Configuration Manager 環境に統合されていない場合の Endpoint Protection 更新ファイルを構成します。

## <a name="to-synchronize-endpoint-protection-definition-updates-in-standalone-wsus"></a>スタンドアロン WSUS で Endpoint Protection の定義ファイルの更新を同期するには

1.  WSUS 管理コンソールで、 **[コンピューター]** を展開して **[オプション]** をクリックし、 **[製品と分類]** をクリックします。

2.  WSUS で更新される **[製品]** を指定します。

    -   Windows 8.1 以前では、 **[ソフトウェアの更新ポイント コンポーネントのプロパティ]** ダイアログ ボックスの **[製品]** タブで、 **[Forefront Endpoint Protection 2010]** チェック ボックスをオンにします。

    -   Windows 10 以降では、 **[ソフトウェアの更新ポイント コンポーネントのプロパティ]** ダイアログ ボックスの **[製品]** タブで、 **[Windows Defender]** チェック ボックスをオンにします。

3.  **[製品と分類]** ダイアログ ボックスの **[分類]** タブで、 **[定義ファイルの更新]** と **[更新プログラム]** チェック ボックスをオンにします。

## <a name="approving-definition-updates"></a>定義ファイルの更新の承認
 Endpoint Protection 定義ファイルの更新を承認して、WSUS サーバーにダウンロードしてからでないと、使用可能な更新の一覧を要求するクライアントに対して更新を提供することはできません。 クライアントは WSUS サーバーに接続して、適用できる更新プログラムをチェックし、最新の承認済み定義ファイルの更新を要求します。

### <a name="to-approve-definitions-and-updates-in-wsus"></a>WSUS で定義ファイルと更新を承認するには

1.  WSUS 管理コンソールで **[更新]** をクリックし、 **[すべての更新プログラム]** または承認する更新の分類をクリックします。

2.  更新の一覧で、インストールを承認する更新を右クリックして **[承認]** をクリックします。

3.  **[更新プログラムの承認]** ダイアログ ボックスで更新を承認するコンピューター グループを選び、 **[インストールの承認]** をクリックします。

 手動の承認だけでなく、定義ファイルの更新や Endpoint Protection 更新プログラムの自動承認規則も設定できます。 これによって、WSUS でダウンロードした Endpoint Protection 定義ファイルの更新を自動的に承認するように WSUS を構成します。

### <a name="to-configure-an-automatic-approval-rule"></a>自動承認規則を構成するには

1.  WSUS 管理コンソールで **[オプション]** をクリックし、 **[自動承認]** をクリックします。

2.  **[更新規則]** タブで **[新しい規則]** をクリックします。

3.  **[ルールの追加]** ダイアログ ボックスで、 **[ステップ 1: プロパティを選択します]** を選び、 **[更新が特定の分類に含まれる場合]** チェック ボックスをオンにします。

4.  **[ステップ 2: プロパティを変更します]** で **任意の分類**をクリックします。

5.  **[定義ファイルの更新]** 以外のすべてのチェック ボックスをオフにし、 **[OK]** をクリックします。

6.  **[ルールの追加]** ダイアログ ボックスで、 **[ステップ 1: プロパティを選択します]** を選び、 **[更新が特定の製品に含まれる場合]** チェック ボックスをオンにします。

7.  **[ステップ 2: プロパティを変更します]** で **任意の製品**をクリックします。

8.  Windows 8.1 以前については **[Forefront Endpoint Protection]** 、Windows 10 以降については **[Windows Defender]** 以外のすべてのチェック ボックスをオフにし、 **[OK]** をクリックします。

9. **[ステップ 3: 名前を指定します]** で、規則の名前を入力して **[OK]** をクリックします。

10. **[自動承認]** ダイアログ ボックスで、新しく作成した規則のチェック ボックスをオンにし、 **[規則の実行]** をクリックします。

> [!NOTE]
>  WSUS サーバーとクライアント コンピューターのパフォーマンスを最大化するには、前の定義ファイルの更新を拒否します。 このタスクを実行するには、リビジョンの自動承認と、有効期限が切れた更新プログラムの自動拒否を構成します。 詳細については、 [マイクロソフト サポート技術情報の記事 938947](http://go.microsoft.com/fwlink/p/?LinkId=204078)をご覧ください。

> [!div class="button"]
[次のステップ >](endpoint-antimalware-policies.md)

> [!div class="button"]
[戻る >](endpoint-configure-alerts.md)
