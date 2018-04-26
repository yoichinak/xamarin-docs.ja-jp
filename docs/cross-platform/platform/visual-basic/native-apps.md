---
title: Xamarin iOS および Android で visual Basic.NET
description: XThis チュートリアルでは、Visual Basic.NET で記述されたビジネス ロジックを利用する、Xamarin.iOS および Xamarin.Android のネイティブ アプリをビルドする方法を示します。
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 54231d42383d491678b6152e67c01c5e39a1f958
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
# <a name="visual-basicnet-in-xamarin-ios-and-android"></a>Xamarin iOS および Android で visual Basic.NET

[TaskyPortable](/samples/mobile/VisualBasic/TaskyPortableVB/)サンプル アプリケーションは、Visual Basic コードをポータブル クラス ライブラリにコンパイルを使用して、Xamarin を使用した方法を示します。 IOS、Android、Windows Phone で実行されている結果として得られるアプリの一部のスクリーン ショットを次に示します。

 [![](native-apps-images/image5.png "iOS、Android、および Windows phone を Visual Basic でビルドされたアプリを実行しています。")](native-apps-images/image5.png#lightbox)

IOS、Android および Windows Phone プロジェクトの例ではすべて c# で記述します。 各アプリケーションのユーザー インターフェイスがネイティブのテクノロジでビルドされた (ストーリー ボードなど、Xml、および Xaml それぞれ)、中に、`TodoItem`管理は、Visual Basic のポータブル クラス ライブラリによって提供されるを使用して、`IXmlStorage`で提供される実装ネイティブ プロジェクトです。

## <a name="sample-walkthrough"></a>サンプルのチュートリアル

このガイドでの Visual Basic の実装方法について説明します、 [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) iOS および Android 用の Xamarin のサンプルです。

> [!NOTE]
> 指示を確認[Visual Basic.NET Pcl](/guides/cross-platform/application_fundamentals/pcl/portable_visual_basic_net/)このガイドに続行する前にします。

## <a name="visualbasicportablelibrary"></a>VisualBasicPortableLibrary

Visual Basic のポータブル クラス ライブラリは、Visual Studio でのみ作成できます。
サンプルのライブラリには、次の 4 つの Visual Basic ファイルで、アプリケーションの基礎が含まれています。

-  IXmlStorage.vb
-  TodoItem.vb
-  TodoItemManager.vb
-  TodoItemRepositoryXML.vb


### <a name="ixmlstoragevb"></a>IXmlStorage.vb

ファイル アクセスの動作は、プラットフォーム間を大幅に異なる、ため、ポータブル クラス ライブラリに渡さないように`System.IO`ファイルの任意のプロファイルでは、ストレージ Api です。 これは、移植可能なコードのファイル システムと直接やり取りする場合、必要がある各プラットフォームでのネイティブ プロジェクトにコールバックすることを意味します。  各プラットフォームで c# で実装できるシンプルなインターフェイスに対して、Visual Basic コードを記述して、ファイル システムへのアクセスを保持している Visual Basic コードを共有可能なおことができます。

2 つのメソッドを含むこの非常に単純なインターフェイスを使用するサンプル コード: シリアル化された Xml ファイルを読み書きします。

```vb
Public Interface IXmlStorage
    Function ReadXml(filename As String) As List(Of TodoItem)
    Sub WriteXml(tasks As List(Of TodoItem), filename As String)
End Interface
```

iOS、Android および Windows Phone の実装をこのインターフェイスが、ガイドの後半で表示されます。

### <a name="todoitemvb"></a>TodoItem.vb

このクラスには、アプリケーション全体で使用するビジネス オブジェクトが含まれています。 Visual Basic で定義され、iOS、Android および Windows Phone と c# で記述されているプロジェクトを共有します。

クラス定義を次に示します。

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

このサンプルでは、XML シリアル化および逆シリアル化を使用して読み込みおよび TodoItem オブジェクトを保存します。

### <a name="todoitemmanagervb"></a>TodoItemManager.vb

マネージャー クラスでは、'の API' は、コードの移植性を表示します。 基本的な CRUD 操作を提供、`TodoItem`クラスがこれらの操作の実装のないです。

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String, storage As IXmlStorage)
        _repository = New TodoItemRepositoryXML(filename, storage)
    End Sub
    Public Function GetTask(id As Integer) As TodoItem
        Return _repository.GetTask(id)
    End Function
    Public Function GetTasks() As List(Of TodoItem)
        Return New List(Of TodoItem)(_repository.GetTasks())
    End Function
    Public Function SaveTask(item As TodoItem) As Integer
        Return _repository.SaveTask(item)
    End Function
    Public Function DeleteTask(item As TodoItem) As Integer
        Return _repository.DeleteTask(item.ID)
    End Function
End Class
```

IXmlStorage のインスタンスは、コンス トラクターは、パラメーターとして受け取ります。 これにより、各プラットフォームで共有できるその他の機能について説明するコードの移植性をまだながら独自作業実装を提供します。

### <a name="todoitemrepositoryvb"></a>TodoItemRepository.vb

リポジトリ クラスには、TodoItem オブジェクトの一覧を管理するためのロジックが含まれています。 完全なコードは、追加またはコレクションから削除すると、TodoItems 全体で一意の ID 値を管理するには、主にロジックが存在する – 以下に示します。

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String, storage As IXmlStorage)
        _filename = filename
        _storage = storage
        _tasks = _storage.ReadXml(filename)
    End Sub
    ''' <summary>Inefficient search for a Task by ID</summary>
    Public Function GetTask(id As Integer) As TodoItem
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                Return _tasks(t)
            End If
        Next
        Return New TodoItem() With {.ID = id}
    End Function
    ''' <summary>List all the Tasks</summary>
    Public Function GetTasks() As IEnumerable(Of TodoItem)
        Return _tasks
    End Function
    ''' <summary>Save a Task to the Xml file
    ''' Calculates the ID as the max of existing IDs</summary>
    Public Function SaveTask(item As TodoItem) As Integer
        Dim max As Integer = 0
        If _tasks.Count > 0 Then
            max = _tasks.Max(Function(t As TodoItem) t.ID)
        End If
        If item.ID = 0 Then
            item.ID = ++max
            _tasks.Add(item)
        Else
            Dim j = _tasks.Where(Function(t) t.ID = item.ID).First()
            j = item
        End If
        _storage.WriteXml(_tasks, _filename)
        Return max
    End Function
    ''' <summary>Removes the task from the XMl file</summary>
    Public Function DeleteTask(id As Integer) As Integer
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                _tasks.RemoveAt(t)
                _storage.WriteXml(_tasks, _filename)
                Return 1
            End If
        Next
        Return -1
    End Function
End Class
```

> [!NOTE]
> このコードは、非常に基本的なデータ ストレージ機構の例を示します。
> ポータブル クラス ライブラリできます (ここでは、読み込みと Xml ファイルを保存) プラットフォーム固有の機能にアクセスするインターフェイスに対するコードの方法を示すために提供されます。 これにするものでありません、運用環境品質のデータベースの代替です。

## <a name="ios-android-and-windows-phone-application-projects"></a>iOS、Android、Windows Phone アプリケーション プロジェクト

このセクションでは、IXmlStorage インターフェイスのプラットフォーム固有の実装を含む、各アプリケーションでの使用方法を示します。 C# の場合は、アプリケーション プロジェクトすべて書き込まれます。

### <a name="ios-and-android-ixmlstorage"></a>iOS および Android IXmlStorage

Xamarin.iOS および Xamarin.Android を完全提供`System.IO`機能を簡単に読み込むし、次のクラスを使用して Xml ファイルを保存できるようにします。

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        if (File.Exists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new FileStream(filename, FileMode.Open))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(filename))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

IOS アプリケーションで、`TodoItemManager`と`XmlStorageImplementation`で作成された、 **<code>appdelegate.cs</code>** ファイルの次のコード スニペットで示すようにします。 最初の 4 つの行がデータの格納場所です。 ファイルへのパスを構築するだけ最後の 2 つの行は、インスタンス化されている 2 つのクラスを示しています。

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Android アプリケーションで、`TodoItemManager`と`XmlStorageImplementation`で作成された、 **Application.cs**ファイルの次のコード スニペットで示すようにします。 最初の 3 行がデータの格納場所です。 ファイルへのパスを構築するだけ最後の 2 つの行は、インスタンス化されている 2 つのクラスを示しています。

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new AndroidTodo.XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

使用して、ユーザー インターフェイスを懸念して、アプリケーション コードの残りの部分は、主に、`TaskMgr`クラスの読み込みし、保存を`TodoItem`クラスです。

### <a name="windows-phone-ixmlstorage"></a>Windows Phone IXmlStorage

Windows Phone の管轄外デバイスのファイル システムに完全なアクセス IsolatedStorage API の代わりに公開します。 `IXmlStorage` Windows Phone の実装は、次のようになります。

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        if (fileStorage.FileExists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new StreamReader(new IsolatedStorageFileStream(filename, FileMode.Open, fileStorage)))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(new IsolatedStorageFileStream(filename, FileMode.OpenOrCreate, fileStorage)))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

`TodoItemManager`と`XmlStorageImplementation`で作成された、 **App.xaml.cs**ファイルの次のコード スニペットで示すようにします。

```csharp
var filename = "TodoList.xml";
var xmlStorage = new XmlStorageImplementation();
TodoMgr = new TodoItemManager(filename, xmlStorage);
```

Xaml および C# の場合、ユーザー インターフェイスを作成および使用するは、Windows Phone アプリケーションの残りの部分、`TodoMgr`クラスの読み込みし、保存を`TodoItem`オブジェクト。

## <a name="visual-basic-pcl-in-visual-studio-for-mac"></a>Visual Basic で Visual Studio for Mac PCL

Visual Studio for Mac は、Visual Basic 言語をサポートしていません: を作成または for mac Visual Studio と Visual Basic プロジェクトをコンパイルすることはできません。

ポータブル クラス ライブラリの Mac のサポート用の visual Studio では、Visual Basic からビルドされた PCL アセンブリを参照することを意味します。

このセクションでは、Visual Studio での PCL アセンブリをコンパイルしているがバージョン管理システムに格納され、他のプロジェクトで参照されていることを確認する方法について説明します。

### <a name="keeping-the-pcl-output-from-visual-studio"></a>Visual Studio から PCL 出力を維持すること

既定でを無視する (TFS および Git を含む)、ほとんどのバージョン管理システムを構成するは、 **/bin/** ディレクトリには、コンパイル済みのために PCL をアセンブリの意味は格納されません。 つまりへの参照を追加する Mac 用の Visual Studio を実行しているコンピューターに手動でコピーする必要があります。

バージョン管理システムが PCL アセンブリの出力を保存させるには、プロジェクトのルートにコピーするビルド後のスクリプトを作成できます。 このビルド後のステップにより、アセンブリを簡単にソース管理に追加および他のプロジェクトと共有します。

#### <a name="visual-studio-2017"></a>Visual Studio 2017

1. プロジェクトを右クリックし、選択、**プロパティ > ビルド イベント**セクションです。

2. 追加、_ビルド後_、出力の DLL をこのプロジェクトのプロジェクトのルート ディレクトリにコピーするスクリプト (の外部である **/bin/**)。 構成によっては、バージョン コントロール、DLL ことができますをソース管理に追加します。

  [![](native-apps-images/image6-vs-sml.png "VB の dll ファイルをコピーする post ビルド スクリプトのビルド イベント")](native-apps-images/image6-vs.png#lightbox)

#### <a name="visual-studio-2015"></a>Visual Studio 2015

1.  プロジェクトを右クリックし、選択**プロパティ > コンパイル**マス目のボックスの左上のすべての構成が選択されていることを確認します。 クリックして、**イベントを作成しています.** 右下にあるボタンをクリックします。

    [![](native-apps-images/image6.png "プロジェクトのプロパティ セクションをコンパイルします。")](native-apps-images/image6.png#lightbox)

1.  このプロジェクトのプロジェクトのルート ディレクトリに、出力の DLL をコピーするビルド後のスクリプトを追加 (これは外側の **/bin/** )。 構成によっては、バージョン コントロール、DLL ことができますをソース管理に追加します。

    [![](native-apps-images/image7.png "ビルド イベント ウィンドウ")](native-apps-images/image7.png#lightbox)

#### <a name="all-versions"></a>すべてのバージョン

次にプロジェクトをビルドすると、チェックで/コミット/プッシュ、DLL、変更できる場合と、プロジェクトのルートに、ポータブル クラス ライブラリ アセンブリがコピーされます (できるようにするには、Mac 用 Visual Studio での Mac にダウンロードできます) を格納します。

  [![](native-apps-images/image8-sml.png "Visual Basic アセンブリの出力のファイルの場所")](native-apps-images/image8.png#lightbox)


このアセンブリ追加できます Visual Studio での Xamarin のプロジェクトに for Mac を Visual Basic 言語そのものは Xamarin iOS または Android プロジェクトではサポートされていません。

### <a name="referencing-the-pcl-in-visual-studio-for-mac"></a>Mac 用 Visual Studio では、PCL を参照します。

Xamarin では、Visual Basic はサポートされていないためこのスクリーン ショットに示すように、PCL プロジェクト (も Windows Phone アプリ) をロードできませんでした。

 [![](native-apps-images/image9.png "Mac ソリューション用の visual Studio")](native-apps-images/image9.png#lightbox)

Xamarin.iOS および Xamarin.Android プロジェクトで Visual Basic PCL アセンブリ DLL お含めることができます。

1.  右クリックし、**参照**ノード**参照の編集.**

    [![](native-apps-images/image10.png "プロジェクトの参照 メニューの編集")](native-apps-images/image10.png#lightbox)

1.  選択、 **.Net アセンブリ**タブし、Visual Basic プロジェクト ディレクトリに、出力の DLL に移動します。 場合でも、Visual Studio for Mac は、プロジェクトを開くことはできません、ソース管理から、すべてのファイルが存在する必要があります。 をクリックして**追加**し**OK** iOS および Android のアプリケーションにこのアセンブリを追加します。

    [![](native-apps-images/image11-sml.png "IOS および Android のアプリケーションにこのアセンブリを追加するには、追加、[ok] をクリックします。")](native-apps-images/image11.png#lightbox)

1.  IOS および Android アプリケーションでは、Visual Basic のポータブル クラス ライブラリによって提供されるアプリケーション ロジックを含めることができますようになりました。 このスクリーン ショットは、Visual Basic PCL の参照をそのライブラリの機能を使用してコードを持つ iOS アプリケーションを示しています。

    [![](native-apps-images/image12-sml.png "編集の参照は、.NET アセンブリ ウィンドウを追加")](native-apps-images/image12.png#lightbox)


Visual Studio で Visual Basic プロジェクトにプロジェクトをビルド、ソース管理に、結果として得られるアセンブリ DLL を保存し、Visual Studio for Mac をビルドできるようにソース管理からご使用の Mac にその新しい DLL をプルし、変更が加えられた場合に、最新バージョンが含まれています。機能します。


## <a name="summary"></a>まとめ

この記事の内容が示されている Visual Studio およびポータブル クラス ライブラリを使用して Xamarin アプリケーションで Visual Basic コードを使用する方法です。 場合でも、Xamarin で Visual Basic が直接サポートされていませんが、iOS と Android アプリに含まれる Visual Basic で記述されたコードを Visual Basic にコンパイルするために PCL をできます。

## <a name="related-links"></a>関連リンク

- [TaskyPortableVB (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [.NET Framework (Microsoft) を使用したクロスプラット フォーム開発](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
