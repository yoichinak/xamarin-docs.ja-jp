---
title: Xamarin Android と Xamarin の Visual Basic
description: このチュートリアルでは、Visual Basic.NET で記述されたビジネスロジックを使用するネイティブ Xamarin iOS および Xamarin Android アプリをビルドする方法について説明します。
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
author: davidortinau
ms.author: daortin
ms.date: 04/24/2019
ms.openlocfilehash: 9f227f51596a4ed93fd830c3f3495a90c1f7f722
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014559"
---
# <a name="visual-basic-in-xamarin-android-and-ios"></a>Xamarin Android および iOS の Visual Basic

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-taskyvb/)

[Taskyvb](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-taskyvb/)サンプルアプリケーションは、.NET Standard ライブラリにコンパイルされた Visual Basic コードを Xamarin で使用する方法を示しています。 Android および iOS で実行されているアプリのスクリーンショットを次に示します。

 [Visual Basic でビルドされたアプリを実行している Android と iOS![](native-apps-images/simulators-sml.png)](native-apps-images/simulators.png#lightbox)

この例の Android および iOS プロジェクトはすべて、でC#記述されています。 各アプリケーションのユーザーインターフェイスはネイティブテクノロジを使用して構築されていますが、`TodoItem` 管理は XML ファイルを使用して Visual Basic .NET Standard ライブラリによって提供されます (完全なデータベースではなく、デモンストレーションの目的で)。

## <a name="sample-walkthrough"></a>サンプルのチュートリアル

このガイドでは、iOS および Android 用の[Taskyvb](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyVB) Xamarin サンプルに Visual Basic を実装する方法について説明します。

> [!NOTE]
> このガイドを続行する前に、 [Visual Basic と .NET Standard](index.md)の手順を確認してください。
>
> Visual Basic の手順[を使用して Xamarin のフォーム](xamarin-forms.md)を参照し、共有ユーザーインターフェイス Visual Basic コードを使用してアプリを構築する方法を確認してください。

## <a name="visualbasicnetstandard"></a>VisualBasicNetStandard

Visual Basic .NET Standard ライブラリは、Windows の Visual Studio でのみ作成できます。
このサンプルライブラリには、次の Visual Basic ファイルのアプリケーションの基本が含まれています。

- TodoItem
- TodoItemManager
- TodoItemRepositoryXML
- XmlStorage .vb

### <a name="todoitemvb"></a>TodoItem

このクラスには、アプリケーション全体で使用されるビジネスオブジェクトが含まれています。 これは Visual Basic で定義され、でC#記述されている Android および iOS プロジェクトと共有されます。

クラス定義を次に示します。

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

このサンプルでは、XML のシリアル化と逆シリアル化を使用して、TodoItem オブジェクトを読み込んで保存します。

### <a name="todoitemmanagervb"></a>TodoItemManager

Manager クラスは、移植可能なコードの "API" を提示します。 `TodoItem` クラスに対する基本的な CRUD 操作を提供しますが、これらの操作を実装することはできません。

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String)
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

コンストラクターは、パラメーターとして IXmlStorage のインスタンスを受け取ります。 これにより、各プラットフォームは独自の動作実装を提供できるようになりますが、移植可能なコードには共有できる他の機能が記述されます。

### <a name="todoitemrepositoryvb"></a>TodoItemRepository

Repository クラスには、TodoItem オブジェクトの一覧を管理するためのロジックが含まれています。 完全なコードを次に示します。ロジックは主に、コレクションに追加および削除されたときに、TodoItems 全体で一意の ID 値を管理するために存在します。

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String)
        _filename = filename
        _storage = New XmlStorage
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
> このコードは、非常に基本的なデータストレージメカニズムの一例です。
> これは、.NET Standard ライブラリがインターフェイスに対してコードを記述し、プラットフォーム固有の機能 (この場合は XML ファイルの読み込みと保存) にアクセスする方法を示すために用意されています。 これは、実稼働品質のデータベースの代わりとして使用することを目的としたものではありません。

## <a name="android-and-ios-application-projects"></a>Android と iOS のアプリケーションプロジェクト

### <a name="ios"></a>iOS

IOS アプリケーションでは、次のコードスニペットに示すように、`TodoItemManager` と `XmlStorageImplementation` が**AppDelegate.cs**ファイルに作成されます。 最初の4行は、データが格納されるファイルへのパスを作成するだけです。最後の2行は、インスタンス化されている2つのクラスを示しています。

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);

TaskMgr = new TodoItemManager(path);
```

### <a name="android"></a>Android

Android アプリケーションでは、次のコードスニペットに示すように、`TodoItemManager` と `XmlStorageImplementation` が**Application.cs**ファイルに作成されます。 最初の3行は、データが格納されるファイルへのパスを作成するだけです。最後の2行は、インスタンス化されている2つのクラスを示しています。

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);

TaskMgr = new TodoItemManager(path);
```

アプリケーションコードの残りの部分は、主にユーザーインターフェイスを使用し、`TaskMgr` クラスを使用して `TodoItem` クラスを読み込んで保存します。

## <a name="visual-studio-2019-for-mac"></a>Visual Studio 2019 for Mac

> [!WARNING]
> Visual Studio for Mac は Visual Basic 言語の編集をサポートしていません。 Visual Basic プロジェクトまたはファイルを作成するためのメニュー項目はありません。 **.Vb**を開いた場合、言語構文の強調表示、オートコンプリート、IntelliSense はありません。

Visual Studio 2019 for Mac では、Windows 上で作成された .NET Standard プロジェクトをコンパイル_できる_ので、iOS アプリはこれらのプロジェクトを参照できます。

Visual Studio 2017 では Visual Basic プロジェクトをまったくビルド_できません_。

## <a name="summary"></a>まとめ

この記事では、Visual Studio と .NET Standard ライブラリを使用して Xamarin アプリケーションの Visual Basic コードを使用する方法について説明しました。 Xamarin は Visual Basic を直接サポートしていませんが、Visual Basic を .NET Standard ライブラリにコンパイルすると、Visual Basic で記述されたコードが iOS および Android アプリに含まれるようになります。

## <a name="related-links"></a>関連リンク

- [TaskyVB (.NET Standard サンプル)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyVB)
- [.NET Standard の新機能](https://docs.microsoft.com/dotnet/standard/whats-new/whats-new-in-dotnet-standard?tabs=csharp)
