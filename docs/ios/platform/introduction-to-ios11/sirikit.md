---
title: IOS 11 での SiriKit の更新
description: このドキュメントでは、iOS 11 で SiriKit を使用する方法について説明します。 具体的には、タスクとメモを操作する方法と、アプリケーションの代替名を指定する方法について説明します。
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/07/2017
ms.openlocfilehash: 8983ac0c860dafb3a3a0e4c90bd82bdf87c4c4f8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70752394"
---
# <a name="sirikit-updates-in-ios-11"></a>IOS 11 での SiriKit の更新

SiriKit は iOS 10 で導入され、多数のサービスドメイン (ワークスペース、乗り物予約、通話の作成など) を備えています。 Sirikit の概念と、アプリに SiriKit を実装する方法については、 [「sirikit」セクション](~/ios/platform/sirikit/index.md)を参照してください。

![Siri タスク一覧のデモ](sirikit-images/sirikit.png)

IOS 11 の SiriKit では、次のような新しいインテントドメインが追加されています。

- [**リストとメモ**](#listsnotes)–新機能 タスクとメモを処理するアプリの API を提供します。
- **ビジュアルコード**–新規! Siri では、連絡先情報を共有したり、支払いトランザクションに参加したりするための QR コードを表示できます。
- **支払い**–支払い操作のための検索および転送インテントが追加されました。
- **乗り物予約**–キャンセル乗り物とフィードバックインテントを追加しました。

その他に次の新機能があります。

- [**代替アプリ名**](#alternativenames)–ユーザーが代替の名前/発音を提供することで、アプリを対象とするように siri に指示するために役立つエイリアスを提供します。
- 作業の**開始**–バックグラウンドでトレーニングを開始する機能を提供します。

これらの機能の一部を以下に説明します。 その他の詳細については、 [Apple の SiriKit のドキュメント](https://developer.apple.com/documentation/sirikit)を参照してください。

<a name="listsnotes" />

## <a name="lists-and-notes"></a>リストとメモ

新しいリストとメモドメインには、Siri 音声要求を介してタスクとメモを処理するアプリの API が用意されています。

**タスク**

- タイトルと完了ステータスを取得します。
- 必要に応じて、期限と場所を指定します。

**メモ**

- タイトルとコンテンツフィールドがあること。

タスクとメモの両方をグループにまとめることができます。 このセクションの残りの部分では、[タスクノート SiriKit の例](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample)を使用して、sirikit でこの新しいドメインを実装する方法について説明します。

### <a name="how-to-process-a-sirikit-request"></a>SiriKit 要求を処理する方法

次の手順に従って、SiriKit 要求を処理します。

1. **解決**–パラメーターを検証し、必要に応じてユーザーから追加情報を要求します。
2. [確認] –要求を処理できることを**確認**します。
3. **Handle** –操作を実行します (データの更新またはネットワーク操作の実行)。

最初の2つの手順は省略可能ですが (推奨されます)、最後の手順が必要です。
詳細な手順については、 [「Sirikit」セクション](~/ios/platform/sirikit/index.md)を参照してください。

### <a name="resolve-and-confirm-methods"></a>解決方法と確認方法

これらの省略可能なメソッドを使用すると、コードで検証を実行したり、既定値を選択したり、ユーザーに追加情報を要求したりすることができます。

たとえば、 `IINCreateTaskListIntent`インターフェイスの場合、必要なメソッドは`HandleCreateTaskList`です。 Siri の相互作用をより詳細に制御できる4つの省略可能なメソッドがあります。

- `ResolveTitle`–タイトルを検証し、必要に応じて既定のタイトルを設定するか、データが不要であることを通知します。
- `ResolveTaskTitles`–ユーザーによって読み上げられたタスクの一覧を検証します。
- `ResolveGroupName`–グループ名を検証したり、既定のグループを選択したり、データが不要であることを通知したりします。
- `ConfirmCreateTaskList`–コードが要求された操作を実行できることを検証しますが、実行`Handle*`しません (メソッドのみがデータを変更する必要があります)。

### <a name="handle-the-intent"></a>目的を処理する

リストとメモのドメインには6つの目的があり、タスクには3つ、メモには3つあります。
これらのインテントを処理するために実装する必要があるメソッドは次のとおりです。

- タスクの場合:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- メモ:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

各メソッドには、特定のインテント型が渡されます。これには、siri がユーザーの要求から解析したすべての情報`Resolve*` ( `Confirm*`およびメソッドとメソッドで更新された可能性があります) が含まれます。
アプリは、提供されたデータを解析し、何らかのアクションを実行してデータを格納または処理する必要があります。また、Siri によってユーザーに表示される結果を返します。

### <a name="response-codes"></a>応答コード

必須メソッド`Handle*`と省略`Confirm*`可能なメソッドは、完了ハンドラーに渡すオブジェクトの値を設定することによって、応答コードを示します。 応答は列挙から`INCreateTaskListIntentResponseCode`取得されます。

- `Ready`–確認フェーズ中にを返します ( `Confirm*`メソッドからではなく`Handle*` 、メソッドから)。
- `InProgress`–長時間実行されるタスク (ネットワーク/サーバー操作など) に使用されます。
- `Success`–成功した操作の詳細を使用して応答し`Handle*`ます (メソッドの場合のみ)。
- `Failure`–エラーが発生したため、操作を完了できませんでした。
- `RequiringAppLaunch`–インテントによって処理することはできませんが、アプリでは操作が可能です。
- `Unspecified`–使用しない: エラーメッセージがユーザーに表示されます。

これらのメソッドと応答の詳細については、Apple の[Sirikit リストとメモのドキュメント](https://developer.apple.com/documentation/sirikit/lists_and_notes)を参照してください。

### <a name="implementing-lists-and-notes"></a>リストとメモの実装

次の手順に従って、空の iOS アプリに SiriKit サポートを追加することで、[タスクノート SiriKit の例](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample)が作成されました。

まず、SiriKit サポートを追加するには、iOS アプリで次の手順を実行します。

1. 資格のある**plist**でサイド**キット**をご利用ください。
2. **プライバシー-Siri の使用説明**キーを、ユーザーのメッセージと共に、**情報 plist**に追加します。
3. アプリケーションで`INPreferences.RequestSiriAuthorization`メソッドを呼び出して、siri との対話を許可するようにユーザーに要求します。
4. 開発者ポータルのアプリ ID に SiriKit を追加し、プロビジョニングプロファイルを再作成して新しい権利を追加します。

次に、Siri 要求を処理する新しい拡張機能プロジェクトをアプリに追加します。

1. ソリューションを右クリックし、[**追加] > [新しいプロジェクトの追加**] の順に選択します。
2. **IOS > 拡張機能の > インテント拡張機能**テンプレートを選択します。
3. 2つの新しいプロジェクトが追加されます。インテントと IntentUI。 UI のカスタマイズは省略可能であるため、このサンプルでは**インテント**プロジェクトのコードのみが含まれています。

拡張プロジェクトは、すべての SiriKit 要求が処理される場所です。 別の拡張機能として、メインアプリと通信する方法は自動的にはありません。これは通常、アプリグループを使用して共有ファイルストレージを実装することによって解決されます。

#### <a name="configure-the-intenthandler"></a>IntentHandler を構成する

クラスは、siri 要求のエントリポイントです。すべてのインテントが`GetHandler`メソッドに渡されます。このメソッドは、要求を処理できるオブジェクトを返します。 `IntentHandler`

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

クラスはから`INExtension`継承する必要があります。また、サンプルはリストとメモインテントを処理するため`IINNotebookDomainHandling`、も実装します。

> [!NOTE]
> - .Net では、インターフェイスのプレフィックスとして大文字`I`を付ける規則があります。これは、iOS SDK からプロトコルをバインドするときに Xamarin に準拠します。
> - Xamarin では、iOS の型名も保持し、Apple は型名の最初の2文字を使用して、型が属しているフレームワークを反映します。
> - フレームワークでは、型にプレフィックスが`IN*`付きます (例として、 `Intents` `INExtension`) ですが、これらはインターフェイスでは_ありません_。
> - また、このプロトコル (のインターフェイスにC#なる) は、 `I` `IINAddTasksIntentHandling`のように2つのになります。

#### <a name="handling-intents"></a>処理インテント

各インテント (タスクの追加、タスク属性の設定など) は、次に示すような1つの方法で実装されます。 このメソッドは、次の3つの主な機能を実行する必要があります。

1. **インテントの処理**– siri によって解析されたデータは`intent` 、インテントの種類に固有のオブジェクトで使用できます。 アプリケーションで、オプション`Resolve*`のメソッドを使用してデータを検証した可能性があります。
2. **データストアの検証と更新**–データをファイルシステムに保存します (アプリグループを使用して、メイン iOS アプリがアクセスできるようにします)。または、ネットワーク要求を介してデータを保存します。
3. **応答を提供**する– `completion`ハンドラーを使用して、siri に応答を送信し、ユーザーに読み取り/表示します。

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

が 2 `null`番目のパラメーターとして応答に渡されることに注意してください。これは user activity パラメーターであり、指定されていない場合は既定値が使用されます。
IOS `NSUserActivityTypes` **アプリでサポートされて**いる限り、カスタムアクティビティの種類を設定できます。 次に、アプリを開いたときにこのケースを処理し、特定の操作 (関連するビューコントローラーを開き、Siri 操作からデータを読み込むなど) を実行できます。

また、この例では`Success`結果をハードコーディングしていますが、実際のシナリオでは、適切なエラー報告を追加する必要があります。

### <a name="test-phrases"></a>テスト語句

サンプルアプリでは、次のテスト語句が機能します。

- "仕事のメモで、bananas と pears を使って食料品の一覧を作成する"
- "タスクメモでのタスクの WWDC の追加"
- "タスクノートのトレーニングリストにタスク WWDC を追加する"
- "タスクノートで、WWDC に完了の印を付ける"
- "仕事用のノートで、自宅にいるときに iphone を購入するように通知する"
- "タスクノートで、iPhone を完了として購入することをマークする"
- 「仕事用のノートで午前8時に自宅を去ることを通知する」

![新しいリストの作成の例](sirikit-images/createtasklist-sml.png) ![タスクを完了として設定する例](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> IOS 11 シミュレーターは、Siri を使用したテストをサポートしています (以前のバージョンとは異なります)。
>
> 実際のデバイスでテストしている場合は、SiriKit サポート用のアプリ ID とプロビジョニングプロファイルを忘れずに構成するようにしてください。

<a name="alternativenames" />

## <a name="alternative-names"></a>代替名

この新しい iOS 11 機能は、ユーザーが Siri で適切にトリガーできるように、アプリの代替名を構成できることを意味します。 IOS アプリプロジェクトの**情報**ファイルに、次のキーを追加します。

![別のアプリ名のキーと値を示す情報 plist](sirikit-images/alternative-names.png)

別のアプリ名を設定すると、次のフレーズもサンプルアプリで使用できます (実際には、**タスクノート**と呼ばれます)。

- " _MonkeyNotes_での bananas と pears による食料品の一覧の作成"
- " _Monkeytodo_でのタスクの wwdc の追加"

## <a name="troubleshooting"></a>トラブルシューティング

サンプルを実行するとき、または独自のアプリケーションに SiriKit を追加するときに発生する可能性があるエラーを次に示します。

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_目標-C 例外がスローされました。名前:NSInternalInconsistencyException の理由:クラス < INPreferences を使用します。アプリからの 0x60400082ff00 > には、資格のが必要です。Xcode プロジェクトで Siri 機能を有効にしましたか?_

- SiriKit は、権利の**plist**に含まれています。
- **権利**は、 **iOS バンドル署名 > ビルド > プロジェクトオプション**で構成されます。

  [![権利が正しく設定されたプロジェクトオプション](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (デバイス展開の場合)アプリ ID には、SiriKit が有効になっており、プロビジョニングプロファイルがダウンロードされています。

## <a name="related-links"></a>関連リンク

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [タスクノート SiriKit サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample)
- [SiriKit (WWDC) の新機能 (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/214/)
