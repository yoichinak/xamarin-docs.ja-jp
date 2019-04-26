---
title: クロス プラットフォーム アプリのケース スタディ:Tasky
description: このドキュメントでは、Tasky Portable サンプル アプリケーションを設計およびクロス プラットフォーム モバイル アプリケーションとして構築する方法について説明します。 これは、アプリの要件、インターフェイス、データ モデル、コア機能、実装、および詳細について説明します。
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 23deddae61452d532a87c51cc1ec3bc53eb91c9f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61279119"
---
# <a name="cross-platform-app-case-study-tasky"></a>クロス プラットフォーム アプリのケース スタディ:Tasky

*Tasky* *ポータブル*簡単な to do リスト アプリケーションします。 このドキュメントは、設計および構築、のガイダンスに従ってその方法について説明します、[クロス プラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)ドキュメント。 次の領域について説明します。

<a name="Design_Process" />

## <a name="design-process"></a>設計プロセス

作成することをお勧め、コーディングを開始する前に実現するために必要なもののロードマップの。 これは、複数の方法で公開される機能を構築している、クロス プラットフォーム開発用に特に当てはまります。 方はビルド時間を節約し、労力、開発サイクルの後半の明確な考えを開始しています。

 <a name="Requirements" />

### <a name="requirements"></a>必要条件

アプリケーションの設計に最初の手順では、目的の機能を特定します。 高度な目標はまたはユース ケースを詳しく説明します。 Tasky 簡単な機能の要件があります。

 -  タスクの一覧を表示します。
 -  追加、編集、およびタスクを削除します。
 -  タスクの状態を設定する 'done'

プラットフォーム固有の機能の使用を検討する必要があります。  Tasky を利用して iOS geofencing 機能または Windows Phone のライブ タイルのでしょうか。 最初のバージョンでプラットフォーム固有の機能を使用しない場合でも、ビジネスおよびデータ層に対応できることを確認する事前を計画する必要があります。

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>ユーザー インターフェイス デザイン

ターゲット プラットフォームで実装できる高度なデザインを起動します。 注プラットフォーム固有の UI の制約に対処します。 たとえば、 `TabBarController` iOS でボタンを表示できる 5 つを超える、一方、Windows Phone と同等には、4 つ表示できます。
(用紙が)、任意のツールを使用して画面のフローを描画します。

 [![](case-study-tasky-images/taskydesign.png "選択した用紙がツールを使用して画面のフローの描画します。")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>データ モデル

どのようなデータを格納する必要がありますを知ることを使用する永続化メカニズムの決定に役立ちます。 参照してください[クロスプラット フォームでデータ アクセス](~/cross-platform/app-fundamentals/index.md)については、使用可能記憶域メカニズムとそれらの間を決定します。 このプロジェクトで使用する SQLite.NET をします。

Tasky、各 'TaskItem' の 3 つのプロパティを格納する必要があります。

 -  **名前**– 文字列
 -  **ノート**– 文字列
 -  **完了**– ブール値

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>中心的機能

ユーザー インターフェイスは、要件を満たすために使用する必要がある API を検討してください。 To do リストには、次の関数が必要です。

 -   **すべてのタスクを一覧表示**– すべての利用可能なタスクのメイン画面の一覧を表示
 -  **1 つのタスクを取得**– タスク行をタッチすると
 -  **1 つのタスクを保存**タスクを編集する際に –
 -  **1 つのタスクを削除**– タスクを削除します。
 -  **空のタスクを作成する**– 新しいタスクが作成されたとき

コードの再利用を実現するためにこの API に実装して、1 回、*ポータブル クラス ライブラリ*します。

 <a name="Implementation" />

### <a name="implementation"></a>実装

アプリケーションの設計は、合意されている、クロス プラットフォーム アプリケーションとしての実装方法がありますを検討してください。 これには、アプリケーションのアーキテクチャになります。 あるガイダンスに従って、[クロス プラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)ドキュメント、アプリケーション コードは壊れていることを次の部分にダウン。

 -   **共通コード**– タスクのデータを保存するための再利用可能なコードが含まれています。 モデル クラスと保存を管理するための API を公開する一般的なプロジェクトとデータの読み込み。
 -   **プラットフォーム固有コード**– 'バックエンド' として一般的なコードを使用して、各オペレーティング システム用のネイティブ UI を実装するプラットフォームに固有のプロジェクト。

 [![](case-study-tasky-images/taskypro-architecture.png "プラットフォーム固有のプロジェクトをバックエンドとして一般的なコードを使用して、各オペレーティング システム用のネイティブ UI を実装します。")](case-study-tasky-images/taskypro-architecture.png#lightbox)

これら 2 つの部分は、次のセクションで説明します。

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>一般的な (PCL) コード

Tasky ポータブルでは、一般的なコードを共有するため、ポータブル クラス ライブラリの戦略を使用します。 参照してください、[コードの共有オプション](~/cross-platform/app-fundamentals/code-sharing.md)コード共有オプションの説明のドキュメント。

データ アクセス層、データベース コード、およびコントラクトを含むすべての一般的なコードは、ライブラリ プロジェクトに配置されます。

完全な PCL プロジェクトを次に示します。 すべてのポータブル ライブラリでコードが各対象プラットフォームと互換性のあります。 展開すると、各ネイティブ アプリはそのライブラリを参照します。

![](case-study-tasky-images/portable-project.png "各ネイティブ アプリがそのライブラリを参照が展開されると、")

次のクラス図では、レイヤーでグループ化されたクラスを示します。 `SQLiteConnection`クラスは、Sqlite NET パッケージからの定型コードです。 クラスの残りの部分は、Tasky 用のカスタム コードです。 `TaskItemManager`と`TaskItem`クラスは、プラットフォーム固有のアプリケーションに公開されている API を表します。

 [![](case-study-tasky-images/classdiagram-core.png "プラットフォーム固有のアプリケーションに公開されている API を表す、TaskItemManager および TaskItem クラス")](case-study-tasky-images/classdiagram-core.png#lightbox)

レイヤーを分離する名前空間の使用は、各レイヤーの間の参照の管理に役立ちます。 プラットフォーム固有のプロジェクトは、だけを含める必要があります、`using`ビジネス層のステートメント。 によって公開される API では、データ アクセス層とデータ層をカプセル化する必要があります`TaskItemManager`ビジネス層でします。

 <a name="References" />

### <a name="references"></a>関連項目

ポータブル クラス ライブラリは、それぞれのプラットフォームとフレームワークの機能のサポートのさまざまなレベルに複数のプラットフォームで使用できるようにする必要があります。 そのためは、パッケージとフレームワーク ライブラリを使用することができますを制限があります。 たとえば、Xamarin.iOS はサポートされませんが、c#`dynamic`キーワード、ため場合でも、このようなコードは、Android では機能、ポータブル クラス ライブラリは、動的なコードに依存するすべてのパッケージを使用できません。 Visual Studio for Mac で互換性のないパッケージや参照の追加を防止しますが、後で予想外の問題を回避するために考慮する制限します。

メモ:プロジェクトが使用されていないフレームワーク ライブラリを参照することを確認します。 この参照は、Xamarin プロジェクト テンプレートの一部として含まれています。 リンク プロセスが、参照されていないコードをあって削除はアプリがコンパイルされると、`System.Xml`されましたが、参照、されません、最終的なアプリケーションで任意の Xml の関数を使用していないためです。

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>データ層 (DL)

データ層には、データの物理記憶域をデータベース、フラット ファイル、またはその他のメカニズムにはコードが含まれています。 Tasky データ層は、2 つの部分で構成されています。 SQLite NET ライブラリと、カスタム コードのように追加します。

Tasky 依存 (Frank Kreuger によって発行された) Sqlite net nuget パッケージをデータベースのオブジェクト リレーショナル マッピング (ORM) インターフェイスを提供する SQLite NET コードを埋め込みます。 `TaskItemDatabase`クラスから継承`SQLiteConnection`し、必要な作成、読み取り、更新、SQLite にデータを読み書きするメソッドを削除 (CRUD) を追加します。 他のプロジェクトで再利用可能性があるジェネリックの CRUD メソッドの単純な定型実装です。

`TaskItemDatabase`シングルトン、同じインスタンスに対してすべてのアクセスが発生することを確認します。 ロックは、複数のスレッドから同時実行のアクセスを防ぐために使用されます。

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>Windows Phone での SQLite

IOS と Android が付属していますオペレーティング システムの一部としての SQLite の使用中に Windows Phone では、互換性のあるデータベース エンジンは含まれません。 3 つすべてのプラットフォームでコードを共有するには、Windows Phone のネイティブ バージョンの SQLite が必要です。 参照してください[ローカル Database](~/xamarin-forms/app-fundamentals/databases.md) Sqlite for Windows Phone プロジェクトの設定の詳細について。

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>データ アクセスを一般化するインターフェイスを使用します。

データ層には、依存関係`BL.Contracts.IBusinessIdentity`主キーを必要とする抽象データ アクセス メソッドを実装できるようにします。 インターフェイスを実装する任意のビジネス層のクラスは、データ層で保持できます。

インターフェイスには、主キーとして機能する整数のプロパティだけを指定します。

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

基本クラスは、インターフェイスを実装し、自動インクリメントの主キーとしてマークを付けるに SQLite NET 属性を追加します。 この基本クラスを実装するビジネス層内のクラスは、データ層で保持できます。

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

インターフェイスを使用して、データ層で、ジェネリック メソッドの例は、この`GetItem<T>`メソッド。

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>同時アクセスを防ぐためにロック

A[ロック](https://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx)内に実装されて、`TaskItemDatabase`クラス、データベースに同時アクセスできないようにします。 これは異なるスレッドから同時実行のアクセスはシリアル化することを確認する (それ以外の場合 UI コンポーネントからバック グラウンド スレッドで更新が同時に、データベースの読み取りを試みることもあります)。 ロックを実装する方法の例を次に示します。

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

データ層のコードの大部分は、他のプロジェクトで再使用できます。 レイヤーにのみアプリケーション固有のコードは、`CreateTable<TaskItem>`呼び出し、`TaskItemDatabase`コンス トラクター。

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>データ アクセス層 (DAL)

`TaskItemRepository`クラスにカプセル化できる、厳密に型指定された API を使用したデータ ストレージ機構`TaskItem`オブジェクトを作成するには、削除、取得および更新します。

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>条件付きコンパイルを使用します。

クラスでは、条件付きコンパイルを使用して、ファイルの場所を設定する - これは、プラットフォームの相違を実装する例。 パスを返すプロパティは、各プラットフォームでさまざまなコードをコンパイルします。 コードとプラットフォーム固有のコンパイラ ディレクティブが表示されます。

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

プラットフォームによって出力されます。"<app
path>/Library/TaskDB.db3"、iOS の"<app
path>/Documents/TaskDB.db3"だけ"TaskDB.db3"Windows Phone の Android 用。

### <a name="business-layer-bl"></a>ビジネス層 (BL)

ビジネス層では、モデル クラスとそれらを管理するファサードを実装します。
Tasky モデルは、`TaskItem`クラスと`TaskItemManager`を管理するための API を提供するファサード パターンを実装`TaskItems`します。

 <a name="Façade" />

#### <a name="faade"></a>ファサード

 `TaskItemManager` ラップ、 `DAL.TaskItemRepository` Get を提供する保存し、アプリケーション層と UI 層によって参照されるメソッドを削除します。

ビジネス ルールとロジックは、ここで – オブジェクトが保存される前に満たす必要がある検証ルールなどが必要な場合。

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>プラットフォーム固有のコード用の API

一般的なコードを記述すると、収集し、それによって公開されるデータを表示するユーザー インターフェイスを構築する必要があります。 `TaskItemManager`クラスへのアクセスに、アプリケーション コードの単純な API を提供するファサード パターンを実装します。

各プラットフォーム固有プロジェクトで記述されたコードは、デバイスのネイティブ SDK を一般に、密に結合する、およびによって定義された API を使用して、一般的なコードにのみアクセス、`TaskItemManager`します。 メソッドが含まれます、ビジネスなどのクラスが公開、`TaskItem`します。

イメージがないプラットフォームで共有しますが、個別に各プロジェクトに追加されません。 これは、機能は、各プラットフォーム イメージの処理が異なる、別のファイル名、ディレクトリ、および解決策を使用しているため重要です。

残りのセクションでは、Tasky UI のプラットフォームに固有の実装の詳細について説明します。

 <a name="iOS_App" />

## <a name="ios-app"></a>iOS アプリ

ごく少数の iOS の一般的な PCL プロジェクトを使用してデータを格納および取得 Tasky アプリケーションを実装するために必要なクラスがあります。 完全な iOS の Xamarin.iOS プロジェクトは、以下に示します。

 ![](case-study-tasky-images/taskyios-solution.png "iOS プロジェクトが次に示します")

この図は、レイヤーにグループ化では、クラスが表示されます。

 [![](case-study-tasky-images/classdiagram-android.png "この図は、レイヤーにグループ化では、クラスが表示されます。")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>関連項目

IOS アプリでは、次のようなプラットフォーム固有の SDK ライブラリ – を参照します。 Xamarin.iOS および MonoTouch.Dialog-1。

これを参照する必要がありますも、 `TaskyPortableLibrary` PCL プロジェクト。
参照の一覧を次に示します。

 ![](case-study-tasky-images/taskyios-references.png "参照の一覧が表示されます。")

アプリケーション層とユーザー インターフェイス層は、これらの参照を使用してこのプロジェクトで実装されます。

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>アプリケーション層 (AL)

アプリケーション層には、PCL に UI によって公開されているオブジェクトをバインドするために必要なプラットフォーム固有のクラスが含まれています。 IOS に固有のアプリケーションでは、タスクを表示するための 2 つのクラスがあります。

 -   **EditingSource** – タスクの一覧をユーザー インターフェイスにバインドするこのクラスを使用します。 `MonoTouch.Dialog`使用されたタスクの一覧で、必要がありますでスワイプ-削除機能を有効にするには、このヘルパーを実装するために、`UITableView`します。 スワイプ-削除が iOS の特定のプロジェクトはそれを実装する 1 つだけで、iOS、Android やが Windows Phone では、一般的です。
 -   **ダイアログ ・** – 1 つのタスクを UI にバインドするこのクラスを使用します。 使用して、 `MonoTouch.Dialog` 'を 'ラップするリフレクション API、`TaskItem`正しく書式設定する入力画面を許可する適切な属性を格納するクラスを含むオブジェクト。

`TaskDialog`クラスで使用`MonoTouch.Dialog`クラスのプロパティに基づいて、画面を作成する属性。 クラスのようになります。

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

注意してください、`OnTap`属性がメソッド名が必要です: これらのメソッドがクラス内に存在する必要があります、`MonoTouch.Dialog.BindingContext`が作成されます (ここで、`HomeScreen`クラスは、次のセクションで説明)。

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>ユーザー インターフェイス層 (UI)

ユーザー インターフェイス レイヤーは、次のクラスで構成されます。

1.   **AppDelegate** – アプリケーションで使用する色とフォント スタイルを設定する外観 API への呼び出しが含まれています。 Tasky は単純なアプリケーションで実行されている他の初期化タスクがないように`FinishedLaunching`します。
2.   **画面**– のサブクラス`UIViewController`各画面とその動作を定義します。 画面が結び付けると、アプリケーション層のクラスと UI と共通の API ( `TaskItemManager` )。 コードでは、この例では、画面の作成が、デザインした Xcode の Interface Builder またはストーリー ボード デザイナーを使用します。
3.   **イメージ**– ビジュアル要素はすべてのアプリケーションの重要な部分です。 Tasky がスプラッシュ スクリーンとアイコン イメージ、iOS の標準モードと Retina 解像度で指定する必要があります。

 <a name="Home_Screen" />

#### <a name="home-screen"></a>ホーム画面

ホーム画面では、 `MonoTouch.Dialog` SQLite データベースからのタスクの一覧を表示する画面。 継承`DialogViewController`を設定するコードを実装し、`Root`のコレクションを格納する`TaskItem`表示するオブジェクト。

 [![](case-study-tasky-images/ios-taskylist.png "DialogViewController から継承し、表示用 TaskItem オブジェクトのコレクションを格納するルートを設定するコードを実装します。")](case-study-tasky-images/ios-taskylist.png#lightbox)

タスク一覧の表示とインタラクションに関連する 2 つの主な方法は次のとおりです。

1.   **PopulateTable** – はビジネス層の使用して`TaskManager.GetTasks`のコレクションを取得するメソッドを`TaskItem`を表示するオブジェクト。
2.   **選択した**–、行にアクセスすると、新しい画面にタスクが表示されます。

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>タスクの詳細 画面

タスクの詳細は、編集または削除するタスクを許可する入力画面です。

Tasky 使用`MonoTouch.Dialog`の画面を表示するリフレクション API がない`UIViewController`実装します。 代わりに、`HomeScreen`クラスをインスタンス化し、表示、`DialogViewController`を使用して、`TaskDialog`アプリケーション レイヤーからのクラス。

このスクリーン ショットは、空の画面を示す、`Entry`属性でのウォーターマーク テキストの設定、**名前**と**ノート**フィールド。

 [![](case-study-tasky-images/ios-taskydetail.png "このスクリーン ショットは、エントリ属性の名とメモ フィールドでのウォーターマーク テキストの設定を示す空の画面を示しています。")](case-study-tasky-images/ios-taskydetail.png#lightbox)

機能、**タスクの詳細**で画面 (保存、タスクの削除など) を実装する必要があります、`HomeScreen`クラス、このような場合があるため、`MonoTouch.Dialog.BindingContext`が作成されます。 次`HomeScreen`メソッドは、タスクの詳細画面をサポートします。

1.   **ShowTaskDetails** – 作成、`MonoTouch.Dialog.BindingContext`画面を表示するためにします。 リフレクションを使用して、プロパティ名を取得する入力画面を作成し、型、`TaskDialog`クラス。 ウォーターマーク テキストの入力ボックスなどの追加情報は、プロパティの属性で実装されます。
2.   **SaveTask** – でこのメソッドが参照されている、`TaskDialog`クラスを使用して、`OnTap`属性。 呼び出されたときに**保存**を押すと、使用して、`MonoTouch.Dialog.BindingContext`を使用して変更を保存する前にユーザーが入力したデータを取得する`TaskItemManager`します。
3.   **DeleteTask** – でこのメソッドが参照されている、`TaskDialog`クラスを使用して、`OnTap`属性。 使用して`TaskItemManager`主キー (ID プロパティ) を使用してデータを削除します。

 <a name="Android_App" />

## <a name="android-app"></a>Android アプリ

完全な Xamarin.Android プロジェクトは、次に示します。

 ![](case-study-tasky-images/taskyandroid-solution.png "Android プロジェクトの図に示します")

クラス図、レイヤーでグループ化されたクラスを使用:

 [![](case-study-tasky-images/classdiagram-android.png "クラス図のレイヤーでグループ化されたクラス")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>関連項目

Android アプリ プロジェクトは、Android SDK のクラスにアクセスするプラットフォーム固有 Xamarin.Android アセンブリを参照する必要があります。

(例: PCL プロジェクトを参照、ことも必要があります。 TaskyPortableLibrary) をクリックし、一般的なデータとビジネス層のコードにアクセスします。

 ![](case-study-tasky-images/taskyandroid-references.png "一般的なデータとビジネス層のコードにアクセスする TaskyPortableLibrary")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>アプリケーション層 (AL)

これまで見てきた iOS バージョンのような以前では、Android のバージョンのアプリケーション層にが含まれています、ui のコアで公開されているオブジェクトをバインドするために必要なプラットフォーム固有のクラスには。

 **TaskListAdapter** – 一覧を表示する<T>のオブジェクトでのカスタム オブジェクトを表示するためのアダプターを実装する必要があります、`ListView`します。 リスト内の各項目のレイアウトが使用されるアダプターの制御 – Android の組み込みレイアウトを使用してこのケースで`SimpleListItemChecked`します。

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>ユーザー インターフェイス (UI)

Android アプリのユーザー インターフェイス層は、コードと XML マークアップの組み合わせです。

 -   **リソース/レイアウト**– 画面のレイアウト、および行のセル デザイン AXML ファイルとして実装します。 手動で、記述またはレイアウトを視覚的に、Android 用 Xamarin UI デザイナーを使用して、AXML ができます。
 -   **リソース/ディスプレイ**– イメージ (アイコン) とカスタム ボタンをクリックします。
 -   **画面**– 各画面とその動作を定義するアクティビティ サブクラスです。 アプリケーション層のクラスと UI と共通の API を結びつける (`TaskItemManager`)。

 <a name="Home_Screen" />

#### <a name="home-screen"></a>ホーム画面

アクティビティのサブクラスをホーム画面から成る`HomeScreen`、`HomeScreen.axml`レイアウト (ボタンとタスク リストの位置) を定義するファイル。 画面のようになります。

 [![](case-study-tasky-images/android-taskylist.png "次のような画面")](case-study-tasky-images/android-taskylist.png#lightbox)

ホーム画面のコードは、ボタンをクリックし、リスト内の項目をクリックしてだけでなくでリストを生成するのハンドラーを定義、`OnResume`メソッド (そのため、タスクの詳細画面で行われた変更を反映)。 ビジネス層を使用してデータが読み込まれる`TaskItemManager`と`TaskListAdapter`アプリケーション レイヤーから。

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>タスクの詳細 画面

タスクの詳細 画面から成るも、`Activity`サブクラスと AXML レイアウト ファイルです。 レイアウトは、入力コントロールの場所を決定し、C#クラスを読み込んで保存動作を定義します`TaskItem`オブジェクト。

 [![](case-study-tasky-images/android-taskydetail.png "クラスを読み込んで保存 TaskItem オブジェクトの動作を定義します。")](case-study-tasky-images/android-taskydetail.png#lightbox)

PCL ライブラリへのすべての参照は、`TaskItemManager`クラス。

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Windows Phone アプリ
完全な Windows Phone プロジェクト。

 ![](case-study-tasky-images/taskywp7-solution.png "Windows Phone アプリ、完全な Windows Phone プロジェクト")

次の図には、レイヤーにグループ化するクラスが表示されます。

 [![](case-study-tasky-images/classdiagram-wp7.png "この図は、レイヤーにグループ化するクラスを表示します。")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>関連項目

プラットフォーム固有のプロジェクトは、必要なプラットフォーム固有のライブラリを参照する必要があります (など`Microsoft.Phone`と`System.Windows`) 有効な Windows Phone アプリケーションを作成します。

(例: PCL プロジェクトを参照、ことも必要があります。 `TaskyPortableLibrary`) を利用する、`TaskItem`クラスとデータベースです。

 ![](case-study-tasky-images/taskywp7-references.png "TaskyPortableLibrary TaskItem クラスとデータベースを利用するには")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>アプリケーション層 (AL)

ここでも、iOS と Android のバージョンと同様、アプリケーション層がのユーザー インターフェイスにデータをバインドするための非ビジュアル要素で構成されます。

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

Viewmodel が、PCL からデータをラップ ( `TaskItemManager`) し、Silverlight と XAML データ バインディングで使用できる方法で表示します。 これは、(クロス プラットフォーム アプリケーションのドキュメントで説明した) ようにプラットフォーム固有の動作の例です。

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>ユーザー インターフェイス (UI)

XAML は、マークアップで宣言することができますし、オブジェクトを表示するために必要なコードの量を減らす、固有のデータ バインディング機能があります。

1.   **ページ**– XAML ファイルとその分離コード、ユーザー インターフェイスを定義し、表示し、データを収集するには、ビューモデルと PCL プロジェクトを参照します。
2.   **イメージ**– スプラッシュ スクリーン、背景色、アイコンのイメージは、ユーザー インターフェイスの重要な要素です。

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

MainPage クラスの使用、 `TaskListViewModel` XAML のデータ バインディング機能を使用してデータを表示します。 ページの`DataContext`が非同期的に設定されているビュー モデルに設定されます。 `{Binding}` XAML の構文は、データの表示方法を決定します。

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>TaskDetailsPage

各タスクは、バインドによって表示される、 `TaskViewModel` TaskDetailsPage.xaml で定義されている XAML にします。 タスクのデータの取得を使用して、`TaskItemManager`ビジネス層でします。

 <a name="Results" />

## <a name="results"></a>結果

結果として得られるアプリケーションは、各プラットフォームで次のようになります。

 <a name="iOS" />

#### <a name="ios"></a>iOS

アプリケーションの追加 ボタンなどの iOS の標準のユーザー インターフェイスのデザインを使用して、ナビゲーション バーに配置されていると、組み込みの**プラス (+)** アイコン。 また、既定値を使用して`UINavigationController`[戻る] ボタンの動作と、テーブルの ' スワイプ delete' をサポートしています。

 [![](case-study-tasky-images/ios-taskylist.png "また既定 UINavigationController [戻る] ボタンの動作をサポートしてスワイプ-削除の表に")](case-study-tasky-images/ios-taskylist.png#lightbox) [![](case-study-tasky-images/ios-taskylist.png "既定 UINavigationController でも使用ボタンの動作をバックアップし、テーブルにスワイプして、削除をサポートしています")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

#### <a name="android"></a>Android

Android アプリでは、組み込みレイアウトの表示 'ティック' が必要な行を含む組み込みのコントロールを使用します。 ハードウェア/システムのバックの動作は、画面に表示される [戻る] ボタンだけでなくでサポートされています。

 [![](case-study-tasky-images/android-taskylist.png "ハードウェア/システムのバックの動作が、画面に表示される [戻る] ボタンだけでなくサポートされている")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "に加え、ハードウェア/システムのバックの動作がサポートされている、画面に表示されます。[戻る] ボタン")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

#### <a name="windows-phone"></a>Windows Phone

Windows Phone アプリでは、上部のナビゲーション バーではなく、画面の下部にあるアプリ バーの設定、標準的なレイアウトを使用します。

 [![](case-study-tasky-images/wp-taskylist.png "Windows Phone アプリで使用する標準のレイアウトでは、上部のナビゲーション バーではなく、画面の下部にあるアプリ バーを設定する")](case-study-tasky-images/wp-taskylist.png#lightbox) [![](case-study-tasky-images/wp-taskylist.png "Windows Phone アプリ標準を使用します。レイアウト、上部のナビゲーション バーではなく、画面の下部にあるアプリ バーを設定します。")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>まとめ

階層型アプリケーションの設計の原則を次の 3 つのモバイル プラットフォーム間でコードの再利用を容易にする単純なアプリケーションに適用されている方法の詳細についてはこのドキュメントを提供します。 iOS、Android、Windows Phone です。

アプリケーション層をデザインするために使用するプロセスを説明し、どのようなコードの説明と&amp;機能は、各レイヤーに実装されています。

コードをからダウンロードできます[github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)します。

## <a name="related-links"></a>関連リンク

- [クロス プラットフォーム アプリケーションの構築 (メイン文書)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky ポータブル サンプル アプリ (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
