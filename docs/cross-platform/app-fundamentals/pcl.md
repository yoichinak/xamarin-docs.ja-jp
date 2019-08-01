---
title: ポータブルクラスライブラリ (PCL) の概要
description: この記事では、ポータブルクラスライブラリ (PCL) プロジェクトを紹介し、Visual Studio for Mac と Visual Studio で PCL プロジェクトを作成および使用する手順について説明します。
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: a4ee81f7d59c9fb680dfd371a7aaba7660fb3343
ms.sourcegitcommit: f255aa286bd52e8a80ffa620c2e93c97f069f8ec
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2019
ms.locfileid: "68681075"
---
# <a name="portable-class-libraries-pcl"></a>ポータブル クラス ライブラリ (PCL)

> [!TIP]
> ポータブルクラスライブラリ (Pcl) は、最新バージョンの Visual Studio では非推奨と見なされます。
> 新しいプロジェクトの場合でも、PCLs を開いて編集し、コンパイルすることはできますが、 [.NET Standard ライブラリ](~/cross-platform/app-fundamentals/net-standard.md)を使用して、より大きな API 領域にアクセスすることをお勧めします。

クロスプラットフォームアプリケーションを構築するための重要なコンポーネントは、プラットフォーム固有のさまざまなプロジェクト間でコードを共有できることです。 ただし、これは複雑になります。これは、プラットフォームによって .NET 基底クラスライブラリ (BCL) の異なるサブセットを使用することが多いため、実際には別の .NET Core ライブラリプロファイルにビルドされるためです。 つまり、各プラットフォームで使用できるのは、同じプロファイルをターゲットとするクラスライブラリのみであるため、プラットフォームごとに個別のクラスライブラリプロジェクトが必要になります。

この問題に対処するには、 **.NET Standard プロジェクト**、**共有アセットプロジェクト**、および**ポータブルクラスライブラリ (PCL) プロジェクト**という3つの主要な方法があります。

- **.NET Standard プロジェクト**は、.net コードを共有するために推奨される方法です。 [.NET Standard プロジェクトと Xamarin](~/cross-platform/app-fundamentals/net-standard.md)の詳細については、こちらを参照してください。
- **共有アセットプロジェクト**は、1セットのファイルを使用し、ソリューション内でコードを共有するためのすばやく簡単な方法を提供します。また、一般的には、条件付きコンパイルディレクティブを使用して、それを使用するさまざまなプラットフォームのコードパスを指定します (詳細については、詳細については、[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)に関する記事を参照してください。
- **PCL**プロジェクトは、BCL クラス/機能の既知のセットをサポートする特定のプロファイルを対象としています。 ただし、PCL の場合は、プロファイル固有のコードを独自のライブラリに分離するために、追加のアーキテクチャ作業が必要になることがよくあります。

このページでは、特定のプロファイルを対象とする**PCL**プロジェクトを作成する方法について説明します。このプロジェクトは、複数のプラットフォーム固有のプロジェクトで参照できます。

## <a name="what-is-a-portable-class-library"></a>ポータブルクラスライブラリとは何ですか。

アプリケーションプロジェクトまたはライブラリプロジェクトを作成すると、生成される DLL は、作成される特定のプラットフォームでの作業に限定されます。 これにより、Windows アプリのアセンブリを作成し、Xamarin の iOS と Xamarin Android で再利用することができなくなります。

ただし、ポータブルクラスライブラリを作成する場合は、コードを実行するプラットフォームの組み合わせを選択できます。 ポータブルクラスライブラリを作成するときの互換性の選択肢は、ライブラリがサポートするプラットフォームを示す "プロファイル" 識別子に変換されます。

次の表は、.NET プラットフォームによって異なる機能の一部を示しています。 特定のデバイスまたはプラットフォームで実行することが保証されている PCL アセンブリを作成するには、プロジェクトの作成時に必要なサポートを選択するだけです。

|機能|.NET Framework|UWP アプリ|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|Core|Y|Y|Y|Y|Y|
|LINQ (LINQ)|Y|Y|Y|Y|Y|
|IQueryable|Y|Y|Y|7.5 +|Y|
|シリアル化|Y|Y|Y|Y|Y|
|データの注釈|4.0.3 +|Y|Y||Y|

Xamarin 列には、Visual Studio に付属しているすべてのプロファイルが Xamarin. iOS と Xamarin Android でサポートされていることが反映されています。また、作成したライブラリの機能の可用性は、サポート対象として選択した他のプラットフォームによってのみ制限されます。

これには、次の組み合わせのプロファイルが含まれます。

- .NET 4 または .NET 4.5
- Silverlight 5
- Windows Phone 8
- UWP アプリ

[Microsoft の web サイト](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)にあるさまざまなプロファイルの機能の詳細を確認し、サポートされているフレームワーク情報やその他の注意事項を含む別のコミュニティメンバーの[PCL プロファイルの概要](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY)を参照できます。

**利点**

1. 一元化されたコード共有–1つのプロジェクトでコードを記述し、テストして、他のライブラリやアプリケーションが使用できるようにします。
2. リファクタリング操作は、ソリューションに読み込まれたすべてのコード (ポータブルクラスライブラリおよびプラットフォーム固有のプロジェクト) に影響します。
3. PCL プロジェクトは、ソリューション内の他のプロジェクトから簡単に参照できます。また、出力アセンブリを他のユーザーがソリューション内で参照できるように共有することもできます。

**短所**

1. 同じポータブルクラスライブラリは複数のアプリケーション間で共有されるため、プラットフォーム固有のライブラリを参照することはできません (例: CsharpSqlite. WP7)。
2. ポータブルクラスライブラリのサブセットには、Monotouch.dialog と Mono for Android (DllImport や system.object など) の両方で使用できるクラスが含まれていない場合があります。

> [!NOTE]
> ポータブルクラスライブラリは、最新バージョンの Visual Studio では非推奨とされており、代わりに[.NET Standard ライブラリ](net-standard.md)をお勧めします。

一部の短所は、プロバイダーパターンまたは依存関係の挿入を使用して、ポータブルクラスライブラリで定義されているインターフェイスまたは基本クラスに対してプラットフォームプロジェクトの実際の実装をコーディングすることを回避することができます。

次の図は、ポータブルクラスライブラリを使用してコードを共有するクロスプラットフォームアプリケーションのアーキテクチャを示しています。また、依存関係の挿入を使用してプラットフォームに依存する機能を渡すこともできます。

[![](pcl-images/image1.png "この図は、ポータブルクラスライブラリを使用してコードを共有するクロスプラットフォームアプリケーションのアーキテクチャを示しています。また、依存関係の挿入を使用してプラットフォームに依存する機能を渡すこともできます。")](pcl-images/image1.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac チュートリアル

このセクションでは、Visual Studio for Mac を使用してポータブルクラスライブラリを作成して使用する方法について説明します。 完全な実装については、「PCL の例」を参照してください。

### <a name="creating-a-pcl"></a>PCL の作成

ポータブルクラスライブラリをソリューションに追加することは、通常のライブラリプロジェクトを追加することとよく似ています。

1. **[新しいプロジェクト]** ダイアログで、 **[マルチプラットフォーム > ライブラリ > ポータブルライブラリ]** オプションを選択します。

    ![新しい PCL プロジェクトを作成する](pcl-images/image2.png)

1. PCL が Visual Studio for Mac に作成されると、その PCL は、Xamarin および Xamarin Android で動作するプロファイルで自動的に構成されます。 PCL プロジェクトは、次のスクリーンショットのように表示されます。

    ![Solution pad の PCL プロジェクト](pcl-images/image3.png)

PCL は、コードを追加する準備ができました。 他のプロジェクト (アプリケーションプロジェクト、ライブラリプロジェクト、およびその他の PCL プロジェクト) で参照することもできます。

### <a name="editing-pcl-settings"></a>PCL 設定の編集

このプロジェクトの PCL 設定を表示および変更するには、プロジェクトを右クリックし、[オプション] をクリックして **[> 全般]** を選択し、次に示す画面を表示 > ます。

[![プロファイルを設定する PCL プロジェクトオプション](pcl-images/image4-sml.png)](pcl-images/image4.png#lightbox)

このポータブルクラスライブラリのターゲットプロファイルを変更するには、[**変更.** ..] をクリックします。

コードが既に PCL に追加された後にプロファイルが変更された場合、新しく選択されたプロファイルに含まれていない機能をコードが参照していると、ライブラリがコンパイルされなくなる可能性があります。

## <a name="working-with-a-pcl"></a>PCL の使用

PCL ライブラリにコードが記述されている場合、Visual Studio for Mac エディターは、選択したプロファイルの制限を認識し、それに応じてオートコンプリートオプションを調整します。 たとえば、次のスクリーンショットは Visual Studio for Mac で使用されている既定のプロファイル (Profile136) を使用した System.IO のオートコンプリートオプションを示しています。使用可能なクラスの半分を示す scrollbar が表示されています (実際には14のみです)。使用可能なクラス)。

[![PCL の System.IO クラスに含まれる14クラスの Intellisense の一覧](pcl-images/image6.png)](pcl-images/image6.png#lightbox)

Xamarin. iOS または xamarin. Android プロジェクトの System.IO オートコンプリートと比較すると、PCL プロファイルに含まれていないやなど`File`の一般的に使用されるクラスを含む、 `Directory`使用可能なクラスが40あります。

[![.NET Framework System.IO 名前空間の40クラスの Intellisense の一覧](pcl-images/image7.png)](pcl-images/image7.png#lightbox)

これは、PCL の使用の基礎となるトレードオフを反映しています。多くのプラットフォームでコードをシームレスに共有できるため、特定の Api を使用できないことがあります。これは、すべてのプラットフォームで同等の実装がないためです。

### <a name="using-pcl"></a>PCL の使用

PCL プロジェクトを作成したら、互換性のあるアプリケーションまたはライブラリプロジェクトから参照を追加できます。通常は参照を追加するのと同じ方法で行います。 Visual Studio for Mac で、参照 ノードを右クリックし、**参照の編集** をクリックします。次に、次のように **プロジェクト** タブに切り替えます。

[![[参照の編集] オプションを使用して PCL への参照を追加する](pcl-images/image8.png)](pcl-images/image8.png#lightbox)

次のスクリーンショットは、TaskyPortable サンプルアプリのソリューションパッドを示しています。このアプリでは、PCL ライブラリが下部に表示され、その PCL ライブラリが Xamarin. iOS プロジェクトに参照されています。

[![PCL プロジェクトを示す TaskyPortable サンプルソリューション](pcl-images/image9.png)](pcl-images/image9.png#lightbox)

PCL からの出力 (つまり、結果のアセンブリ DLL) は、ほとんどのプロジェクトへの参照として追加することもできます。 このため、PCL はクロスプラットフォームのコンポーネントとライブラリを出荷するための理想的な方法です。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Visual Studio のチュートリアル

このセクションでは、Visual Studio を使用してポータブルクラスライブラリを作成して使用する方法について説明します。 完全な実装については、「PCL の例」を参照してください。

### <a name="creating-a-pcl"></a>PCL の作成

Visual Studio でソリューションに PCL を追加するのは、通常のプロジェクトを追加する場合と少し異なります。

1. **[新しいプロジェクトの追加]** 画面で、 **[クラスライブラリ (レガシポータブル)]** オプションを選択します。 右側の説明には、このプロジェクトの種類が非推奨とされていることが示されています。

    [![ポータブルクラスライブラリを作成するための [新しいプロジェクト] ウィンドウ](pcl-images/image10-sml.png "ポータブルクラスライブラリ")](pcl-images/image10.png#lightbox)

2. プロファイルを構成できるように、Visual Studio では、次のダイアログボックスが表示されます。
 サポートする必要があるプラットフォームをティックし、[OK] をクリックします。

    [![ライブラリのターゲットプラットフォームを選択します](pcl-images/image11-sml.png "サポートする必要があるプラットフォームをティックし、[OK] をクリックします")。](pcl-images/image11.png#lightbox)

3. Pcl プロジェクトは、pcl であることを示す&ndash;ために、プロジェクト名の横にテキスト **(ポータブル)** が表示さソリューションエクスプローラーに表示されます。

    ![PCL プロファイルによって定義された NET Framework](pcl-images/image12.png "PCL プロファイルによって定義された NET Framework")

PCL は、コードを追加する準備ができました。 他のプロジェクト (アプリケーションプロジェクト、ライブラリプロジェクト、およびその他の PCL プロジェクト) で参照することもできます。

### <a name="editing-pcl-settings"></a>PCL 設定の編集

PCL 設定を表示および変更するには、次のスクリーンショットに示すように、プロジェクトを右クリックし、[**プロパティ] > [ライブラリ**] を選択します。

[![プラットフォームターゲットの編集](pcl-images/image13-sml.png)](pcl-images/image13.png#lightbox)

コードが既に PCL に追加された後にプロファイルが変更された場合、新しく選択されたプロファイルに含まれていない機能をコードが参照していると、ライブラリがコンパイルされなくなる可能性があります。

> [!TIP]
> また、そのことを示すメッセージも表示され**ます。NETStandard は、コードを共有するために推奨される方法です**。 これは、PCLs がまだサポートされていても、.NET Standard にアップグレードすることをお勧めします。

### <a name="working-with-a-pcl"></a>PCL の使用

PCL ライブラリでコードを記述すると、Visual Studio は選択されたプロファイルの制限を認識し、それに応じて Intellisense オプションを調整します。 たとえば、次のスクリーンショットは、既定のプロファイル (Profile136) を使用した System.IO のオートコンプリートオプションを示しています。これは、使用可能なクラスの約半分が表示されていることを示しています (実際に使用できるクラスは14個だけです)。

[![PCL で使用できる IO クラスの数が少なくなっています](pcl-images/image14.png)](pcl-images/image14.png#lightbox)

通常のプロジェクトの System.IO オートコンプリートと比較すると、PCL プロファイルに含まれていないやなど`File`の一般的に使用されるクラスを含む、 `Directory`使用可能なクラスが40あります。

[![.NET Framework で使用できるその他の多くの IO クラス](pcl-images/image15.png)](pcl-images/image15.png#lightbox)

これは、PCL の使用の基礎となるトレードオフを反映しています。多くのプラットフォームでコードをシームレスに共有できるため、特定の Api を使用できないことがあります。これは、すべてのプラットフォームで同等の実装がないためです。

> [!TIP]
> .NET Standard 2.0 は、System.IO 名前空間を含む、PCLs よりもはるかに大きな API サーフェス領域を表します。 新しいプロジェクトの場合は、PCL より .NET Standard が推奨されます。

### <a name="using-pcl"></a>PCL の使用

PCL プロジェクトを作成したら、互換性のあるアプリケーションまたはライブラリプロジェクトから参照を追加できます。通常は参照を追加するのと同じ方法で行います。 Visual Studio の [参照] ノードを右クリックし`Add Reference...` 、次に示すように **[ソリューション > プロジェクト]** タブに切り替えます。

[![[参照プロジェクトの追加] タブを使用して PCL への参照を追加する](pcl-images/image16.png)](pcl-images/image16.png#lightbox)

次のスクリーンショットは、TaskyPortable サンプルアプリの [ソリューション] ウィンドウを示しています。これは、下に PCL ライブラリが表示され、Xamarin. iOS プロジェクトの PCL ライブラリへの参照が表示されます。

[![PCL ライブラリを示す TaskyPortable サンプルソリューション](pcl-images/image17.png)](pcl-images/image17.png#lightbox)

PCL からの出力 (つまり、結果のアセンブリ DLL) は、ほとんどのプロジェクトへの参照として追加することもできます。
このため、PCL はクロスプラットフォームのコンポーネントとライブラリを出荷するための理想的な方法です。

-----

## <a name="pcl-example"></a>PCL の例

[Taskyportable](https://docs.microsoft.com/samples/xamarin/mobile-samples/taskyportable/)サンプルアプリケーションは、Xamarin でポータブルクラスライブラリを使用する方法を示しています。
次に、iOS および Android で実行されるアプリのスクリーンショットをいくつか示します。

[![](pcl-images/image18.png "IOS、Android、および Windows Phone で実行されているアプリのスクリーンショットを次に示します。")](pcl-images/image18.png#lightbox)

これは、純粋に移植可能なコードであるさまざまなデータとロジッククラスを共有します。また、SQLite データベース実装の依存関係の挿入を使用してプラットフォーム固有の要件を組み込む方法も示します。

ソリューションの構造は次のとおりです (Visual Studio for Mac と Visual Studio でそれぞれ)。

[![](pcl-images/image19.png "ここでは、ソリューションの構造を Visual Studio for Mac と Visual Studio にそれぞれ示します。")](pcl-images/image19.png#lightbox)

SQLite NET コードには、デモンストレーションのためにプラットフォーム固有の部分 (各オペレーティングシステムで SQLite 実装を使用するため) があるため、ポータブルクラスライブラリにコンパイルできる抽象クラスにリファクタリングされています。実際のコードは、iOS および Android プロジェクトのサブクラスとして実装されています。

### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

ポータブルクラスライブラリは、サポートできる .NET 機能に限定されています。 複数のプラットフォームで実行するようにコンパイルされるため、SQLite- `[DllImport]` NET で使用される機能を使用することはできません。 代わりに、SQLite-NET は抽象クラスとして実装され、残りの共有コードを通じて参照されます。 抽象 API の抽出を次に示します。

```csharp
public abstract class SQLiteConnection : IDisposable {

    public string DatabasePath { get; private set; }
    public bool TimeExecution { get; set; }
    public bool Trace { get; set; }
    public SQLiteConnection(string databasePath) {
         DatabasePath = databasePath;
    }
    public abstract int CreateTable<T>();
    public abstract SQLiteCommand CreateCommand(string cmdText, params object[] ps);
    public abstract int Execute(string query, params object[] args);
    public abstract List<T> Query<T>(string query, params object[] args) where T : new();
    public abstract TableQuery<T> Table<T>() where T : new();
    public abstract T Get<T>(object pk) where T : new();
    public bool IsInTransaction { get; protected set; }
    public abstract void BeginTransaction();
    public abstract void Rollback();
    public abstract void Commit();
    public abstract void RunInTransaction(Action action);
    public abstract int Insert(object obj);
    public abstract int Update(object obj);
    public abstract int Delete<T>(T obj);

    public void Dispose()
    {
        Close();
    }
    public abstract void Close();

}
```

共有コードの残りの部分では、抽象クラスを使用して、データベースからオブジェクトを "格納" および "取得" します。 この抽象クラスを使用するアプリケーションでは、実際のデータベース機能を提供する完全な実装を渡す必要があります。

### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid と Taskyandroid

IOS と Android のアプリケーションプロジェクトには、PCL で共有コードを接続するために使用される、ユーザーインターフェイスとその他のプラットフォーム固有のコードが含まれています。

また、これらのプロジェクトには、そのプラットフォームで動作する抽象データベース API の実装も含まれています。 IOS と Android では、Sqlite データベースエンジンがオペレーティングシステムに組み込まれているので、実装は`[DllImport]`を使用して、データベース接続の具象実装を提供できます。 プラットフォーム固有の実装コードの抜粋を次に示します。

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```

完全な実装については、サンプルコードを参照してください。

## <a name="summary"></a>Summary

この記事では、Visual Studio for Mac と Visual Studio の内部から PCLs を作成および使用する方法を示した、ポータブルクラスライブラリの利点と落とし穴について簡単に説明しました。最後に、作業中の PCL を示す完全なサンプルアプリケーション (TaskyPortable) が導入されました。

## <a name="related-links"></a>関連リンク

- [TaskyPortable (サンプル)](https://docs.microsoft.com/samples/xamarin/mobile-samples/taskyportable/)
- [クロスプラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [ポータブル Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)
- [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)
- [.NET Framework を使用したクロスプラットフォーム開発 (Microsoft)](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)
