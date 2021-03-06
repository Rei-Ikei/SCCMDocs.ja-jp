1.  Configuration Manager コンソールで、**[ソフトウェア ライブラリ]** ワークスペースに移動し、**[ソフトウェア更新プログラム]** ノードを選択します。  

2.  次のいずれかの方法で、ダウンロードするソフトウェア更新プログラムを選択します。  

    -   **[ソフトウェア更新プログラム グループ]** ノードから 1 つまたは複数のソフトウェア更新プログラム グループを選択します。 次に、リボンで **[ダウンロード]** をクリックします。  

    -   **[すべてのソフトウェア更新プログラム]** ノードから 1 つまたは複数のソフトウェア更新プログラムを選択します。 次に、リボンで **[ダウンロード]** をクリックします。  

        > [!NOTE]  
        >  Configuration Manager の **[すべてのソフトウェア更新プログラム]** ノードには、**[重大]** および **[セキュリティ]** に分類され、過去 30 日以内にリリースされたソフトウェア更新プログラムのみが表示されます。  

        > [!TIP]  
        >  **[条件の追加]** をクリックして **[すべてのソフトウェア更新プログラム]** ノードに表示されるソフトウェア更新プログラムを絞り込みます。 よく使用する検索条件を保存して、保存した検索を **[検索]** タブで管理します。  


3.  ソフトウェア更新プログラムのダウンロード ウィザードの **[展開パッケージ]** ページで、次の設定を構成します。  

    -  **展開パッケージを選択する**: 展開に含まれるソフトウェア更新プログラムの既存の展開パッケージを選びます。  

        > [!NOTE]  
        >  選択した展開パッケージに既にサイトからダウンロードされているソフトウェア更新プログラムは、再ダウンロードされません。  

    -  **新しい展開パッケージを作成する**: ソフトウェア更新プログラムの新しい展開パッケージを作成して展開で使用します。 次の設定を構成します。  

        -   **名前**: 展開パッケージの名前を指定します。 パッケージには、パッケージの内容を簡潔に説明する一意の名前が必要です。 使用できる文字数は最大で 50 文字です。  

        -   **説明**: 展開パッケージに関する情報を示す説明を指定します。 省略可能な説明に使用できる文字数は、最大で 127 文字です。    

        -   **パッケージ ソース**: ソフトウェア更新プログラムのソース ファイルの場所を指定します。 ソースの場所に対応するネットワーク パス (例: `\\server\sharename\path`) を入力するか、**[参照]** をクリックして、該当するネットワークの場所を見つけます。 次のページに進む前に、展開パッケージのソース ファイル用の共有フォルダーを作成します。  

             - 指定した場所を別のソフトウェア展開パッケージのソースとして使用することはできません。  

             - Configuration Manager によって展開パッケージが作成された後で、展開パッケージのプロパティで、パッケージ ソースの場所を変更できます。 そうする場合、最初に、元のパッケージ ソースから新しいパッケージ ソースの場所にコンテンツをコピーします。  

             -  SMS プロバイダーのコンピューター アカウントと、ウィザードを実行してソフトウェア更新プログラムをダウンロードするユーザーには、ダウンロード先に対する **書き込み** アクセス許可が必要です。 ダウンロード先へのアクセスを制限します。 この制限は、攻撃者がソフトウェア更新プログラムのソース ファイルを改ざんするリスクを軽減します。  

        - **バイナリ差分レプリケーションを有効にする**: サイト間のネットワーク トラフィックを最小限に抑えるには、この設定を有効にします。 バイナリ差分レプリケーション (BDR) は、パッケージ全体の内容を更新するのではなく、パッケージ内の変更されたコンテンツのみを更新します。 詳細については、[「バイナリ差分レプリケーション」](/sccm/core/plan-design/hierarchy/fundamental-concepts-for-content-management#binary-differential-replication)をご覧ください。  

4.  **[配布ポイント]** ページで、ソフトウェア更新ファイルをホストする配布ポイントまたは配布ポイント グループを指定します。 配布ポイントの詳細については、「[配布ポイントの構成](/sccm/core/servers/deploy/configure/install-and-configure-distribution-points#bkmk_configs)」を参照してください。 このページは、ソフトウェア更新プログラムの新しい展開パッケージを作成する場合にのみ、使用することができます。  

5.  **[配布の設定]** ページは、ソフトウェア更新プログラムの新しい展開パッケージを作成する場合にのみ、使用することができます。 次の設定を指定します。  

    -   **配布の優先順位**: 展開パッケージの配布の優先度を指定します。 配布の優先度は、展開パッケージが子サイトの配布ポイントに送信されるときに適用されます。 展開パッケージは優先順位に従って送信されます。[高]、[中]、[低] の順です。 パッケージの優先順位が同じ場合は、作成された順に送信されます。 バックログがない場合、パッケージは優先順位に関係なく、すぐに処理されます。 既定では、サイトからパッケージは優先度 **[中]** で送信されます。  

    -   **オンデマンド配布を有効にする**: この設定を使用すると、この機能に対して構成されている配布ポイント、およびクライアントの現在の境界グループ内で構成されている配布ポイントへのオンデマンドのコンテンツ配布を有効にすることができます。 この設定を有効にすると、パッケージのコンテンツがクライアントによって要求されたが、それが利用できない場合に、配布マネージャーによってそのコンテンツを該当するすべての配布ポイントに配布するためのトリガーが管理ポイントで作成されます。 詳細については、「[オンデマンドのコンテンツ配布](/sccm/core/plan-design/hierarchy/fundamental-concepts-for-content-management#on-demand-content-distribution)」を参照してください。  

    -   **事前設定済みの配布ポイントの設定**: どのようにしてコンテンツを事前設定済みの配布ポイントに配布するかを指定します。 次のいずれかのオプションを選択します。  

        -   **パッケージが配布ポイントに割り当てられたときにコンテンツを自動的にダウンロードする**: 事前設定を無視してコンテンツを配布ポイントに配布します。   

        -   **配布ポイントにコンテンツの変更箇所のみダウンロードする**: 配布ポイントに対する初期コンテンツを事前設定し、コンテンツの変更内容を配布ポイントに配布します。  

        -   **このパッケージのコンテンツを配布ポイントに手動でコピーする**: 配布ポイントのコンテンツを常に事前設定します。 これが既定のオプションです。  

        配布ポイントへのコンテンツの事前設定の詳細については、「[事前設定されたコンテンツの使用](/sccm/core/servers/deploy/configure/deploy-and-manage-content#bkmk_prestage)」を参照してください。  


6.  **[ダウンロード先]** ページで、ソフトウェア更新プログラムのソース ファイルをダウンロードするために Configuration Manager で使用する場所を指定します。 次のいずれかのオプションを使用します。  

    -   **インターネットからソフトウェア更新プログラムをダウンロードする**: この設定を選択すると、ソフトウェア更新プログラムがインターネット上の場所からダウンロードされます。 これが既定のオプションです。  

    -   **マイ ネットワーク上の場所からソフトウェア更新プログラムをダウンロードする**: この設定を選択すると、ローカル ディレクトリまたは共有フォルダーからソフトウェア更新プログラムがダウンロードされます。 ウィザードを実行するコンピューターからインターネットにアクセスできない場合は、この設定が役立ちます。 インターネットにアクセスできる任意のコンピューターによって、ソフトウェア更新プログラムを事前にダウンロードしておくことができます。 その後、ウィザードを実行するコンピューターでアクセスできるローカル ネットワーク上の場所にそれらを格納します。  


7.  **[言語の選択]** ページ上で、選択したソフトウェア更新プログラムをサイトからダウンロードする言語を選択します。 サイトでは、選択した言語で利用できる場合にのみ、これらの更新プログラムがダウンロードされます。 特定の言語向けでないソフトウェア更新プログラムは、常にダウンロードされます。 既定では、ソフトウェアの更新ポイントのプロパティで構成した言語がウィザードで選択されます。 次のページに進む前に、言語を少なくとも 1 つ選択する必要があります。 ソフトウェア更新プログラムでサポートされていない言語のみを選択した場合、更新プログラムのダウンロードは失敗します。  

8. **[概要]** ページで、ウィザードで選択した設定を確認し、**[次へ]** をクリックして、ソフトウェア更新プログラムをダウンロードします。  

9. **[完了]** ページで、ソフトウェア更新プログラムが正常にダウンロードされたことを確認し、**[閉じる]** をクリックします。  
