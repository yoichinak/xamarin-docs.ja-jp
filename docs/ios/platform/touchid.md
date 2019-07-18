---
title: Xamarin.iOS で touch ID
description: このドキュメントでは、Xamarin.iOS アプリで Touch ID、Apple の指紋認証テクノロジを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 25ace6d7febe495164378b3633f06371806e2f82
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832306"
---
# <a name="touch-id-in-xamarinios"></a>Xamarin.iOS で touch ID

Touch ID は、ユーザーのパスコードのような認証の手段として、iOS 7 で導入されました。 ただし、デバイスのロックを解除、App Store を使用して、iTunes を使用して、iCloud キーチェーンのみの認証に制限されていました。

ローカル認証 API を使用して iOS 8 アプリケーションの認証メカニズムとして Touch ID を使用する 2 つの方法はようになりました。 現在これは、ローカルの認証を使用して、リモートで認証することはできません。

Touch ID とその分を完全に理解をする必要があります Keychain サービスについて説明し、ユーザーのデータのこれらの新しい変更が何を意味します。 キーチェーン アクセスは、新しいアクセス制御リスト (Acl) 機能を使用して ios 8 時にも拡張されています。

## <a name="keychain--secure-enclave"></a>キーチェーン & セキュリティで保護されたエンクレーブ

キーチェーンは、パスワード、キー、証明書およびノートの個々 の Apple ID を 1 つのセキュリティで保護されたストレージを提供する大規模なデータベース IOS 8 で、アプリケーションは常に、独自の一意のキーチェーン項目にアクセスして、他のアプリケーションのすべてのキーチェーン項目にアクセスできません。 これは、Keychain サービス対応アプリケーションが、キーチェーンを使用して、キーチェーンが 1 つのパスワードを使用してロック OS X とは異なります。 この記事では、キーチェーンが iOS 8 で動作する方法について説明します。

キーチェーンが各行と呼ばれます、特殊化されたデータベースを_キーチェーン項目_します。 各項目は、キーチェーン属性によって定義され、は暗号化された値で構成されます。 キーチェーンを効率的に使用できるように、小さいアイテムに最適ですか_シークレット_します。
キーチェーンの各項目は、ユーザーのパスコードと一意デバイス シークレットで保護されます。 ユーザーが自分のデバイスを使用していない場合でも、キーチェーンの項目を保護する必要があります。 これに実装されている iOS デバイスがロックされている場合は、使用可能になる項目のみを許可-デバイスがロックされているときに利用できなくなった。 暗号化されたバックアップに格納することもできます。 キーチェーンの主な機能の 1 つが; のアクセス制御を適用するにはアプリケーションは、キーチェーンの部分にアクセスでき、その他のすべてのアプリケーションは実行されません。 次の図は、アプリケーションが、キーチェーンをやり取りする方法を示しています。

[![](touchid-images/image1.png "この図は、アプリケーションが、キーチェーンをやり取りする方法を示しています。")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>セキュリティで保護されたエンクレーブ

キーチェーンことはできません。 単独でキーチェーンの項目を復号化します。代わりに行われますが、*セキュリティで保護されたエンクレーブ*します。 セキュリティで保護されたエンクレーブとは、指紋データに対して登録されている印刷、Touch ID のセンサーからの一致を判断するには、A7 チップ内で共同プロセッサです。 されキーチェーンの項目を復号化し、キーチェーンに復号化されたシークレットを返します。

### <a name="working-with-keychain"></a>キーチェーンの操作

まず、アプリケーションは、キーチェーンにパスワードが存在するかどうかにクエリを実行する必要があります。 存在しない場合は、ユーザーに求めていない継続的に、パスワードを要求する必要があります。 パスワードを更新する場合は、新しいパスワードの入力を求めます、キーチェーンに更新された値を渡します。

> [!NOTE]
> キーチェーンから取得したシークレットを使用して、データへのすべての参照をメモリから削除する必要があります。 グローバル変数に割り当てることはありません。

## <a name="keychain-acl-and-touch-id"></a>キーチェーンの ACL とタッチ ID

アクセス制御リストは、発生する特定の操作を許可するように必ず発生する問題に関する情報を記述する iOS 8 で新しいキーチェーン項目属性です。 アラート ダイアログを表示またはパスコードを要求の形式になります。 ACL は、アクセシビリティとキーチェーン項目に対する認証を設定することができます。 次の図は、この新しい属性が、残りのキーチェーンの項目を結び付ける方法を示しています。

[![](touchid-images/image2.png "この図は、この新しい属性が、残りのキーチェーンの項目を結び付ける方法を示しています。")](touchid-images/image2.png#lightbox)

時点で、8、iOS がある新しいユーザーのプレゼンス ポリシーの`SecAccessControl`、これは、iPhone 5 以降では、セキュリティで保護されたエンクレーブによって強制されます。 だけ方法、デバイスの構成に影響を与えるポリシーの評価、次の表でわかります。

|デバイスの構成|ポリシーの評価|バックアップ メカニズム|
|--- |--- |--- |
|デバイスのパスコードなし|アクセスなし|なし|
|デバイスのパスコードを|パスコードが必要です。|なし|
|Touch ID を持つデバイス|Touch ID が優先されます。|パスコードは、します。|

セキュリティで保護されたエンクレーブ内のすべての操作には、互いを信頼できます。 つまり、キーチェーン項目は復号化を承認するために Touch ID の認証の結果を使用しました。 セキュリティで保護されたエンクレーブには、パスコードを使用する元に戻す場合、ユーザーが必要があります、Touch ID 照合の失敗のカウンターも保持されます。
呼ばれる iOS 8 の新しいフレームワーク_ローカル認証_認証、デバイス内のこのプロセスをサポートしています。 この次のセクションで説明されます。

## <a name="local-authentication"></a>ローカル認証

前のセクションで確立されるとアプリケーションは、デバイスに設定されているセキュリティ ポリシーに準拠しているユーザーを認証するのにローカル認証を使用できます。

現時点では、API は、2 つだけの機能を提供します。まず、新しいキーチェーン アクセス制御リスト (Acl) を使用して既存の Keychain サービスを支援します。 キーチェーンのデータは、ユーザーの指紋認証が成功したロックされていることができます。

次に、LocalAuthentication は、アプリケーションをローカルで認証する 2 つのメソッドを提供します。 開発者が使用する必要があります`CanEvaluatePolicy`デバイスがタッチの ID を受信できるかどうかが決定し、`EvaluatePolicy`認証操作を開始します。

両方の機能には、ローカルの認証が提供して、アプリケーションまたはユーザーは、リモート サーバーに対して認証するためのメカニズムは提供されません。
ローカル認証は、認証用の新しい標準のユーザー インターフェイスを提供します。 Touch ID では、場合これは、次のように 2 つのボタンで、アラートの表示です。 Cancel、および認証 – パスコードの代替手段を使用する 1 つ 1 つをクリックします。 設定する必要があるカスタム メッセージもあります。 これを使用して、ユーザーがタッチ ID の認証が必要な理由を説明することをお勧めします。

[![](touchid-images/image12.png "Touch ID の認証のアラート")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>Keychain サービスの使用

キーチェーンの項目は、どの少し前にしました復号化、セキュリティで保護されたエンクレーブを使用して、パスコードを確認します。 Ios 8 でローカル認証アクセス制御リスト機能、フォールバック メカニズム、またはパスワードの実装を提供すると共に、Touch ID 検証を要求に使用できます。
私たちが使用する ACL を使用する、`SecAccessControl`ポリシー、および使用して、デバイスの状態を確認し、`SecAccessible.WhenPasscodeSetThisDeviceOnly`または`SecAccessible.WhenUnlocked`します。

#### <a name="considerations-with-acl"></a>ACL での考慮事項

キーチェーンで ACL を使用する場合に注意してください多くのことがあるし、これらのいくつか以下に示します。

- バック グラウンド スレッドが、呼び出しは失敗でキーチェーン操作を呼び出す場合は、フォア グラウンド アプリケーションでのみ使用します。
- キーチェーンの項目の更新の追加とは、認証を必要があります。
- 場合は、要求は、キーチェーンに複数の一致する項目を返します、認証が必要な場合があります。
- ACL で保護された項目はデバイスのみ、そのため同期やではなくバックアップします。

### <a name="using-local-authentication-without-keychain-services"></a>Keychain サービスせずローカル認証を使用します。

ローカル認証は、パスコードまたはタッチ ID をなどの資格情報を収集して、ユーザーの認証を完了するセキュリティで保護されたエンクレーブを使用する方法として作成されました。 アプリケーションと互いと通信できる直接ではなく、セキュリティで保護されたエンクレーブの間の仲介役として考えます。 これは、アプリケーションのポリシー評価のためも使用できます。

このアプリケーションを実行するには、ローカル認証は、セキュリティで保護されたエンクレーブ内で操作を開始内でポリシーの評価を呼び出します。 直接クエリ/にアクセスするセキュリティで保護されたエンクレーブせず、アプリへの認証を提供するこれを利用できます。

[![](touchid-images/image13a.png "Keychain サービスせずローカル認証を使用します。")](touchid-images/image13a.png#lightbox)

ローカル認証を使用して、アプリケーションで、個々 のサポート機能の保護者や銀行取引アプリケーションなどなど、デバイスの所有者から見るのためだけの機能のロックを解除する例については、ユーザーの認証を実装する簡単な方法を提供しますアプリケーション。 既に存在している認証を拡張する手段として使用することもできます – など、セキュリティで保護するには、その情報をユーザーがオプションがありますようにもします。

ローカル認証のセキュリティは、キーチェーンの動作とは異なります。 たとえば、キーチェーンを使用する場合、信頼はオペレーティング システムとセキュリティで保護されたエンクレーブとの間は。 ローカル認証は、アプリケーションとオペレーティング システム、セキュリティで保護されたエンクレーブいない、セキュリティで保護されたエンクレーブ自体の結果へのアクセスのみがあることを意味するです。

セキュリティに関しても非常に重要ですがあることを把握する**アクセスなし**に登録されている本の指、または指紋の画像。 セキュリティで保護されたエンクレーブは、この情報の所有者と、その他のシステム コンポーネントへのアクセスがないためです。

キーチェーンせず Touch ID を使用して、ローカルの認証 API を活用することを使用できるいくつかの関数があります。 これらを次に説明します。

*   `CanEvaluatePolicy` – これは、デバイスがタッチ ID を受信できるかどうかを確認してだけです。
*   `EvaluatePolicy` – これ認証操作を開始し、UI が表示され、を返します、`true`または`false`回答します。
*   `DeviceOwnerAuthenticationWithBiometrics` – これは、ポリシー、Touch ID の画面が表示に使用できます。 パスコードのフォールバック メカニズムは、ここではありませんを代わりに Touch ID の認証をスキップするユーザーを許可するためにアプリケーションでこのフォールバックを実装する必要がありますの注目に値します。

以下に示されているローカルの認証を使用すると、いくつかの注意事項があります。

*   キーチェーンと同様にのみ実行できますフォア グラウンドで。 バック グラウンド スレッドで呼び出しが失敗することになります。
*   ポリシーの評価が失敗することに注意してください。 パスコード ボタンは、フォールバックとして実装する必要があります。
*   指定する必要があります、`localizedReason`認証が必要な理由を説明します。 これにより、ユーザーとの信頼を構築します。

次に、以下のセクションでは方法について説明を次の点を考慮して、API を実装します。

## <a name="adding-touch-id-to-your-application"></a>Touch ID をアプリケーションに追加します。

前のセクションでキーチェーンとローカル認証を使用して認証とアクセスの背後にある原理について説明しました。 Touch ID をアプリケーションに統合する方法を見て今すぐかかります。

### <a name="walkthrough"></a>チュートリアル

アプリケーションへのタッチ ID の一部の認証の追加を見てみましょう。 このチュートリアルで更新するつもりが、[ストーリー ボード テーブル](https://developer.xamarin.com/samples/StoryboardTable/)サンプルでは、ローカルの認証を追加するように動作するよう、[ストーリー ボード テーブル-ローカル認証](https://developer.xamarin.com/samples/monotouch/StoryboardTable_LocalAuthentication/)サンプルは、のみを許可日々 のタスクを一覧に追加するユーザーを認証します。

1. サンプルをダウンロードして Visual studio for mac。 実行
2. ダブルクリックします。 `MainStoryboard.Storyboard` iOS Designer のサンプルを開きます。 このサンプルは、認証を制御する、アプリケーションに新しい画面を追加します。 現在の前に移動する この`MasterViewController`します。
3. 新しいドラッグ**ビュー コント ローラー**から、**ツールボックス**を**デザイン サーフェイス**します。 設定として、**ルート ビュー コント ローラー**によって**ctrl キーを押しながらドラッグ**から、**ナビゲーション コント ローラー**:

    [![](touchid-images/image4.png "ルート ビュー コント ローラーを設定します。")](touchid-images/image4.png#lightbox)
4.  新しいビュー コント ローラーの名前を付けます`AuthenticationViewController`します。
5. 次に、ボタンをドラッグし、上に配置、`AuthenticationViewController`します。 これを呼び出す`AuthenticateButton`、し、テキストを付けます`Add a Chore`します。
6. イベントを作成、`AuthenticateButton`と呼ばれる`AuthenticateMe`します。
7. 手動作成からセグエ`AuthenticationViewController`下部にある黒いバーをクリックして、 **ctrl キーを押しながらドラッグ**バーから、`MasterViewController`を選択して**プッシュ**(または**表示**サイズ クラスを使用): 場合

    [![](touchid-images/image5.png "プッシュを選択して、MasterViewController バーからドラッグするか、表示します。")](touchid-images/image6.png#lightbox)
8. をクリックして、新しく作成したセグエし、識別子を付けます`AuthenticationSegue`以下に示すように。

    [![](touchid-images/image7.png "AuthenticationSegue にセグエ識別子を設定します。")](touchid-images/image7.png#lightbox)
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

これは、ローカルの認証を使用して、Touch ID 認証を実装するために必要なすべてのコードです。 次の図で強調表示された行は、ローカルの認証の使用を示しています。

[![](touchid-images/image8.png "強調表示された行に表示するローカル認証の使用")](touchid-images/image8.png#lightbox)

最初に、デバイスがタッチの ID を使用して、入力を受信できるかどうかを確立する必要があります、`CanEvaluatePolicy`ポリシーを渡して`DeviceOwnerAuthenticationWithBiometrics`します。 これは、true のかどうかを使用してタッチ ID UI を表示します`EvaluatePolicy`します。 3 つの情報を渡す必要がある`EvaluatePolicy`– ポリシー自体、認証が必要な場合は、理由を説明する文字列、および応答ハンドラー。 応答ハンドラーは、成功または失敗すると、認証の場合に実行する動作をアプリケーションに指示します。 応答ハンドラーについて詳しく見てみましょう。

[![](touchid-images/image9.png "応答ハンドラー")](touchid-images/image9.png#lightbox)

応答ハンドラーが型の指定された`LAContextReplyHandler`、パラメーター成功 – これにより、`bool`値、および`NSError`と呼ばれる`error`します。 成功した場合、これは実際に実行場所は – を認証するここで、新しい面倒な作業を追加お知らせする画面を表示します。 ローカル認証の注意事項の 1 つは、フォア グラウンドで実行には、使用することでなければならないことに注意してください`InvokeOnMainThread`:。

[![](touchid-images/image10.png "InvokeOnMainThread を使用して、ローカルの認証")](touchid-images/image10.png#lightbox)

最後に、する場合、認証が成功した、することへの移行、`MasterViewController`します。 `PerformSegue`メソッドは、これを行うために使用できます。

[![](touchid-images/image11.png "PerformSegue メソッドを呼び出して、MasterViewController への移行")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>Summary
このガイドでは、キーチェーンと iOS でのこのしくみを説明しました。 ACL、キーチェーンについても学習しましたし、iOS でこれを変更します。 次に、iOS 8 の新機能であり、アプリケーションで Touch ID の認証の実装について説明しましたが、ローカル認証フレームワークを参照してくださいをしました。

## <a name="related-links"></a>関連リンク

- [ストーリー ボード テーブル-ローカル認証](https://developer.xamarin.com/samples/monotouch/StoryboardTable_LocalAuthentication/) 
- [キーチェーン WWDC サンプル](https://developer.xamarin.com/samples/KeychainTouchID/)
- [キーチェーン (サンプル)](https://developer.xamarin.com/samples/Keychain/)
