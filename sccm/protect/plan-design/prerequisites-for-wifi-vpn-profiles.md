---
title: Wi-Fi と VPN プロファイルの前提条件
titleSuffix: Configuration Manager
description: System Center Configuration Manager で証明書プロファイル、Wi-Fiプロファイル、および VPN プロファイルを管理するために必要なセキュリティ アクセス許可について説明します。
ms.date: 11/23/2016
ms.prod: configuration-manager
ms.technology: configmgr-protect
ms.topic: conceptual
ms.assetid: d2dacb2d-ab3b-42a2-8dc8-94da31f993c2
author: aczechowski
manager: dougeby
ms.author: aaroncz
ms.openlocfilehash: 894717df4b5acb7142aa7d171a8b8b63a28f8dc0
ms.sourcegitcommit: 0b0c2735c4ed822731ae069b4cc1380e89e78933
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
ms.locfileid: "32347384"
---
# <a name="prerequisites-for-wi-fi-and-vpn-profiles-in-system-center-configuration-manager"></a>System Center Configuration Manager の Wi-Fi プロファイルおよび VPN プロファイルの前提条件

*適用対象: System Center Configuration Manager (Current Branch)*

System Center Configuration Manager の Wi-Fi プロファイルおよび VPN プロファイルには、製品内の依存関係のみがあります。  

 証明書プロファイルや Wi-Fi プロファイル、VPN プロファイルなどの会社のリソースのアクセス設定を管理するには、次のセキュリティのアクセス許可が必要です。  

-   Wi-Fi プロファイルのアラートとレポートを表示して管理する場合: **アラート**オブジェクトの**作成**、**削除**、**変更**、**レポートの変更**、**読み取り**、**レポートの実行**。  

-   証明書プロファイルを作成して管理する場合: **証明書プロファイル**オブジェクトの **作成者ポリシー**、 **レポートの変更**、 **読み取り** 、 **レポートの実行** 。  

-   Wi-Fi プロファイル、証明書プロファイル、および VPN プロファイルの展開を管理する場合: **コレクション**オブジェクトの **構成ポリシーの展開**、 **クライアント ステータス アラートの変更**、 **読み取り** 、 **リソースの読み取り** 。  

-   すべての構成ポリシーを管理する場合: **構成のポリシー**オブジェクトの **作成**、 **削除**、 **変更**、 **読み取り** 、 **セキュリティ スコープの設定** 。  

-   Wi-Fi プロファイルおよび VPN プロファイルに関連するクエリを実行する場合: **クエリ** オブジェクトの**読み取り**アクセス許可。  

-   System Center Configuration Manager コンソールで Wi-Fiプロファイルおよび VPN プロファイル情報を表示する場合: **サイト** オブジェクトの**読み取り**アクセス許可。  

-   Wi-Fi プロファイルおよび VPN プロファイルのステータス メッセージを表示する場合: **ステータス メッセージ** オブジェクトの **読み取り** アクセス許可。  

-   信頼された証明機関証明書プロファイルを作成および変更する場合: **信頼された証明機関証明書プロファイル**オブジェクトの **作成者ポリシー**、 **レポートの変更**、 **読み取り** 、 **レポートの実行** 。  

-   VPN プロファイルを作成して管理する場合: **VPN プロファイル**オブジェクトの **作成者ポリシー**、 **レポートの変更**、 **読み取り** 、 **レポートの実行** 。  

-   Wi-Fi プロファイルを作成して管理する場合: **Wi-Fi プロファイル**オブジェクトの **作成者ポリシー**、 **レポートの変更**、 **読み取り** 、 **レポートの実行** 。  

 **[会社リソース アクセス マネージャー]** セキュリティ ロールには、System Center Configuration Manager で Wi-Fi プロファイルを管理するのに必要な上記のアクセス許可が付与されています。 詳細については、「[System Center Configuration Manager でのセキュリティの構成](../../core/plan-design/security/configure-security.md)」を参照してください。
