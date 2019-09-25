---
title: クロスプラットフォームアプリのケーススタディ:Tasky
description: このドキュメントでは、Tasky ポータブルサンプルアプリケーションをクロスプラットフォームモバイルアプリケーションとして設計および構築する方法について説明します。 アプリの要件、インターフェイス、データモデル、コア機能、実装などについて説明します。
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 246ee002404fdf6fe1120c19701aceb3c2dee7db
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "71249784"
---
# <a name="cross-platform-app-case-study-tasky"></a>クロスプラットフォームアプリのケーススタディ:Tasky

*Tasky* *ポータブル*は、単純な to do list アプリケーションです。 このドキュメントでは、[クロスプラットフォームアプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)に関するドキュメントのガイダンスに従って、設計および構築された方法について説明します。 ここでは、次の領域について説明します。

<a name="Design_Process" />

## <a name="design-process"></a>デザインプロセス

コーディングを開始する前に、目的に応じて、道路マップのを作成することをお勧めします。 これは、複数の方法で公開される機能を構築するクロスプラットフォームの開発に特に当てはまります。 構築している内容を明確に理解しておくと、開発サイクルの後半で時間と労力を節約できます。

 <a name="Requirements" />

### <a name="requirements"></a>要件

アプリケーションを設計するための最初の手順は、目的の機能を特定することです。 これには、高レベルの目標や詳細なユースケースがあります。 Tasky には、簡単な機能要件があります。

- タスクの一覧を表示する
- タスクの追加、編集、削除
- タスクの状態を [完了] に設定します

プラットフォーム固有の機能の使用を検討する必要があります。  Tasky では、iOS ジオフェンシングまたは Windows Phone ライブタイルを利用できますか。 最初のバージョンでプラットフォーム固有の機能を使用しない場合でも、ビジネス & データレイヤーに対応できるかどうかを事前に計画しておく必要があります。

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>ユーザー インターフェイス デザイン

まず、ターゲットプラットフォーム全体に実装できる高レベルの設計を使用します。 プラットフォーム特定の UI 制約に注意してください。 たとえば、iOS の`TabBarController`は5つ以上のボタンを表示できますが、同等の Windows Phone には最大4つのボタンを表示できます。
任意のツールを使用して画面フローを描画します (paper works)。

 [![](case-study-tasky-images/taskydesign.png "選択した用紙のツールを使用して画面フローを描画する")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>データ モデル

どのようなデータを格納する必要があるかを把握することは、どの永続化メカニズムを使用するかを決定するのに役立ちます。 使用可能なストレージメカニズムの詳細については、「[クロスプラットフォームデータアクセス](~/cross-platform/app-fundamentals/index.md)」を参照してください。 このプロジェクトでは、SQLite.NET を使用します。

Tasky では、' TaskItem ' ごとに3つのプロパティを格納する必要があります。

- **名前**–文字列
- **メモ**–文字列
- **完了**–ブール値

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>中心的機能

ユーザーインターフェイスが要件を満たすために使用する必要がある API を検討します。 To do リストには、次の関数が必要です。

- **すべてのタスクを一覧**表示する–使用可能なすべてのタスクのメイン画面の一覧を表示します。
- **1 つのタスクを取得**する–タスク行にタッチするとき
- **1 つ**のタスクを保存する–タスクが編集されたとき
- **1 つ**のタスクを削除する–タスクが削除されたとき
- **空のタスクの作成**–新しいタスクが作成されたとき

コードの再利用を実現するには、この API を*ポータブルクラスライブラリ*に1回実装する必要があります。

 <a name="Implementation" />

### <a name="implementation"></a>実装

アプリケーションの設計に同意したら、それがクロスプラットフォームアプリケーションとしてどのように実装されるかを検討します。 これがアプリケーションのアーキテクチャになります。 [クロスプラットフォームアプリケーションのビルド](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)に関するドキュメントのガイダンスに従って、アプリケーションコードを次の部分に分割する必要があります。

- **共通コード**-タスクデータを格納するための再有効コードを含む共通のプロジェクトです。モデルクラスと API を公開して、データの保存と読み込みを管理します。
- **プラットフォーム固有のコード**– "バックエンド" として共通のコードを使用して、各オペレーティングシステムのネイティブ UI を実装するプラットフォーム固有のプロジェクト。

[![](case-study-tasky-images/taskypro-architecture.png "プラットフォーム固有のプロジェクトは、一般的なコードをバックエンドとして使用して、各オペレーティングシステムのネイティブ UI を実装します。")](case-study-tasky-images/taskypro-architecture.png#lightbox)

以下のセクションでは、これらの2つの部分について説明します。

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>共通 (PCL) コード

Tasky ポータブルは、共通コードを共有するためのポータブルクラスライブラリ戦略を使用します。 コード共有のオプションの説明については、[コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)に関するドキュメントを参照してください。

データアクセス層、データベースコード、およびコントラクトを含むすべての共通コードは、ライブラリプロジェクトに配置されます。

次に、完全な PCL プロジェクトを示します。 ポータブルライブラリ内のすべてのコードは、対象となる各プラットフォームと互換性があります。 デプロイされると、各ネイティブアプリはそのライブラリを参照します。

![](case-study-tasky-images/portable-project.png "デプロイされると、各ネイティブアプリはそのライブラリを参照します。")

次のクラス図は、レイヤーごとにグループ化されたクラスを示しています。 クラス`SQLiteConnection`は、Sqlite-NET パッケージの定型コードです。 クラスの残りの部分は Tasky のカスタムコードです。 クラス`TaskItemManager` と`TaskItem`クラスは、プラットフォーム固有のアプリケーションに公開されている API を表します。

 [![](case-study-tasky-images/classdiagram-core.png "TaskItemManager クラスと TaskItem クラスは、プラットフォーム固有のアプリケーションに公開されている API を表します。")](case-study-tasky-images/classdiagram-core.png#lightbox)

名前空間を使用してレイヤーを分離すると、各レイヤー間の参照を管理するのに役立ちます。 プラットフォーム固有のプロジェクトでは、ビジネス層の`using`ステートメントのみを含める必要があります。 データアクセス層とデータ層は、ビジネス層でによって`TaskItemManager`公開される API によってカプセル化される必要があります。

 <a name="References" />

### <a name="references"></a>関連項目

ポータブルクラスライブラリは、プラットフォームとフレームワークの機能のサポートレベルが異なる複数のプラットフォームで使用できる必要があります。 そのため、使用できるパッケージとフレームワークライブラリには制限があります。 たとえば、Xamarin では c# `dynamic`のキーワードがサポートされていないため、ポータブルクラスライブラリは、このようなコードが Android で動作する場合でも、動的なコードに依存するパッケージを使用することはできません。 Visual Studio for Mac では、互換性のないパッケージと参照を追加することはできませんが、後での使用を回避するために、制限事項を考慮する必要があります。

メモ:使用していないフレームワークライブラリがプロジェクトによって参照されていることがわかります。 これらの参照は、Xamarin プロジェクトテンプレートの一部として含まれています。 アプリがコンパイルされると、リンクプロセスによって参照されて`System.Xml`いないコードが削除されます。そのため、が参照されている場合でも、Xml 関数を使用していないため、最終的なアプリケーションには含まれません。

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>データ層 (DL)

データ層には、データベース、フラットファイル、またはその他のメカニズムのいずれであるかにかかわらず、データの物理的なストレージを行うコードが含まれています。 Tasky データレイヤーは2つの部分で構成されます。 SQLite-NET ライブラリと、それを接続するためのカスタムコードです。

Tasky は、(Frank Kreuger によって発行された) Sqlite-net nuget パッケージを使用して、オブジェクトリレーショナルマッピング (ORM) データベースインターフェイスを提供する SQLite-NET コードを埋め込みます。 クラス`TaskItemDatabase`はから`SQLiteConnection`継承され、必要な Create、Read、Update、Delete (CRUD) の各メソッドを追加して、データの読み取りと書き込みを SQLite に行います。 これは、他のプロジェクトで再利用できる汎用 CRUD メソッドの単純な定型的な実装です。

`TaskItemDatabase`はシングルトンであり、すべてのアクセスが同じインスタンスに対して行われることを保証します。 ロックは、複数のスレッドからの同時アクセスを防ぐために使用されます。

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>Windows Phone の SQLite

IOS と Android には両方とも、オペレーティングシステムの一部として SQLite が付属していますが、Windows Phone には互換性のあるデータベースエンジンが含まれていません。 3つのすべてのプラットフォームでコードを共有するには、Windows phone ネイティブバージョンの SQLite が必要です。 Sqlite 用の Windows Phone プロジェクトを設定する方法の詳細については[、「ローカルデータベースの操作](~/xamarin-forms/data-cloud/data/databases.md)」を参照してください。

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>インターフェイスを使用したデータアクセスの一般化

データ層は、主キーを`BL.Contracts.IBusinessIdentity`必要とする抽象データアクセスメソッドを実装できるように、への依存関係を取得します。 その後、インターフェイスを実装するビジネス層クラスをデータ層に保持できます。

インターフェイスは、主キーとして機能する整数プロパティを指定するだけです。

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

基底クラスはインターフェイスを実装し、SQLite-NET 属性を追加して、自動的にインクリメントする主キーとしてマークします。 この基本クラスを実装するビジネス層のクラスはすべて、データ層に保存できます。

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

インターフェイスを使用するデータ層のジェネリックメソッドの例を`GetItem<T>`次に示します。

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>同時アクセスを防止するためのロック

[ロック](https://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx)は、データベースへの`TaskItemDatabase`同時アクセスを防ぐために、クラス内に実装されます。 これは、異なるスレッドからの同時アクセスを確実にシリアル化するためです (そうしないと、バックグラウンドスレッドが更新しているときに、UI コンポーネントがデータベースの読み取りを試行することがあります)。 次に、ロックの実装方法の例を示します。

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

ほとんどのデータレイヤーコードは、他のプロジェクトで再利用できます。 レイヤー内のアプリケーション固有のコードは、 `CreateTable<TaskItem>` `TaskItemDatabase`コンストラクターの呼び出しだけです。

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>データアクセス層 (DAL)

クラス`TaskItemRepository`は、オブジェクトの作成、削除、取得、および更新を可能`TaskItem`にする厳密に型指定された API を使用して、データストレージ機構をカプセル化します。

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>条件付きコンパイルの使用

クラスは、条件付きコンパイルを使用してファイルの場所を設定します。これは、プラットフォームの相違を実装する例です。 パスを返すプロパティは、プラットフォームごとに異なるコードにコンパイルされます。 コードおよびプラットフォーム固有のコンパイラディレクティブを次に示します。

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

プラットフォームによっては、iOS<app
path><app
path>の場合は "db3"、Android の場合は "/Windows Phone"、または "taskdb..." のように出力されます。

### <a name="business-layer-bl"></a>ビジネス層 (BL)

ビジネス層は、モデルクラスとファサードを実装してそれらを管理します。
Tasky では、モデルは`TaskItem`クラスで`TaskItemManager`あり、を管理`TaskItems`するための API を提供するためにファサードパターンを実装します。

 <a name="Façade" />

#### <a name="faade"></a>ファサード

 `TaskItemManager`をラップ`DAL.TaskItemRepository`して、アプリケーションおよび UI レイヤーによって参照される Get、Save、および Delete メソッドを提供します。

ビジネスルールとロジックは、必要に応じてここに配置されます。たとえば、オブジェクトを保存する前に満たす必要がある検証規則などです。

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>プラットフォーム固有のコードの API

共通コードが記述されたら、ユーザーインターフェイスを作成して、それによって公開されたデータを収集して表示する必要があります。 クラス`TaskItemManager`は、アプリケーションコードにアクセスするための単純な API を提供するファサードパターンを実装します。

各プラットフォーム固有のプロジェクトで記述されたコードは、通常、そのデバイスのネイティブ SDK と密接に結合され、で`TaskItemManager`定義されている API を使用してのみ共通コードにアクセスします。 これに`TaskItem`は、などのメソッドとビジネスクラスが公開されます。

イメージはプラットフォーム間で共有されませんが、各プロジェクトに個別に追加されます。 プラットフォームごとに異なるファイル名、ディレクトリ、および解決方法を使用してイメージを処理するので、この方法が重要です。

残りのセクションでは、Tasky UI のプラットフォーム固有の実装の詳細について説明します。

 <a name="iOS_App" />

## <a name="ios-app"></a>iOS アプリ

データの格納と取得に共通 PCL プロジェクトを使用して iOS Tasky アプリケーションを実装するために必要なクラスはごくわずかです。 完全な iOS Xamarin. iOS プロジェクトを次に示します。

 ![](case-study-tasky-images/taskyios-solution.png "iOS プロジェクトはここに表示されます")

クラスは、この図のようにレイヤーにグループ化されています。

 [![](case-study-tasky-images/classdiagram-android.png "クラスは、この図のようにレイヤーにグループ化されています。")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>関連項目

IOS アプリはプラットフォーム固有の SDK ライブラリを参照します。例: Xamarin. iOS と Monotouch.dialog-1。

`TaskyPortableLibrary` PCL プロジェクトも参照する必要があります。
参照の一覧を次に示します。

 ![](case-study-tasky-images/taskyios-references.png "参照の一覧を次に示します。")

アプリケーションレイヤーとユーザーインターフェイスレイヤーは、これらの参照を使用して、このプロジェクトに実装されます。

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>アプリケーションレイヤー (AL)

アプリケーションレイヤーには、PCL によって公開されるオブジェクトを UI に "バインド" するために必要なプラットフォーム固有のクラスが含まれています。 IOS 固有のアプリケーションには、タスクの表示に役立つ2つのクラスがあります。

- **編集ソース**–このクラスは、タスクのリストをユーザーインターフェイスにバインドするために使用されます。 は`MonoTouch.Dialog`タスク一覧で使用されているため、このヘルパーを実装して、 `UITableView`でスワイプアンド削除機能を有効にする必要があります。 スワイプ-delete は iOS では一般的ですが、Android や Windows Phone ではありません。そのため、iOS 固有のプロジェクトは、それを実装する唯一のものです。
- **Taskdialog** -このクラスは、1つのタスクを UI にバインドするために使用されます。 この例で`MonoTouch.Dialog`は、リフレクション API を使用し`TaskItem`て、正しい属性を含むクラスを使用してオブジェクトを "ラップ" し、入力画面を正しく書式設定できるようにします。

クラス`TaskDialog`は、 `MonoTouch.Dialog`属性を使用して、クラスのプロパティに基づいて画面を作成します。 クラスは次のようになります。

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

属性に`OnTap`はメソッド名が必要であることに注意してください。これら`MonoTouch.Dialog.BindingContext`のメソッドは、が作成される`HomeScreen`クラス (この場合は次のセクションで説明するクラス) に存在する必要があります。

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>ユーザーインターフェイスレイヤー (UI)

ユーザーインターフェイスレイヤーは、次のクラスで構成されています。

1. **Appdelegate** –アプリケーションで使用されるフォントと色のスタイルを表示するための、外観 API の呼び出しが含まれています。 Tasky は単純なアプリケーションであるため、で`FinishedLaunching`実行されている他の初期化タスクはありません。
2. **画面–各**画面`UIViewController`とその動作を定義するのサブクラスです。 画面は、UI とアプリケーションレイヤークラスおよび共通 API ( `TaskItemManager` ) を相互に結び付けます。 この例では、画面はコードで作成されますが、Xcode の Interface Builder またはストーリーボードデザイナーを使用して設計されている可能性があります。
3. **イメージ**–ビジュアル要素は、すべてのアプリケーションの重要な部分です。 Tasky にはスプラッシュスクリーンとアイコンの画像があります。このイメージは、iOS 用の標準解像度と Retina 解像度で指定する必要があります。

 <a name="Home_Screen" />

#### <a name="home-screen"></a>ホーム画面

ホーム画面は、SQLite `MonoTouch.Dialog`データベースからのタスクの一覧を表示する画面です。 を継承`DialogViewController`し、コードを実装して`Root` 、表示するオブジェクトの`TaskItem`コレクションを格納するようにを設定します。

 [![](case-study-tasky-images/ios-taskylist.png "このメソッドは、コントロールを表示し、表示するために TaskItem オブジェクトのコレクションを含むようにルートを設定するコードを実装します。")](case-study-tasky-images/ios-taskylist.png#lightbox)

タスク一覧の表示と対話に関連する2つの主要な方法を次に示します。

1. **PopulateTable** –ビジネス層の`TaskManager.GetTasks`メソッドを使用して、表示するオブジェクトの`TaskItem`コレクションを取得します。
2. **選択**–行にタッチすると、によって新しい画面にタスクが表示されます。

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>タスクの詳細画面

タスクの詳細とは、タスクの編集や削除を可能にする入力画面です。

Tasky で`MonoTouch.Dialog`は、リフレクション API を使用して画面を表示する`UIViewController`ため、実装はありません。 代わりに、クラス`HomeScreen`は、アプリケーション層の`DialogViewController` `TaskDialog`クラスを使用してをインスタンス化して表示します。

このスクリーンショットは、 **[名前]** フィールド`Entry`と **[メモ]** フィールドにウォーターマークテキストを設定する属性を示す空の画面を示しています。

 [![](case-study-tasky-images/ios-taskydetail.png "このスクリーンショットでは、\"名前\" フィールドと \"メモ\" フィールドにウォーターマークテキストを設定する Entry 属性を示す空の画面を示しています。")](case-study-tasky-images/ios-taskydetail.png#lightbox)

**タスクの詳細**画面 (タスクの保存や削除など) の機能は、 `HomeScreen` `MonoTouch.Dialog.BindingContext`が作成される場所であるため、クラスに実装する必要があります。 次`HomeScreen`のメソッドでは、タスクの詳細画面がサポートされています。

1. **Showtaskdetails** –画面を`MonoTouch.Dialog.BindingContext`表示するを作成します。 リフレクションを使用して入力画面を作成し、 `TaskDialog`クラスからプロパティ名と型を取得します。 入力ボックスのウォーターマークテキストなどの追加情報は、プロパティの属性を使用して実装されます。
2. **Savetask** –このメソッドは、属性を`TaskDialog` `OnTap`使用してクラスで参照されます。 このメソッドは、 **[保存]** を押したとき`MonoTouch.Dialog.BindingContext`に呼び出され、を使用して`TaskItemManager`変更を保存する前に、を使用してユーザーが入力したデータを取得します。
3. **Deletetask** –このメソッドは、属性を`TaskDialog` `OnTap`使用してクラスで参照されます。 を使用`TaskItemManager`して、主キー (ID プロパティ) を使用してデータを削除します。

 <a name="Android_App" />

## <a name="android-app"></a>Android アプリ

完全な Xamarin. Android プロジェクトを次に示します。

 ![](case-study-tasky-images/taskyandroid-solution.png "Android プロジェクトはここに示されています")

クラス図。クラスはレイヤーごとにグループ化されています。

 [![](case-study-tasky-images/classdiagram-android.png "クラス図 (クラスをレイヤー別にグループ化)")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>関連項目

Android アプリプロジェクトは、Android SDK からクラスにアクセスするために、プラットフォーム固有の Xamarin. Android アセンブリを参照する必要があります。

また、PCL プロジェクトを参照する必要があります (例として、 TaskyPortableLibrary) を使用して、共通のデータとビジネスレイヤーのコードにアクセスします。

 ![](case-study-tasky-images/taskyandroid-references.png "共通データとビジネスレイヤーコードにアクセスするための TaskyPortableLibrary")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>アプリケーションレイヤー (AL)

前に見た iOS のバージョンと同様に、Android バージョンのアプリケーションレイヤーには、コアによって公開されたオブジェクトを UI に "バインド" するために必要なプラットフォーム固有のクラスが含まれています。

 **Tasklistadapter** –オブジェクトのリスト\<T > を表示するには、 `ListView`カスタムオブジェクトをに表示するアダプターを実装する必要があります。 アダプターは、リスト内の各項目に使用されるレイアウトを制御します。この場合、コードは Android の組み込み`SimpleListItemChecked`レイアウトを使用します。

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>ユーザーインターフェイス (UI)

Android アプリのユーザーインターフェイスレイヤーは、コードと XML マークアップを組み合わせたものです。

- **リソース/レイアウト**–画面レイアウトと、axml ファイルとして実装された行セルのデザイン。 AXML は、手動で記述したり、Android 用の Xamarin UI デザイナーを使用して視覚的に配置したりすることができます。
- **リソース/** 描画機能: イメージ (アイコン) とカスタムボタン。
- **画面–各**画面とその動作を定義するアクティビティサブクラスです。 UI とアプリケーションレイヤークラスおよび共通 API (`TaskItemManager`) を結び付けます。

 <a name="Home_Screen" />

#### <a name="home-screen"></a>ホーム画面

ホーム画面は、アクティビティサブクラス`HomeScreen` `HomeScreen.axml`と、レイアウト (ボタンとタスク一覧の位置) を定義するファイルで構成されています。 画面は次のようになります。

 [![](case-study-tasky-images/android-taskylist.png "画面は次のようになります。")](case-study-tasky-images/android-taskylist.png#lightbox)

ホーム画面のコードでは、ボタンをクリックしてリスト内の項目をクリックするためのハンドラーを定義します`OnResume` 。また、メソッドにリストを設定することもできます (タスクの詳細画面で行った変更が反映されます)。 データは、ビジネス層`TaskItemManager` `TaskListAdapter`とアプリケーション層のを使用して読み込まれます。

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>タスクの詳細画面

また、[タスクの詳細] 画面`Activity`は、サブクラスと axml レイアウトファイルで構成されています。 レイアウトによって入力コントロールの位置が決定さC#れ、クラスによってオブジェクトの`TaskItem`読み込みと保存の動作が定義されます。

 [![](case-study-tasky-images/android-taskydetail.png "クラスは、TaskItem オブジェクトを読み込んで保存する動作を定義します。")](case-study-tasky-images/android-taskydetail.png#lightbox)

PCL ライブラリへのすべての参照は、 `TaskItemManager`クラスを介して行われます。

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Windows Phone アプリ
完成した Windows Phone プロジェクト:

 ![](case-study-tasky-images/taskywp7-solution.png "完全な Windows Phone プロジェクトを Windows Phone アプリ")

次の図は、レイヤーにグループ化されたクラスを示しています。

 [![](case-study-tasky-images/classdiagram-wp7.png "この図は、レイヤーにグループ化されたクラスを示しています。")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>関連項目

プラットフォーム固有のプロジェクトでは、有効な Windows Phone アプリケーションを作成するために`Microsoft.Phone`必要`System.Windows`なプラットフォーム固有のライブラリ (やなど) を参照する必要があります。

また、PCL プロジェクトを参照する必要があります (例として、 `TaskyPortableLibrary`) を使用し`TaskItem`て、クラスとデータベースを使用します。

 ![](case-study-tasky-images/taskywp7-references.png "TaskItem クラスとデータベースを利用する TaskyPortableLibrary")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>アプリケーションレイヤー (AL)

ここでも、iOS と Android のバージョンと同様に、アプリケーションレイヤーは、ユーザーインターフェイスにデータをバインドするのに役立つ非ビジュアル要素で構成されています。

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

Viewmodel は PCL ( `TaskItemManager`) からデータをラップし、Silverlight/XAML データバインディングで使用できるようにします。 プラットフォーム固有の動作の例を次に示します (「クロスプラットフォームアプリケーション」ドキュメントをご説明します)。

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>ユーザーインターフェイス (UI)

XAML には、マークアップで宣言し、オブジェクトを表示するために必要なコードの量を減らすことができる、独自のデータバインディング機能があります。

1. **ページ**– XAML ファイルとその分離コードによってユーザーインターフェイスが定義され、VIEWMODEL と PCL プロジェクトを参照してデータを表示および収集します。
2. **イメージ**–スプラッシュスクリーン、背景およびアイコンのイメージは、ユーザーインターフェイスの重要な部分です。

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

Mainpage.xaml クラスは、を`TaskListViewModel`使用して、XAML のデータバインディング機能を使用してデータを表示します。 ページの`DataContext`がビューモデルに設定されます。これは非同期に設定されます。 XAML `{Binding}`の構文によって、データの表示方法が決まります。

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>TaskDetailsPage

各タスクは、 `TaskViewModel` TaskDetailsPage で定義されている xaml にをバインドすることによって表示されます。 タスクデータは、ビジネス層の`TaskItemManager`を通じて取得されます。

 <a name="Results" />

## <a name="results"></a>結果

結果として得られるアプリケーションは、各プラットフォームで次のようになります。

 <a name="iOS" />

### <a name="ios"></a>iOS

アプリケーションでは、[追加] ボタンをナビゲーションバーに配置し、組み込みの**正符号 (+)** アイコンを使用するなど、iOS 標準のユーザーインターフェイスデザインを使用します。 また、既定`UINavigationController`の [戻る] ボタンの動作も使用され、テーブル内の "スワイプして削除" をサポートしています。

 [![](case-study-tasky-images/ios-taskylist.png "また既定 UINavigationController [戻る] ボタンの動作をサポートしてスワイプ-削除の表に")](case-study-tasky-images/ios-taskylist.png#lightbox) [![](case-study-tasky-images/ios-taskylist.png "既定 UINavigationController でも使用ボタンの動作をバックアップし、テーブルにスワイプして、削除をサポートしています")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

### <a name="android"></a>Android

Android アプリでは、' ティック ' を必要とする行に組み込みのレイアウトを含む組み込みのコントロールを使用します。 ハードウェア/システムのバック動作は、画面上に戻るボタンに加えてサポートされています。

 [![](case-study-tasky-images/android-taskylist.png "ハードウェア/システムのバック動作は、画面")](case-study-tasky-images/android-taskylist.png#lightbox)[![]上に戻るボタンに加えて、(case-study-tasky-images/android-taskylist.png "ハードウェア/システムのバック動作が")サポートされています。](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

### <a name="windows-phone"></a>Windows Phone

Windows Phone アプリでは、標準レイアウトを使用して、上部にナビゲーションバーではなく、画面の下部にアプリバーを設定します。

 [![](case-study-tasky-images/wp-taskylist.png "Windows Phone アプリで使用する標準のレイアウトでは、上部のナビゲーション バーではなく、画面の下部にあるアプリ バーを設定する")](case-study-tasky-images/wp-taskylist.png#lightbox) [![](case-study-tasky-images/wp-taskylist.png "Windows Phone アプリ標準を使用します。レイアウト、上部のナビゲーション バーではなく、画面の下部にあるアプリ バーを設定します。")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>まとめ

このドキュメントでは、3つのモバイルプラットフォーム (iOS、Android、Windows Phone) でコードの再利用を容易にするために、階層型アプリケーションの設計の原則が簡単なアプリケーションにどのように適用されているかについて詳しく説明しました。

ここでは、アプリケーション層の設計に使用されるプロセスについ&amp;て説明し、各レイヤーに実装されているコード機能について説明しました。

このコードは[github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)からダウンロードできます。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォームアプリケーションのビルド (メインドキュメント)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky ポータブルサンプルアプリ (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
