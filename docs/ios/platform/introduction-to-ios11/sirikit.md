---
title: SiriKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/07/2017
ms.openlocfilehash: 0240dd5e381694a31ba9ebb12dd166ca0ef54750
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="sirikit"></a>SiriKit

SiriKit は、サービスのドメイン (ワークアウト、飛行予約、および呼び出しを含む) の数が 10、iOS で導入されました。 参照してください、 [SiriKit セクション](~/ios/platform/sirikit/index.md)SiriKit 概念と、アプリに SiriKit を実装する方法です。

![Siri タスク一覧のデモ](sirikit-images/sirikit.png)

SiriKit iOS 11 では、これらの新規および更新インテント ドメインを追加します。

- [**一覧し、ノート**](#listsnotes) – 新しい! タスク メモを処理するには、アプリ用 API を提供します。
- **ビジュアル コード**– 新しい! Siri が連絡先情報を共有または支払トランザクションに参加する QR コードを表示できます。
- **支払**– 支払いの相互作用の検索と転送のインテントを追加します。
- **予約を車**追加 – 飛行とフィードバックのインテントをキャンセルします。

他の新機能は次のとおりです。

- [**別のアプリケーション名を**](#alternativenames) – 支援提供エイリアス名/発音の代替が提供されるアプリの対象に Siri に指示します。
- **開始ワークアウト**– バック グラウンドで、トレーニングを開始する機能を提供します。

これらの機能について詳しく説明します。 その他の詳細についてを参照してください[Apple の SiriKit ドキュメント](https://developer.apple.com/documentation/sirikit)です。

<a name="listsnotes" />

## <a name="lists-and-notes"></a>リストと注意事項

新しいリストとノート ドメインは、タスクおよび Siri 音声の要求を使用してノートを処理するには、アプリ用 API を提供します。

**タスク**

- タイトルと、完了ステータスがあります。
- 必要に応じて、期限と場所が含まれます。

**ノート**

- タイトルとコンテンツのフィールドがあります。

両方のタスクとノートは、グループに分類できます。 このセクションの残りの部分で SiriKit、この新しいドメインを実装する方法の説明を使用して、 [TasksNotes SiriKit 例](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)です。

### <a name="how-to-process-a-sirikit-request"></a>SiriKit 要求を処理する方法

次の手順に従って SiriKit 要求を処理します。

1. **解決するには**– パラメーターを検証し、詳細情報が要求をユーザーから (必要な場合)。
2. **確認**– 最後の確認および検証の要求を処理することができます。
3. **処理**– (データを更新またはネットワーク操作の実行) の操作を実行します。

最初の 2 つの手順は、(推奨) には省略可能な最後の手順が必要とします。
手順の詳細については、 [SiriKit セクション](~/ios/platform/sirikit/index.md)です。

### <a name="resolve-and-confirm-methods"></a>解決して、メソッドの確認

これらのオプションのメソッドでは、コードが検証、既定値、または追加情報を要求を実行するユーザーから使用できます。

たとえばの`IINCreateTaskListIntent`インターフェイス、必要なメソッドは`HandleCreateTaskList`します。 Siri の相互作用より詳細に制御を提供する 4 つの省略可能な方法はあります。

- `ResolveTitle` – タイトルの検証、設定の既定のタイトル (該当する場合)、またはデータが必要ないことを通知します。
- `ResolveTaskTitles` – ユーザーが読み上げるタスクの一覧を検証します。
- `ResolveGroupName` – 検証グループ名、既定のグループを選択またはデータが必要ないことを通知します。
- `ConfirmCreateTaskList` – 検証コードが要求された操作を実行することができますが、これは行いません (だけ、`Handle*`メソッドは、データを変更する必要があります)。

### <a name="handle-the-intent"></a>目的を処理します。

リストとノート ドメイン内の 6 つのインテント、3 つのタスクとノートの 3 つがあります。
これらの目的を処理するために実装する必要がある方法を示します。

- タスクの場合。
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- ノートを。
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

各メソッドには、特定目的の種類が渡された Siri がユーザーの要求から解析されるすべての情報が含まれて (で更新される可能性のあると、`Resolve*`と`Confirm*`メソッド)。
アプリは、指定すると、データを解析し、一部を格納するアクションまたはそれ以外の場合、データを処理を実行し、Siri が話すし、ユーザーに表示される結果を返す必要があります。

### <a name="response-codes"></a>応答コード

必要な`Handle*`と省略可能な`Confirm*`メソッドが、完了ハンドラーに渡されるオブジェクトの値を設定して、応答コードを指定します。 応答に由来、`INCreateTaskListIntentResponseCode`列挙します。

- `Ready` – 確認段階で返します (ie。 から、`Confirm*`メソッドからではなく、`Handle*`メソッド)。
- `InProgress` – (ネットワーク/サーバーの操作) などの実行時間の長いタスクのために使用します。
- `Success` – 正常に操作の詳細を含む応答 (からのみ、`Handle*`メソッド)。
- `Failure` – にするとエラーが発生しました、操作を完了できませんでした。
- `RequiringAppLaunch` – 意図的に処理ことはできませんが、アプリでは、操作します。
- `Unspecified` – 使用しないでください。 エラー メッセージがユーザーに表示されます。

詳細については、これらのメソッドと Apple の反応[SiriKit が一覧表示し、ドキュメントをノート](https://developer.apple.com/documentation/sirikit/lists_and_notes)です。

### <a name="implementing-lists-and-notes"></a>リストおよび注意事項を実装します。

[TasksNotes SiriKit 例](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)SiriKit サポートを空白の iOS アプリに追加する、次の手順を使用して作成されました。

最初に、SiriKit サポートを追加するには、iOS アプリのこれらの手順に従います。

1. ティック**SiriKit**で**Entitlements.plist**です。
2. 追加、**プライバシー – Siri 用途説明**キーを**Info.plist**お客様のメッセージと共にします。
3. 呼び出す、 `INPreferences.RequestSiriAuthorization` Siri の相互作用を許可するユーザー入力を求め、アプリ内のメソッドです。
4. 開発者ポータルでアプリ ID を SiriKit を追加し、新しい権利を含むプロビジョニング プロファイルを再作成します。

Siri 要求を処理するアプリに新しい拡張機能プロジェクトを追加し、します。

1. ソリューションを右クリックし、選択**追加 > 新しいプロジェクトの追加.**.
2. 選択、 **iOS > 拡張子 > インテント拡張子**テンプレート。
3. 2 つの新しいプロジェクトが追加されます。 目的および IntentUI です。 サンプルでは、内のコードしか含まれないように、UI のカスタマイズは省略可能で、**インテント**プロジェクト。

拡張機能プロジェクトでは、SiriKit のすべての要求を処理します。 別個の拡張機能として自動的がないアプリ グループを使用して共有ファイル ストレージを実装することによってこの解決は通常、メインのアプリ – と通信する方法です。

#### <a name="configure-the-intenthandler"></a>IntentHandler を構成します。

`IntentHandler` Siri 要求 – に渡されるすべての目的のクラスは、エントリ ポイント、`GetHandler`メソッドで、要求を処理できるオブジェクトを返します。

次のコードは、単純な実装を示しています。

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

クラスを継承する必要があります`INExtension`も実装するため、サンプルでは、リストを処理して、インテントをノート、および`IINNotebookDomainHandling`です。

> [!NOTE]
> **名前付けに関するメモ:** 、規則に従う大文字の付いたを指定するインターフェイスを .NET では`I`Xamarin は iOS SDK からのプロトコルをバインドするときに準拠します。
>
> Xamarin では、iOS からの型の名前も保持され、Apple は、型が属するフレームワークを反映するように型名の最初の 2 つの文字を使用します。
>
> `Intents` Framework、種類が付きます。`IN*`などです。 `INExtension`) が、_いない_インターフェイスです。
> プロトコル (c# でのインターフェイスになります) が終わるを 2 つにも従う`I`s など`IINAddTasksIntentHandling`です。

#### <a name="handling-intents"></a>処理の目的

各目的とした (タスクの追加、タスクの属性の設定など) は、次に示すように 1 つのメソッドに実装されます。 メソッドは、次の 3 つの主な機能を実行してください。

1. **目的の処理**–、Siri によって解析されるは利用できるように、`intent`目的の種類に固有のオブジェクト。 オプションを使用してそのデータがアプリによって検証`Resolve*`メソッドです。
2. **検証およびデータ ストアを更新する**– (メインの iOS アプリではアクセスもできるようにするアプリ グループを使用)、ファイル システムまたはネットワーク要求を使用してデータを保存します。
3. **応答を提供**– を使用して、 `completion` Siri がユーザーに読み取り/表示にへの応答を送信するハンドラー。

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

注意して`null`渡される – 応答には、2 番目のパラメーターとして、これは、ユーザー アクティビティのパラメーターとが指定されない場合、既定値が適用されます。
経由で、iOS アプリをサポートしている限り、カスタム アクティビティの種類を設定することができます、`NSUserActivityTypes`キー **Info.plist**です。 アプリを開いたときに、このケースを処理し、(関連するビューのコント ローラーを開き、Siri 操作からのデータの読み込み) などの特定の操作を実行します。

例もがハードコーディングされています、`Success`結果が実際のシナリオで適切なエラーを報告する必要がありますに追加できません。

### <a name="test-phrases"></a>語句をテストします。

次のテスト フレーズは、サンプル アプリで作業する必要があります。

- 「TasksNotes でリンゴ、バナナ、なしと食料品リストを作成する」
- Add タスク WWDC TasksNotes"
- "タスク WWDC トレーニングの一覧に追加 TasksNotes"
- 「マークときに通っていた WWDC TasksNotes 不完全として」
- 「TasksNotes で通知するホーム出た場合は、iphone を購入する」
- "マークは、TasksNotes 完了済みとしてに iPhone を buy"
- 「通知 TasksNotes 午前 8 時にホームのままにする」

![新しいリストの例を作成します。](sirikit-images/createtasklist-sml.png) ![完全な例として一連のタスク](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> 11、iOS シミュレーターは、(以前のバージョン) とは異なり、Siri でテストをサポートします。
>
> 実際のデバイスでテストする場合は、アプリ ID と SiriKit サポート用のプロファイルのプロビジョニングを構成してください。

<a name="alternativenames" />

## <a name="alternative-names"></a>代替名

この新しい iOS 11 機能の場合、アプリ ユーザー トリガーが正しく Siri を支援するための代替名を構成することができます。 次のキーを追加して、 **Info.plist** iOS アプリ プロジェクトのファイル。

![Info.plist 代替アプリ名のキーと値の表示](sirikit-images/alternative-names.png)

代替アプリ名が設定された次のフレーズはまた、サンプル アプリの機能 (実際に指定するは**TasksNotes**)。

- "リンゴ、バナナでなしと食料品リストを作成_MonkeyNotes_"
- "を追加で WWDC タスク_MonkeyTodo_"


## <a name="troubleshooting"></a>トラブルシューティング

サンプルを実行したり、独自のアプリケーションに SiriKit を追加するときに生じる可能性のあるいくつかのエラー:

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Objective C の例外がスローされます。[名前]: NSInternalInconsistencyException 理由: クラスの使用 < INPreferences: 0x60400082ff00 > アプリから権利 com.apple.developer.siri が必要です。Xcode プロジェクトで Siri 機能を有効にしました。_

- SiriKit がでチェック マーク**Entitlements.plist**です。
- **Entitlements.plist**で構成された、**プロジェクトのオプション > ビルド > iOS バンドル署名 ***です。

  [![プロジェクトの権利が正しく設定の表示オプション](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png)

- (デバイス デプロイメント用)アプリ ID は有効になっている SiriKit を持ち、プロビジョニング プロファイルをダウンロードします。


## <a name="related-links"></a>関連リンク

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [TasksNotes SiriKit サンプル](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [新で SiriKit (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/214/)
