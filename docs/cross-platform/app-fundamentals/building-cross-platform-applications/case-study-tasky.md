---
title: 'クロスプラットフォームアプリのケーススタディ: Tasky'
description: このドキュメントでは、Tasky ポータブルサンプルアプリケーションをクロスプラットフォームモバイルアプリケーションとして設計および構築する方法について説明します。 アプリの要件、インターフェイス、データモデル、コア機能、実装などについて説明します。
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 803b42cfcc27dc86b0d4bc78fc4745af5565e8cb
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457550"
---
# <a name="cross-platform-app-case-study-tasky"></a>クロスプラットフォームアプリのケーススタディ: Tasky

*Tasky* *ポータブル* は、単純な to do list アプリケーションです。 このドキュメントでは、 [クロスプラットフォームアプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) に関するドキュメントのガイダンスに従って、設計および構築された方法について説明します。 ここでは、次の領域について説明します。

<a name="Design_Process"></a>

## <a name="design-process"></a>設計プロセス

コーディングを開始する前に、目的に応じて、道路マップのを作成することをお勧めします。 これは、複数の方法で公開される機能を構築するクロスプラットフォームの開発に特に当てはまります。 構築している内容を明確に理解しておくと、開発サイクルの後半で時間と労力を節約できます。

 <a name="Requirements"></a>

### <a name="requirements"></a>必要条件

アプリケーションを設計するための最初の手順は、目的の機能を特定することです。 これには、高レベルの目標や詳細なユースケースがあります。 Tasky には、簡単な機能要件があります。

- タスクの一覧を表示する
- タスクの追加、編集、削除
- タスクの状態を [完了] に設定します

プラットフォーム固有の機能の使用を検討する必要があります。  Tasky では、iOS ジオフェンシングまたは Windows Phone ライブタイルを利用できますか。 最初のバージョンでプラットフォーム固有の機能を使用しない場合でも、ビジネス & データレイヤーに対応できるかどうかを事前に計画しておく必要があります。

 <a name="User_Interface_Design"></a>

### <a name="user-interface-design"></a>ユーザー インターフェイス デザイン

まず、ターゲットプラットフォーム全体に実装できる高レベルの設計を使用します。 プラットフォーム特定の UI 制約に注意してください。 たとえば、 `TabBarController` iOS のは5つ以上のボタンを表示できますが、同等の Windows Phone には最大4つのボタンを表示できます。
任意のツールを使用して画面フローを描画します (paper works)。

 [![選択した用紙のツールを使用して画面フローを描画する](case-study-tasky-images/taskydesign.png)](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model"></a>

### <a name="data-model"></a>データ モデル

どのようなデータを格納する必要があるかを把握することは、どの永続化メカニズムを使用するかを決定するのに役立ちます。 使用可能なストレージメカニズムの詳細については、「 [クロスプラットフォームデータアクセス](~/cross-platform/app-fundamentals/index.md) 」を参照してください。 このプロジェクトでは、SQLite.NET を使用します。

Tasky では、' TaskItem ' ごとに3つのプロパティを格納する必要があります。

- **Name** – 文字列
- **メモ** –文字列
- **完了** –ブール値

 <a name="Core_Functionality"></a>

### <a name="core-functionality"></a>中心的機能

ユーザーインターフェイスが要件を満たすために使用する必要がある API を検討します。 To do リストには、次の関数が必要です。

- **すべてのタスクを一覧** 表示する–使用可能なすべてのタスクのメイン画面の一覧を表示します。
- **1 つのタスクを取得** する–タスク行にタッチするとき
- **1 つ** のタスクを保存する–タスクが編集されたとき
- **1 つ** のタスクを削除する–タスクが削除されたとき
- **空のタスクの作成** –新しいタスクが作成されたとき

コードの再利用を実現するには、この API を *ポータブルクラスライブラリ*に1回実装する必要があります。

 <a name="Implementation"></a>

### <a name="implementation"></a>実装

アプリケーションの設計に同意したら、それがクロスプラットフォームアプリケーションとしてどのように実装されるかを検討します。 これがアプリケーションのアーキテクチャになります。 [クロスプラットフォームアプリケーションのビルド](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)に関するドキュメントのガイダンスに従って、アプリケーションコードを次の部分に分割する必要があります。

- **共通コード** -タスクデータを格納するための再有効コードを含む共通のプロジェクトです。モデルクラスと API を公開して、データの保存と読み込みを管理します。
- **プラットフォーム固有のコード** – "バックエンド" として共通のコードを使用して、各オペレーティングシステムのネイティブ UI を実装するプラットフォーム固有のプロジェクト。

[![プラットフォーム固有のプロジェクトは、一般的なコードをバックエンドとして使用して、各オペレーティングシステムのネイティブ UI を実装します。](case-study-tasky-images/taskypro-architecture.png)](case-study-tasky-images/taskypro-architecture.png#lightbox)

以下のセクションでは、これらの2つの部分について説明します。

 <a name="Common_(PCL)_Code"></a>

## <a name="common-pcl-code"></a>共通 (PCL) コード

Tasky ポータブルは、共通コードを共有するためのポータブルクラスライブラリ戦略を使用します。 コード共有のオプションの説明については、 [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md) に関するドキュメントを参照してください。

データアクセス層、データベースコード、およびコントラクトを含むすべての共通コードは、ライブラリプロジェクトに配置されます。

次に、完全な PCL プロジェクトを示します。 ポータブルライブラリ内のすべてのコードは、対象となる各プラットフォームと互換性があります。 デプロイされると、各ネイティブアプリはそのライブラリを参照します。

![デプロイされると、各ネイティブアプリはそのライブラリを参照します。](case-study-tasky-images/portable-project.png)

次のクラス図は、レイヤーごとにグループ化されたクラスを示しています。 クラスは、 `SQLiteConnection` Sqlite-NET パッケージの定型コードです。 クラスの残りの部分は Tasky のカスタムコードです。 `TaskItemManager`クラスと `TaskItem` クラスは、プラットフォーム固有のアプリケーションに公開されている API を表します。

 [![TaskItemManager クラスと TaskItem クラスは、プラットフォーム固有のアプリケーションに公開されている API を表します。](case-study-tasky-images/classdiagram-core.png)](case-study-tasky-images/classdiagram-core.png#lightbox)

名前空間を使用してレイヤーを分離すると、各レイヤー間の参照を管理するのに役立ちます。 プラットフォーム固有のプロジェクトでは、ビジネス層のステートメントのみを含める必要があり `using` ます。 データアクセス層とデータ層は、ビジネス層でによって公開される API によってカプセル化される必要があり `TaskItemManager` ます。

 <a name="References"></a>

### <a name="references"></a>リファレンス

ポータブルクラスライブラリは、プラットフォームとフレームワークの機能のサポートレベルが異なる複数のプラットフォームで使用できる必要があります。 そのため、使用できるパッケージとフレームワークライブラリには制限があります。 たとえば、Xamarin では c# のキーワードがサポートされていない `dynamic` ため、ポータブルクラスライブラリは、このようなコードが Android で動作する場合でも、動的なコードに依存するパッケージを使用することはできません。 Visual Studio for Mac では、互換性のないパッケージと参照を追加することはできませんが、後での使用を回避するために、制限事項を考慮する必要があります。

注: 使用していないフレームワークライブラリがプロジェクトによって参照されていることがわかります。 これらの参照は、Xamarin プロジェクトテンプレートの一部として含まれています。 アプリがコンパイルされると、リンクプロセスによって参照されていないコードが削除されます。そのため、が参照されている場合でも、 `System.Xml` Xml 関数を使用していないため、最終的なアプリケーションには含まれません。

 <a name="Data_Layer_(DL)"></a>

### <a name="data-layer-dl"></a>データ層 (DL)

データ層には、データベース、フラットファイル、またはその他のメカニズムのいずれであるかにかかわらず、データの物理的なストレージを行うコードが含まれています。 Tasky データレイヤーは2つの部分で構成されます。 SQLite-NET ライブラリと、それを接続するためのカスタムコードです。

Tasky は、(Frank Kによって発行された) Sqlite-net NuGet パッケージを使用して、オブジェクトリレーショナルマッピング (ORM) データベースインターフェイスを提供する SQLite NET コードを埋め込みます。 クラスはから継承され、 `TaskItemDatabase` `SQLiteConnection` 必要な Create、Read、Update、DELETE (CRUD) の各メソッドを追加して、データの読み取りと書き込みを SQLite に行います。 これは、他のプロジェクトで再利用できる汎用 CRUD メソッドの単純な定型的な実装です。

は `TaskItemDatabase` シングルトンであり、すべてのアクセスが同じインスタンスに対して行われることを保証します。 ロックは、複数のスレッドからの同時アクセスを防ぐために使用されます。

 <a name="SQLite_on_WIndows_Phone"></a>

#### <a name="sqlite-on-windows-phone"></a>Windows Phone の SQLite

IOS と Android には両方とも、オペレーティングシステムの一部として SQLite が付属していますが、Windows Phone には互換性のあるデータベースエンジンが含まれていません。 3つのすべてのプラットフォームでコードを共有するには、Windows phone ネイティブバージョンの SQLite が必要です。 Sqlite 用の Windows Phone プロジェクトを設定する方法の詳細については [、「ローカルデータベースの操作](~/xamarin-forms/data-cloud/data/databases.md) 」を参照してください。

 <a name="Using_an_Interface_to_Generalize_Data_Access"></a>

#### <a name="using-an-interface-to-generalize-data-access"></a>インターフェイスを使用したデータアクセスの一般化

データ層は、 `BL.Contracts.IBusinessIdentity` 主キーを必要とする抽象データアクセスメソッドを実装できるように、への依存関係を取得します。 その後、インターフェイスを実装するビジネス層クラスをデータ層に保持できます。

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

インターフェイスを使用するデータ層のジェネリックメソッドの例を次に示し `GetItem<T>` ます。

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access"></a>

#### <a name="locking-to-prevent-concurrent-access"></a>同時アクセスを防止するためのロック

[ロック](/previous-versions/visualstudio/visual-studio-2010/c5kehkcz(v=vs.100))は、 `TaskItemDatabase` データベースへの同時アクセスを防ぐために、クラス内に実装されます。 これは、異なるスレッドからの同時アクセスを確実にシリアル化するためです (そうしないと、バックグラウンドスレッドが更新しているときに、UI コンポーネントがデータベースの読み取りを試行することがあります)。 次に、ロックの実装方法の例を示します。

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

ほとんどのデータレイヤーコードは、他のプロジェクトで再利用できます。 レイヤー内のアプリケーション固有のコードは、コンストラクターの `CreateTable<TaskItem>` 呼び出しだけです `TaskItemDatabase` 。

 <a name="Data_Access_Layer_(DAL)"></a>

### <a name="data-access-layer-dal"></a>データ アクセス層 (DAL)

クラスは、 `TaskItemRepository` `TaskItem` オブジェクトの作成、削除、取得、および更新を可能にする厳密に型指定された API を使用して、データストレージ機構をカプセル化します。

 <a name="Using_Conditional_Compilation"></a>

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

プラットフォームによっては、iOS の場合は "db3"、 <app
path> <app
path> Android の場合は "/Windows Phone"、または "taskdb..." のように出力されます。

### <a name="business-layer-bl"></a>ビジネス層 (BL)

ビジネス層は、モデルクラスとファサードを実装してそれらを管理します。
Tasky では、モデルは `TaskItem` クラスで `TaskItemManager` あり、を管理するための API を提供するためにファサードパターンを実装し `TaskItems` ます。

 <a name="Façade"></a>

#### <a name="faade"></a>ファサード

 `TaskItemManager` をラップして、 `DAL.TaskItemRepository` アプリケーションおよび UI レイヤーによって参照される Get、Save、および Delete メソッドを提供します。

ビジネスルールとロジックは、必要に応じてここに配置されます。たとえば、オブジェクトを保存する前に満たす必要がある検証規則などです。

 <a name="API_for_Platform-Specific_Code"></a>

### <a name="api-for-platform-specific-code"></a>プラットフォーム固有のコードの API

共通コードが記述されたら、ユーザーインターフェイスを作成して、それによって公開されたデータを収集して表示する必要があります。 クラスは、 `TaskItemManager` アプリケーションコードにアクセスするための単純な API を提供するファサードパターンを実装します。

各プラットフォーム固有のプロジェクトで記述されたコードは、通常、そのデバイスのネイティブ SDK と密接に結合され、で定義されている API を使用してのみ共通コードにアクセスし `TaskItemManager` ます。 これには、などのメソッドとビジネスクラスが公開され `TaskItem` ます。

イメージはプラットフォーム間で共有されませんが、各プロジェクトに個別に追加されます。 プラットフォームごとに異なるファイル名、ディレクトリ、および解決方法を使用してイメージを処理するので、この方法が重要です。

残りのセクションでは、Tasky UI のプラットフォーム固有の実装の詳細について説明します。

 <a name="iOS_App"></a>

## <a name="ios-app"></a>iOS アプリ

データの格納と取得に共通 PCL プロジェクトを使用して iOS Tasky アプリケーションを実装するために必要なクラスはごくわずかです。 完全な iOS Xamarin. iOS プロジェクトを次に示します。

 ![iOS プロジェクトはここに表示されます](case-study-tasky-images/taskyios-solution.png)

クラスは、この図のようにレイヤーにグループ化されています。

 [![クラスは、この図のようにレイヤーにグループ化されています。](case-study-tasky-images/classdiagram-android.png)](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References"></a>

### <a name="references"></a>リファレンス

IOS アプリはプラットフォーム固有の SDK ライブラリを参照します。例: Xamarin. iOS と Monotouch.dialog-1。

PCL プロジェクトも参照する必要があり `TaskyPortableLibrary` ます。
参照の一覧を次に示します。

 ![参照の一覧を次に示します。](case-study-tasky-images/taskyios-references.png)

アプリケーションレイヤーとユーザーインターフェイスレイヤーは、これらの参照を使用して、このプロジェクトに実装されます。

 <a name="Application_Layer_(AL)"></a>

### <a name="application-layer-al"></a>アプリケーションレイヤー (AL)

アプリケーションレイヤーには、PCL によって公開されるオブジェクトを UI に "バインド" するために必要なプラットフォーム固有のクラスが含まれています。 IOS 固有のアプリケーションには、タスクの表示に役立つ2つのクラスがあります。

- **編集ソース** –このクラスは、タスクのリストをユーザーインターフェイスにバインドするために使用されます。 はタスク一覧で使用されているため、 `MonoTouch.Dialog` このヘルパーを実装して、でスワイプアンド削除機能を有効にする必要があり  `UITableView` ます。 スワイプ-delete は iOS では一般的ですが、Android や Windows Phone ではありません。そのため、iOS 固有のプロジェクトは、それを実装する唯一のものです。
- **Taskdialog** -このクラスは、1つのタスクを UI にバインドするために使用されます。 この例では、リフレクション API を使用して、正しい属性を含むクラスを使用して `MonoTouch.Dialog` オブジェクトを "ラップ" し、 `TaskItem` 入力画面を正しく書式設定できるようにします。

クラスは、 `TaskDialog` 属性を使用し `MonoTouch.Dialog` て、クラスのプロパティに基づいて画面を作成します。 クラスは次のようになります。

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

属性にはメソッド名が必要であることに注意して `OnTap` ください。これらのメソッドは、が作成されるクラス `MonoTouch.Dialog.BindingContext` (この場合は次のセクションで説明するクラス) に存在する必要があり `HomeScreen` ます。

 <a name="User_Interface_Layer_(UI)"></a>

### <a name="user-interface-layer-ui"></a>ユーザーインターフェイスレイヤー (UI)

ユーザーインターフェイスレイヤーは、次のクラスで構成されています。

1. **Appdelegate** –アプリケーションで使用されるフォントと色のスタイルを表示するための、外観 API の呼び出しが含まれています。 Tasky は単純なアプリケーションであるため、で実行されている他の初期化タスクはありません  `FinishedLaunching` 。
2. **画面–** `UIViewController` 各画面とその動作を定義するのサブクラスです。 画面は、UI とアプリケーションレイヤークラスおよび共通 API () を相互に結び付け `TaskItemManager` ます。 この例では、画面はコードで作成されますが、Xcode の Interface Builder またはストーリーボードデザイナーを使用して設計されている可能性があります。
3. **イメージ** –ビジュアル要素は、すべてのアプリケーションの重要な部分です。 Tasky にはスプラッシュスクリーンとアイコンの画像があります。このイメージは、iOS 用の標準解像度と Retina 解像度で指定する必要があります。

 <a name="Home_Screen"></a>

#### <a name="home-screen"></a>ホーム画面

ホーム画面は、 `MonoTouch.Dialog` SQLite データベースからのタスクの一覧を表示する画面です。 を継承し、コードを実装して `DialogViewController` 、表示する `Root` オブジェクトのコレクションを格納するようにを設定し `TaskItem` ます。

 [![このメソッドは、コントロールを表示し、表示するために TaskItem オブジェクトのコレクションを含むようにルートを設定するコードを実装します。](case-study-tasky-images/ios-taskylist.png)](case-study-tasky-images/ios-taskylist.png#lightbox)

タスク一覧の表示と対話に関連する2つの主要な方法を次に示します。

1. **PopulateTable** –ビジネス層のメソッドを使用して、  `TaskManager.GetTasks` 表示するオブジェクトのコレクションを取得  `TaskItem` します。
2. **選択** –行にタッチすると、によって新しい画面にタスクが表示されます。

 <a name="Task_Details_Screen"></a>

#### <a name="task-details-screen"></a>タスクの詳細画面

タスクの詳細とは、タスクの編集や削除を可能にする入力画面です。

Tasky では `MonoTouch.Dialog` 、リフレクション API を使用して画面を表示するため、実装はありません `UIViewController` 。 代わりに、クラスは、 `HomeScreen` アプリケーション層のクラスを使用してをインスタンス化して表示し `DialogViewController` `TaskDialog` ます。

このスクリーンショットは、[ `Entry` **名前** ] フィールドと [ **メモ** ] フィールドにウォーターマークテキストを設定する属性を示す空の画面を示しています。

 [![このスクリーンショットでは、"名前" フィールドと "メモ" フィールドにウォーターマークテキストを設定する Entry 属性を示す空の画面を示しています。](case-study-tasky-images/ios-taskydetail.png)](case-study-tasky-images/ios-taskydetail.png#lightbox)

**タスクの詳細**画面 (タスクの保存や削除など) の機能は、が作成される場所であるため、クラスに実装する必要があり `HomeScreen` `MonoTouch.Dialog.BindingContext` ます。 次の `HomeScreen` メソッドでは、タスクの詳細画面がサポートされています。

1. **Showtaskdetails** –画面を  `MonoTouch.Dialog.BindingContext` 表示するを作成します。 リフレクションを使用して入力画面を作成し、クラスからプロパティ名と型を取得し  `TaskDialog` ます。 入力ボックスのウォーターマークテキストなどの追加情報は、プロパティの属性を使用して実装されます。
2. **Savetask** –このメソッドは、属性を使用してクラスで参照され  `TaskDialog`  `OnTap` ます。 このメソッドは、[  **保存** ] を押したときに呼び出され、を使用して変更を保存する前に、を使用して  `MonoTouch.Dialog.BindingContext` ユーザーが入力したデータを取得し  `TaskItemManager` ます。
3. **Deletetask** –このメソッドは、属性を使用してクラスで参照され  `TaskDialog`  `OnTap` ます。 を使用し  `TaskItemManager` て、主キー (ID プロパティ) を使用してデータを削除します。

 <a name="Android_App"></a>

## <a name="android-app"></a>Android アプリ

完全な Xamarin. Android プロジェクトを次に示します。

 ![Android プロジェクトはここに示されています](case-study-tasky-images/taskyandroid-solution.png)

クラス図。クラスはレイヤーごとにグループ化されています。

 [![クラス図 (クラスをレイヤー別にグループ化)](case-study-tasky-images/classdiagram-android.png)](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References"></a>

### <a name="references"></a>リファレンス

Android アプリプロジェクトは、Android SDK からクラスにアクセスするために、プラットフォーム固有の Xamarin. Android アセンブリを参照する必要があります。

また、PCL プロジェクトを参照する必要があります (例として、 TaskyPortableLibrary) を使用して、共通のデータとビジネスレイヤーのコードにアクセスします。

 ![共通データとビジネスレイヤーコードにアクセスするための TaskyPortableLibrary](case-study-tasky-images/taskyandroid-references.png)

 <a name="Application_Layer_(AL)"></a>

### <a name="application-layer-al"></a>アプリケーションレイヤー (AL)

前に見た iOS のバージョンと同様に、Android バージョンのアプリケーションレイヤーには、コアによって公開されたオブジェクトを UI に "バインド" するために必要なプラットフォーム固有のクラスが含まれています。

 **Tasklistadapter** –オブジェクトの一覧を表示するに \<T> は、カスタムオブジェクトをに表示するアダプターを実装する必要があり `ListView` ます。 アダプターは、リスト内の各項目に使用されるレイアウトを制御します。この場合、コードは Android の組み込みレイアウトを使用し `SimpleListItemChecked` ます。

 <a name="User_Interface_(UI)"></a>

### <a name="user-interface-ui"></a>ユーザー インターフェイス (UI)

Android アプリのユーザーインターフェイスレイヤーは、コードと XML マークアップを組み合わせたものです。

- **リソース/レイアウト** –画面レイアウトと、axml ファイルとして実装された行セルのデザイン。 AXML は、手動で記述したり、Android 用の Xamarin UI デザイナーを使用して視覚的に配置したりすることができます。
- **リソース/** 描画機能: イメージ (アイコン) とカスタムボタン。
- **画面–各** 画面とその動作を定義するアクティビティサブクラスです。 UI とアプリケーションレイヤークラスおよび共通 API () を結び付け `TaskItemManager` ます。

 <a name="Home_Screen"></a>

#### <a name="home-screen"></a>ホーム画面

ホーム画面は、アクティビティサブクラス `HomeScreen` と、 `HomeScreen.axml` レイアウト (ボタンとタスク一覧の位置) を定義するファイルで構成されています。 画面は次のようになります。

 [![画面は次のようになります。](case-study-tasky-images/android-taskylist.png)](case-study-tasky-images/android-taskylist.png#lightbox)

ホーム画面のコードでは、ボタンをクリックしてリスト内の項目をクリックするためのハンドラーを定義します。また、メソッドにリストを設定する `OnResume` こともできます (タスクの詳細画面で行った変更が反映されます)。 データは、ビジネス層とアプリケーション層のを使用して読み込まれ `TaskItemManager` `TaskListAdapter` ます。

 <a name="Task_Details_Screen"></a>

#### <a name="task-details-screen"></a>タスクの詳細画面

また、[タスクの詳細] 画面は、 `Activity` サブクラスと AXML レイアウトファイルで構成されています。 レイアウトは入力コントロールの位置を決定し、C# クラスはオブジェクトを読み込んで保存する動作を定義し `TaskItem` ます。

 [![クラスは、TaskItem オブジェクトを読み込んで保存する動作を定義します。](case-study-tasky-images/android-taskydetail.png)](case-study-tasky-images/android-taskydetail.png#lightbox)

PCL ライブラリへのすべての参照は、クラスを介して行われ `TaskItemManager` ます。

 <a name="Windows_Phone_App"></a>

## <a name="windows-phone-app"></a>Windows Phone アプリ
完成した Windows Phone プロジェクト:

 ![完全な Windows Phone プロジェクトを Windows Phone アプリ](case-study-tasky-images/taskywp7-solution.png)

次の図は、レイヤーにグループ化されたクラスを示しています。

 [![この図は、レイヤーにグループ化されたクラスを示しています。](case-study-tasky-images/classdiagram-wp7.png)](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References"></a>

### <a name="references"></a>リファレンス

プラットフォーム固有のプロジェクトでは、 `Microsoft.Phone` `System.Windows` 有効な Windows Phone アプリケーションを作成するために必要なプラットフォーム固有のライブラリ (やなど) を参照する必要があります。

また、PCL プロジェクトを参照する必要があります (例として、 `TaskyPortableLibrary`) を使用し `TaskItem` て、クラスとデータベースを使用します。

 ![TaskItem クラスとデータベースを利用する TaskyPortableLibrary](case-study-tasky-images/taskywp7-references.png)

 <a name="Application_Layer_(AL)"></a>

### <a name="application-layer-al"></a>アプリケーションレイヤー (AL)

ここでも、iOS と Android のバージョンと同様に、アプリケーションレイヤーは、ユーザーインターフェイスにデータをバインドするのに役立つ非ビジュアル要素で構成されています。

 <a name="ViewModels"></a>

#### <a name="viewmodels"></a>ViewModels

Viewmodel は PCL () からデータをラップ `TaskItemManager` し、Silverlight/XAML データバインディングで使用できるようにします。 プラットフォーム固有の動作の例を次に示します (「クロスプラットフォームアプリケーション」ドキュメントをご説明します)。

 <a name="User_Interface_(UI)"></a>

### <a name="user-interface-ui"></a>ユーザー インターフェイス (UI)

XAML には、マークアップで宣言し、オブジェクトを表示するために必要なコードの量を減らすことができる、独自のデータバインディング機能があります。

1. **ページ** – XAML ファイルとその分離コードによってユーザーインターフェイスが定義され、VIEWMODEL と PCL プロジェクトを参照してデータを表示および収集します。
2. **イメージ** –スプラッシュスクリーン、背景およびアイコンのイメージは、ユーザーインターフェイスの重要な部分です。

 <a name="MainPage"></a>

#### <a name="mainpage"></a>MainPage

Mainpage.xaml クラスは、を使用し `TaskListViewModel` て、XAML のデータバインディング機能を使用してデータを表示します。 ページの `DataContext` がビューモデルに設定されます。これは非同期に設定されます。 XAML の構文によって、 `{Binding}` データの表示方法が決まります。

 <a name="TaskDetailsPage"></a>

#### <a name="taskdetailspage"></a>TaskDetailsPage

各タスクは、 `TaskViewModel` TaskDetailsPage で定義されている xaml にをバインドすることによって表示されます。 タスクデータは、ビジネス層のを通じて取得され `TaskItemManager` ます。

 <a name="Results"></a>

## <a name="results"></a>結果

結果として得られるアプリケーションは、各プラットフォームで次のようになります。

 <a name="iOS"></a>

### <a name="ios"></a>iOS

アプリケーションでは、[追加] ボタンをナビゲーションバーに配置し、組み込みの **正符号 (+)** アイコンを使用するなど、iOS 標準のユーザーインターフェイスデザインを使用します。 また、既定 `UINavigationController` の [戻る] ボタンの動作も使用され、テーブル内の "スワイプして削除" をサポートしています。

 [ ![ また、既定の UINavigationController back ボタンの動作を使用し、テーブルでのスワイプ](case-study-tasky-images/ios-taskylist.png)](case-study-tasky-images/ios-taskylist.png#lightbox)からの削除もサポートします。この場合、 [ ![ 既定の UINavigationController back ボタンの動作が使用](case-study-tasky-images/ios-taskylist.png)](case-study-tasky-images/ios-taskylist.png#lightbox)され、テーブルでのスワイプからの削除がサポートされます。

 <a name="Android"></a>

### <a name="android"></a>Android

Android アプリでは、' ティック ' を必要とする行に組み込みのレイアウトを含む組み込みのコントロールを使用します。 ハードウェア/システムのバック動作は、画面上に戻るボタンに加えてサポートされています。

 [ ![ ハードウェア/システムのバック動作は、画面](case-study-tasky-images/android-taskylist.png)](case-study-tasky-images/android-taskylist.png#lightbox)上に戻るボタンに加えて、[ ![ ハードウェア/システムのバック動作が](case-study-tasky-images/android-taskylist.png)](case-study-tasky-images/android-taskylist.png#lightbox)サポートされています。

 <a name="Windows_Phone"></a>

### <a name="windows-phone"></a>Windows Phone

Windows Phone アプリでは、標準レイアウトを使用して、上部にナビゲーションバーではなく、画面の下部にアプリバーを設定します。

 Windows Phone アプリでは、 [ ![ 標準レイアウトを使用します。画面の上部にあるナビゲーションバーではなく、画面の下部にアプリバー](case-study-tasky-images/wp-taskylist.png)](case-study-tasky-images/wp-taskylist.png#lightbox)を設定します。 [ ![ Windows Phone アプリでは標準レイアウトが使用され、画面の一番上にあるナビゲーションバーではなく、アプリバーが画面の下部に](case-study-tasky-images/wp-taskylist.png)](case-study-tasky-images/wp-taskylist.png#lightbox)設定されます。

 <a name="Summary"></a>

## <a name="summary"></a>まとめ

このドキュメントでは、3つのモバイルプラットフォーム (iOS、Android、Windows Phone) でコードの再利用を容易にするために、階層型アプリケーションの設計の原則が簡単なアプリケーションにどのように適用されているかについて詳しく説明しました。

ここでは、アプリケーション層の設計に使用されるプロセスについて説明し、 &amp; 各レイヤーに実装されているコード機能について説明しました。

このコードは [github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)からダウンロードできます。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォームアプリケーションのビルド (メインドキュメント)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky ポータブルサンプルアプリ (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)