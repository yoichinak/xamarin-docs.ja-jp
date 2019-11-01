---
title: Xamarin. iOS のタッチ ID
description: このドキュメントでは、Xamarin iOS アプリでタッチ ID、Apple の生体認証指紋認証テクノロジを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 112a2a038be9f749f37d2d3260d08f2e58b0c597
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031428"
---
# <a name="touch-id-in-xamarinios"></a>Xamarin. iOS のタッチ ID

タッチ ID は、ユーザーを認証する手段として iOS 7 で導入されました。パスコードに似ています。 ただし、アプリストアを使用してデバイスのロックを解除し、iTunes を使用して iCloud キーチェーンのみを認証することに制限されていました。

ローカル認証 API を使用した iOS 8 アプリケーションで、タッチ ID を認証メカニズムとして使用する方法は2つあります。 現在、ローカル認証を使用してリモートで認証することはできません。

Touch ID とその価値を完全に理解するには、キーチェーンサービスと、ユーザーのデータに対するこれらの新しい変更の意味について説明します。 新しい Access Control リスト (Acl) 機能を使用して、キーチェーンアクセスも iOS 8 で拡張されました。

## <a name="keychain--secure-enclave"></a>キーチェーン & セキュリティで保護されたエンクレーブ

キーチェーンは、1つの Apple ID に対してパスワード、キー、証明書、およびメモのセキュリティで保護されたストレージを提供する大規模なデータベースです。 IOS 8 では、アプリケーションは常に独自の一意のキーチェーン項目にアクセスでき、他のアプリケーションのキーチェーン項目にアクセスすることはできません。 これは、キーチェーンが1つのパスワードでロック解除される OS X とは異なり、キーチェーンサービスに対応したアプリケーションでは、キーチェーンが使用されます。 この記事では、iOS 8 でキーチェーンがどのように機能するかについて重点的に説明します。

キーチェーンは特化されたデータベースで、各行は_キーチェーン項目_と呼ばれます。 各項目は、キーチェーン属性によって記述され、暗号化された値で構成されます。 キーチェーンを効率的に使用できるようにするために、小さな項目 (_シークレット_) に最適化されています。
各キーチェーン項目は、ユーザーのパスコードと一意のデバイスシークレットによって保護されます。 キーチェーン項目は、ユーザーが自分のデバイスを使用していない場合でも保護する必要があります。 IOS では、デバイスのロックが解除されたときに項目を使用できるようにするだけで済みます。デバイスがロックされていると、デバイスが使用できなくなります。 また、暗号化されたバックアップに格納することもできます。 キーチェーンの主な機能の1つは、アクセス制御を適用することです。アプリケーションは、キーチェーンの部分にアクセスでき、他のすべてのアプリケーションは使用できなくなります。 次の図は、アプリケーションがキーチェーンとどのように対話するかを示しています。

[![](touchid-images/image1.png "This diagram illustrates how an application interacts with the keychain")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>セキュリティで保護されたエンクレーブ

キーチェーンは、キーチェーン項目を単独で復号化することはできません。代わりに、*セキュリティで保護さ*れたエンクレーブで実行されます。 Secure エンクレーブは、A7 チップ内のクロスプロセッサであり、タッチ ID センサーからの指紋データと登録された印刷との一致を確認します。 次に、キーチェーン項目の暗号化を解除し、復号化されたシークレットをキーチェーンに返します。

### <a name="working-with-keychain"></a>キーチェーンの操作

まず、アプリケーションでキーチェーンに対してクエリを実行し、パスワードが存在するかどうかを確認する必要があります。 存在しない場合は、ユーザーが継続的に要求されないように、パスワードの入力を求められることがあります。 パスワードを更新する必要がある場合は、ユーザーに新しいパスワードの入力を求め、更新された値をキーチェーンに渡します。

> [!NOTE]
> キーチェーンから取得したシークレットを使用した後、データへのすべての参照をメモリから削除する必要があります。 グローバル変数には割り当てないでください。

## <a name="keychain-acl-and-touch-id"></a>キーチェーン ACL とタッチ ID

Access Control 一覧は、iOS 8 の新しいキーチェーン項目属性で、特定の操作の実行を許可する必要があることに関する情報を記述します。 これは、アラートダイアログを表示したり、パスコードを要求したりする形になります。 ACL を使用すると、キーチェーン項目のアクセシビリティと認証を設定できます。 次の図は、この新しい属性が、キーチェーン項目の残りの部分とどのように結び付いているかを示しています。

[![](touchid-images/image2.png "This diagram shows how this new attribute ties in with the rest of the keychain item")](touchid-images/image2.png#lightbox)

IOS 8 の時点で、新しいユーザープレゼンスポリシー `SecAccessControl`が追加されました。これは、iPhone 5s 以降の secure エンクレーブによって適用されます。 次の表に、デバイスの構成がポリシーの評価に与える影響について説明します。

|デバイスの構成|ポリシーの評価|バックアップメカニズム|
|--- |--- |--- |
|パスコードのないデバイス|アクセスなし|None|
|パスコードを含むデバイス|パスコードが必要です|None|
|Touch ID を持つデバイス|タッチ ID を優先する|パスコードを許可する|

セキュリティで保護されたエンクレーブ内のすべての操作は相互に信頼できます。 これは、Touch ID 認証の結果を使用して、キーチェーン項目の復号化を承認できることを意味します。 Secure エンクレーブでは、失敗したタッチ ID 一致のカウンターも保持されます。この場合、ユーザーはパスコードを使用するように戻す必要があります。
IOS 8 の新しいフレームワークである_Local Authentication_は、デバイス内でのこの認証プロセスをサポートしています。 これについては、次のセクションで説明します。

## <a name="local-authentication"></a>ローカル認証

前のセクションで説明したように、アプリケーションはローカル認証を使用して、デバイスに設定されているセキュリティポリシーに準拠してユーザーを認証できます。

現時点では、API は2つの機能のみを提供します。最初に、新しいキーチェーン Access Control リスト (Acl) を使用して、既存のキーチェーンサービスを支援します。 キーチェーンデータは、ユーザーの指紋の認証が成功したときにロックを解除することができます。

次に、LocalAuthentication は、アプリケーションをローカルで認証する2つの方法を提供します。 開発者は、`CanEvaluatePolicy` を使用して、デバイスがタッチ ID を受け入れることができるかどうかを判断し、`EvaluatePolicy` 認証操作を開始する必要があります。

どちらの機能もローカル認証を提供しますが、アプリケーションまたはユーザーがリモートサーバーに対して認証を行うためのメカニズムを提供しません。
ローカル認証は、認証用の新しい標準ユーザーインターフェイスを提供します。 Touch ID の場合、これは次に示す2つのボタンを持つアラートビューです。 1つのボタンをキャンセルし、もう1つは認証の代替手段としてパスコードを使用します。 また、カスタムメッセージも設定する必要があります。 タッチ ID 認証が必要な理由をユーザーに説明するために、この方法を使用することをお勧めします。

[![](touchid-images/image12.png "The Touch ID authentication alert")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>キーチェーンサービスを使用する

ここでは、secure エンクレーブを使用してパスコードを検証することで、キーチェーン項目の暗号化を解除する方法を少し前に見てきました。 IOS 8 では、ローカル認証を使用して、タッチ ID の検証を Access Control リスト機能と組み合わせて要求できます。これにより、フォールバックメカニズムまたはパスワードが実装されます。
ACL を使用するには、`SecAccessControl` ポリシーを使用し、`SecAccessible.WhenPasscodeSetThisDeviceOnly` または `SecAccessible.WhenUnlocked`を使用してデバイスの状態を確認する必要があります。

#### <a name="considerations-with-acl"></a>ACL に関する考慮事項

キーチェーンで ACL を使用する場合は、次のような多くの点に注意する必要があります。

- [フォアグラウンドアプリケーションでのみ使用する] –バックグラウンドスレッドでキーチェーン操作を呼び出すと、呼び出しは失敗します。
- キーチェーン項目を追加および更新するには、認証が必要になることがあります。
- 要求がキーチェーン内の複数の一致する項目を返す場合は、認証が必要になることがあります。
- ACL で保護された項目はデバイス専用であるため、同期またはバックアップされません。

### <a name="using-local-authentication-without-keychain-services"></a>キーチェーンサービスを使用しないローカル認証の使用

パスコードやタッチ ID などの資格情報を収集し、セキュリティで保護されたエンクレーブと連携してユーザーの認証を完了する方法として、ローカル認証が作成されました。 アプリケーションとセキュリティで保護されたエンクレーブの間のブリッジと考えてください。相互に直接通信することはできません。 また、アプリケーションのポリシーの評価に使用することもできます。

これを行うために、アプリケーションは、セキュリティで保護されたエンクレーブ内で操作を開始するローカル認証内でポリシー評価を呼び出します。 これを活用して、セキュリティで保護されたエンクレーブに直接クエリを実行したり、アクセスしたりすることなく、アプリに認証を提供できます。

[![](touchid-images/image13a.png "Using Local Authentication without Keychain Services")](touchid-images/image13a.png#lightbox)

アプリケーションでローカル認証を使用すると、ユーザー検証を実装する簡単な方法が提供されます。たとえば、銀行取引アプリケーションなどのデバイス所有者の目に対してのみ機能のロックを解除したり、個人に対して保護者による制限を支援したりすることが可能です。適用. また、既に存在する認証を拡張する方法として使用することもできます。ユーザーの情報はセキュリティで保護されますが、オプションもあります。

ローカル認証のセキュリティは、キーチェーンのセキュリティとは異なります。 たとえば、キーチェーンを使用する場合、オペレーティングシステムとセキュリティで保護されたエンクレーブの間に信頼関係があります。 ローカル認証では、アプリケーションとオペレーティングシステムの間にあるため、セキュリティで保護されたエンクレーブ自体ではなく、セキュリティで保護されたエンクレーブの結果のみにアクセスできます。

セキュリティの対象として、登録された指やフィンガープリントに**アクセスできない**ことも非常に重要です。 Secure エンクレーブはこの情報の所有者であるため、他のシステムコンポーネントがアクセスすることはできません。

ローカル認証 API を活用してキーチェーンなしで Touch ID を使用するには、使用できる関数がいくつかあります。 これらの詳細を以下に示します。

- `CanEvaluatePolicy` –デバイスがタッチ ID を受け入れることができるかどうかを確認するだけです。
- `EvaluatePolicy`-認証操作を開始し、UI を表示して、`true` または `false` 回答を返します。
- `DeviceOwnerAuthenticationWithBiometrics`-タッチ ID 画面を表示するために使用できるポリシーです。 ここではパスコードフォールバックメカニズムがないことに注意してください。代わりに、ユーザーが Touch ID 認証をスキップできるように、このフォールバックをアプリケーションに実装する必要があります。

次に示すローカル認証の使用に関する注意事項がいくつかあります。

- キーチェーンの場合と同様に、フォアグラウンドでのみ実行できます。 バックグラウンドスレッドでこのメソッドを呼び出すと、エラーが発生します。
- ポリシーの評価が失敗する可能性があることに注意してください。 パスコードボタンは、フォールバックとして実装する必要があります。
- 認証が必要な理由を説明するために `localizedReason` を提供する必要があります。 これにより、ユーザーとの信頼関係を構築できます。

次に、以下のセクションでは、これらの注意事項を考慮して API を実装する方法について説明します。

## <a name="adding-touch-id-to-your-application"></a>アプリケーションにタッチ ID を追加する

前のセクションでは、キーチェーンとローカル認証を使用したアクセスと認証の背後にある理論を見てきました。 ここでは、タッチ ID をアプリケーションに統合する方法について説明します。

### <a name="walkthrough"></a>チュートリアル

では、アプリケーションにタッチ ID 認証を追加する方法を見てみましょう。 このチュートリアルでは[、ストーリーボードテーブルのサンプルを](https://docs.microsoft.com/samples/xamarin/ios-samples/data/storyboardtable/)更新します。これは、[ストーリーボードテーブルのローカル認証](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable-localauthentication)サンプルと同様に機能するように、ローカル認証を追加します。これにより、認証されたユーザーのみがリストに作業を追加できるようになります。

1. サンプルをダウンロードし、Visual Studio for Mac で実行します。
2. `MainStoryboard.Storyboard` をダブルクリックして、iOS Designer でサンプルを開きます。 このサンプルでは、アプリケーションに新しい画面を追加します。これにより、認証が制御されます。 これは、現在の `MasterViewController`の前になります。
3. 新しい**ビューコントローラー**を**ツールボックス**から**デザインサーフェイス**にドラッグします。 **ナビゲーションコントローラー**から**Ctrl キーを押しながらドラッグ**して、**ルートビューコントローラー**として設定します。

    [![](touchid-images/image4.png "Set the Root View Controller")](touchid-images/image4.png#lightbox)
4. 新しいビューコントローラーに `AuthenticationViewController`という名前を指定します。
5. 次に、ボタンをドラッグして `AuthenticationViewController`に置きます。 この `AuthenticateButton`を呼び出し、テキスト `Add a Chore`を指定します。
6. `AuthenticateMe`と呼ばれる `AuthenticateButton` でイベントを作成します。
7. `AuthenticationViewController` から手動のセグエを作成するには、下部にある黒いバーをクリックし、 **Ctrl キーを押しながら**バーから `MasterViewController` にドラッグして、 **[プッシュ]** を選択します (または、[サイズクラスを使用している場合は**表示]** )。

    [![](touchid-images/image5.png "Drag from the bar to the MasterViewController and choosing push or show")](touchid-images/image6.png#lightbox)
8. 新しく作成したセグエをクリックし、次に示すように、識別子 `AuthenticationSegue`を指定します。

    [![](touchid-images/image7.png "Set the segue identifier to AuthenticationSegue")](touchid-images/image7.png#lightbox)
9. `AuthenticationViewController` に次のコードを追加します。

    ```csharp
    partial void AuthenticateMe (UIButton sender)
    {
        var context = new LAContext();
        NSError AuthError;
        var myReason = new NSString("To add a new chore");

        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError)){
            var replyHandler = new LAContextReplyHandler((success, error) => {
                this.InvokeOnMainThread(()=> {
                    if(success)
                    {
                        Console.WriteLine("You logged in!");
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        // Show fallback mechanism here
                    }
                });
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, myReason, replyHandler);
        };
    }
    ```

これは、ローカル認証を使用してタッチ ID 認証を実装するために必要なすべてのコードです。 次の図で強調表示されている行は、ローカル認証の使用を示しています。

[![](touchid-images/image8.png "The highlighted lines show the use of Local Authentication")](touchid-images/image8.png#lightbox)

まず、`CanEvaluatePolicy` を使用し、ポリシー `DeviceOwnerAuthenticationWithBiometrics`に渡すことによって、デバイスがタッチ ID 入力を受け入れることができるかどうかを設定する必要があります。 True の場合、`EvaluatePolicy`を使用して Touch ID UI を表示できます。 `EvaluatePolicy` に渡す必要がある情報には、ポリシー自体、認証が必要な理由を説明する文字列、および応答ハンドラーの3つがあります。 応答ハンドラーは、認証が成功したか失敗したかをアプリケーションに通知します。 次に、応答ハンドラーについて詳しく見てみましょう。

[![](touchid-images/image9.png "The reply handler")](touchid-images/image9.png#lightbox)

応答ハンドラーは `LAContextReplyHandler`型で指定されます。これは、成功したパラメーター (`bool` 値) と `error`と呼ばれる `NSError` を受け取ります。 正常に実行された場合は、ここで認証するものを何でも実行します。この例では、新しい作業を追加できる画面が表示されます。 ローカル認証の注意事項の1つは、フォアグラウンドで実行する必要があることを忘れないでください。 `InvokeOnMainThread`を必ず使用してください。

[![](touchid-images/image10.png "Use InvokeOnMainThread for Local Authentication")](touchid-images/image10.png#lightbox)

最後に、認証が成功したら、`MasterViewController`に移行します。 `PerformSegue` メソッドを使用して、次の操作を行うことができます。

[![](touchid-images/image11.png "Call PerformSegue method to transition to the MasterViewController")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>まとめ

このガイドでは、キーチェーンと、これが iOS でどのように機能するかを見てきました。 また、キーチェーン ACL と iOS での変更についても説明します。 次に、iOS 8 の新機能であるローカル認証フレームワークについて説明し、アプリケーションでの Touch ID 認証の実装について見てきました。

## <a name="related-links"></a>関連リンク

- [ストーリーボードテーブル–ローカル認証](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable-localauthentication)
- [キーチェーン WWDC サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-keychaintouchid/)
- [キーチェーン (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/keychain/)
