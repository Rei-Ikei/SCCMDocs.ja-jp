---
title: タスクの自動化を計画する
titleSuffix: Configuration Manager
description: タスク シーケンスを作成する前に、Configuration Manager でのタスクの自動化を計画します。
ms.date: 08/17/2018
ms.prod: configuration-manager
ms.technology: configmgr-osd
ms.topic: conceptual
ms.assetid: fc497a8a-3c54-4529-8403-6f6171a21c64
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: 1ea1b104dfbdf23a080bc71da94b88cdcad31fa1
ms.sourcegitcommit: be8c0182db9ef55a948269fcbad7c0f34fd871eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/23/2018
ms.locfileid: "42755950"
---
# <a name="planning-considerations-for-automating-tasks-in-configuration-manager"></a>Configuration Manager でのタスクの自動化計画に関する考慮事項

*適用対象: System Center Configuration Manager (Current Branch)*

 Configuration Manager 環境でタスクを自動化するためのタスク シーケンスを作成できます。 これらのタスクには、参照コンピューターでの OS のキャプチャや、1 台以上の展開先コンピューターへの OS の展開などがあります。 タスク シーケンス アクションは、シーケンスの個別のステップで定義されます。 タスク シーケンスを実行すると、ローカル システム コンテキストのコマンド ライン レベルで各ステップのアクションが実行されます。 この動作は、タスク シーケンスの実行が完全に自動化されていてユーザーの介入が必要ないことを意味します。 



##  <a name="BKMK_TSStepsActions"></a> タスク シーケンスのステップとアクション  

 ステップは、タスク シーケンスの基本コンポーネントです。 次のようなコマンドを含めることができます。  
   - 参照コンピューターの OS を構成してキャプチャする  
   - Windows、ハードウェア ドライバー、Configuration Manager クライアント、ソフトウェアを対象のコンピューターにインストールする   


 ステップのアクションでは、タスク シーケンスのステップのコマンドが定義されています。 2 種類のアクションがあります。  
   - コマンド ラインの文字列で定義するアクションは、"*カスタム アクション*" と呼ばれます。  
   - Configuration Manager であらかじめ定義されているアクションは、"*組み込みアクション*" と呼ばれます。  


 タスク シーケンスは、カスタム アクションと組み込みアクションを任意に組み合わせて実行できます。  

 タスク シーケンスのステップには、ステップの動作方法を制御する条件を含めることもできます。 これらの動作としては、タスク シーケンスの停止や、エラーが発生した場合のタスク シーケンスの続行などがあります。 条件の種類の 1 つは、タスク シーケンス変数です。 たとえば、**SMSTSLastActionRetCode** 変数を使用して、前のステップの条件をテストできます。 条件は、1 つのステップまたはステップのグループに追加します。  

 タスク シーケンスでは、ステップが順番に処理されます。 このシーケンスには、ステップのアクションとステップでのすべての条件が含まれます。 Configuration Manager によって開始されたタスク シーケンスのステップの処理では、前のアクションが完了するまで次のステップは開始されません。 

 タスク シーケンスは、次の場合に完了とみなされます。 
   - すべてのステップが完了した  
   - すべてのステップが完了する前に、失敗したステップのために Configuration Manager がタスク シーケンスの実行を停止した。  


 たとえば、タスク シーケンスのステップで、配布ポイント上の参照イメージやパッケージが見つからない場合は、タスク シーケンスには壊れた参照が含まれます。 Configuration Manager は、失敗したステップにエラー発生時続行の条件が設定されていない場合、その時点でタスク シーケンスの実行を停止します。  

 > [!IMPORTANT]  
 >  既定では、1 つのステップまたはアクションが失敗すると、タスク シーケンスが失敗します。 ステップでエラーが発生した後もタスク シーケンスを続行するには、タスク シーケンスを編集し、 [オプション] タブをクリックして、 [エラー時に続行する]」を参照してください。  

 タスク シーケンスに追加できるステップの詳細については、「[タスク シーケンスのステップ](/sccm/osd/understand/task-sequence-steps)」を参照してください。  



##  <a name="BKMK_TSGroups"></a> タスク シーケンス グループ  

 タスク シーケンス内の複数のステップをグループ化できます。 タスク シーケンス グループは、名前、省略可能な説明、および省略可能な条件で構成されます。 タスク シーケンスは、グループの条件を 1 つの単位として評価から、次のステップに進みます。 グループを入れ子にしたり、ステップとサブグループの組み合わせを含めたりします。 グループは、1 つの条件を共有する複数のステップをまとめる場合に便利です。  

 タスク シーケンス グループに名前を割り当てます。 一意である必要はありません。 また、オプションで、タスク シーケンス グループの説明を指定することもできます。  

 > [!IMPORTANT]  
 >  既定では、グループ内のステップまたは入れ子のグループの 1 つが失敗すると、タスク シーケンス グループが失敗します。 ステップまたは埋め込まれたグループが失敗したときにタスク シーケンスを続行したい場合は、ステップまたはグループで **[エラー時に続行する]** オプションを設定します。  

 次の表には、ステップをグループ化した場合、**[エラー時に続行する]** オプションがどのように動作するかを示します。  

 この例では、2 つのタスク シーケンス グループそれぞれに、3 つのタスク シーケンス ステップが含まれています。  

 |タスク シーケンス グループまたはステップ|[エラー時に続行する] の設定状況|  
 |---------------------------------|-------------------------------|  
 |**タスク シーケンス グループ 1**|**[エラー時に続行する]** がオン。|  
 |タスク シーケンス ステップ 1|**[エラー時に続行する]** がオン。|  
 |タスク シーケンス ステップ 2|未設定。|  
 |タスク シーケンス ステップ 3|未設定。|  
 |**タスク シーケンス グループ 2**|未設定。|  
 |タスク シーケンス ステップ 4|未設定。|  
 |タスク シーケンス ステップ 5|未設定。|  
 |タスク シーケンス ステップ 6|未設定。|  


 -   タスク シーケンス ステップ 1 が失敗すると、タスク シーケンスはタスク シーケンス ステップ 2 へ進みます。  

 -   タスク シーケンス ステップ 2 が失敗すると、タスク シーケンスはタスク シーケンス ステップ 3 を実行しません。 タスク シーケンス グループ 1 は**エラー時に続行**に構成されているので、タスク シーケンスはタスク シーケンス グループ 2 を続行します。 次にタスク シーケンス ステップ 4 が実行されます。  

 -   タスク シーケンス ステップ 4 が失敗した場合、以降のステップは実行されません。 タスク シーケンス グループ 2 に対しては **[エラー時に続行]** の設定が構成されていないため、タスク シーケンスは失敗します。  



## <a name="add-child-task-sequences-to-a-task-sequence"></a>子タスク シーケンスをタスク シーケンスに追加する
 <!--1261338--> Configuration Manager バージョン 1710 以降では、別のタスク シーケンスを実行する新しいタスク シーケンス ステップを追加できます。 このステップでは、タスク シーケンス間の親子関係を作成します。 このステップを使用することで、再利用可能なモジュール型のタスク シーケンスをより多く作成できます。  

 詳しくは、「[タスク シーケンスの実行](/sccm/osd/understand/task-sequence-steps#child-task-sequence)」をご覧ください。 

 > [!Note]  
 > Configuration Manager では、このオプション機能は既定で無効です。 この機能は、使用する前に有効にする必要があります。 詳細については、「[更新プログラムのオプション機能の有効化](/sccm/core/servers/manage/install-in-console-updates#bkmk_options)」を参照してください。<!--505213-->  



##  <a name="BKMK_TSVariables"></a> タスク シーケンス変数  

 タスク シーケンス変数は、名前と値のペアのセットです。 これらは、Configuration Manager クライアント上のコンピューター、OS、ユーザーの状態構成タスクに対して、構成と OS 展開の設定を提供します。 タスク シーケンス変数は、タスク シーケンス ステップの構成およびカスタマイズを行うメカニズムです。  

 タスク シーケンスを実行すると、多くのタスク シーケンス設定は環境変数として保存されます。 組み込みタスク シーケンス変数の値を参照または変更できます。 新しいタスク シーケンス変数を作成したり、対象コンピュータでのタスク シーケンスの実行方法をカスタマイズしたりすることもできます。  

 次のアクションを実行するにはタスク シーケンス変数を使用します。  

 -   タスク シーケンス アクションの設定を構成する  

 -   タスク シーケンス ステップにコマンド ライン引数を指定する  

 -   タスク シーケンス ステップまたはグループを実行するかどうかを判断する条件を評価する  

 -   タスク シーケンスで使用されるカスタム スクリプトに値を提供する  


 たとえば、**ドメインまたはワークグループに参加**タスク シーケンス ステップを含むタスク シーケンスがあるものとします。 ドメイン メンバーシップによってコレクションのメンバーシップが決定される、さまざまなコレクションにこのタスク シーケンスを展開します。 各コレクションのドメイン名に対して、コレクションごとのタスク シーケンス変数を指定します。 その後、そのタスク シーケンス変数を使って、タスク シーケンスで適切なドメイン名を提供します。  

 詳しくは、「[How to use task sequence variables](/sccm/osd/understand/using-task-sequence-variables)」(タスク シーケンス変数の使い方) をご覧ください。



##  <a name="BKMK_TSCreate"></a> タスク シーケンスを作成する  

 タスク シーケンスの作成ウィザードを使用して、タスク シーケンスを作成します。 ウィザードでは、特定のタスクを実行する組み込みタスク シーケンスまたはさまざまな異なるタスクを実行できるカスタム タスク シーケンスを作成できます。 このウィザードでは、次の種類のタスク シーケンスを作成することができます。

 - 対象のコンピュータに既存の OS イメージをインストールする  

 - 参照コンピューターの OS イメージをビルドしてキャプチャする  

 - 対象のコンピュータにある OS アップグレード パッケージから Windows 10 にアップグレードする   

 - カスタマイズされたタスクまたは特別な OS の展開を実行するカスタム タスク シーケンスを作成する  


 詳しくは、「[タスク シーケンスの作成](/sccm/osd/deploy-use/manage-task-sequences-to-automate-tasks#BKMK_CreateTaskSequence)」をご覧ください。  



##  <a name="BKMK_TSEdit"></a> タスク シーケンスの編集  

 **タスク シーケンス エディター**を使用してタスク シーケンスを編集します。 エディターでは、タスク シーケンスに対して次のような変更を行えます。  

 - タスク シーケンスのステップを追加または削除する  

 - タスク シーケンスのステップの順序を変更する  

 - ステップのグループを追加または削除する  

 - エラーが発生した場合にタスク シーケンスを続行するかどうかを指定する  

 - タスク シーケンスのステップとグループに条件を追加する  


 > [!IMPORTANT]  
 >  編集の結果としてオブジェクトに関連付けられていない参照がタスク シーケンスにできた場合、閉じる前に参照の修正を求められます。 次のように対処できます。  
 > - 参照を修正する  
 > - 参照されていないオブジェクトをタスク シーケンスから削除する  
 > - 壊れた参照が修正または削除されるまで、失敗したタスク シーケンス ステップを一時的に無効にする  


 タスク シーケンスの編集方法について詳しくは、「[タスク シーケンスの編集](/sccm/osd/deploy-use/manage-task-sequences-to-automate-tasks#BKMK_ModifyTaskSequence)」を参照してください。  



##  <a name="BKMK_TSDeploy"></a> タスク シーケンスの展開  

 任意の Configuration Manager コレクションにある展開先コンピューターに対してタスク シーケンスを展開します。 オペレーティング システムを不明なコンピューターに展開するには、組み込みの**すべての不明なコンピューター** コレクションを使用します。 タスク シーケンスをユーザー コレクションに展開することはできません。  

 > [!IMPORTANT]  
 >  オペレーティング システムをインストールするタスク シーケンスを不適切なコレクションに展開しないでください。 タスク シーケンスを展開するコレクションには、OS をインストールするコンピューターのみが含まれていることを確認してください。 意図しない OS の展開を防ぐには、危険度の高い展開の設定を構成します。 詳細については、「[危険度の高い展開を管理するための設定](/sccm/core/servers/manage/settings-to-manage-high-risk-deployments)」を参照してください。  

 タスク シーケンスを受信する各展開先コンピューターは、展開で指定された設定に従ってタスク シーケンスを実行します。 タスク シーケンスには、関連するファイルやプログラムは含まれません。 タスク シーケンスによって参照されるすべてのファイルは、展開先コンピューターか、クライアントがアクセスできる配布ポイントに既に存在している必要があります。 

 > [!NOTE]  
 > プログラムまたはパッケージが既に展開先コンピューターにインストールされている場合でも、タスク シーケンスはプログラムによって参照されるパッケージをインストールします。 
 > 
 > タスク シーケンスでアプリケーションをインストールする場合、アプリケーションの要件規則が満たされ、アプリケーションがまだインストールされていない場合にのみ、アプリケーションに指定された探索方法に基づいてアプリケーションがインストールされます。  

 Configuration Manager クライアントは、クライアント ポリシーのダウンロード時にタスク シーケンスの展開を実行します。 次のポーリング サイクルまで待機せずにこのアクションをトリガーする場合は、「[Configuration Manager クライアントのポリシーの取得開始](/sccm/core/clients/manage/manage-clients#BKMK_PolicyRetrieval)」をご覧ください。  

 書き込みフィルターを有効にした Windows Embedded デバイスにタスク シーケンスを展開する場合、展開中はデバイスで書き込みフィルターを無効にし、展開後にデバイスを再起動するかどうかを指定できます。 書き込みフィルターが無効な場合、タスク シーケンスは一時オーバーレイに展開され、デバイスを再起動すると使用できなくなります。  

 > [!NOTE]  
 >  タスク シーケンスを Windows Embedded デバイスに展開する場合、デバイスが、メンテナンス期間が構成されたコレクションのメンバーであることを確認します。 これによって、書き込みフィルターを無効または有効にするタイミングや、デバイスを再起動するタイミングを管理できます。  
 >   
 >  メンテナンス期間以外でクライアントがタスク シーケンスをダウンロードする場合、タスク シーケンスは 2 度ダウンロードされます。 このシナリオのクライアントは、タスク シーケンスをダウンロードし、書き込みフィルターを無効にし、コンピューターを再起動してから、タスク シーケンスをもう一度ダウンロードします。 このような動作になるのは、タスク シーケンスがもともと一時的なオーバーレイにダウンロードされていて、デバイスが再起動するとクリアされるためです。  

 タスク シーケンスの展開方法の詳細については、「[タスク シーケンスの展開](/sccm/osd/deploy-use/manage-task-sequences-to-automate-tasks#BKMK_DeployTS)」を参照してください。  



##  <a name="BKMK_TSExportImport"></a> タスク シーケンスのエクスポートおよびインポート  

 Configuration Manager ではタスク シーケンスをエクスポートおよびインポートできます。 タスク シーケンスをエクスポートする場合、タスク シーケンスで参照されるオブジェクトを含めることができます。 

 詳しくは、「[タスク シーケンスのエクスポートおよびインポート](/sccm/osd/deploy-use/manage-task-sequences-to-automate-tasks#BKMK_ExportImport)」をご覧ください。  



##  <a name="BKMK_TSRun"></a> タスク シーケンスの実行  

 タスク シーケンスは常にローカル システム アカウントを使用して実行されます。 タスク シーケンスを実行するときに、Configuration Manager クライアントはタスク シーケンスのステップを開始する前に、参照されているパッケージを最初に確認します。 参照されているパッケージを検証またはダウンロードできない場合、タスク シーケンスは関連付けられているタスク シーケンス ステップについてエラーを返します。  

 > [!Note]  
 > タスク シーケンスの**コマンド ラインの実行**ステップでは、別のアカウントとしてコマンドを実行できます。  

 ダウンロードして実行するようにタスク シーケンスの展開を構成すると、Configuration Manager クライアントは依存するすべてのコンテンツをそのキャッシュにダウンロードします。 クライアント キャッシュのサイズが小さすぎる場合、またはコンテンツが見つからない場合、タスク シーケンスは失敗します。 クライアントは、状態メッセージを生成します。 

 必要なときにのみクライアントがコンテンツをダウンロードするように指定することもできます。 そのためには、タスク シーケンスの展開で **[実行中のタスク シーケンスでコンテンツが必要になったときにローカルにダウンロードする]** を選択します。 もう 1 つのオプションは **[配布ポイントからプログラムを実行する]** ことです。 このオプションでは、クライアントはファイルを配布ポイントから直接インストールし、最初にキャッシュにダウンロードしません。 

 タスク シーケンスの展開を**利用可能**として構成すると、クライアントがタスク シーケンスの依存コンテンツを特定できない場合は、すぐにエラーが送信されます。 **必須**の展開の場合、このような状況では Configuration Manager クライアントは待機します。 クライアントがアクセスできるコンテンツの場所にコンテンツがレプリケートされていない場合、クライアントは期限までコンテンツのダウンロードを再試行します。  

 タスク シーケンスが正常に完了または失敗した場合、Configuration Manager はこの状態をクライアント履歴に記録します。 

 コンピューターでタスク シーケンスが開始した後では、それをキャンセルまたは停止することはできません。  

 > [!IMPORTANT]  
 >  タスク シーケンスのステップでコンピューターを再起動する必要がある場合、クライアントはフォーマット済みのディスク パーティションに起動できる必要があります。 そうしないと、タスク シーケンスに指定されているエラー処理に関係なく、タスク シーケンスは失敗します。  

 タスク シーケンスの依存オブジェクトが新しいバージョンに更新されると、そのパッケージを参照しているすべてのタスク シーケンスが自動的に更新されます。 展開した更新プログラムの数に関係なく、最新バージョンが参照されます。  



##  <a name="BKMK_TSMaintenanceWindow"></a> メンテナンス期間を使用して、タスク シーケンスをいつ実行できるかを指定する  

 デバイス コレクションのメンテナンス期間を定義することで、タスク シーケンスが実行されるタイミングを指定できます。 メンテナンス期間の開始日、開始時刻、終了時刻、および繰り返しパターンを構成します。 メンテナンス期間のスケジュールを設定するときに、メンテナンス期間がタスク シーケンスにのみ適用されることを指定できます。 詳細については、「[メンテナンス期間を使用する方法](/sccm/core/clients/manage/collections/use-maintenance-windows)」を参照してください。  

 > [!IMPORTANT]  
 >  タスク シーケンスを実行するメンテナンス期間を構成した場合、タスク シーケンスが開始された後は、メンテナンス期間が終了してもタスク シーケンスは続行されます。  



##  <a name="BKMK_TSNetworkAccessAccount"></a> タスク シーケンスとネットワーク アクセス アカウント  

 タスク シーケンスはローカル システム アカウントのコンテキストでのみ実行されます。次のような状況では、[ネットワーク アクセス アカウント](/sccm/core/plan-design/hierarchy/accounts#network-access)の構成が必要になることがあります。  

 - タスク シーケンスが配布ポイント上の Configuration Manager コンテンツにアクセスを試みる場合。 ネットワーク アクセス アカウントを正しく構成しないと、タスク シーケンスは失敗します。   

 - ブート イメージを使用して OS の展開を開始するとき。 この場合、Configuration Manager は Windows PE 環境を使用し、これは完全な OS ではありません。 Windows PE 環境では、自動的に生成されたランダムな名前が使用されます。この名前はどのドメインのメンバーでもありません。 ネットワーク アクセス アカウントを正しく構成しないと、コンピューターはタスク シーケンスに必要なコンテンツにアクセスできません。  


 > [!NOTE]  
 >  プログラムの実行、アプリケーションのインストール、更新のインストール、タスク シーケンスの実行のためのセキュリティ コンテキストとして、ネットワーク アクセス アカウントが使用されることはありません。 ネットワーク アクセス アカウントは、ネットワーク上の関連付けられているリソースへのアクセスにのみ使用されます。  


 ネットワーク アクセス アカウントについて詳しくは、「[ネットワーク アクセス アカウント](/sccm/core/plan-design/hierarchy/manage-accounts-to-access-content#bkmk_NAA)」をご覧ください。  



##  <a name="BKMK_TSCreateMedia"></a> タスク シーケンス用のメディアの作成  

 タスク シーケンスおよび関連するファイル、依存関係を複数の種類のメディアに書き込むことができます。 Configuration Manager は、キャプチャ メディア、スタンドアロン メディア、および起動可能なメディアとして、DVD や USB フラッシュ ドライブなどのリムーバブル メディアをサポートします。 事前設定されたメディアは、Windows イメージ (WIM) ファイルを使用します。  

 メディアを作成するときに、アクセスを制御するためのパスワードを指定します。 その後、ユーザーがタスク シーケンスを実行するには、対象のコンピューターでパスワードを入力する必要があります。  

 タスク シーケンスをメディアから実行すると、メディアの指定されているプロセッサ アーキテクチャが認識されません。 指定されているアーキテクチャが対象のコンピュータと一致しない場合でも、タスク シーケンスは実行を試みます。 メディアのアーキテクチャが対象のコンピューターのアーキテクチャと一致しない場合、タスク シーケンスは失敗します。  

 詳しくは、「[タスク シーケンス メディアの作成](/sccm/osd/deploy-use/create-task-sequence-media)」をご覧ください。  


### <a name="media-types"></a>メディアの種類
 Configuration Manager では、次の種類のメディアがサポートされています。  

#### <a name="capture-media"></a>［キャプチャ メディア］
 キャプチャ メディアは、Configuration Manager インフラストラクチャ外で構成および作成された OS イメージをキャプチャします。 キャプチャ メディアには、タスク シーケンスの実行前に実行できるカスタム プログラムを収録できます。 カスタム プログラムを使用すると、デスクトップと通信したり、ユーザーに値の入力を要求したり、タスク シーケンスが使用する変数を作成したりできます。  

 詳細については、「[キャプチャ メディアを作成する](/sccm/osd/deploy-use/create-capture-media)」を参照してください。  

#### <a name="stand-alone-media"></a>［スタンドアロン メディア］
 スタンドアロン メディアには、タスク シーケンスと、そのタスク シーケンスの実行に必要なすべての関連オブジェクトが含まれます。 スタンドアロン メディアのタスク シーケンスは、Configuration Manager からネットワークへの接続に制限があるか、ネットワーク接続できない場合に実行します。 スタンドアロン メディアは、次の方法で実行します。  

 - 対象のコンピューターが起動されていない場合は、タスク シーケンスに関連付けられている Windows PE イメージをスタンドアロン メディアから使用して、タスク シーケンスが開始します。  

 - スタンドアロン メディアを手動で開始します。 ユーザーがコンピューターにサインインしている場合、メディアからタスク シーケンスを開始できます。  


 > [!IMPORTANT]  
 >  スタンドアロン メディアのタスク シーケンスのステップは、ネットワークからデータを取得せずに実行できる必要があります。 そうでない場合、タスク シーケンス ステップはデータを取得しようとして失敗します。 たとえば、パッケージを取得するために配布ポイントが必要なタスク シーケンス ステップは失敗します。 スタンドアロン メディアに必要なパッケージが含まれている場合、タスク シーケンス ステップは成功します。  


 詳細については、「[スタンドアロン メディアの作成](/sccm/osd/deploy-use/create-stand-alone-media)」を参照してください。  

#### <a name="bootable-media"></a>起動可能なメディア
 起動可能なメディアには、Configuration Manager インフラストラクチャに接続できるように、対象のコンピューターの起動に必要なファイルが含まれます。 その後、コレクションのメンバーシップに基づいて、実行するタスク シーケンスが決定されます。 このメディアには、タスク シーケンスまたは依存オブジェクトは含まれません。 代わりに、クライアントはネットワーク経由でコンテンツをダウンロードします。 この方法は、新しいコンピューターやベアメタルの展開、または対象のコンピューターに OS がない場合に便利です。  

 詳細については、「[起動可能なメディアの作成](/sccm/osd/deploy-use/create-bootable-media)」を参照してください。  

#### <a name="prestaged-media"></a>事前設定されたメディア
 事前設定されたメディアは、事前準備されていない対象のコンピューターに OS イメージを展開します。 事前設定されたメディアは、Windows イメージ (WIM) ファイルとして格納されます。 このファイルは、製造元または企業のステージング センターで、ベアメタル コンピューターにインストールできます。 事前設定されたメディアの利点は、これらの場所で Configuration Manager 環境に接続する必要がないことです。  

 詳細については、「[事前設定されたメディアの作成](/sccm/osd/deploy-use/create-prestaged-media)」を参照してください。  
