---
title: ポータブル クラス ライブラリ (PCL) の概要
description: この記事では、ポータブル クラス ライブラリ (PCL) プロジェクトを紹介し、作成および Mac と Visual Studio の Visual Studio での PCL プロジェクトを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 221ee49e282b3b038d03f659d238336710283a66
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/18/2019
ms.locfileid: "58175409"
---
# <a name="portable-class-libraries-pcl"></a>ポータブル クラス ライブラリ (PCL)

> [!TIP]
> ポータブル クラス ライブラリ (Pcl) では、最新バージョンの Visual Studio では非推奨と見なされます。
> 引き続き、編集を開いて Pcl をコンパイル、新しいプロジェクトが勧めを使用する[.NET Standard ライブラリ](~/cross-platform/app-fundamentals/net-standard.md)より大きな API サーフェス領域にアクセスします。

クロス プラットフォーム アプリケーションを構築するための重要なコンポーネントは、さまざまなプラットフォーム固有プロジェクトでコードを共有できることです。 ただし、さまざまなプラットフォームが多くの場合、.NET 基本クラス ライブラリ (BCL) の異なるサブ セットを使用し、そのため、別の .NET Core ライブラリのプロファイルに構築されて実際に、これは複雑です。 つまり、各プラットフォームは、プラットフォームごとに別のクラス ライブラリ プロジェクトを要求するように出現するために同じプロファイルを対象にしたクラス ライブラリにのみ使用できます。

この問題に対処するコードを共有するの 3 つの主要な方法はあります **.NET Standard プロジェクト**、**共有資産プロジェクト**、および**ポータブル クラス ライブラリ (PCL) プロジェクト**.

- **.NET standard プロジェクト**についての詳細の .NET コードを共有するための推奨されるアプローチは[.NET Standard プロジェクト、および Xamarin](~/cross-platform/app-fundamentals/net-standard.md)します。
- **共有資産プロジェクト**と一般に、ソリューション内のコードを共有するための簡単な方法 (詳細についてはそれを使用するさまざまなプラットフォーム用のコード パスを指定する条件付きコンパイル ディレクティブを使用してファイルおよびプランのセットを 1 つを使用して、情報を参照してください、[共有プロジェクト記事](~/cross-platform/app-fundamentals/shared-projects.md))。
- **PCL**プロジェクト既知の BCL クラス/機能のセットをサポートする特定のプロファイルを対象にします。 ただし、PCL に欠点は、余分なアーキテクチャ作業は独自のライブラリにプロファイルの特定のコードを分離する多くの場合、必要があることにします。

このページを作成する方法を説明します、 **PCL** 、複数のプラットフォーム固有プロジェクトで参照できますが、特定のプロファイルを対象とするプロジェクト。

## <a name="what-is-a-portable-class-library"></a>ポータブル クラス ライブラリとは何ですか。

アプリケーション プロジェクトまたはライブラリ プロジェクトを作成するときに、結果の DLL は、それが作成された特定のプラットフォームでの作業に制限されます。 これは、Windows アプリでは、アセンブリを記述したり、Xamarin.iOS および Xamarin.Android で再利用できないようにします。

ポータブル クラス ライブラリを作成するときに、上で実行するコードを作成プラットフォームの組み合わせを選択できます。 ポータブル クラス ライブラリを作成するときに行った互換性選択肢は、ライブラリがサポートされるプラットフォームについて説明します「プロファイル」の識別子に変換されます。

次の表では、.NET プラットフォームによって異なります機能の一部を示します。 特定のデバイス/プラットフォーム上で実行することが保証される PCL アセンブリを記述するには、単に選択したプロジェクトを作成するときに、どのサポートが必要です。

|機能|.NET Framework|UWP アプリ|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|コア|Y|Y|Y|Y|Y|
|LINQ|Y|Y|Y|Y|Y|
|IQueryable|Y|Y|Y|7.5 +|Y|
|シリアル化|Y|Y|Y|Y|Y|
|データの注釈|4.0.3 +|Y|Y||Y|

Xamarin の列には、Xamarin.iOS および Xamarin.Android は、Visual Studio に付属のすべてのプロファイルをサポートするいると、使用可能なすべてのライブラリを作成する機能をサポートするその他のプラットフォームによってのみ制限されますが反映されます。

これには、組み合わせのプロファイルが含まれます。

- .NET 4 または .NET 4.5
- Silverlight 5
- Windows Phone 8
- UWP アプリ

詳細のさまざまなプロファイルの機能について[マイクロソフトの web サイト](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)して別のコミュニティ メンバーの[PCL プロファイルの概要](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY)framework 情報とその他のメモにサポートが含まれています。

**特典**

1. 集められたコードの共有 – 記述し、その他のライブラリやアプリケーションで使用できる 1 つのプロジェクトでコードをテストします。
2. リファクタリング操作は、すべてのコードに影響を与えるが (ポータブル クラス ライブラリとプラットフォーム固有プロジェクト) のソリューションに読み込まれています。
3. PCL プロジェクトをソリューションでは、他のプロジェクトで簡単に参照できる、または自社のソリューションで参照する他の出力アセンブリを共有することができます。

**短所**

1. 同じポータブル クラス ライブラリは、複数のアプリケーション間で共有される、ため、プラットフォーム固有のライブラリを (例: 参照ことはできません。 Community.CsharpSqlite.WP7)。
2. ポータブル クラス ライブラリのサブセットは、それ以外の場合 MonoTouch と Mono for Android (DllImport System.IO.File など) の両方で使用できるクラスを含まない場合があります。

> [!NOTE]
> ポータブル クラス ライブラリは、Visual Studio の最新バージョンで非推奨とされましたが、 [.NET Standard ライブラリ](net-standard.md)は代わりにお勧めします。

ある程度はプロバイダー パターンまたは依存関係の挿入、ポータブル クラス ライブラリで定義されているインターフェイスまたは基本クラスに対してプラットフォーム プロジェクトで実際の実装のコードを使用して両方の欠点を回避できます。

この図は、ポータブル クラス ライブラリを使用して、コードを共有するが、渡すプラットフォームに依存する機能にも依存関係の挿入を使用して、クロス プラットフォーム アプリケーションのアーキテクチャを示しています。

[![](pcl-images/image1.png "この図は、ポータブル クラス ライブラリを使用して、コードを共有するが、渡すプラットフォームに依存する機能にも依存関係の挿入を使用して、クロス プラットフォーム アプリケーションのアーキテクチャを示しています。")](pcl-images/image1.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac チュートリアルの

このセクションで説明を作成して、Visual Studio for mac を使用するポータブル クラス ライブラリを使用する方法 参照してください、完全な実装の PCL の使用例」にします。

### <a name="creating-a-pcl"></a>PCL を作成します。

ポータブル クラス ライブラリを追加するソリューションには、正規表現ライブラリ プロジェクトを追加することによく似ています。

1. **新しいプロジェクト**ダイアログの 、**マルチプラット フォーム > ライブラリ > ポータブル ライブラリ**オプション。

    ![新しい PCL プロジェクトを作成します。](pcl-images/image2.png)

1. Visual studio for Mac を PCL が作成されたときに、Xamarin.iOS と Xamarin.Android にプロファイルを使用して自動的に構成します。 このスクリーン ショットで示すように、PCL プロジェクトが表示されます。

    ![Solution pad で、PCL プロジェクト](pcl-images/image3.png)

PCL は、コードを追加する準備ができました。 他のプロジェクト (アプリケーション プロジェクト、ライブラリ プロジェクトとその他の PCL プロジェクト) で参照することもできます。

### <a name="editing-pcl-settings"></a>PCL の設定の編集

表示し、このプロジェクトの PCL の設定を変更、プロジェクトを右クリックし、選択**オプション > ビルド > 全般**ここで示すように画面を表示します。

[![PCL プロジェクトのオプション、プロファイルを設定するには](pcl-images/image4-sml.png)](pcl-images/image4.png#lightbox)

クリックして**変更しています.** このポータブル クラス ライブラリのターゲット プロファイルを変更します。

コードは既に、PCL に追加した後、プロファイルが変更されると、そのコードは、新しく選択されたプロファイルの一部ではない機能を参照している場合、ライブラリがコンパイルされなく可能性があります。

## <a name="working-with-a-pcl"></a>PCL の使用

PCL ライブラリ コードが書き込まれると、Visual Studio for Mac エディターは、選択したプロファイルの制限事項を認識し、オートコンプリートのオプションを必要に応じて調整します。 たとえば、このスクリーン ショット、オートコンプリートのオプションを示します System.IO for Mac に Visual Studio で使用される既定のプロファイル (Profile136) を使用して – を示す使用可能なクラスの約半分に表示されるスクロール バーに注意してください (実際のところ、14 のみクラスを使用)。

[![PCL の System.IO クラスで 14 のクラスの Intellisense の一覧](pcl-images/image6.png)](pcl-images/image6.png#lightbox)

比較 – Xamarin.iOS または Xamarin.Android プロジェクトでオート コンプリート System.IO があることなどのクラスを使用する一般的な使用など、40 クラス`File`と`Directory`PCL プロファイルではありません。

[![.NET Framework の System.IO 名前空間の 40 クラスの Intellisense の一覧](pcl-images/image7.png)](pcl-images/image7.png#lightbox)

これは PCL を使用しての基になるトレードオフを反映 – 多くのプラットフォーム間でシームレスにコードを共有する機能は、特定の Api は比較可能な実装を可能なすべてのプラットフォームがあるないため、使用できるはことを意味します。

### <a name="using-pcl"></a>PCL を使用してください。

PCL プロジェクトが作成されたら、通常の参照を追加する同じ方法で任意の互換性のあるアプリケーションまたはライブラリ プロジェクトからへの参照を追加できます。 Visual studio for Mac では、[参照] ノードを右クリックし、選択**参照の編集.** に切り替えて、**プロジェクト**に示すようにタブします。

[![参照の編集オプションを使用して、PCL への参照を追加します。](pcl-images/image8.png)](pcl-images/image8.png#lightbox)

次のスクリーン ショットでは、Solution pad で Xamarin.iOS プロジェクト下部とその PCL ライブラリへの参照を PCL ライブラリが表示されて、TaskyPortable サンプル アプリを示します。

[![TaskyPortable サンプル ソリューションが表示された PCL プロジェクト](pcl-images/image9.png)](pcl-images/image9.png#lightbox)

PCL からの出力 (ie。 結果として得られるアセンブリ DLL) としてほとんどのプロジェクトへの参照を追加することもできます。 これにより、PCL はクロスプラット フォーム対応のコンポーネントとライブラリを出荷するための望ましい方法です。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Visual Studio チュートリアル

このセクションで作成して Visual Studio を使用して、ポータブル クラス ライブラリを使用する方法について説明します。 参照してください、完全な実装の PCL の使用例」にします。

### <a name="creating-a-pcl"></a>PCL を作成します。

Visual Studio でソリューションに PCL を追加することは、通常のプロジェクトを追加するのには若干異なるは。

1. **新しいプロジェクトの追加**画面で、、**クラス ライブラリ (レガシ ポータブル)** オプション。 このプロジェクトの種類は非推奨とされている右側の 説明が表示されます。

    [![ポータブル クラス ライブラリを作成する新しいプロジェクト ウィンドウ](pcl-images/image10-sml.png "ポータブル クラス ライブラリ")](pcl-images/image10.png#lightbox)

2. Visual Studio をプロファイルを構成できるようにする、すぐに、次のダイアログで求められます。
 [Ok] をサポートする必要のあるプラットフォームをオンにします。

    [![ライブラリのターゲット プラットフォームを選択](pcl-images/image11-sml.png "目盛りと、[ok] をサポートする必要のあるプラットフォーム")](pcl-images/image11.png#lightbox)

3. PCL プロジェクトがソリューション エクスプ ローラーで示すように表示されます&ndash;テキスト **(ポータブル)** PCL があることを示すプロジェクト名の横に表示されます。

    ![PCL プロファイルで定義されている NET Framework](pcl-images/image12.png "PCL プロファイルで定義されている NET Framework")

PCL は、コードを追加する準備ができました。 他のプロジェクト (アプリケーション プロジェクト、ライブラリ プロジェクト、およびその他の PCL プロジェクト) で参照することもできます。

### <a name="editing-pcl-settings"></a>PCL の設定の編集

PCL の設定を表示および変更でプロジェクトを右クリックし、選択できる**プロパティ > ライブラリ**このスクリーン ショットで示すように、します。

[![ターゲット プラットフォームを編集します。](pcl-images/image13-sml.png)](pcl-images/image13.png#lightbox)

コードは既に、PCL に追加した後、プロファイルが変更されると、そのコードは、新しく選択されたプロファイルの一部ではない機能を参照している場合、ライブラリがコンパイルされなく可能性があります。

> [!TIP]
> 示すメッセージが表示されるも**します。NETStandard コードを共有するための推奨される方法は、** します。 これは、Pcl の中にはまだサポートされていることを示しています、.NET Standard にアップグレードすることをお勧めします。

### <a name="working-with-a-pcl"></a>PCL の使用

PCL ライブラリ コードが書き込まれると、Visual Studio は、選択したプロファイルの制限事項を認識し、Intellisense オプションを必要に応じて調整します。 たとえば、このスクリーン ショットは System.IO のオート コンプリート オプション、既定のプロファイル (Profile136) を使用して – を示す使用可能なクラスの約半分に表示されるスクロール バーに注意してください (実際のところ、のみ 14 クラス使用できます)。

[![PCL で使用できる IO クラスの数が少なくなりますが](pcl-images/image14.png)](pcl-images/image14.png#lightbox)

比較 – 通常のプロジェクトのオート コンプリート System.IO があることなどのクラスを使用する一般的な使用など、40 クラス`File`と`Directory`PCL プロファイルではありません。

[![多くな複数 IO のクラス、.NET Framework で利用可能](pcl-images/image15.png)](pcl-images/image15.png#lightbox)

これは PCL を使用しての基になるトレードオフを反映 – 多くのプラットフォーム間でシームレスにコードを共有する機能は、特定の Api は比較可能な実装を可能なすべてのプラットフォームがあるないため、使用できるはことを意味します。

> [!TIP]
> .NET standard 2.0 に大きな API サーフェス領域より System.IO 名前空間を含む Pcl を表します。 新しいプロジェクトでは、.NET Standard は PCL をお勧めします。

### <a name="using-pcl"></a>PCL を使用してください。

PCL プロジェクトが作成されたら、通常の参照を追加する同じ方法で任意の互換性のあるアプリケーションまたはライブラリ プロジェクトからへの参照を追加できます。 Visual Studio で、[参照] ノードを右クリックし、選択`Add Reference...`に切り替えて、**ソリューション > プロジェクト**に示すようにタブします。

[![参照プロジェクトの追加 タブを使用して、PCL への参照を追加します。](pcl-images/image16.png)](pcl-images/image16.png#lightbox)

次のスクリーン ショットでは、Xamarin.iOS プロジェクト下部とその PCL ライブラリへの参照を PCL ライブラリが表示されて、TaskyPortable サンプル アプリの [ソリューション] ウィンドウを示します。

[![PCL ライブラリが表示されて TaskyPortable サンプル ソリューション](pcl-images/image17.png)](pcl-images/image17.png#lightbox)

PCL からの出力 (ie。 結果として得られるアセンブリ DLL) としてほとんどのプロジェクトへの参照を追加することもできます。
これにより、PCL はクロスプラット フォーム対応のコンポーネントとライブラリを出荷するための望ましい方法です。

-----

## <a name="pcl-example"></a>PCL の使用例

[TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/)サンプル アプリケーションは、ポータブル クラス ライブラリを使用して、Xamarin を使用した方法を示します。
IOS と Android で実行されている結果として得られるアプリの一部のスクリーン ショットを次に示します。

[![](pcl-images/image18.png "IOS、Android、Windows Phone で実行されている結果として得られるアプリの一部のスクリーン ショットを次に示します")](pcl-images/image18.png#lightbox)

多数は純粋な移植可能なコードは、データとロジックのクラスを共有して、SQLite データベースの実装の依存関係の挿入を使用してプラットフォーム固有の要件を組み込む方法も示します。

ソリューションの構造を次に示します (Visual Studio for Mac と Visual Studio でそれぞれ)。

[![](pcl-images/image19.png "ソリューション構造がで示される Visual Studio for Mac と Visual Studio それぞれ")](pcl-images/image19.png#lightbox)

SQLite NET コードにクラスが抽象にリファクタリングされました目的でデモをポータブル クラス ライブラリにコンパイルできます (それぞれ別のオペレーティング システムでの SQLite の実装では機能) をプラットフォーム固有部分があるため、実際のコードでは、iOS と Android プロジェクト内のサブクラスとして実装されています。

### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

ポータブル クラス ライブラリは、サポートできる .NET の機能に制限されます。 行うことはできずがコンパイルを複数のプラットフォームで実行するための使用`[DllImport]`SQLite NET で使用されている機能です。 代わりに SQLite NET が抽象クラスとして実装し、共有コードの残りの部分で参照されます。 抽象 API から抽出した内容は、以下に示します。

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

共有コードの残りの部分では、抽象クラスを使用して、「保存」をデータベースからオブジェクトを「取得」します。 この抽象クラスを使用するアプリケーションで実際のデータベース機能を提供する完全な実装を渡す必要があります。

### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid と TaskyiOS

IOS および Android アプリケーション プロジェクトには、ユーザー インターフェイスとその他のプラットフォーム固有コードを PCL で共有コードに接続するために使用が含まれています。

これらのプロジェクトには、抽象データベース プラットフォームで動作する API の実装も含まれます。 IOS と Android の Sqlite データベース エンジンは、オペレーティング システムに組み込まれている実装を使用できるように`[DllImport]`データベース接続の具象実装を提供するようにします。 プラットフォーム固有の実装コードの抜粋を次に示します。

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```

完全な実装は、サンプル コードで確認できます。

## <a name="summary"></a>まとめ

この記事でのメリットとポータブル クラス ライブラリの落とし穴について説明、作成してから Pcl を使用する方法を示しましたがについて簡単に Visual Studio for Mac と Visual Studio の内部アクションで、PCL を示す完全なサンプル アプリケーション – TaskyPortable – 最後に導入されています。

## <a name="related-links"></a>関連リンク

- [TaskyPortable (サンプル)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [クロスプラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [移植可能な Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)
- [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)
- [.NET Framework (マイクロソフト) を使用したクロス プラットフォーム開発](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)
