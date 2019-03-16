---
title: Xamarin iOS アプリと Android で visual basic.net
description: このチュートリアルでは、Visual Basic.NET で記述されたビジネス ロジックを使用する、ネイティブの Xamarin.iOS および Xamarin.Android アプリを構築する方法を示します。
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f62d3cb076019ba49303f2c82f009975d9fbdc50
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2019
ms.locfileid: "58070970"
---
# <a name="visual-basicnet-in-xamarin-ios-and-android"></a>Xamarin iOS アプリと Android で visual basic.net

[TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)サンプル アプリケーションは、ポータブル クラス ライブラリにコンパイルされる Visual Basic コードを使用して、Xamarin を使用した方法を示します。 IOS、Android、Windows Phone で実行されている結果として得られるアプリの一部のスクリーン ショットを次に示します。

 [![](native-apps-images/image5.png "iOS、Android、および Windows phone、Visual Basic を使用してビルドされたアプリを実行しています。")](native-apps-images/image5.png#lightbox)

IOS、Android および Windows Phone の例では、プロジェクトがすべてで記述されたC#します。 各アプリケーションのユーザー インターフェイスがネイティブのテクノロジで構築された (ストーリー ボード、Xml と Xaml それぞれ) 中、`TodoItem`管理が Visual Basic のポータブル クラス ライブラリによって提供されるを使用して、`IXmlStorage`実装によって提供されます。ネイティブ プロジェクトです。

## <a name="sample-walkthrough"></a>サンプルのチュートリアル

このガイドでの Visual Basic の実装方法について説明します、 [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) iOS および Android 用の Xamarin サンプル。

> [!NOTE]
> 手順を確認[移植可能な Visual Basic.NET](index.md)このガイドに進む前にします。

## <a name="visualbasicportablelibrary"></a>VisualBasicPortableLibrary

Visual Basic のポータブル クラス ライブラリは、Visual Studio でのみ作成できます。
サンプルのライブラリには、次の 4 つの Visual Basic ファイルで、アプリケーションの基礎が含まれています。

-  IXmlStorage.vb
-  TodoItem.vb
-  TodoItemManager.vb
-  TodoItemRepositoryXML.vb


### <a name="ixmlstoragevb"></a>IXmlStorage.vb

ポータブル クラス ライブラリを持たないため、ファイル アクセスの動作は、プラットフォーム間を大幅に異なる、`System.IO`ファイル ストレージ Api で任意のプロファイル。 つまり、移植可能なコードで、ファイル システムと直接やり取りする場合、各プラットフォームでネイティブ プロジェクトにコールバックして必要があります。  実装できる、シンプルなインターフェイスに対して、Visual Basic コードを記述してC#、各プラットフォームでは、ファイル システムへのアクセスはまだ共有可能な Visual Basic のコードを持つことができます。

この 2 つのメソッドを含む単純なインターフェイスを使用するサンプル コード: シリアル化された Xml ファイルを読み書きします。

```vb
Public Interface IXmlStorage
    Function ReadXml(filename As String) As List(Of TodoItem)
    Sub WriteXml(tasks As List(Of TodoItem), filename As String)
End Interface
```

iOS、Android および Windows Phone の実装で、このインターフェイスは、ガイドの後半で表示されます。

### <a name="todoitemvb"></a>TodoItem.vb

このクラスには、アプリケーション全体で使用するビジネス オブジェクトが含まれています。 Visual Basic で定義し、iOS、Android および Windows Phone で記述されているプロジェクトと共有C#します。

クラス定義を次に示します。

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

サンプルは、読み込みし、保存の TodoItem オブジェクトに XML シリアル化および逆シリアル化を使用します。

### <a name="todoitemmanagervb"></a>TodoItemManager.vb

Manager クラスは、移植可能なコードの 'API' を表示します。 基本的な CRUD 操作を提供します、`TodoItem`クラスがこれらの操作の実装のないです。

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

IXmlStorage のインスタンスは、コンス トラクターは、パラメーターとして受け取ります。 これにより、各プラットフォームで共有できるその他の機能について説明する移植可能なコードをまだながら、独自のワーキング実装を提供できます。

### <a name="todoitemrepositoryvb"></a>TodoItemRepository.vb

リポジトリ クラスには、TodoItem オブジェクトの一覧を管理するためのロジックが含まれています。 完全なコードは、追加またはコレクションから削除すると、TodoItems の間で一意の ID 値を管理するには、主に、ロジックが存在する – 以下に示します。

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
> このコードは、非常に基本的なデータ ストレージ メカニズムの例です。
> ポータブル クラス ライブラリをできます (この場合、読み込みと Xml ファイルの保存は) をプラットフォーム固有の機能にアクセスするインターフェイスに対するコードの方法を示すために提供されます。 ありません、実稼働品質のデータベースの代替を指定します。

## <a name="ios-android-and-windows-phone-application-projects"></a>iOS、Android、Windows Phone アプリケーション プロジェクト

このセクションでは、IXmlStorage インターフェイスのプラットフォームに固有の実装を格納し、各アプリケーションでの使用方法を示します。 アプリケーション プロジェクトがすべてで記述されたC#します。

### <a name="ios-and-android-ixmlstorage"></a>iOS および Android IXmlStorage

Xamarin.iOS および Xamarin.Android が完全に提供`System.IO`機能を簡単に読み込むし、次のクラスを使用して Xml ファイルを保存できるようにします。

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

IOS アプリケーションで、`TodoItemManager`と`XmlStorageImplementation`で作成された、 **AppDelegate.cs**ファイルの次のコード スニペットで示すようにします。 最初の 4 つの行がデータを保存する場所のファイルのパスを構築します。最後の 2 つの行では、インスタンス化されている 2 つのクラスを表示します。

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Android アプリケーションで、`TodoItemManager`と`XmlStorageImplementation`で作成された、 **Application.cs**ファイルの次のコード スニペットで示すようにします。 最初の 3 つの行がデータを保存する場所のファイルのパスを構築します。最後の 2 つの行では、インスタンス化されている 2 つのクラスを表示します。

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new AndroidTodo.XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

ユーザー インターフェイスに関心を使用して、アプリケーション コードの残りの部分は主に、`TaskMgr`クラスの読み込みし、保存を`TodoItem`クラス。

### <a name="windows-phone-ixmlstorage"></a>Windows Phone IXmlStorage

Windows Phone は提供されません、デバイスのファイル システムに完全なアクセス、IsolatedStorage API を代わりに公開します。 `IXmlStorage` Windows Phone の実装は、次のようになります。

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

Windows Phone アプリケーションの残りの部分から成る Xaml とC#ユーザー インターフェイスを作成して使用する、`TodoMgr`クラスの読み込みし、保存を`TodoItem`オブジェクト。

## <a name="visual-basic-pcl-in-visual-studio-for-mac"></a>Visual Basic では、Visual Studio for Mac の PCL

Visual Studio for Mac は、Visual Basic 言語をサポートしていません-作成または for mac。 Visual Studio と Visual Basic プロジェクトをコンパイルすることはできません。

Visual Studio for Mac のポータブル クラス ライブラリのサポートは、Visual Basic からビルドされたアセンブリを PCL を参照できることを意味します。

このセクションでは、Visual Studio での PCL のアセンブリをコンパイルしてしたがバージョン コントロール システムに格納され、他のプロジェクトで参照されていることを確認する方法について説明します。

### <a name="keeping-the-pcl-output-from-visual-studio"></a>Visual Studio から PCL の出力を保持します。

既定 (TFS および Git を含む)、ほとんどのバージョン コントロール システムが無視するよう構成する、 **/bin/** PCL のコンパイル済みのアセンブリをつまりディレクトリは格納されません。 つまり、Visual Studio for Mac への参照の追加を実行しているコンピューターに手動でコピーする必要があります。

バージョン コントロール システムは、PCL のアセンブリの出力を格納できますさせるには、プロジェクトのルートにコピーするビルド後のスクリプトを作成できます。 このビルド後の手順により、アセンブリを簡単にソース管理に追加および他のプロジェクトと共有することを確認します。

#### <a name="visual-studio-2017"></a>Visual Studio 2017

1. プロジェクトを右クリックし、選択、**プロパティ > ビルド イベント**セクション。

2. 追加、 _post-build_出力 DLL をこのプロジェクトのプロジェクトのルート ディレクトリにコピーするスクリプト (の外部である **/bin/**)。 バージョン コントロールの構成によって、DLL のソース管理に追加することがようになりました場合があります。

  [![](native-apps-images/image6-vs-sml.png "ビルド イベントの後のビルド スクリプト VB DLL をコピーするには")](native-apps-images/image6-vs.png#lightbox)

次回プロジェクトをビルドするをプロジェクトのルートを使用して、変更、DLL はれますチェック-/コミット/プッシュしたポータブル クラス ライブラリ アセンブリがコピーされます (その for Mac で Visual Studio を Mac にダウンロードできます) を格納します。

  [![](native-apps-images/image8-sml.png "Visual Basic アセンブリの出力のファイルの場所")](native-apps-images/image8.png#lightbox)


このアセンブリことができますし、プロジェクトに追加 Xamarin Visual studio for Mac、場合でも、Xamarin iOS または Android プロジェクトでは、Visual Basic 言語自体はサポートされていません。

### <a name="referencing-the-pcl-in-visual-studio-for-mac"></a>Mac を Visual Studio で、PCL を参照します。

Xamarin は Visual Basic をサポートしていないために読み込むことができない、PCL プロジェクト (も Windows Phone アプリ) このスクリーン ショットで示すようにします。

 [![](native-apps-images/image9.png "Visual Studio for Mac ソリューション")](native-apps-images/image9.png#lightbox)

Xamarin.iOS および Xamarin.Android プロジェクトで Visual Basic の PCL アセンブリ DLL 含めることができます。

1.  右クリックし、**参照**ノード**参照の編集.**

    [![](native-apps-images/image10.png "プロジェクトの編集の参照 メニュー")](native-apps-images/image10.png#lightbox)

1.  選択、 **.Net アセンブリ**タブし、Visual Basic のプロジェクト ディレクトリに出力 DLL に移動します。 場合でも、Visual Studio for Mac では、プロジェクトを開くことができません、すべてのファイルがソース管理からがありますする必要があります。 クリックして**追加**し**OK** iOS および Android アプリケーションにこのアセンブリを追加します。

    [![](native-apps-images/image11-sml.png "IOS および Android アプリケーションにこのアセンブリを追加するには、追加、[ok] をクリックします。")](native-apps-images/image11.png#lightbox)

1.  IOS および Android アプリケーションでは、Visual Basic のポータブル クラス ライブラリによって提供されるアプリケーション ロジックを含めることができますようになりました。 このスクリーン ショットでは、iOS アプリケーションを Visual Basic の PCL を参照し、そのライブラリの機能を使用してコードを示します。

    [![](native-apps-images/image12-sml.png "編集の参照は、.NET アセンブリ ウィンドウを追加")](native-apps-images/image12.png#lightbox)


Visual Studio で Visual Basic プロジェクトにプロジェクトをビルド、ソース管理、結果として得られるアセンブリ DLL に格納し、Visual Studio for Mac をビルドするためには、ソース管理から Mac にその新しい DLL を取得し、変更が加えられた場合は、最新バージョンを含めることが機能。


## <a name="summary"></a>まとめ

この記事で説明した方法: Visual Studio とポータブル クラス ライブラリを使用して Xamarin アプリケーションでの Visual Basic コードを使用します。 IOS および Android アプリに含まれる Visual Basic で記述されたコードを PCL に Visual Basic のコンパイルのにより、場合でも、Xamarin で Visual Basic が直接サポートされていません。

## <a name="related-links"></a>関連リンク

- [TaskyPortableVB (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [.NET Framework (マイクロソフト) を使用したクロス プラットフォーム開発](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
