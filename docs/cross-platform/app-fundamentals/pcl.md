---
title: ポータブル クラス ライブラリの概要
description: この記事では、ポータブル クラス ライブラリ (PCL) プロジェクトを紹介し、Mac と Visual Studio の Visual Studio での PCL プロジェクトの作成および手順について説明します。
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: feef0a4083d2455cc189ddab6ed22762c044d848
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-portable-class-libraries"></a>ポータブル クラス ライブラリの概要

_この記事では、ポータブル クラス ライブラリ (PCL) プロジェクトを紹介し、Mac と Visual Studio の Visual Studio での PCL プロジェクトの作成および手順について説明します。_


クロス プラットフォーム アプリケーションの構築の主要なコンポーネントは、さまざまなプラットフォーム固有のプロジェクト間でコードを共有できることです。 ただし、という事実のさまざまなプラットフォーム サブ一連の異なる .NET 基本クラス ライブラリ (BCL) を使用して多くの場合、そのため、.NET Core ライブラリの別のプロファイルに構築されて実際には、これは複雑です。 これは、各プラットフォームは、プラットフォームごとに別個のクラス ライブラリ プロジェクトを要求するようになりますので、同じプロファイルを対象とするクラス ライブラリを使用することができますのみことを意味します。

この問題に対処するコードを共有するの 3 つの主要な方法はあります: **.NET 標準プロジェクト**、**ポータブル クラス ライブラリ (PCL) プロジェクト**、および**共有アセット プロジェクト**.

- **標準の .NET プロジェクト** [.NET 標準](~/cross-platform/app-fundamentals/net-standard.md)です。
-  **PCL**プロジェクトは既知の BCL クラス/機能のセットをサポートする特定のプロファイルを対象します。 ただし、PCL にマイナス面は、多くの場合、独自のライブラリにプロファイルの特定のコードを分離するアーキテクチャ上余分な労力が必要です。 これら 2 つの方法の詳細については、次を参照してください。、[コードの共有オプションに従って](~/cross-platform/app-fundamentals/code-sharing.md)です。
-  **共有アセット プロジェクト**ソリューションに含まれると、通常のコードを共有するために迅速かつ簡単な方法はさまざまなプラットフォーム (詳細はそれを使用するコード パスを指定する条件付きコンパイル ディレクティブを使用してファイルおよびサービスの単一セットの使用情報を参照してください、[共有プロジェクト記事](~/cross-platform/app-fundamentals/shared-projects.md)と[Xamarin クロスプラット フォーム ソリューション ガイドを設定する](~/cross-platform/app-fundamentals/code-sharing.md))。


このページを作成する方法を説明する、 **PCL**プラットフォーム固有の複数のプロジェクトで参照できます、特定のプロファイルを対象とするプロジェクトです。


## <a name="what-is-a-portable-class-library"></a>ポータブル クラス ライブラリとは何ですか。

アプリケーション プロジェクト、またはライブラリ プロジェクトを作成するときに、生成された DLL は自動的に作成、特定のプラットフォームで操作に制限されます。 これは、for Windows アプリでは、アセンブリを記述し、Xamarin.iOS および Xamarin.Android を再利用が阻止されます。

ポータブル クラス ライブラリを作成するときに、上で実行するコードを作成するプラットフォームの組み合わせを選択できます。 互換性可能な選択肢をポータブル クラス ライブラリを作成するときに、ライブラリがサポートされるプラットフォームについて説明します"のプロファイル"の識別子に変換されます。

次の表は、.NET プラットフォームによって異なり、機能の一部を示します。 特定のデバイスとプラットフォームで実行することが保証するために PCL をアセンブリに書き込むだけを選択するプロジェクトを作成するときに、サポートが必要。

|機能|.NET Framework|UWP アプリ|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|コア|Y|Y|Y|Y|Y|
|LINQ|Y|Y|Y|Y|Y|
|IQueryable|Y|Y|Y|7.5 +|Y|
|シリアル化|Y|Y|Y|Y|Y|
|データの注釈|4.0.3 +|Y|Y||Y|

Xamarin の列には、Xamarin.iOS および Xamarin.Android は、Visual Studio に付属のすべてのプロファイルをサポートしているいて作成するすべてのライブラリの機能の可用性をサポートするその他のプラットフォームによってのみ制限されますが反映されます。

これには、組み合わせのプロファイルが含まれます。

-  .NET 4 または .NET 4.5
-  Silverlight 5
-  Windows Phone 8
-  UWP アプリ

詳細を読み取ることができますに別のプロファイルの機能に関する[マイクロソフトの web サイト](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)別コミュニティ メンバーを表示および[PCL プロファイルの概要](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY)framework 情報や他の注意事項はサポートが含まれています。



コードを共有するために PCL を作成するには、さまざまな長所と短所ファイル リンクの代わりとがあります。


**特典**

1. 一元的なコードを共有 –、記述し、他のライブラリやアプリケーションで使用できる 1 つのプロジェクトでコードをテストします。
1. すべてのコードに影響がリファクタリング操作 (ポータブル クラス ライブラリおよびプラットフォーム固有のプロジェクト) ソリューションに読み込まれます。
1. PCL プロジェクトをソリューションでは、他のプロジェクトで簡単に参照できますか、他のユーザーがソリューション内で参照を出力アセンブリを共有することができます。


**短所**

1. 同じポータブル クラス ライブラリは、複数のアプリケーション間で共有される、ため、プラットフォーム固有のライブラリは (参照できません。 Community.CsharpSqlite.WP7).
1. ポータブル クラス ライブラリのサブセットは、それ以外の場合になる MonoTouch と (DllImport System.IO.File など) の Android 用の Mono の両方で使用可能なクラスを含まない場合があります。



よってある程度は、プロバイダーのパターンまたは依存関係の挿入、ポータブル クラス ライブラリで定義されているインターフェイスまたは基本クラスに対するプラットフォームのプロジェクトでは、実際の実装のコードを使用して両方の欠点を回避することができます。



この図は、ポータブル クラス ライブラリを使用して、コードを共有するが、依存関係の挿入を使用してもプラットフォームに依存する機能に渡す、クロス プラットフォーム アプリケーションのアーキテクチャを示しています。



[![](pcl-images/image1.png "このダイアグラムは、ポータブル クラス ライブラリを使用して、コードを共有するが、依存関係の挿入を使用してもプラットフォームに依存する機能に渡す、クロス プラットフォーム アプリケーションのアーキテクチャを示しています。")](pcl-images/image1.png#lightbox)



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac のチュートリアル


このセクションで作成および for mac Visual Studio を使用して、ポータブル クラス ライブラリを使用する方法について説明します。 参照してください、完全な実装のために PCL を例」のセクションにします。



### <a name="creating-a-pcl"></a>PCL を作成します。


ポータブル クラス ライブラリをソリューションに追加することは、通常のライブラリ プロジェクトを追加することによく似ています。


1. 新しいプロジェクト ダイアログで、選択、`Multiplatform > Library > Portable Library`オプション


    ![](pcl-images/image2.png "マルチプラット フォーム > ライブラリ > ポータブル ライブラリ")


1. Mac 用 PCL が Visual Studio で作成されたときに、Xamarin.iOS および Xamarin.Android に対して機能するプロファイルを使用して自動的に構成されます。 PCL プロジェクトは、このスクリーン ショットに示すように表示されます。

    ![](pcl-images/image3.png "このスクリーン ショットに示すように、PCL プロジェクトが表示されます。")

PCL は、コードを追加する準備ができました。 これは、他のプロジェクト (アプリケーション プロジェクト、ライブラリのプロジェクトおよびその他の PCL プロジェクト) で参照できます。



### <a name="editing-pcl-settings"></a>PCL 設定の編集


表示し、このプロジェクトの PCL 設定を変更する、プロジェクトを右クリックし、選択**オプション > ビルド > 全般**次に示す画面を表示します。



[![](pcl-images/image4.png "表示し、このプロジェクトの PCL 設定を変更する、プロジェクトを右クリックし、次に示す画面を表示するオプションを構築全般 を選択")](pcl-images/image4.png#lightbox)



この画面の設定は、この特定の PCL と互換性のあるプラットフォームを制御します。 この PCL は、さらに制御できる機能で使用されているプロファイルを変更してこれらのオプションのいずれかを変更するコードの移植性のために使用します。



いずれかの変更、`Target Framework`オプションが自動的に更新され、 `Current Profile`; 画面も警告が表示されます、互換性のないオプションが選択されている場合。



[![](pcl-images/image5.png "現在のプロファイルを更新するターゲット フレームワークのオプションのいずれかを自動的に変更する画面も警告が表示されます、互換性のないオプションが選択されている場合")](pcl-images/image5.png#lightbox)



コードは既に、PCL に追加された後、プロファイルが変更されると、そのコードは、新しく選択されたプロファイルの一部ではない機能を参照している場合、ライブラリはコンパイルされなく可能性があります。


## <a name="working-with-a-pcl"></a>PCL の操作


コードが PCL ライブラリに書き込まれると、Mac エディター用の Visual Studio は、選択したプロファイルの制限事項を認識し、オートコンプリート オプションは必要に応じて調整します。 たとえば、このスクリーン ショット オートコンプリート オプションを示しています System.IO の Mac を Visual Studio で使用される既定のプロファイル (Profile136) を使用して – 使用可能なクラスの約半分が表示されることを示しますが、スクロール バーに注意してください (実際は 14 のみ使用できるクラス)。



[![](pcl-images/image6.png "Profile136 Mac は、実際に存在可能なクラスの約半分表示されることを示しますが、スクロール バーは 14 クラスの使用可能なのみに注意してください。 の Visual Studio で使用される既定のプロファイルを使用する IO")](pcl-images/image6.png#lightbox)



Xamarin.iOS または Xamarin.Android プロジェクト – で自動完了 System.IO であることなどのクラスを使用する利用可能ななどよく 40 のクラスと比較`File`と`Directory`PCL プロファイルではありません。



[![](pcl-images/image7.png "任意の PCL プロファイルではないファイルやディレクトリなどのクラスを使用する利用可能ななど一般的 40 のクラスがあります。")](pcl-images/image7.png#lightbox)



PCL を使用する基になるトレードオフが反映されます – 多くのプラットフォーム間でシームレスにコードを共有できるため、可能なすべてのプラットフォーム間で比較可能な実装がないために、特定の Api を使用することはできません。



### <a name="using-pcl"></a>PCL を使用します。


PCL プロジェクトを作成すると、通常の参照を追加する同じ方法で任意の互換性のあるアプリケーションまたはライブラリ プロジェクトからへの参照を追加できます。 Mac 用 Visual Studio で参照ノードを右クリックして選択`Edit References…`ように、プロジェクトのタブに切り替えます。



[![](pcl-images/image8.png "Mac 用 Visual Studio で参照ノードを右クリックし、参照の編集をクリックしてように、[プロジェクト] タブに切り替えます")](pcl-images/image8.png#lightbox)



次のスクリーン ショットは、Xamarin.iOS プロジェクトの下部に配置されその PCL ライブラリへの参照を PCL ライブラリが表示されて、TaskyPortable サンプル アプリのソリューション パッドを示しています。



[![](pcl-images/image9.png "TaskyPortable サンプル アプリのソリューション パッド")](pcl-images/image9.png#lightbox)



PCL からの出力 (ie。 結果として得られるアセンブリ DLL) としてほとんどのプロジェクトへの参照を追加することもできます。 これにより、PCL はクロス プラットフォームのコンポーネント、およびライブラリを出荷するための望ましい方法です。




# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)




## <a name="visual-studio-walkthrough"></a>Visual Studio チュートリアル


このセクションで作成して Visual Studio を使用して、ポータブル クラス ライブラリを使用する方法について説明します。 参照してください、完全な実装のために PCL を例」のセクションにします。



### <a name="creating-a-pcl"></a>PCL を作成します。


Visual Studio でソリューションに PCL を追加することは、通常のプロジェクトを追加するのには若干異なるです。



1. [新しいプロジェクトの追加] 画面で、選択、`Portable Class Library`オプション


    ![](pcl-images/image10.png "ポータブル クラス ライブラリ")


1. Visual Studio は、プロファイルを構成するためにすぐに次のダイアログ ボックスで求められます。
 サポートし、[ok] を押し必要のあるプラットフォームをマークします。


    ![](pcl-images/image11.png "軸目盛りをサポートし、[ok] を押す必要のあるプラットフォーム")


1. PCL プロジェクトは、ソリューション エクスプ ローラーで示すように表示されます。 [参照] ノードは、ライブラリ (PCL プロファイルで定義された) .NET Framework のサブセットで使用することを示します。

    ![](pcl-images/image12.png "PCL プロファイルで定義された .NET Framework")

PCL は、コードを追加する準備ができました。 これは、他のプロジェクト (アプリケーション プロジェクト、ライブラリのプロジェクトおよびその他の PCL プロジェクト) で参照できます。



### <a name="editing-pcl-settings"></a>PCL 設定の編集


PCL 設定を表示し、プロジェクトを右クリックして を選択して変更できます**プロパティ > ライブラリ**このスクリーン ショットに示すように、します。



[![](pcl-images/image13.png "PCL 設定を表示およびこのスクリーン ショットに示すようにプロジェクトを右クリックして、プロパティのライブラリ を選択して変更できます。")](pcl-images/image13.png#lightbox)



コードは既に、PCL に追加された後、プロファイルが変更されると、そのコードは、新しく選択されたプロファイルの一部ではない機能を参照している場合、ライブラリはコンパイルされなく可能性があります。



### <a name="working-with-a-pcl"></a>PCL の操作


コードが PCL ライブラリに書き込まれると、Visual Studio は、選択したプロファイルの制限事項を認識し、Intellisense オプションは必要に応じて調整します。 たとえば、このスクリーン ショット オートコンプリート オプションを示しています System.IO の既定のプロファイル (Profile136) を使用して – 使用可能なクラスの約半分が表示されることを示しますが、スクロール バーに注意してください (実際はのみ 14 クラス使用できます)。



[![](pcl-images/image14.png "既定のプロファイル Profile136 を使用する IO")](pcl-images/image14.png#lightbox)



– 通常のプロジェクトのオート コンプリート System.IO であることなどのクラスを使用する利用可能ななどよく 40 のクラスと比較`File`と`Directory`PCL プロファイルではありません。



[![](pcl-images/image15.png "通常のプロジェクトのオート コンプリート")](pcl-images/image15.png#lightbox)



PCL を使用する基になるトレードオフが反映されます – 多くのプラットフォーム間でシームレスにコードを共有できるため、可能なすべてのプラットフォーム間で比較可能な実装がないために、特定の Api を使用することはできません。



### <a name="using-pcl"></a>PCL を使用します。


PCL プロジェクトを作成すると、通常の参照を追加する同じ方法で任意の互換性のあるアプリケーションまたはライブラリ プロジェクトからへの参照を追加できます。 Visual Studio で参照ノードを右クリックして選択`Add Reference...`に切り替えて、**ソリューション: プロジェクト**ように タブします。



[![](pcl-images/image16.png "プロジェクトの タブを表示")](pcl-images/image16.png#lightbox)



次のスクリーン ショットは、Xamarin.iOS プロジェクトの下部に配置されその PCL ライブラリへの参照を PCL ライブラリが表示されて、TaskyPortable サンプル アプリのソリューション ウィンドウを示しています。



[![](pcl-images/image17.png "TaskyPortable サンプル アプリのソリューション ペイン")](pcl-images/image17.png#lightbox)



PCL からの出力 (ie。 結果として得られるアセンブリ DLL) としてほとんどのプロジェクトへの参照を追加することもできます。
これにより、PCL はクロス プラットフォームのコンポーネント、およびライブラリを出荷するための望ましい方法です。




-----



## <a name="pcl-example"></a>PCL 例


[TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/)サンプル アプリケーションは、Xamarin でポータブル クラス ライブラリを使用する方法を示しています。
IOS、Android、Windows Phone で実行されている結果として得られるアプリの一部のスクリーン ショットを次に示します。



[![](pcl-images/image18.png "ここでは、iOS、Android、Windows Phone で実行されている結果として得られるアプリの一部のスクリーン ショットです。")](pcl-images/image18.png#lightbox)



純粋な移植可能なコードであるデータおよびロジックのクラスの数を共有してと、SQLite データベースの実装の依存関係の挿入を使用してプラットフォーム固有の要件を組み込む方法も示します。




ソリューションの構造を次に示します (Mac と Visual Studio の Visual Studio でそれぞれ)。



[![](pcl-images/image19.png "ソリューションの構造は、次に示す Visual Studio で Mac と Visual Studio のそれぞれ")](pcl-images/image19.png#lightbox)



有して SQLite NET コード (それぞれ異なるオペレーティング システムで SQLite 実装と連携する) をプラットフォームに固有の部分をデモする目的では、抽象にリファクタリングされてクラスは、ポータブル クラス ライブラリにコンパイルすることができ、実際のコードでは、iOS および Android のプロジェクト内のサブクラスとして実装します。



### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

ポータブル クラス ライブラリは、サポートできる .NET の機能に制限されます。 行えないので、複数のプラットフォーム上で実行することをコンパイルすると使用の`[DllImport]`SQLite NET で使用されている機能です。 代わりに SQLite NET は抽象クラスとして実装され、共有コードの残りの手順から参照されています。 抽象 API の抽出は、次に示します。


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


共有コードの残りの部分は、「格納」および""のオブジェクト、データベースから取得する抽象クラスを使用します。 この抽象クラスを使用するアプリケーションで実際のデータベース機能を提供する、完全な実装に渡す必要があります。



### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid と TaskyiOS


IOS と Android アプリケーション プロジェクトは、ユーザー インターフェイスとワイヤ アップ、PCL に共有コードを使用する他のプラットフォーム固有のコードが含まれます。



これらのプロジェクトには、抽象データベース プラットフォームで動作する API の実装も含まれます。 IOS および Android、Sqlite でデータベース エンジンは、オペレーティング システムに組み込み実装が使用できるように`[DllImport]`ようなデータベース接続の具体的な実装を提供します。 プラットフォーム固有の実装コードの抜粋を次に示します。


```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```


完全な実装は、サンプル コードで確認できます。

### <a name="taskywinphone"></a>TaskyWinPhone


Windows Phone アプリケーションは、XAML でビルドされた UI を持ち、ユーザー インターフェイスと、共有オブジェクトを接続するその他のプラットフォーム固有のコードが含まれています。



IOS および Android の使用の実装とは異なり、Windows Phone アプリ必要があります作成および使用のインスタンス、`Community.Sqlite.dll`その抽象の一部としてデータベース API です。 使用するのではなく`DllImport`で参照されている Community.Sqlite アセンブリに対して開くなどのメソッドが実装されている、`TaskWinPhone`プロジェクト。 抜粋は、iOS と Android バージョン上記の比較についてはここでは、表示します。


```csharp
public static Result Open(string filename, out Sqlite3.sqlite3 db)
{
    db = new Sqlite3.sqlite3();
    return (Result)Sqlite3.sqlite3_open(filename, ref db);
}

public static Result Close(Sqlite3.sqlite3 db)
{
    return (Result)Sqlite3.sqlite3_close(db);
}
```


## <a name="summary"></a>まとめ


この記事は、簡単な利点とポータブル クラス ライブラリの落とし穴について説明、作成してから Pcl を使用する方法を示しました Mac と; Visual Studio の Visual Studio 内アクションのために PCL を示す完全なサンプル アプリケーション – TaskyPortable – を最後に導入されたとします。


## <a name="related-links"></a>関連リンク

- [TaskyPortable (サンプル)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [クロスプラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [ポータブル Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)
- [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)
- [.NET Framework (Microsoft) を使用したクロスプラット フォーム開発](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
