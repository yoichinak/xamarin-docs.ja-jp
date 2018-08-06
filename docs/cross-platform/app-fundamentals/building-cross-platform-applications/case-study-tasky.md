---
title: 'クロスプラット フォーム アプリのケース スタディ: Tasky'
description: このドキュメントでは、Tasky Portable サンプル アプリケーションに設計されていて、クロス プラットフォーム モバイル アプリケーションとしてビルド方法について説明します。 これは、アプリの要件、インターフェイス、データ モデル、コア機能、実装、および詳細について説明します。
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 48650445d06ad3bc7ca6d4da84c9b8837f8a0f88
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782236"
---
# <a name="cross-platform-app-case-study-tasky"></a>クロスプラット フォーム アプリのケース スタディ: Tasky

*Tasky* *ポータブル*単純 to do リスト アプリケーションです。 このドキュメントは、どのが設計、構築できる、次のガイダンスについて説明します、[クロス プラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)ドキュメント。 次の領域について説明します。

<a name="Design_Process" />

## <a name="design-process"></a>設計プロセス

作成することをお勧め、コーディングを開始する前に実現するために必要なものの道路マップのです。 これは、複数の方法で公開される機能を構築するクロスプラット フォーム開発では、特に当てはまります。 新機能をビルドする保存時間と労力開発サイクルの後半で明確な目的で開始しています。

 <a name="Requirements" />

### <a name="requirements"></a>必要条件

アプリケーションの設計の最初の手順では、目的の機能を識別します。 高度な目標を指定できますまたはユース ケースを詳しく説明します。 Tasky 簡単な機能の要件があります。

 -  タスクの一覧を表示します。
 -  追加、編集、およびタスクを削除します。
 -  タスクの状態を設定する 'done'

プラットフォーム固有の機能の使用を検討する必要があります。  Tasky を利用して iOS ジオフェンシングまたは Windows Phone のライブ タイルのですか。 最初のバージョンでプラットフォーム固有の機能を使用しない場合でも立てる必要があります事前ビジネス & データ レイヤーはそれらで対応できるかどうかを確認します。

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>ユーザー インターフェイス デザイン

ターゲット プラットフォームで実装できる高度な設計を開始します。 注プラットフォーム固有の UI の制約に注意します。 たとえば、 `TabBarController` iOS でボタンを表示できる 5 つを超える、一方、Windows Phone の該当するショートカットは 4 つを表示できます。
(用紙機能します)、任意のツールを使用して、画面フローを描画します。

 [![](case-study-tasky-images/taskydesign.png "選択肢用紙が機能するのツールを使用して、画面フローの描画します。")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>データ モデル

どのようなデータを格納する必要がありますを知るを決定する永続化メカニズムを使用することができます。 参照してください[クロスプラット フォームのデータ アクセス](~/cross-platform/app-fundamentals/index.md)について使用可能記憶域メカニズムとそれらの間を判断するには。 このプロジェクトで使用する SQLite.NET です。

Tasky、各 'TaskItem' の 3 つのプロパティを格納する必要があります。

 -  **名前**– 文字列
 -  **ノート**– 文字列
 -  **完了**– ブール

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>中心的機能

ユーザー インターフェイスは、要件を満たすために使用する必要があります、API を検討してください。 To do リストには、次の関数が必要です。

 -   **すべてのタスクを一覧表示**– すべての利用可能なタスクのメイン画面の一覧を表示
 -  **1 つのタスクを取得する**– タスク行を変更するとき
 -  **1 つのタスクを保存**タスクを編集するとき。
 -  **1 つのタスクを削除**タスクが削除されたとき。
 -  **空のタスクを作成する**– で新しいタスクを作成するとき

コードの再利用を実現するためにこの API に実装して、1 回、*ポータブル クラス ライブラリ*です。

 <a name="Implementation" />

### <a name="implementation"></a>実装

アプリケーションの設計が承諾されると、クロスプラット フォームのアプリケーションとしての実装方法がありますを検討してください。 これには、アプリケーションのアーキテクチャになります。 ガイダンスに従って、[クロス プラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)ドキュメント、アプリケーション コードが壊れているがある次の部分にダウン。

 -   **一般的なコード**– タスクのデータを保存するための再利用可能なコードが含まれているモデルのクラスと保存を管理するために API を公開共通プロジェクトとデータの読み込みします。
 -   **プラットフォーム固有のコード**– 'バックエンド' として一般的なコードを使用して、各オペレーティング システムのネイティブ UI を実装するプラットフォーム固有のプロジェクトです。

 [![](case-study-tasky-images/taskypro-architecture.png "プラットフォーム固有のプロジェクトのバックエンドとして一般的なコードを使用して、各オペレーティング システムのネイティブ UI を実装します。")](case-study-tasky-images/taskypro-architecture.png#lightbox)

これら 2 つの部分は、次のセクションで説明します。

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>(PCL) の共通コード

Tasky Portable は、一般的なコードを共有するため、ポータブル クラス ライブラリの戦略を使用します。 参照してください、[コードの共有オプション](~/cross-platform/app-fundamentals/code-sharing.md)コード共有のオプションの詳細についてはドキュメントです。

データ アクセス層、データベース コード、およびコントラクトを含むすべての一般的なコードは、ライブラリ プロジェクトに配置されます。

完全な PCL プロジェクトを次に示します。 すべてのポータブル ライブラリでのコードは、各対象プラットフォームと互換性のあります。 展開すると、各ネイティブ アプリはそのライブラリを参照します。

![](case-study-tasky-images/portable-project.png "各ネイティブ アプリがそのライブラリを参照が展開されると、")

次のクラス図は、レイヤーでグループ化されたクラスを示します。 `SQLiteConnection`クラス Sqlite NET パッケージからの定型コードに示します。 クラスの残りの部分は、Tasky 用のカスタム コードです。 `TaskItemManager`と`TaskItem`クラスは、プラットフォーム固有のアプリケーションに公開されている API を表します。

 [![](case-study-tasky-images/classdiagram-core.png "TaskItemManager と TaskItem クラスを表すプラットフォーム固有のアプリケーションに公開されている API")](case-study-tasky-images/classdiagram-core.png#lightbox)

各レイヤー間の参照の管理に役立つレイヤーを分離する名前空間を使用します。 プラットフォーム固有のプロジェクトにのみを含める必要、`using`ビジネス層のステートメント。 によって公開される API では、データ アクセス層およびデータ層をカプセル化する必要があります`TaskItemManager`ビジネス レイヤーでします。

 <a name="References" />

### <a name="references"></a>参照

ポータブル クラス ライブラリは、それぞれのプラットフォームおよびフレームワークの機能のサポートのさまざまなレベルに、複数のプラットフォームで使用できるようにする必要があります。 そのためは、パッケージとフレームワーク ライブラリを使用することができますを制限があります。 たとえば、Xamarin.iOS はサポートされて、c#`dynamic`キーワード、ため、Android でこのようなコードは機能が、ポータブル クラス ライブラリは、動的なコードに依存する任意のパッケージを使用できません。 Visual Studio for Mac できなくなります互換性のないパッケージと参照を追加する必要しますが、あります内に保持する制限事項を後で予想外の問題を回避することに注意します。

注: プロジェクトが使用されていないフレームワーク ライブラリを参照するはずです。 これらの参照は、Xamarin プロジェクト テンプレートの一部として含まれています。 リンク プロセスで参照されていないコードは、の削除があってアプリがコンパイルされると、`System.Xml`されましたが、参照、されません最終的なアプリケーションでは、Xml の関数は使用しないためです。

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>データ層 (DL)

データ層には、データの物理的な記憶域をデータベース、フラット ファイルまたはその他のメカニズムには、コードが含まれています。 Tasky データ レイヤーは、2 つの部分で構成されています。 SQLite net-library とを関連付けることに追加するカスタム コード。

Tasky 依存 (Frank Kreuger によって発行された) Sqlite net nuget パッケージをオブジェクト リレーショナル マッピング (ORM) データベース インターフェイスを提供する SQLite NET コードを埋め込みます。 `TaskItemDatabase`クラスから継承`SQLiteConnection`し、必要な作成、読み取り、更新、SQLite にデータを読み書きする削除 (CRUD) メソッドを追加します。 他のプロジェクトで再利用の可能性があるジェネリックの CRUD メソッドの単純な定型実装することをお勧めします。

`TaskItemDatabase`シングルトン、同じインスタンスに対してすべてのアクセスが発生したことを確認します。 ロックは、複数のスレッドから同時実行のアクセスを防ぐために使用されます。

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>Windows Phone で SQLite

IOS および Android が付属しています、オペレーティング システムの一部として SQLite と、中に Windows Phone では、互換性のあるデータベース エンジンは含まれません。 3 つすべてのプラットフォーム間でコードを共有するには、SQLite の Windows phone ネイティブ バージョンが必要です。 参照してください[ローカル データベースで作業](~/xamarin-forms/app-fundamentals/databases.md)詳細については、Sqlite 用 Windows Phone プロジェクトの設定。

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>データ アクセスを一般化するインターフェイスを使用します。

データ層には、依存関係`BL.Contracts.IBusinessIdentity`主キーを必要とする抽象データ アクセス メソッドを実装できるようにします。 インターフェイスを実装する任意のビジネス層クラスは、データ層で、永続化できます。

インターフェイスには、主キーとして機能する整数のプロパティだけを指定します。

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

基本クラスは、インターフェイスを実装し、自動インクリメントの主キーとしてマークする SQLite NET 属性を追加します。 ビジネス層で、この基本クラスを実装する任意のクラスは、データ層で、永続化できます。

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

インターフェイスを使用して、データ層内のジェネリック メソッドの例は、この`GetItem<T>`メソッド。

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>同時実行のアクセスを防ぐロック

A[ロック](http://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx)内部に実装されて、`TaskItemDatabase`データベースへの同時アクセスを防ぐためにクラスです。 これは異なるスレッドから同時実行のアクセスはシリアル化することを確認する (それ以外の場合 UI コンポーネントからバック グラウンド スレッドによって更新されて、同時にデータベースの読み取りを試みる可能性があります)。 ロックを実装する方法の例を次に示します。

```csharp
static object locker = new object ();
public IEnumerable<T> GetItems<T> () where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return (from i in Table<T> () select i).ToList ();
    }
}
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

データ層のコードの大部分は、他のプロジェクトで再利用可能性があります。 レイヤーにのみアプリケーション固有のコードは、`CreateTable<TaskItem>`で呼び出し、`TaskItemDatabase`コンス トラクターです。

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>データ アクセス層 (DAL)

`TaskItemRepository`クラスにカプセル化する、厳密に型指定された API のデータ ストレージ機構`TaskItem`オブジェクトを作成するには削除、取得および更新します。

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>条件付きコンパイルを使用します。

ファイルの場所を設定する条件付きコンパイルを使用するクラス - これは、プラットフォームの相違を実装する例。 パスを返すプロパティは、各プラットフォームで別のコードをコンパイルします。 コードとプラットフォーム固有のコンパイラ ディレクティブは、次に示します。

```csharp
public static string DatabaseFilePath {
    get {
        var sqliteFilename = "TaskDB.db3";
#if SILVERLIGHT
        // Windows Phone expects a local path, not absolute
        var path = sqliteFilename;
#else
#if __ANDROID__
        // Just use whatever directory SpecialFolder.Personal returns
        string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
        // we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder
#endif
        var path = Path.Combine (libraryPath, sqliteFilename);
                #endif
                return path;
    }
}
```

、プラットフォームによっては、出力になります"<app
path>/Library/TaskDB.db3"ios の場合"<app
path>/Documents/TaskDB.db3"の Android またはだけ"TaskDB.db3"Windows Phone 用です。

### <a name="business-layer-bl"></a>ビジネス層 (BL)

ビジネス層は、モデルのクラスおよびそれらを管理するファサードを実装します。
Tasky モデルは、`TaskItem`クラスと`TaskItemManager`を管理するための API を提供するファサード パターンを実装`TaskItems`です。

 <a name="Façade" />

#### <a name="faade"></a>ファサード

 `TaskItemManager` ラップ、 `DAL.TaskItemRepository` Get、保存およびアプリケーションと UI レイヤーによって参照されるメソッドを削除します。

ビジネス ルールとロジックが配置されますここで必要な – たとえばその検証ルールをオブジェクトが保存される前に満たす必要がある場合。

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>プラットフォーム固有のコード用の API

共通のコードを記述すると、収集し、これによって公開されているデータを表示するユーザー インターフェイスを構築する必要があります。 `TaskItemManager`クラスにアクセスするアプリケーション コードの単純な API を提供するファサード パターンを実装します。

各プラットフォームに固有のプロジェクトで記述されたコードが、デバイスのネイティブの SDK に密接に結び付いて通常とによって定義された API を使用して、一般的なコードにのみアクセス、`TaskItemManager`です。 メソッドが含まれます、ビジネスなどのクラス、公開、`TaskItem`です。

イメージはプラットフォーム間で共有せずに個別に各プロジェクトに追加されません。 これは、機能は、各プラットフォーム イメージの処理が異なる別のファイル名、ディレクトリ、および解決方法を使用するため重要です。

残りのセクションでは、Tasky UI のプラットフォーム固有の実装の詳細について説明します。

 <a name="iOS_App" />

## <a name="ios-app"></a>iOS アプリ

少数のクラスを iOS Tasky を格納およびデータを取得する共通の PCL プロジェクトを使用してアプリケーションを実装する必要があります。 完全な iOS Xamarin.iOS プロジェクトには、次に示します。

 ![](case-study-tasky-images/taskyios-solution.png "iOS プロジェクトを示します")

クラスは、レイヤーにグループ化、このダイアグラムに表示されます。

 [![](case-study-tasky-images/classdiagram-android.png "この図は、レイヤーにグループ化で、クラスが表示されます。")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>参照

IOS アプリでは、たとえばプラットフォーム固有の SDK ライブラリ – を参照します。 Xamarin.iOS および MonoTouch.Dialog-1。

これを参照する必要がありますも、 `TaskyPortableLibrary` PCL プロジェクト。
参照の一覧を次に示します。

 ![](case-study-tasky-images/taskyios-references.png "参照の一覧を示します")

アプリケーション層とユーザー インターフェイス層は、これらの参照を使用してこのプロジェクトで実装されます。

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>アプリケーション層 (省略可)

アプリケーション層には、UI に PCL によって公開されているオブジェクトをバインドするために必要なプラットフォーム固有のクラスが含まれています。 IOS に固有のアプリケーションには、タスクを表示するための 2 つのクラスがあります。

 -   **EditingSource** – このクラスは、ユーザー インターフェイスにタスクの一覧をバインドに使用します。 `MonoTouch.Dialog`使用されたスワイプ-削除機能を有効にするには、このヘルパーを実装する必要はタスク一覧については、`UITableView`です。 スワイプ-削除はで Android または Windows Phone ではできませんが、iOS、iOS の特定のプロジェクトのみであることを実装するようにします。
 -   **ダイアログ ・** – UI に 1 つのタスクをバインドするこのクラスを使用します。 使用して、 `MonoTouch.Dialog` 'wrap' へのリフレクション API、`TaskItem`を正しく書式設定する入力画面を許可する適切な属性を格納するクラスを持つオブジェクト。

`TaskDialog`クラス`MonoTouch.Dialog`クラスのプロパティに基づいて、画面を作成する属性。 クラスは、次のようになります。

```csharp
public class TaskDialog {
    public TaskDialog (TaskItem task)
    {
        Name = task.Name;
        Notes = task.Notes;
        Done = task.Done;
    }
    [Entry("task name")]
    public string Name { get; set; }
    [Entry("other task info")]
    public string Notes { get; set; }
    [Entry("Done")]
    public bool Done { get; set; }
    [Section ("")]
    [OnTap ("SaveTask")]    // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Save;
    [Section ("")]
    [OnTap ("DeleteTask")]  // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Delete;
}
```

注意してください、`OnTap`属性がメソッド名が必要 – これらのメソッドがクラスに存在する必要があります場所、`MonoTouch.Dialog.BindingContext`が作成される (この場合、`HomeScreen`クラスは、次のセクションで説明)。

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>ユーザー インターフェイス レイヤー (UI)

ユーザー インターフェイス レイヤーは、次のクラスで構成されます。

1.   **AppDelegate** – フォントと、アプリケーションで使用される色のスタイルを設定する外観 API への呼び出しが含まれています。 Tasky は単純なアプリケーションで実行されている他の初期化タスクがないように`FinishedLaunching`です。
2.   **画面**– のサブクラス`UIViewController`各画面とその動作を定義します。 画面を結び付けアプリケーション レイヤーのクラスと、UI と一般的な API ( `TaskItemManager` )。 コードでは、この例では、画面の作成が、それらにデザインした Xcode のインターフェイスのビルダーまたはストーリー ボード デザイナーを使用します。
3.   **イメージ**– ビジュアル要素は、すべてのアプリケーションの重要な部分です。 Tasky がスプラッシュ スクリーンとアイコン イメージ、iOS の標準モードと Retina 解像度で指定する必要があります。

 <a name="Home_Screen" />

#### <a name="home-screen"></a>ホーム画面

ホーム画面には、 `MonoTouch.Dialog` SQLite データベースからのタスクの一覧を表示する画面です。 継承`DialogViewController`を設定するコードを実装して、`Root`のコレクションを格納する`TaskItem`表示するオブジェクト。

 [![](case-study-tasky-images/ios-taskylist.png "DialogViewController から継承し、表示する TaskItem オブジェクトのコレクションを含めるにはルートを設定するコードを実装すること")](case-study-tasky-images/ios-taskylist.png#lightbox)

表示して、タスクの一覧との対話に関連する 2 つの主な方法は次のとおりです。

1.   **PopulateTable** – ビジネス層を使用して`TaskManager.GetTasks`のコレクションを取得する方法を`TaskItem`を表示するオブジェクト。
2.   **選択した**– と行を変更すると、新しい画面にタスクを表示します。

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>タスクの詳細画面

タスクの詳細は、入力画面を編集または削除するタスクです。

Tasky 使用`MonoTouch.Dialog`のリフレクション API、画面を表示するのにはない`UIViewController`実装します。 代わりに、`HomeScreen`クラスのインスタンスを作成し、表示、`DialogViewController`を使用して、`TaskDialog`アプリケーション レイヤーからのクラスです。

このスクリーン ショットを示す空の画面を示しています、`Entry`属性で、透かしテキストの設定、**名前**と**ノート**フィールド。

 [![](case-study-tasky-images/ios-taskydetail.png "このスクリーン ショットは、名前とメモ フィールドで、透かしテキストの設定、エントリ属性を示す空の画面を示しています。")](case-study-tasky-images/ios-taskydetail.png#lightbox)

機能、**タスクの詳細**で画面 (保存のタスクを削除するなど) を実装する必要があります、`HomeScreen`クラス、このような場合があるため、`MonoTouch.Dialog.BindingContext`を作成します。 次`HomeScreen`メソッドは、タスクの詳細画面をサポートします。

1.   **ShowTaskDetails** : 作成、`MonoTouch.Dialog.BindingContext`画面を表示するためにします。 リフレクションを使用して、プロパティ名を取得する入力画面を作成してから型の`TaskDialog`クラスです。 プロパティの属性を持つ入力のボックスで、透かしテキストなどの追加情報が実装されます。
2.   **SaveTask** – でこのメソッドが参照されている、`TaskDialog`クラスを使用して、`OnTap`属性。 ときに呼び出された**保存**が押され、使用、`MonoTouch.Dialog.BindingContext`を使用して変更を保存する前に、ユーザーが入力したデータを取得する`TaskItemManager`です。
3.   **DeleteTask** – でこのメソッドが参照されている、`TaskDialog`クラスを使用して、`OnTap`属性。 使用して`TaskItemManager`を主キー (ID プロパティ) を使用してデータを削除します。

 <a name="Android_App" />

## <a name="android-app"></a>Android アプリ

完全な Xamarin.Android プロジェクトは、次に示します。

 ![](case-study-tasky-images/taskyandroid-solution.png "Android プロジェクトの図に示します")

クラス図、レイヤーでグループ化されたクラスを使用:

 [![](case-study-tasky-images/classdiagram-android.png "レイヤーでグループ化されたクラスと、クラス ダイアグラム")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>参照

Android アプリ プロジェクトは、Android SDK のクラスにアクセスするプラットフォーム固有 Xamarin.Android アセンブリを参照する必要があります。

(PCL プロジェクトを参照、ことも必要があります。 TaskyPortableLibrary) をクリックし、一般的なデータとビジネス層のコードにアクセスします。

 ![](case-study-tasky-images/taskyandroid-references.png "一般的なデータとビジネス層のコードにアクセスする TaskyPortableLibrary")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>アプリケーション層 (省略可)

考えた iOS のバージョンのような以前で、Android のバージョンのアプリケーション層にはクラスが含まれますプラットフォーム固有には UI にコアによって公開されているオブジェクトをバインドするために必要です。

 **TaskListAdapter** – 一覧を表示する<T>オブジェクト内のカスタム オブジェクトを表示するためのアダプターを実装する必要があります、`ListView`です。 リスト内の各項目のレイアウトが使用されるアダプターの制御 – コードが Android の組み込みのレイアウトを使用してこのケースで`SimpleListItemChecked`です。

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>ユーザー インターフェイス (UI)

Android アプリのユーザー インターフェイス層は、コードと XML マークアップの組み合わせです。

 -   **リソース/レイアウト**– 画面レイアウトと行のセルのデザイン AXML ファイルとして実装します。 AXML には、手動で記述されたまたはレイアウトを視覚的にデザイナーを使用して、Xamarin UI for Android を指定できます。
 -   **リソース/ディスプレイ**– イメージ (アイコン) とカスタム ボタンをクリックします。
 -   **画面**– 各画面とその動作を定義するアクティビティ サブクラスです。 アプリケーション レイヤーのクラスと UI と共通の API を結びつける (`TaskItemManager`)。

 <a name="Home_Screen" />

#### <a name="home-screen"></a>ホーム画面

ホーム画面から成るアクティビティ サブクラス`HomeScreen`と`HomeScreen.axml`レイアウト (ボタンをクリックし、タスク リストの位置) を定義するファイル。 次のような画面が表示。

 [![](case-study-tasky-images/android-taskylist.png "次のような画面")](case-study-tasky-images/android-taskylist.png#lightbox)

ホーム画面のコードがボタンをクリックし、リスト内の項目をクリックするの一覧を設定するハンドラーを定義、`OnResume`メソッド (つまり、タスクの詳細 画面で行われた変更を反映)。 ビジネス層を使用してデータが読み込まれる`TaskItemManager`と`TaskListAdapter`アプリケーション レイヤーからです。

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>タスクの詳細画面

タスクの詳細画面にはまた、`Activity`サブクラスと AXML レイアウト ファイルです。 レイアウトは、入力コントロールの場所を決定し、c# のクラス定義の読み込みし、保存の動作`TaskItem`オブジェクト。

 [![](case-study-tasky-images/android-taskydetail.png "クラスを読み込んで TaskItem オブジェクトを保存する動作を定義します。")](case-study-tasky-images/android-taskydetail.png#lightbox)

PCL ライブラリへのすべての参照は、`TaskItemManager`クラスです。

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Windows Phone アプリ
完全な Windows Phone プロジェクト:

 ![](case-study-tasky-images/taskywp7-solution.png "Windows Phone アプリ、完全な Windows Phone プロジェクト")

次の図には、レイヤーにグループ化するクラスが表示されます。

 [![](case-study-tasky-images/classdiagram-wp7.png "この図のレイヤーにグループ化するクラスを表示します。")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>参照

プラットフォーム固有のプロジェクトは、必要なプラットフォーム固有のライブラリを参照する必要があります (など`Microsoft.Phone`と`System.Windows`) を有効な Windows Phone アプリケーションを作成します。

(PCL プロジェクトを参照、ことも必要があります。 `TaskyPortableLibrary`) を利用する、`TaskItem`クラスとデータベースです。

 ![](case-study-tasky-images/taskywp7-references.png "TaskItem クラスとデータベースを利用する TaskyPortableLibrary")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>アプリケーション層 (省略可)

もう一度と同様に、iOS および Android のバージョンでは、アプリケーション層がデータをユーザー インターフェイスにバインドするための非ビジュアル要素ので構成されます。

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

ViewModels PCL からデータをラップする ( `TaskItemManager`) し、Silverlight と XAML データ バインディングで使用できるように提示します。 これは、(前述のクロス プラットフォーム アプリケーションのドキュメントで) プラットフォーム固有の動作の例です。

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>ユーザー インターフェイス (UI)

XAML は、マークアップで宣言することができ、オブジェクトを表示するために必要なコードの量を削減する独自のデータ バインディング機能があります。

1.   **ページ**– XAML ファイルとその分離コード ユーザー インターフェイスを定義し、ViewModels と PCL プロジェクトを表示し、データの収集を参照します。
2.   **イメージ**– スプラッシュ スクリーン、背景色、アイコンの画像は、ユーザー インターフェイスの重要な部分です。

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

MainPage クラスの使用、 `TaskListViewModel` XAML のデータ バインディング機能を使用してデータを表示します。 ページの`DataContext`が非同期的に設定されているビュー モデルに設定されています。 `{Binding}` Xaml 構文では、データの表示方法を決定します。

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>TaskDetailsPage

各タスクを表示するバインディングを`TaskViewModel`TaskDetailsPage.xaml で定義されている XAML にします。 使用してタスクのデータを取得、`TaskItemManager`ビジネス レイヤーでします。

 <a name="Results" />

## <a name="results"></a>結果

結果として得られるアプリケーションは、各プラットフォームで、次のようになります。

 <a name="iOS" />

#### <a name="ios"></a>iOS

'Add' ボタンなどの iOS の標準のユーザー インターフェイスのデザインを使用して、ナビゲーション バーに配置されていると、組み込みを使用して**プラス (+)** アイコン。 既定値も使用`UINavigationController`[戻る] ボタンの動作とテーブルの ' スワイプ delete' をサポートしています。

 [![](case-study-tasky-images/ios-taskylist.png "また既定 UINavigationController [戻る] ボタンの動作をサポートしてスワイプ-削除の表に")](case-study-tasky-images/ios-taskylist.png#lightbox) [![](case-study-tasky-images/ios-taskylist.png "既定 UINavigationController でも使用ボタンの動作をバックアップし、テーブルにスワイプして、削除をサポートしています")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

#### <a name="android"></a>Android

Android アプリでは、組み込みの表示 'ティック' が必要な行のレイアウトを含む組み込みのコントロールを使用します。 ハードウェア/システムのバックの動作は、画面に表示される [戻る] ボタンに加えサポートします。

 [![](case-study-tasky-images/android-taskylist.png "画面に表示される戻るボタンに加えてハードウェア/システムのバックの動作がサポートされている")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "ハードウェア/システムのバックの動作が他に、サポートされている、画面に表示されます。[戻る] ボタン")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

#### <a name="windows-phone"></a>Windows Phone

Windows Phone アプリでは、上部のナビゲーション バーではなく、画面の下部にあるアプリ バーを設定する、標準のレイアウトを使用します。

 [![](case-study-tasky-images/wp-taskylist.png "Windows Phone アプリで使用する標準のレイアウトでは、上部のナビゲーション バーではなく、画面の下部にあるアプリ バーを設定する")](case-study-tasky-images/wp-taskylist.png#lightbox) [![](case-study-tasky-images/wp-taskylist.png "Windows Phone アプリ標準を使用します。レイアウト、上部のナビゲーション バーではなく、画面の下部にあるアプリ バーを設定します。")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>まとめ

このドキュメントが 3 つのモバイル プラットフォーム間でコードの再利用を容易に単純なアプリケーションに、複数層のアプリケーションの設計の原則が適用される方法の詳細についてを指定します。 iOS、Android、Windows Phone です。

アプリケーション レイヤーをデザインするために使用するプロセスを説明したが、どのようなコードを説明した&amp;機能は、各レイヤーに実装されています。

コードからダウンロードできます。 [github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)です。

## <a name="related-links"></a>関連リンク

- [クロス プラットフォーム アプリケーションの構築 (メイン ドキュメント)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky Portable サンプル アプリ (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
