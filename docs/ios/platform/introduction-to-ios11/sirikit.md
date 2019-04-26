---
title: IOS 11 で SiriKit の更新プログラム
description: このドキュメントでは、iOS 11 で SiriKit を使用する方法について説明します。 具体的には、タスクとノートを操作する方法と、アプリケーションの代替名を指定する方法を確認します。
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/07/2017
ms.openlocfilehash: 7e895dc2865880ec2789a40f8cdf047a20f8693b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61400301"
---
# <a name="sirikit-updates-in-ios-11"></a>IOS 11 で SiriKit の更新プログラム

SiriKit は、サービスのドメイン (ワークアウト、乗車の予約、および呼び出しを含む) の数が 10、iOS で導入されました。 参照してください、 [SiriKit セクション](~/ios/platform/sirikit/index.md)SiriKit の概念と、アプリで SiriKit を実装する方法。

![Siri のタスク一覧のデモ](sirikit-images/sirikit.png)

SiriKit iOS 11 では、これらの新規および更新されたインテント ドメインを追加します。

- [**ノートを示し、** ](#listsnotes) -新規! タスクとノートを処理するには、アプリの API を提供します。
- **ビジュアル コード**-新規! Siri が連絡先情報を共有または支払いトランザクションに参加する QR コードを表示できます。
- **支払**– 支払いの相互作用の検索と転送のインテントを追加します。
- **予約に乗る**– 追加乗り物とフィードバックのインテントをキャンセルします。

その他に次の新機能があります。

- [**別のアプリ名**](#alternativenames) – 提供エイリアスを支援する指示 Siri 名/発音の代替を提供することで、アプリの対象にします。
- **開始ワークアウト**– バック グラウンドで、トレーニングを開始する機能を提供します。

以下は、これらの機能の一部について説明します。 その他の詳細についてを参照してください[Apple の SiriKit ドキュメント](https://developer.apple.com/documentation/sirikit)します。

<a name="listsnotes" />

## <a name="lists-and-notes"></a>リストと注意事項

新しいリストおよびノートのドメインでは、タスクと Siri の音声要求を使用してノートを処理するには、アプリの API を提供します。

**タスク**

- タイトルと完了状態があります。
- 必要に応じて期限と場所が含まれます。

**ノート**

- タイトルとコンテンツのフィールドがあります。

タスクおよびノートは、グループに整理できます。 このセクションの残りの部分は、SiriKit、この新しいドメインを実装する方法を説明します。 を使用して、 [TasksNotes SiriKit 例](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)します。

### <a name="how-to-process-a-sirikit-request"></a>SiriKit の要求を処理する方法

次の手順に従って、SiriKit の要求を処理します。

1. **解決**– パラメーターを検証し、詳細情報が要求ユーザーから (必要な場合)。
2. **確認**– 最終的な検証と検証の要求を処理することができます。
3. **処理**– (データの更新またはネットワーク操作の実行) の操作を実行します。

最初の 2 つの手順は (推奨) は省略可能な最後の手順が必要とします。
詳細な手順については、 [SiriKit セクション](~/ios/platform/sirikit/index.md)します。

### <a name="resolve-and-confirm-methods"></a>解決し、メソッドを確認します。

これらのオプションのメソッドは、コードがユーザーから実行する検証、既定値の選択、または要求の追加情報を使用できます。

たとえばの`IINCreateTaskListIntent`インターフェイス、必要なメソッドは`HandleCreateTaskList`します。 Siri の対話をより細かく制御する 4 つの省略可能な方法はあります。

- `ResolveTitle` – タイトルの検証、設定の既定のタイトル (該当する場合)、またはデータが必要ないことを通知します。
- `ResolveTaskTitles` – ユーザーが話されるタスクの一覧を検証します。
- `ResolveGroupName` – 検証グループ名、既定のグループを選択またはデータが必要ないことを通知します。
- `ConfirmCreateTaskList` – 検証コードは、要求された操作を実行できますが、これを行いません (だけ、`Handle*`メソッドは、データを変更する必要があります)。

### <a name="handle-the-intent"></a>目的を処理します。

リストおよびノートのドメイン内の 6 つのインテント、タスクの 3 つ、ノートの 3 つがあります。
これらのインテントを処理するために実装する必要がありますメソッドは次のとおりです。

- タスクの場合。
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- ノート。
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

各メソッドが Siri がユーザーの要求から解析されたすべての情報を含む特定のインテント種類に、渡された (で更新される可能性があると、`Resolve*`と`Confirm*`メソッド)。
アプリは、提供されたデータを解析し、実行の一部を格納するアクションまたはそれ以外の場合、データを処理と Siri に講演を行い、ユーザーに表示される結果を返す必要があります。

### <a name="response-codes"></a>応答コード

必要な`Handle*`と省略可能な`Confirm*`メソッドは、完了ハンドラーに渡されるオブジェクトの値を設定して、応答コードを示します。 応答に由来します`INCreateTaskListIntentResponseCode`列挙体。

- `Ready` – 確認フェーズ中に返します (ie。 から、`Confirm*`メソッドがからではなく、`Handle*`メソッド)。
- `InProgress` – (ネットワーク/サーバーの操作) などの実行時間の長いタスクを使用します。
- `Success` – 成功した操作の詳細を含む応答 (からのみ、`Handle*`メソッド)。
- `Failure` – によりエラーが発生し、操作を完了できませんでした。
- `RequiringAppLaunch` – 意図的に処理ことはできませんが、操作は、アプリで使用できます。
- `Unspecified` – 使用しないでください。 エラー メッセージがユーザーに表示されます。

詳細については、これらのメソッドと apple の応答は[SiriKit のリストし、ドキュメントをノート](https://developer.apple.com/documentation/sirikit/lists_and_notes)します。

### <a name="implementing-lists-and-notes"></a>リストと注意事項を実装します。

[TasksNotes SiriKit 例](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)SiriKit のサポートを空の iOS アプリに追加する、次の手順を使用して作成されました。

最初に、SiriKit のサポートを追加するには、iOS アプリは、この手順に従います。

1. ティック**SiriKit**で**Entitlements.plist**します。
2. 追加、**プライバシー-Siri の使用方法の説明**キー **Info.plist**顧客のためのメッセージと共にします。
3. 呼び出す、 `INPreferences.RequestSiriAuthorization` Siri の対話を許可するユーザー入力を求めるアプリ内のメソッド。
4. 開発者ポータルでのアプリ ID に SiriKit を追加し、新しい権利を含める、プロビジョニング プロファイルを再作成します。

Siri 要求を処理するアプリに新しい拡張機能プロジェクトを追加します。

1. ソリューションを右クリックし、選択**追加 > 新しいプロジェクトの追加.**.
2. 選択、 **iOS > 拡張機能 > Intents の拡張機能**テンプレート。
3. 2 つの新しいプロジェクトが追加されます。目的および IntentUI します。 サンプルには、コードにはのみが含まれていますので、UI のカスタマイズは省略可能で、**インテント**プロジェクト。

拡張機能プロジェクトは、SiriKit のすべての要求を処理します。 別個の拡張機能として自動的がない – メイン アプリとの通信が、通常はアプリのグループを使用して共有ファイル ストレージを実装することで解決する方法。

#### <a name="configure-the-intenthandler"></a>IntentHandler を構成します。

`IntentHandler` Siri 要求 – すべての目的に渡されるために、クラスは、エントリ ポイント、`GetHandler`メソッドで、要求を処理できるオブジェクトを返します。

次のコードでは、単純な実装を示しています。

```csharp
[Register("IntentHandler")]
public partial class IntentHandler : INExtension, IINNotebookDomainHandling
{
  protected IntentHandler(IntPtr handle) : base(handle)
  {}
  public override NSObject GetHandler(INIntent intent)
  {
    // This is the default implementation.  If you want different objects to handle different intents,
    // you can override this and return the handler you want for that particular intent.
    return this;
  }
  // add intent handlers here!
}
```

クラスを継承する必要があります`INExtension`も実装するため、サンプルでは、リストを処理して、インテントをノート、および`IINNotebookDomainHandling`します。

> [!NOTE]
> - 大文字のプレフィックスとして指定するインターフェイスを .NET で、規則がある`I`Xamarin は iOS SDK からのプロトコルをバインドするときに準拠します。
> - Xamarin では、iOS からの型名も保持され、Apple は型名の最初の 2 つの文字を使用して、型に属しているフレームワークを反映するようにします。
> - `Intents`型には、フレームワークでプレフィックス`IN*`(例。 `INExtension`) が、_いない_インターフェイス。
> - そのプロトコルにも従うこと (インターフェイスになるこのC#) 最終的に、2 つ`I`s など`IINAddTasksIntentHandling`。

#### <a name="handling-intents"></a>インテントの処理

(タスクの追加、タスクの属性の設定など) は、各目的は、以下のような 1 つのメソッドに実装されます。 メソッドは、次の 3 つの主な機能を実行する必要があります。

1. **目的の処理**– Siri によって解析されたデータはで利用可能になって、`intent`目的の種類に固有のオブジェクト。 オプションを使用してそのデータがアプリによって検証`Resolve*`メソッド。
2. **検証およびデータ ストアを更新する**– (メイン iOS アプリではアクセスもできるように、アプリ グループを使用)、ファイル システムまたはネットワーク要求を使用してデータを保存します。
3. **応答**– 使用して、`completion`ユーザーに読み取り/表示するための Siri への応答を送信するハンドラー。

```csharp
public void HandleCreateTaskList(INCreateTaskListIntent intent, Action<INCreateTaskListIntentResponse> completion)
{
  var list = TaskList.FromIntent(intent);
  // TODO: have to create the list and tasks... in your app data store
  var response = new INCreateTaskListIntentResponse(INCreateTaskListIntentResponseCode.Success, null)
  {
    CreatedTaskList = list
  };
  completion(response);
}
```

注意`null`が渡される – 応答には、2 番目のパラメーターとして、これは、ユーザー アクティビティのパラメーターとが指定されていないときに、既定値が使用されます。
カスタム アクティビティの種類を設定するには、iOS アプリを使用してサポートしている限り、`NSUserActivityTypes`キー **Info.plist**します。 アプリが開かれたときに、このようなケースを処理し、(関連するビュー コント ローラーを開き、Siri 操作からのデータの読み込み) などの特定の操作を実行します。

例では、ハード コーティングも、`Success`結果が、実際のシナリオで適切なエラーを報告する必要があります追加します。

### <a name="test-phrases"></a>語句をテストします。

次のテスト メッセージは、サンプル アプリで作業する必要があります。

- 「TasksNotes でりんご、bananas、なしと食料品リストを作成する」
- "TasksNotes で WWDC タスクの追加
- "TasksNotes トレーニング リストにタスク WWDC の追加
- 「マークの TasksNotes 不完全として WWDC に出席」
- 「TasksNotes で通知を受け取ります ホームを取得する場合は、iphone を購入する」
- "マークは、TasksNotes 完了済みとしての iPhone を buy"
- 「ホーム TasksNotes 午前 8 時のままにすることを通知する」

![新しいリストの例を作成します。](sirikit-images/createtasklist-sml.png) ![完全な例として一連のタスク](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> IOS 11 シミュレーターは、(以前のバージョン) とは異なり、Siri とテストをサポートします。
>
> 実際のデバイスでテストする場合は、アプリ ID を構成することを忘れないでくださいし、SiriKit のプロビジョニング プロファイルをサポートします。

<a name="alternativenames" />

## <a name="alternative-names"></a>代替名

この新しい iOS 11 の機能は、Siri と正しくトリガー、ユーザーを支援するアプリの代替名を構成できることを意味します。 次のキーを追加して、 **Info.plist** iOS アプリ プロジェクトのファイル。

![別のアプリの名前のキーと値を示す Info.plist](sirikit-images/alternative-names.png)

別のアプリ名が設定された、次のフレーズも、サンプル アプリの動作 (名前は実際に**TasksNotes**)。

- "りんご、bananas でなしと食料品リストを作成_MonkeyNotes_"
- "追加タスクに WWDC _MonkeyTodo_"


## <a name="troubleshooting"></a>トラブルシューティング

サンプルを実行するか、独自のアプリケーションを SiriKit を追加中に発生する可能性がいくつかのエラー:

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Objective C 例外がスローされます。名前:NSInternalInconsistencyException 理由:クラスの使用 < INPreferences:0x60400082ff00 > アプリから権利 com.apple.developer.siri が必要です。Xcode プロジェクトで Siri 機能を有効にしたでしょうか。_

- SiriKit がでオンになって**Entitlements.plist**します。
- **Entitlements.plist**で構成されている場合は、**プロジェクト オプション > ビルド > iOS バンドル署名**します。

  [![プロジェクトのオプションが正しく設定されている権利を表示](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (用デバイスの展開)アプリ ID が有効になっている SiriKit とプロビジョニング プロファイルをダウンロードします。


## <a name="related-links"></a>関連リンク

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [TasksNotes SiriKit のサンプル](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [新機能で SiriKit (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/214/)
