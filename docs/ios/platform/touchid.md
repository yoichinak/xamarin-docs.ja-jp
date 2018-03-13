---
title: "タッチ ID"
description: "タッチ ID は、Apple の指紋認証テクノロジです。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: a2378cb439ceed94751e61fd44b54aae3a65bebd
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="touch-id"></a>タッチ ID

タッチ ID は、ユーザーのパスコードのような認証のための手段として、iOS 7 で導入されました。 ただし、デバイスのロックを解除する、アプリ ストアを使用して、iTunes を使用して、iCloud キーチェーンのみの認証に制限されていました。

ローカルの認証 API を使用してアプリケーションを iOS 8 での認証メカニズムとして Touch ID を使用する 2 つの方法はようになりました。 現在ありませんをリモートでの認証にローカルの認証を使用すること。

Touch ID と価値についてよく理解して、キーチェーンのサービスをナビゲートする必要があり、ユーザーのデータの新しい変更が何を意味します。 キーチェーン アクセスは、新しいアクセス制御リスト (Acl) 機能を使用して iOS 8 の時にも拡張されました。

## <a name="keychain--secure-enclave"></a>キーチェーン & セキュリティで保護された Enclave

キーチェーンはパスワード、キー、証明書およびノートの個々 の Apple ID を 1 つのセキュリティで保護された記憶域を提供する大規模なデータベース IOS 8 でアプリケーションは常に独自の一意のキーチェーン アイテムへのアクセスを持ち、他のアプリケーションのすべてのキーチェーン アイテムにアクセスできません。 これは、キーチェーン サービスに対応するアプリケーションがキーチェーンを使用してキーチェーンは 1 つのパスワードを使用してロック OS X とは異なります。 この記事では、iOS 8 でキーチェーンがどのように動作するかについて説明します。

キーチェーンは各行と呼ばれます、特別なデータベース、_キーチェーン項目_です。 各項目は、キーチェーン属性によって定義され、は暗号化された値で構成されます。 キーチェーンの効率的な使用を許可するのには最適化の小さい項目をまたは_シークレット_です。
キーチェーンの各項目は、ユーザーのパスコードと一意のデバイス秘密によって保護されます。 ユーザーは自分のデバイスを使用していない場合でも、キーチェーンの項目を保護する必要があります。 これを実装、iOS でのみ使用可能になるデバイスのロックが解除されているときに項目を許可する-利用できなくなったデバイスがロックされている場合。 オブジェクトは、暗号化されたバックアップにも格納できます。 キーチェーンの主な機能の 1 つは; アクセス制御を適用するにはアプリケーションは、キーチェーンの部分へのアクセスを持ち、その他のすべてのアプリケーションは実行されません。 次の図は、アプリケーションがキーチェーンとやり取りする方法を示しています。

[![](touchid-images/image1.png "この図は、アプリケーションがキーチェーンとやり取りする方法を示しています。")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>セキュリティで保護された Enclave

キーチェーン暗号化を解除できませんキーチェーン項目自体です。行われます代わりに、 *Secure Enclave*です。 セキュリティで保護された enclave は、1 が登録されている印刷に対して Touch ID センサーから指紋データからの一致を判断する A7 チップ共同プロセッサです。 キーチェーンの項目を復号化し、キーチェーンに暗号化が解除されたシークレットが返すされます。

### <a name="working-with-keychain"></a>キーチェーンの操作

まず、アプリケーションは、パスワードが存在するかどうかは、キーチェーンにクエリを実行する必要があります。 存在しない場合は、ユーザーが要求されていない継続的に、パスワードを要求する必要があります。 場合は、パスワードは、更新する必要があります、ユーザーに新しいパスワードを要求し、キーチェーンに更新された値を渡します。


> ℹ️ **注**: も、機密データが表示されたら、データベースから、それだけのベスト プラクティスではありませんが必要メモリ内の機密情報が保持されません。 期間は必要に応じて、使用し、絶対に割り当てないでグローバル変数としてだけ、シークレットを保持する必要があります!




## <a name="keychain-acl-and-touch-id"></a>キーチェーンの ACL とタッチ ID

アクセス制御リストは、発生する特定の操作を許可するように必ず発生する問題に関する情報を記述する iOS 8 で新しいキーチェーン項目属性です。 これにより、警告のダイアログを表示またはパスコードを要求する形式の可能性があります。 ACL では、ユーザー補助機能とキーチェーン項目に対する認証を設定することができます。 次の図は、この新しい属性が、残りのキーチェーン項目を結び付ける方法を示しています。

[![](touchid-images/image2.png "この図は、この新しい属性が、残りのキーチェーン項目を結び付ける方法を示しています。")](touchid-images/image2.png#lightbox)

8、iOS の時点ではこれで、新しいユーザーが存在するポリシーを`SecAccessControl`、iPhone 5 以降では、セキュリティで保護された enclave によってこれが適用されます。 デバイスの構成がどのようにポリシーの評価に影響できますのみ、次の表で確認できます。

<table width="100%" border="1px">
<thead>
<tr>
    <td>デバイスの構成</td>
    <td>ポリシーの評価</td>
    <td>バックアップのメカニズム</td>
</tr>
</thead>
<tbody>
<tr>
    <td>デバイスのパスコードなし</td>
    <td>アクセスなし</td>
    <td>なし</td>
</tr>
<tr>
    <td>パスコードを含むデバイス</td>
    <td>パスコードが必要です。</td>
    <td>なし</td>
</tr>
<tr>
    <td>Touch ID を持つデバイス</td>
    <td>Touch ID を推奨します。</td>
    <td>パスコードは、します。</td>
</tr>
</tbody>
</table>

セキュリティで保護された Enclave 内のすべての操作は、相互に信頼します。 つまりキーチェーン項目復号化を承認するために Touch ID の認証の結果を使用しました。 セキュリティで保護された Enclave では、テスト_ケース、ユーザーがなります、パスコードを使用してに戻すには、Touch ID 照合の失敗のカウンターも保持されます。
IOS 8 の新しいフレームワークと呼ばれる_ローカル認証_デバイス内の認証でのこのプロセスをサポートしています。 これは、次のセクションでナビゲートはします。

## <a name="local-authentication"></a>ローカルの認証

私たちは、前のセクションで確立されると、アプリケーションはデバイスに設定されているセキュリティ ポリシーに準拠しているユーザーを認証するのにローカルの認証を使用できます。

現時点では、API は、2 つだけの機能を提供します。 まず、新しいキーチェーン アクセス制御リスト (Acl) を使用して既存のキーチェーン サービスを支援します。 キーチェーン データは、ユーザーの指紋の成功した認証を使用してロックできないことができます。

次に、LocalAuthentication は、アプリケーションをローカルでの認証に 2 つのメソッドを提供します。 開発者が使用する必要があります`CanEvaluatePolicy`デバイスが Touch ID を受け入れられるかを判断し、`EvaluatePolicy`認証操作を開始します。

両方の機能には、ローカルの認証が提供して、アプリケーションまたはユーザーがリモート サーバーを認証するためのメカニズムは提供されません。
ローカルの認証は、認証用の新しい標準ユーザー インターフェイスを提供します。 Touch ID の場合これは、以下に示すように 2 つのボタンで、アラートの表示です。 キャンセル、認証-パスコードの代替手段を使用する 1 つをクリックします。 カスタム メッセージを設定する必要もあります。 使用してこの Touch ID の認証が必要な理由をユーザーに説明することをお勧めします。

[![](touchid-images/image12.png "Touch ID の認証のアラート")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>キーチェーン サービス

について説明しました少し早く方法キーチェーン項目は、セキュリティで保護された enclave を使用して、パスコードを確認し、復号化します。 8、iOS でローカル認証フォールバック メカニズム、またはパスワードの実装を提供、アクセス制御リスト機能と組み合わせて Touch ID の検証を要求に使用できます。
ACL を使用する必要がありますを使用する、`SecAccessControl`ポリシー、およびを使用して、デバイスの状態をチェック`SecAccessible.WhenPasscodeSetThisDeviceOnly`または`SecAccessible.WhenUnlocked`です。

#### <a name="considerations-with-acl"></a>ACL での考慮事項

さまざまな方法で留意すべきキーチェーンで ACL を使用する場合があるし、これらのいくつか以下に示します。

-   バック グラウンド スレッドの呼び出しは失敗でキーチェーン操作を呼び出す場合は、フォア グラウンド アプリケーション – にのみ使用します。
-   追加して、キーチェーン アイテムの更新は、認証を必要があります。
-   場合は、要求には、キーチェーンに複数の一致する項目が返されます、認証が必要な場合があります。
-   ACL で保護されている項目は、デバイスのみ、そのため同期やではなくバックアップします。

### <a name="using-local-authentication-without-keychain-services"></a>キーチェーンのサービスのない状態のローカルの認証を使用します。

ローカルの認証は、パスコードまたは Touch ID などの資格情報を収集して、ユーザー認証を完了するセキュリティで保護された Enclave を操作する手段として作成されました。 アプリと相互に直接通信できるセキュリティで保護された Enclave 間のブリッジと考えます。 アプリケーションのポリシーの評価も使用できます。

これを行うアプリケーションには、ローカルの認証は、セキュリティで保護された Enclave 内の操作を開始内のポリシーの評価を呼び出します。 直接クエリを実行する/へのアクセスをセキュリティで保護 Enclave せず、アプリに認証を行うためにこれを利用できます。

[![](touchid-images/image13a.png "キーチェーンのサービスのない状態のローカルの認証を使用します。")](touchid-images/image13a.png#lightbox)

たとえば、個々 のデバイスの所有者、または保護者による制限を支援する銀行取引アプリケーションなどの専用の機能のロックを解除するために、ユーザーの検証を実装する簡単な方法を提供、アプリケーションでローカルの認証を使用アプリケーション。 既に存在するための認証を拡張する手段として使用することもできます – ユーザーなどのセキュリティで保護するには、その情報が、ご利用のオプションにもします。

ローカルの認証のセキュリティは、キーチェーンの動作とは異なります。 たとえば、キーチェーンを使用する場合、信頼は、オペレーティング システムとセキュリティで保護された Enclave 間です。 ローカルの認証が、アプリケーションと、オペレーティング システム、セキュリティで保護された Enclave いない、セキュリティで保護された Enclave 自体の結果へのアクセスのみがあることを意味します。

セキュリティに関しても非常に重要ですがあることを知っている**アクセスなし**指が登録されている、または指紋の画像にします。 セキュリティで保護された Enclave この情報の所有者であるし、他のシステム コンポーネントへのアクセス権がないためです。

キーチェーンせず Touch ID を使用して、ローカルの認証 API を活用することを使用できるいくつかの関数があります。 これら以下で詳しく説明します。

*   `CanEvaluatePolicy` -これを簡単にデバイスがタッチ ID を受け入れられるかどうかがチェックされます。
*   `EvaluatePolicy` – この認証操作を開始し、UI を表示し、返します、、`true`または`false`応答します。
*   `DeviceOwnerAuthenticationWithBiometrics` – これはポリシー Touch ID の画面を表示するために使用できます。 注意が必要、パスコードのフォールバック メカニズムここではありません、代わりに Touch ID の認証のスキップをユーザーに使用できるように、アプリケーションでこのフォールバックを実装する必要があります。

これには、以下に示す、ローカルの認証を使用すると、いくつかの注意事項があります。

*   キーチェーンと同様にのみ実行できますフォア グラウンドで。 バック グラウンド スレッドで呼び出しが失敗することになります。
*   ポリシーの評価が失敗することに注意してください。 パスコードのボタンは、フォールバックとして実装する必要があります。
*   指定する必要があります、`localizedReason`認証が必要な理由を説明します。 これにより、ユーザーとの信頼を構築します。

次に、以下のセクションでを見てみましょうをこれらの注意事項を考慮に入れて、API を実装する方法です。

## <a name="adding-touch-id-to-your-application"></a>Touch ID をアプリケーションに追加します。

前のセクションでは、キーチェーンとローカルの認証を使用して認証とアクセスの背後にある理論的に説明しました。 みましょうで Touch ID をアプリに統合する方法を確認します。

### <a name="walkthrough"></a>チュートリアル

認証の追加によって Touch ID、アプリケーションを見ていきましょう。 このチュートリアルでは、ここを使用して、[ストーリー ボード テーブル](https://developer.xamarin.com/samples/StoryboardTable/)サンプルです。 デバイスの所有者を何かこの一覧に追加できる、だけされないようにするためにすべてのユーザーが項目を追加することで、いっぱいになっていることを確認したいと考えてください。

1.  このサンプルをダウンロードして、for mac の Visual Studio で実行
2.  ダブルクリックして`MainStoryboard.Storyboard`iOS デザイナーでサンプルを開きます。 このサンプルは、アプリケーションでは、認証を制御する新しい画面を追加することができます。 これは、現在の前に移動`MasterViewController`です。
3.  新しいドラッグ**ビュー コント ローラー**から、**ツールボックス**を**デザイン サーフェイス**です。 設定として、**ビュー コント ローラーのルート**によって**ctrl キーを押しながらドラッグ**から、**ナビゲーション コント ローラー**:

    [![](touchid-images/image4.png "ルート ビュー コント ローラーを設定します。")](touchid-images/image4.png#lightbox)
4.  新しいビュー コント ローラーの名前を付けます`AuthenticationViewController`です。
5.  次に、ボタンをドラッグし、配置、`AuthenticationViewController`です。 これを呼び出して`AuthenticateButton`、し、テキストを付けます`Add a Chore`です。
6.  イベントを作成、`AuthenticateButton`と呼ばれる`AuthenticateMe`です。
7.  手動作成から話題`AuthenticationViewController`下部黒のバーをクリックして、 **ctrl キーを押しながらドラッグ**バーから、`MasterViewController`を選択して**プッシュ**(または**表示**クラスを使用するサイズ): 場合

    [![](touchid-images/image5.png "プッシュを選択して、MasterViewController バーからドラッグするかを表示します。")](touchid-images/image6.png#lightbox)
8.  をクリックして、新しく作成された話題し、識別子を与える`AuthenticationSegue`下図のように。

    [![](touchid-images/image7.png "Segue 識別子 AuthenticationSegue に設定します。")](touchid-images/image7.png#lightbox)
9.  `AuthenticationViewController` に次のコードを追加します。

    ```
    partial void AuthenticateMe (UIButton sender)
        {
            var context = new LAContext();
            NSError AuthError;
            var myReason = new NSString("To add a new chore");


            if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError)){
                var replyHandler = new LAContextReplyHandler((success, error) => {

                    this.InvokeOnMainThread(()=>{
                        if(success){
                            Console.WriteLine("You logged in!");
                            PerformSegue("AuthenticationSegue", this);
                        }
                        else{
                            //Show fallback mechanism here
                        }
                    });

                });
                context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, myReason, replyHandler);
            };
        }
    ```

これは、ローカルの認証を使用して Touch ID の認証を実装する必要があるすべてのコードです。 次の図で強調表示された行は、ローカルの認証の使用を示しています。

[![](touchid-images/image8.png "強調表示された行は、ローカルの認証の使用を表示します。")](touchid-images/image8.png#lightbox)

最初に、デバイスがタッチの ID を使用して、入力を受け入れられるかどうかを確立する必要があります、`CanEvaluatePolicy`ポリシーを渡して`DeviceOwnerAuthenticationWithBiometrics`です。 これは true のかどうかは、Touch ID UI を使用して表示できます`EvaluatePolicy`です。 3 つの情報を渡す必要がある`EvaluatePolicy`– ポリシー自体、認証が必要な場合は、理由を説明する文字列と、応答ハンドラー。 応答ハンドラーは、成功または失敗した場合、認証の場合に実行する動作をアプリケーションに指示します。 応答ハンドラー見ていきましょう。

[![](touchid-images/image9.png "応答ハンドラー")](touchid-images/image9.png#lightbox)

応答ハンドラーは型の指定`LAContextReplyHandler`、パラメーターが成功した – を取り、`bool`値、および`NSError`と呼ばれる`error`です。 成功した場合は、これは、ここでは実際に実行何でもは – を認証するここでは新しい面倒な作業を追加お知らせする画面を表示します。 ローカルの認証の際の注意点の 1 つは元にいる必要があります、フォア グラウンドで実行してください使用することを確認して`InvokeOnMainThread`:。

[![](touchid-images/image10.png "ローカルの認証を使用して InvokeOnMainThread")](touchid-images/image10.png#lightbox)

最後に、認証が正常に実行されると、ときにしたいへの移行、`MasterViewController`です。 `PerformSegue`メソッドは、これを行うために使用できます。

[![](touchid-images/image11.png "PerformSegue メソッドを呼び出して、MasterViewController への移行")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>まとめ
このガイドでは、キーチェーンと iOS のしくみについて説明しました。 キーチェーンの ACL も調査し、iOS でこれを変更します。 次に、これは iOS 8 の新機能であり、アプリケーションで Touch ID の認証の実装に検査し、ローカル認証フレームワークを見てをかかりました。

## <a name="related-links"></a>関連リンク

- [タッチ ID のサンプル](https://developer.xamarin.com/samples/StoryboardTable_LocalAuthentication)
- [キーチェーン WWDC サンプル](https://developer.xamarin.com/samples/KeychainTouchID/)
- [キーチェーン (サンプル)](https://developer.xamarin.com/samples/Keychain/)
