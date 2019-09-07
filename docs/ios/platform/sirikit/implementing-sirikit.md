---
title: Xamarin での SiriKit の実装
description: このドキュメントでは、Xamarin iOS アプリで SiriKit サポートを実装するために必要な手順について説明します。 インテント拡張とインテント UI 拡張について説明します。
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: 40bea05c86e83a0b96ad35b49b25bdada89f4201
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769796"
---
# <a name="implementing-sirikit-in-xamarinios"></a>Xamarin での SiriKit の実装

_この記事では、Xamarin iOS アプリで SiriKit サポートを実装するために必要な手順について説明します。_

IOS 10 の新機能である SiriKit を使用すると、Xamarin iOS アプリは、Siri と、iOS デバイス上の Maps アプリを使用して、ユーザーがアクセスできるサービスを提供できます。 この記事では、必要なインテント拡張機能、インテント UI 拡張機能、およびボキャブラリを追加することによって、Xamarin iOS アプリで SiriKit サポートを実装するために必要な手順について説明します。

Siri は、**ドメイン**の概念、関連するタスクの既知のアクションのグループと連携して機能します。 Siri を使用したアプリの相互作用は、次のいずれかの既知のサービスドメインに分類される必要があります。

- オーディオまたはビデオの呼び出し。
- 乗り物を予約します。
- ワークスペースの管理。
- メッセージング.
- 写真を検索しています。
- 支払いの送信または受信。

ユーザーがアプリ拡張機能のいずれかのサービスに関連する Siri の要求を行うと、SiriKit は、ユーザーの要求とサポートデータを記述する**インテント**オブジェクトを拡張機能に送信します。 その後、アプリ拡張機能は、指定された**インテント**に対して適切な**応答**オブジェクトを生成し、拡張機能が要求を処理する方法を詳述します。

このガイドでは、既存のアプリに SiriKit のサポートを含める簡単な例を紹介します。 この例では、フェイク MonkeyChat アプリを使用します。

[![](implementing-sirikit-images/monkeychat01.png "MonkeyChat アイコン")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat では、ユーザーの友人に固有の連絡先ブックが保持されます。各ユーザーは、(Bobo などのように) 画面名に関連付けられています。また、ユーザーは、画面名で各フレンドにテキストチャットを送信できます。

## <a name="extending-the-app-with-sirikit"></a>SiriKit を使用したアプリの拡張

[SiriKit の概念](~/ios/platform/sirikit/understanding-sirikit.md)ガイドに示されているように、sirikit を使用したアプリの拡張には3つの主要な部分が含まれています。

[![](implementing-sirikit-images/elements01.png "SiriKit 図を使用したアプリの拡張")](implementing-sirikit-images/elements01.png#lightbox)

不足している機能には次が含まれます。

1. **インテント拡張**-ユーザーの応答を検証し、アプリが要求を処理できることを確認し、実際にユーザーの要求を満たすためにタスクを実行します。
2. **インテント ui 拡張** - *オプション*は、siri 環境の応答にカスタム UI を提供し、ユーザーのエクスペリエンスを向上させるためにアプリの ui とブランド化を siri に取り込むことができます。
3. **アプリ**-アプリケーションにユーザー固有のボキャブラリを提供して、siri を使用できるようにします。 

これらの要素とアプリに追加する手順については、以下のセクションで詳しく説明します。

## <a name="preparing-the-app"></a>アプリの準備

SiriKit は拡張機能に基づいて構築されていますが、アプリに拡張機能を追加する前に、開発者が SiriKit の導入を支援するために行う必要があるいくつかの点があります。

### <a name="moving-common-shared-code"></a>移動 (共通の共有コードを) 

まず、開発者は、アプリと拡張機能の間で共有されるいくつかの共通コードを_共有プロジェクト_、_ポータブルクラスライブラリ (pcl)_ 、または_ネイティブライブラリ_に移動できます。

拡張機能は、アプリが行うすべての処理を実行できる必要があります。 サンプル MonkeyChat アプリの観点では、連絡先の検索、新しい連絡先の追加、メッセージの送信、メッセージ履歴の取得などを行います。

この共通コードを共有プロジェクト、PCL、またはネイティブライブラリに移動することにより、そのコードを1つの一般的な場所で簡単に管理し、拡張機能と親アプリがユーザーに対して統一されたエクスペリエンスと機能を提供できるようになります。

サンプルアプリ MonkeyChat の場合、データモデルと、ネットワークやデータベースへのアクセスなどの処理コードは、ネイティブライブラリに移動されます。

次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac を開始し、MonkeyChat アプリを開きます。
2. **Solution Pad**でソリューション名を右クリックし、[**新しいプロジェクト**の**追加** > ] を選択します。 

    [![](implementing-sirikit-images/prep01.png "新しいプロジェクトの追加")](implementing-sirikit-images/prep01.png#lightbox)
3. [ **IOS** > **ライブラリ**クラスライブラリ] を選択し、[次へ] ボタンをクリックします。 >  

    [![](implementing-sirikit-images/prep02.png "クラスライブラリの選択")](implementing-sirikit-images/prep02.png#lightbox)
4. 名前`MonkeyChatCommon`として「」と入力し、 **[作成]** ボタンをクリックします。 

    [![](implementing-sirikit-images/prep03.png "名前に「MonkeyChatCommon」と入力します。")](implementing-sirikit-images/prep03.png#lightbox)
5. **ソリューションエクスプローラー**でメインアプリの **[参照]** フォルダーを右クリックし、 **[参照の編集]** を選択します。**Monkeychatcommon**プロジェクトを確認し、 **[OK]** ボタンをクリックします。 

    [![](implementing-sirikit-images/prep05.png "MonkeyChatCommon プロジェクトを確認する")](implementing-sirikit-images/prep05.png#lightbox)
6. **ソリューションエクスプローラー**で、メインアプリからネイティブライブラリに共通の共有コードをドラッグします。
7. MonkeyChat の場合は、メインアプリからネイティブライブラリに**DataModels**と**processor**フォルダーをドラッグします。 

    [![](implementing-sirikit-images/prep06.png "ソリューションエクスプローラーの DataModels および Processor フォルダー")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio を起動し、MonkeyChat アプリを開きます。
2. **ソリューションエクスプローラー**でソリューション名を右クリックし、[**新しいプロジェクト**の**追加** > ] を選択します。
3. [**ビジュアルC#**   > **共有プロジェクト**] を選択し、 **[次へ]** ボタンをクリックします。 

    [![](implementing-sirikit-images/prep02.w157-sml.png "クラスライブラリの選択")](implementing-sirikit-images/prep02.w157.png#lightbox)
4. 名前`MonkeyChatCommon`として「」と入力し、 **[作成]** ボタンをクリックします。
5. **ソリューションエクスプローラー**でメインアプリの **[参照]** フォルダーを右クリックし、 **[参照の編集]** を選択します。**Monkeychatcommon**プロジェクトを確認し、 **[OK]** ボタンをクリックします。 

    [![](implementing-sirikit-images/prep05w.png "MonkeyChatCommon プロジェクトを確認する")](implementing-sirikit-images/prep05w.png#lightbox)
6. **ソリューションエクスプローラー**で、メインアプリから共有プロジェクトに共通の共有コードをドラッグします。
7. MonkeyChat の場合は、メインアプリからネイティブライブラリに**DataModels**と**processor**のフォルダーをドラッグします。

-----

ネイティブライブラリに移動されたファイルを編集し、ライブラリの名前空間と一致するように名前空間を変更します。 たとえば、次の`MonkeyChat`よう`MonkeyChatCommon`に変更します。

```csharp
using System;
namespace MonkeyChatCommon
{
    /// <summary>
    /// A message sent from one user to another within a conversation.
    /// </summary>
    public class MonkeyMessage
    {
        public MonkeyMessage ()
        {
        }
        ...
    }
}
```

次に、メインアプリに戻り、アプリが移動`using`されたクラスの1つを使用するすべての場所で、ネイティブライブラリの名前空間のステートメントを追加します。

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using MonkeyChatCommon;

namespace MonkeyChat
{
    public partial class MasterViewController : UITableViewController
    {
        public DetailViewController DetailViewController { get; set; }

        DataSource dataSource;
        ...
    }
}
```

### <a name="architecting-the-app-for-extensions"></a>拡張機能用のアプリの設計

通常、アプリは複数のインテントにサインアップします。開発者は、適切な数のインテント拡張に合わせてアプリを設計する必要があります。

アプリが複数の目的を必要とする状況では、開発者はすべてのインテント処理を1つのインテント拡張に配置するか、インテントごとに個別のインテント拡張を作成するかを選択できます。

各インテントに対して個別のインテント拡張を作成することを選択した場合、開発者は、各拡張機能に大量の定型コードを複製し、大量のプロセッサとメモリのオーバーヘッドを作成する可能性があります。

2つのオプションのいずれかを選択するには、意図が一体になっているかどうかを確認します。 たとえば、オーディオとビデオの呼び出しを行ったアプリでは、同様のタスクを処理しているため、コードを再利用することができるため、これらのインテントを1つのインテント拡張に含めることが必要になる場合があります。

既存のグループに適合しないインテントまたはインテントのグループについては、アプリのソリューションに新しいインテント拡張を作成し、それらを含めることができます。

### <a name="setting-the-required-entitlements"></a>必要な権利を設定する

SiriKit 統合を含むすべての Xamarin iOS アプリには、正しい権利が設定されている必要があります。 開発者がこれらの必要な権利を正しく設定していない場合、iOS 10 シミュレーターは SiriKit をサポートしていないので、実際の iOS 10 (またはそれ以上) ハードウェアでアプリをインストールしたりテストしたりすることはできません。

次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. `Entitlements.plist` **ソリューションエクスプローラー**内のファイルをダブルクリックして、編集用に開きます。
2. **[ソース]** タブに切り替えます。
3. `Boolean` `Yes`プロパティを追加し、型をに設定し、値をに設定します。 `com.apple.developer.siri` 

    [![](implementing-sirikit-images/setup01.png "\"Com...\" プロパティを追加します。")](implementing-sirikit-images/setup01.png#lightbox)
4. 変更内容をファイルに保存します。
5. **ソリューションエクスプローラー**内の**プロジェクトファイル**をダブルクリックして、編集用に開きます。
6. **[IOS バンドル署名]** を選択し`Entitlements.plist` 、 **[カスタム権利]** フィールドでファイルが選択されていることを確認します。 

    [![](implementing-sirikit-images/setup02.png "[カスタム権利] フィールドで、権利の plist ファイルを選択します。")](implementing-sirikit-images/setup02.png#lightbox)
7. **[OK]** ボタンをクリックして、変更を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. `Entitlements.plist` **ソリューションエクスプローラー**内のファイルをダブルクリックして、編集用に開きます。
2. `Boolean` `Yes`プロパティを追加し、型をに設定し、値をに設定します。 `com.apple.developer.siri` 

    [![](implementing-sirikit-images/setup01w.png "\"Com...\" プロパティを追加します。")](implementing-sirikit-images/setup01w.png#lightbox)
3. 変更内容をファイルに保存します。
4. **ソリューションエクスプローラー**内の**プロジェクトファイル**をダブルクリックして、編集用に開きます。
5. **[IOS バンドル署名]** を選択し`Entitlements.plist` 、 **[カスタム権利]** フィールドでファイルが選択されていることを確認します。

-----

完了すると、アプリの`Entitlements.plist`ファイルは次のようになります (外部エディターで開かれている)。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.siri</key>
    <true/>
</dict>
</plist>
```

### <a name="correctly-provisioning-the-app"></a>アプリを正しくプロビジョニングする

Apple が SiriKit フレームワークの周囲に配置した厳密なセキュリティにより、SiriKit を実装するすべての Xamarin iOS アプリは、正しいアプリ ID と権利を持っている_必要があり_ます (上記のセクションを参照)。また、適切なプロビジョニングプロファイルで署名されている必要があります。

Mac で次の操作を行います。

1. Web ブラウザーで、に[https://developer.apple.com](https://developer.apple.com)移動し、アカウントにログインします。
2. [**証明書**、**識別子**、**プロファイル**] をクリックします。
3. **[プロビジョニングプロファイル]** を選択し、 **[アプリ id]** を選択して、 **+** ボタンをクリックします。
4. 新しいプロファイルの**名前**を入力します。
5. Apple の名前付けに関する推奨事項に従って、**バンドル ID**を入力します。
6. **[App Services]** セクションまで下にスクロールし、 **[sirikit]** を選択して **[続行]** ボタンをクリックします。 

    [![](implementing-sirikit-images/setup03.png "SiriKit の選択")](implementing-sirikit-images/setup03.png#lightbox)
7. すべての設定を確認し、アプリ ID を**送信**します。
8. [**プロビジョニングプロファイル** > の**開発**] を **+** 選択し、ボタンをクリックして、 **Apple ID**を選択し、 **[続行]** をクリックします。
9. **[すべて]** 選択 をクリックし、 **[続行]** をクリックします。
10. **[すべて選択]** を再度クリックし、 **[続行]** をクリックします。
11. Apple の名前付け候補を使用して**プロファイル名**を入力し、[ **Continue (続行**)] をクリックします。
12. Xcode を起動します。
13. Xcode メニューから **基本設定...** を選択します。
14. **[アカウント]** を選択し、 **[詳細の表示...]** をクリックします。 ; 

    [![](implementing-sirikit-images/setup04.png "アカウントの選択")](implementing-sirikit-images/setup04.png#lightbox)
15. 左下隅にある **[すべてのプロファイルをダウンロード]** ボタンをクリックします。 

    [![](implementing-sirikit-images/setup05.png "すべてのプロファイルのダウンロード")](implementing-sirikit-images/setup05.png#lightbox)
16. 上記で作成した**プロビジョニングプロファイル**が Xcode にインストールされていることを確認します。
17. Visual Studio for Mac のに SiriKit サポートを追加するプロジェクトを開きます。
18. `Info.plist` **ソリューションエクスプローラー**内のファイルをダブルクリックします。
19. **バンドル id**が Apple の開発者ポータルで作成したものと一致していることを確認します。 

    [![](implementing-sirikit-images/setup06.png "バンドル識別子")](implementing-sirikit-images/setup06.png#lightbox)
20. **ソリューションエクスプローラー**で、**プロジェクト**を選択します。
21. プロジェクトを右クリックし、 **[オプション]** を選択します。
22. **[IOS バンドル署名]** を選択し、上で作成した**署名 Id**と**プロビジョニングプロファイル**を選択します。 

    [![](implementing-sirikit-images/setup07.png "署名 Id とプロビジョニングプロファイルを選択します")](implementing-sirikit-images/setup07.png#lightbox)
23. **[OK]** ボタンをクリックして、変更を保存します。

> [!IMPORTANT]
> SiriKit のテストは、iOS 10 シミュレーターではなく、実際の iOS 10 ハードウェアデバイスでのみ機能します。 SiriKit が有効になっている Xamarin iOS アプリを実際のハードウェアにインストールする際に問題が発生した場合は、Apple の開発者ポータルと Visual Studio for Mac の両方で、必要な権利、アプリ ID、署名 Id、およびプロビジョニングプロファイルが正しく構成されていることを確認してください。

### <a name="requesting-siri-authorization"></a>Siri 承認の要求

アプリがユーザー固有のボキャブラリを追加する前、またはインテント拡張が Siri に接続する前に、ユーザーから Siri にアクセスするための承認を要求する必要があります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

アプリの`Info.plist`ファイルを編集し、**ソース** `NSSiriUsageDescription`ビューに切り替えて、アプリが siri を使用する方法と送信されるデータの種類を記述する文字列値を使用してキーを追加します。 たとえば、MonkeyChat アプリは、"MonkeyChat 連絡先が Siri に送信される" と表示される場合があります。

[![](implementing-sirikit-images/request01.png "情報 plist エディターの Nssiriている説明")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

アプリの`Info.plist`ファイルを編集し、アプリ`NSSiriUsageDescription`で siri を使用する方法と送信されるデータの種類を記述する文字列値を使用して、キーを追加します。 たとえば、MonkeyChat アプリは、"MonkeyChat 連絡先が Siri に送信される" と表示される場合があります。

[![](implementing-sirikit-images/request01w.png "情報 plist エディターの Nssiriている説明")](implementing-sirikit-images/request01w.png#lightbox)

-----

アプリが`RequestSiriAuthorization`初めて起動`INPreferences`するときに、クラスのメソッドを呼び出します。 クラスを編集し、メソッド`FinishedLaunching`を次のようにします。 `AppDelegate.cs`

```csharp
using Intents;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{

    // Request access to Siri
    INPreferences.RequestSiriAuthorization ((INSiriAuthorizationStatus status) => {
        // Respond to returned status
        switch (status) {
        case INSiriAuthorizationStatus.Authorized:
            break;
        case INSiriAuthorizationStatus.Denied:
            break;
        case INSiriAuthorizationStatus.NotDetermined:
            break;
        case INSiriAuthorizationStatus.Restricted:
            break;
        }
    });

    return true;
}
```

このメソッドが初めて呼び出されたときに、アプリが Siri にアクセスすることを許可するように求めるアラートがユーザーに表示されます。 このアラートには、 `NSSiriUsageDescription`上に追加した開発者によって追加されたメッセージが表示されます。 ユーザーが最初にアクセスを拒否した場合、ユーザーは**設定**アプリを使用してアプリへのアクセスを許可できます。

アプリはいつでも、 `SiriAuthorizationStatus` `INPreferences`クラスのメソッドを呼び出すことによって、アプリケーションが siri にアクセスできるかどうかを確認できます。

### <a name="localization-and-siri"></a>ローカリゼーションと Siri

IOS デバイスでは、ユーザーは、システムの既定値とは異なる、Siri の言語を選択できます。 ローカライズされたデータを使用する場合、アプリは、 `SiriLanguageCode` `INPreferences`クラスのメソッドを使用して、siri から言語コードを取得する必要があります。 例えば:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>ユーザー固有のボキャブラリの追加

ユーザー固有の語彙は、アプリの個々のユーザーに固有の語句を提供します。 これらは、実行時に (アプリの拡張機能ではなく) メインアプリから順序付けされた一連の用語として提供され、ユーザーにとって最も重要な使用優先度で並べ替えられます。

ユーザー固有の語彙は、次のいずれかのカテゴリに属している必要があります。

- 連絡先フレームワークで管理されていない連絡先名。
- フォトタグ。
- フォトアルバム名。
- トレーニング名。

カスタムボキャブラリとして登録する用語を選択するときは、アプリに慣れていないユーザーによって誤解される可能性のある用語のみを選択してください。 "マイトレーニング" や "マイアルバム" などの一般的な用語を登録しないでください。 たとえば、MonkeyChat アプリは、ユーザーのアドレス帳の各連絡先に関連付けられているニックネームを登録します。

アプリは、 `SetVocabularyStrings` `INVocabulary`クラスのメソッドを呼び出し、メインアプリからコレクションを`NSOrderedSet`渡すことによって、ユーザー固有のボキャブラリを提供します。 新しい用語を追加する前`RemoveAllVocabularyStrings`に、アプリは常にメソッドを呼び出して、既存の用語を削除する必要があります。 例えば:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Foundation;
using Intents;

namespace MonkeyChatCommon
{
    public class MonkeyAddressBook : NSObject
    {
        #region Computed Properties
        public List<MonkeyContact> Contacts { get; set; } = new List<MonkeyContact> ();
        #endregion

        #region Constructors
        public MonkeyAddressBook ()
        {
        }
        #endregion

        #region Public Methods
        public NSOrderedSet<NSString> ContactNicknames ()
        {
            var nicknames = new NSMutableOrderedSet<NSString> ();

            // Sort contacts by the last time used
            var query = Contacts.OrderBy (contact => contact.LastCalledOn);

            // Assemble ordered list of nicknames by most used to least
            foreach (MonkeyContact contact in query) {
                nicknames.Add (new NSString (contact.ScreenName));
            }

            // Return names
            return new NSOrderedSet<NSString> (nicknames.AsSet ());
        }

        // This method MUST only be called on a background thread!
        public void UpdateUserSpecificVocabulary ()
        {
            // Clear any existing vocabulary
            INVocabulary.SharedVocabulary.RemoveAllVocabularyStrings ();

            // Register new vocabulary
            INVocabulary.SharedVocabulary.SetVocabularyStrings (ContactNicknames (), INVocabularyStringType.ContactName);
        }
        #endregion
    }
}
```

このコードを配置すると、次のように呼び出されることがあります。

```csharp
using System;
using System.Threading;
using UIKit;
using MonkeyChatCommon;
using Intents;

namespace MonkeyChat
{
    public partial class ViewController : UIViewController
    {
        #region AppDelegate Access
        public AppDelegate ThisApp {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do we have access to Siri?
            if (INPreferences.SiriAuthorizationStatus == INSiriAuthorizationStatus.Authorized) {
                // Yes, update Siri's vocabulary
                new Thread (() => {
                    Thread.CurrentThread.IsBackground = true;
                    ThisApp.AddressBook.UpdateUserSpecificVocabulary ();
                }).Start ();
            }
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

> [!IMPORTANT]
> Siri はカスタムボキャブラリをヒントとして扱い、できるだけ多くの用語を組み込みます。 ただし、カスタムボキャブラリのスペースは限られているので、混乱する可能性のある用語_だけ_を登録して、登録された用語の合計数を最小限に抑えることが重要です。

詳細については、[ユーザー固有のボキャブラリリファレンス](~/ios/platform/sirikit/understanding-sirikit.md)に関する記事を参照してください。また、Apple は[カスタムボキャブラリ参照を指定](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)しています。

### <a name="adding-app-specific-vocabulary"></a>アプリ固有のボキャブラリの追加

アプリ固有のボキャブラリは、車両の種類やトレーニング名など、アプリのすべてのユーザーに認識される特定の語と語句を定義します。 これらはアプリケーションの一部であるため、メインアプリケーションバンドルの`AppIntentVocabulary.plist`一部としてファイルで定義されます。 また、これらの単語と語句はローカライズする必要があります。

アプリ固有のボキャブラリの用語は、次のいずれかのカテゴリに属している必要があります。

- 乗り物オプション。
- トレーニング名。

アプリ固有のボキャブラリファイルには、次の2つのルートレベルキーが含まれています。

- `ParameterVocabularies`**必須**-アプリのカスタム用語と、それらが適用されるインテントパラメーターを定義します。
- `IntentPhrases`**省略可能**-で定義されているカスタム用語を`ParameterVocabularies`使用して、語句の例が含まれています。

の`ParameterVocabularies`各エントリには、ID 文字列、用語、および用語が適用されるインテントを指定する必要があります。 また、1つの用語が複数のインテントに適用される場合もあります。

許容される値と必要なファイルの構造の完全な一覧については、「Apple の[アプリボキャブラリファイル形式のリファレンス](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)」を参照してください。

アプリプロジェクトに`AppIntentVocabulary.plist`ファイルを追加するには、次の手順を実行します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **ソリューションエクスプローラー**でプロジェクト名を右クリックし、[**新しいファイル**の**追加** > ] を選択します。 >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "プロパティリストの追加")](implementing-sirikit-images/plist01.png#lightbox)
2. `AppIntentVocabulary.plist` **ソリューションエクスプローラー**内のファイルをダブルクリックして、編集用に開きます。
3. キーを **+** 追加するには、をクリックします`ParameterVocabularies` 。**名前**をに`Array`設定し、**型**をに設定します。

    [![](implementing-sirikit-images/plist02.png "名前を ParameterVocabularies に、型を配列に設定します。")](implementing-sirikit-images/plist02.png#lightbox)
4. を`ParameterVocabularies`展開して **+** ボタンをクリックし 、型`Dictionary`をに設定します。

    [![](implementing-sirikit-images/plist03.png "型を Dictionary に設定する")](implementing-sirikit-images/plist03.png#lightbox)
5. `ParameterNames` `Array`をクリックして新しいキーを追加し、名前をに、型をに設定します。 **+**

    [![](implementing-sirikit-images/plist04.png "名前を ParameterNames に、型を配列に設定します。")](implementing-sirikit-images/plist04.png#lightbox)
6. をクリックして、の`String`種類がで新しいキーを追加し、値を使用可能なパラメーター名の1つとして追加します。 **+** たとえば、 `INStartWorkoutIntent.workoutName`次のようになります。

    [![](implementing-sirikit-images/plist05.png "Instartワークスペースキーを指定します。")](implementing-sirikit-images/plist05.png#lightbox)
7. キーをの`Array`**種類**でキー`ParameterVocabularies`に追加 `ParameterVocabulary`します。

    [![](implementing-sirikit-images/plist06.png "ParameterVocabulary キーを配列の型と共に Parametervocabulary キーに追加します。")](implementing-sirikit-images/plist06.png#lightbox)
8. の`Dictionary`**種類**の新しいキーを追加します。

    [![](implementing-sirikit-images/plist07.png "辞書の種類を使用して新しいキーを追加する")](implementing-sirikit-images/plist07.png#lightbox)
9. `VocabularyItemIdentifier`の型`String`を持つキーを追加し、用語の一意の ID を指定します。

    [![](implementing-sirikit-images/plist08.png "文字列の型を使用して VocabularyItemIdentifier キーを追加し、一意の ID を指定します。")](implementing-sirikit-images/plist08.png#lightbox)
10. の`VocabularyItemSynonyms` 型`Array`を使用して、キーを追加します。

    [![](implementing-sirikit-images/plist09.png "配列の型を使用して VocabularyItemSynonyms キーを追加します。")](implementing-sirikit-images/plist09.png#lightbox)
11. の`Dictionary`**種類**の新しいキーを追加します。

    [![](implementing-sirikit-images/plist10.png "辞書の種類を使用して新しいキーを追加する")](implementing-sirikit-images/plist10.png#lightbox)
12. の`VocabularyItemPhrase` 種類`String`とアプリで定義されている用語を使用して、キーを追加します。

    [![](implementing-sirikit-images/plist11.png "文字列の種類と、アプリが定義している用語を使用して、VocabularyItemPhrase キーを追加します。")](implementing-sirikit-images/plist11.png#lightbox)
13. という語句の**種類と発音**の発音を持つキーを追加します。`VocabularyItemPronunciation` `String`

    [![](implementing-sirikit-images/plist12.png "文字列の型と語句の発音の発音を含む VocabularyItemPronunciation キーを追加します。")](implementing-sirikit-images/plist12.png#lightbox)
14. の`VocabularyItemExamples` 型`Array`を使用して、キーを追加します。

    [![](implementing-sirikit-images/plist13.png "配列の型を使用して VocabularyItemExamples キーを追加します。")](implementing-sirikit-images/plist13.png#lightbox)
15. 用語の使用`String`例を含むいくつかのキーを追加します。

    [![](implementing-sirikit-images/plist14.png "用語の使用例を含むいくつかの文字列キーを追加する")](implementing-sirikit-images/plist14.png#lightbox)
16. アプリで定義する必要があるその他のカスタム用語に対して、上記の手順を繰り返します。
17. キーを`ParameterVocabularies`折りたたみます。
18. の`IntentPhrases` 型`Array`を使用して、キーを追加します。

    [![](implementing-sirikit-images/plist15.png "配列の型を使用して IntentPhrases キーを追加します。")](implementing-sirikit-images/plist15.png#lightbox)
19. の`Dictionary`**種類**の新しいキーを追加します。

    [![](implementing-sirikit-images/plist16.png "辞書の種類を使用して新しいキーを追加する")](implementing-sirikit-images/plist16.png#lightbox)
20. 次の例のように、 `String`の種類と目的を持つキーを追加します。`IntentName`

    [![](implementing-sirikit-images/plist17.png "この例では、文字列の種類とインテントを使用して IntentName キーを追加します。")](implementing-sirikit-images/plist17.png#lightbox)
21. の`IntentExamples` 型`Array`を使用して、キーを追加します。

    [![](implementing-sirikit-images/plist18.png "配列の型を使用して IntentExamples キーを追加します。")](implementing-sirikit-images/plist18.png#lightbox)
22. 用語の使用`String`例を含むいくつかのキーを追加します。

    [![](implementing-sirikit-images/plist19.png "用語の使用例を含むいくつかの文字列キーを追加する")](implementing-sirikit-images/plist19.png#lightbox)
23. アプリでの使用例を提供する必要がある場合は、上記の手順を繰り返します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**でプロジェクト名を右クリックし、[ **> 新しい項目の追加] を選択します。> Apple > プロパティリスト > 情報 plist**:

    [![](implementing-sirikit-images/plist01.w157-sml.png "新しい情報を追加します。 plist")](implementing-sirikit-images/plist01.w157.png#lightbox)

2. `AppIntentVocabulary.plist` **ソリューションエクスプローラー**内のファイルをダブルクリックして、編集用に開きます。
3. キーを **+** 追加するには、をクリックします`ParameterVocabularies` 。**名前**をに`Array`設定し、**型**をに設定します。

    [![](implementing-sirikit-images/plist02w.png "名前を ParameterVocabularies に、型を配列に設定します。")](implementing-sirikit-images/plist02w.png#lightbox)
4. を`ParameterVocabularies`展開して **+** ボタンをクリックし 、型`Dictionary`をに設定します。

    [![](implementing-sirikit-images/plist03w.png "型を Dictionary に設定する")](implementing-sirikit-images/plist03w.png#lightbox)
5. `ParameterNames` `Array`をクリックして新しいキーを追加し、名前をに、型をに設定します。 **+**

    [![](implementing-sirikit-images/plist04w.png "名前を ParameterNames に、型を配列に設定します。")](implementing-sirikit-images/plist04w.png#lightbox)
6. をクリックして、の`String`種類がで新しいキーを追加し、値を使用可能なパラメーター名の1つとして追加します。 **+** たとえば、 `INStartWorkoutIntent.workoutName`次のようになります。

    [![](implementing-sirikit-images/plist05w.png "Instartワークスペースキーを指定します。")](implementing-sirikit-images/plist05w.png#lightbox)
7. キーをの`Array`**種類**でキー`ParameterVocabularies`に追加 `ParameterVocabulary`します。

    [![](implementing-sirikit-images/plist06w.png "ParameterVocabulary キーを配列の型と共に Parametervocabulary キーに追加します。")](implementing-sirikit-images/plist06w.png#lightbox)
8. の`Dictionary`**種類**の新しいキーを追加します。

    [![](implementing-sirikit-images/plist07w.png "辞書の種類を使用して新しいキーを追加する")](implementing-sirikit-images/plist07w.png#lightbox)
9. `VocabularyItemIdentifier`の型`String`を持つキーを追加し、用語の一意の ID を指定します。

    [![](implementing-sirikit-images/plist08w.png "文字列の型を使用して VocabularyItemIdentifier キーを追加し、用語の一意の ID を指定します。")](implementing-sirikit-images/plist08w.png#lightbox)
10. の`VocabularyItemSynonyms` 型`Array`を使用して、キーを追加します。

    [![](implementing-sirikit-images/plist09w.png "配列の型を使用して VocabularyItemSynonyms キーを追加します。")](implementing-sirikit-images/plist09w.png#lightbox)
11. の`Dictionary`**種類**の新しいキーを追加します。

    [![](implementing-sirikit-images/plist10w.png "辞書の種類を使用して新しいキーを追加する")](implementing-sirikit-images/plist10w.png#lightbox)
12. の`VocabularyItemPhrase` 種類`String`とアプリで定義されている用語を使用して、キーを追加します。

    [![](implementing-sirikit-images/plist11w.png "文字列の種類と、アプリが定義している用語を使用して、VocabularyItemPhrase キーを追加します。")](implementing-sirikit-images/plist11w.png#lightbox)
13. という語句の**種類と発音**の発音を持つキーを追加します。`VocabularyItemPronunciation` `String`

    [![](implementing-sirikit-images/plist12w.png "文字列の型と語句の発音の発音を含む VocabularyItemPronunciation キーを追加します。")](implementing-sirikit-images/plist12w.png#lightbox)
14. の`VocabularyItemExamples` 型`Array`を使用して、キーを追加します。

    [![](implementing-sirikit-images/plist13w.png "配列の型を使用して VocabularyItemExamples キーを追加します。")](implementing-sirikit-images/plist13w.png#lightbox)
15. 用語の使用`String`例を含むいくつかのキーを追加します。

    [![](implementing-sirikit-images/plist14w.png "用語の使用例を含むいくつかの文字列キーを追加する")](implementing-sirikit-images/plist14w.png#lightbox)
16. アプリで定義する必要があるその他のカスタム用語に対して、上記の手順を繰り返します。
17. キーを`ParameterVocabularies`折りたたみます。
18. の`IntentPhrases` 型`Array`を使用して、キーを追加します。

    [![](implementing-sirikit-images/plist15w.png "配列の型を使用して IntentPhrases キーを追加します。")](implementing-sirikit-images/plist15w.png#lightbox)
19. の`Dictionary`**種類**の新しいキーを追加します。

    [![](implementing-sirikit-images/plist16w.png "辞書の種類を使用して新しいキーを追加する")](implementing-sirikit-images/plist16w.png#lightbox)
20. 次の例のように、 `String`の種類と目的を持つキーを追加します。`IntentName`

    [![](implementing-sirikit-images/plist17w.png "この例では、文字列の種類とインテントを使用して IntentName キーを追加します。")](implementing-sirikit-images/plist17w.png#lightbox)
21. の`IntentExamples` 型`Array`を使用して、キーを追加します。

    [![](implementing-sirikit-images/plist18w.png "配列の型を使用して IntentExamples キーを追加します。")](implementing-sirikit-images/plist18w.png#lightbox)
22. 用語の使用`String`例を含むいくつかのキーを追加します。

    [![](implementing-sirikit-images/plist19w.png "用語の使用例を含むいくつかの文字列キーを追加する")](implementing-sirikit-images/plist19w.png#lightbox)
23. アプリでの使用例を提供する必要がある場合は、上記の手順を繰り返します。

-----

> [!IMPORTANT]
> は`AppIntentVocabulary.plist` 、開発中にテストデバイスで siri に登録されます。そのため、siri にカスタムボキャブラリが組み込まれるまでに時間がかかることがあります。 その結果、テスト担当者は、更新されたときにアプリ固有のボキャブラリのテストを試行するまで数分待つ必要があります。

詳細については、[アプリ固有のボキャブラリリファレンス](~/ios/platform/sirikit/understanding-sirikit.md)に関する記事と、Apple による[カスタムボキャブラリ参照の指定](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)に関する記事をご覧ください。

## <a name="adding-an-intents-extension"></a>インテント拡張の追加

これで、アプリが SiriKit を採用する準備ができたので、開発者は Siri 統合に必要なインテントを処理するために、ソリューションに1つ (または複数) のインテント拡張機能を追加する必要があります。

必要なインテント拡張ごとに、次の手順を実行します。

- インテント拡張プロジェクトを Xamarin. iOS アプリソリューションに追加します。
- インテント拡張`Info.plist`ファイルを構成します。
- インテント拡張 main クラスを変更します。

詳細について[は、インテント拡張のリファレンス](~/ios/platform/sirikit/understanding-sirikit.md)に関する記事と、[インテント拡張のリファレンスを作成する](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1)Apple を参照してください。

### <a name="creating-the-extension"></a>拡張機能の作成

インテント拡張をソリューションに追加するには、次の手順を実行します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で**ソリューション名**を右クリックし、 **追加** > 追加 **新しいプロジェクト...** の順に選択します。
2. ダイアログボックスで [ **iOS** > **Extensions** > **インテント拡張**] を選択し、 **[次へ]** ボタンをクリックします。 

    [![](implementing-sirikit-images/intents05.png "インテント拡張機能の選択")](implementing-sirikit-images/intents05.png#lightbox)
3. 次に、インテント拡張機能の**名前**を入力し、 **[次へ]** ボタンをクリックします。 

    [![](implementing-sirikit-images/intents06.png "インテント拡張機能の名前を入力してください")](implementing-sirikit-images/intents06.png#lightbox)
4. 最後に、 **[作成]** ボタンをクリックして、アプリソリューションにインテント拡張を追加します。 

    [![](implementing-sirikit-images/intents07.png "アプリソリューションにインテント拡張機能を追加する")](implementing-sirikit-images/intents07.png#lightbox)
5. **ソリューションエクスプローラー**で、新しく作成されたインテント拡張機能の **[参照]** フォルダーを右クリックします。 共通の共有コードライブラリプロジェクト (上で作成したアプリ) の名前を確認し、 **[OK]** ボタンをクリックします。 

    [![](implementing-sirikit-images/intents08.png "共通の共有コードライブラリプロジェクトの名前を選択します")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で**ソリューション名**を右クリックし、 **追加** > 追加 **新しいプロジェクト...** の順に選択します。
2. ダイアログボックスで **[Visual C# > IOS Extensions > インテント拡張]** を選択し、 **[次へ]** ボタンをクリックします。

    [![](implementing-sirikit-images/intents05.w157-sml.png "インテント拡張機能の選択")](implementing-sirikit-images/intents05.w157.png#lightbox)
3. 次に、インテント拡張機能の**名前**を入力し、 **[OK]** ボタンをクリックします。
4. **ソリューションエクスプローラー**で、新しく作成したインテント拡張の **[参照]** フォルダーを右クリックし、 **[> 参照の追加]** を選択します。 共通の共有コードライブラリプロジェクト (上で作成したアプリ) の名前を確認し、 **[OK]** ボタンをクリックします。

    [![](implementing-sirikit-images/intents08w.png "共通の共有コードライブラリプロジェクトの名前を選択します")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

アプリが必要とするインテント拡張機能の数 (前述の「[拡張機能のアプリの設計](#architecting-the-app-for-extensions)」を参照) に対して、これらの手順を繰り返します。

### <a name="configuring-the-infoplist"></a>情報 plist の構成

アプリのソリューションに追加されたインテント拡張については、アプリで動作するよう`Info.plist`にファイル内でを構成する必要があります。

一般的なアプリ拡張機能と同様に、アプリにはと`NSExtension` `NSExtensionAttributes`の既存のキーがあります。 インテント拡張機能には、次の2つの新しい属性を構成する必要があります。

[![](implementing-sirikit-images/intents01.png "2つの新しい属性を構成する必要があります。")](implementing-sirikit-images/intents01.png#lightbox)

- **Intentssupported** -は必須であり、インテント拡張機能からアプリがサポートするインテントクラス名の配列で構成されます。
- **IntentsRestrictedWhileLocked** -拡張機能のロック画面の動作を指定するための、アプリのオプションのキーです。 インテントクラス名の配列で構成されています。これは、インテント拡張から使用するためにユーザーがログインする必要があることを目的としています。

インテント拡張機能の`Info.plist`ファイルを構成するには、**ソリューションエクスプローラー**でそれをダブルクリックして開き、編集します。 次に、**ソース**ビューに切り替えて、エディター `NSExtension`で`NSExtensionAttributes`キーとキーを展開します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents02.png "エディターの NSExtension キーと NSExtensionAttributes キー")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents02w.png "エディターの NSExtension キーと NSExtensionAttributes キー")](implementing-sirikit-images/intents02w.png#lightbox)

-----

`IntentsSupported`キーを展開し、この拡張機能がサポートするインテントクラスの名前を追加します。 MonkeyChat アプリの例では、次の`INSendMessageIntent`ものがサポートされています。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents09.png "INSendMessageIntent キー")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents09w.png "INSendMessageIntent キー")](implementing-sirikit-images/intents09w.png#lightbox)

-----

アプリで、任意のインテントを使用するためにユーザーがデバイスにログオンすることを必要とする`IntentRestrictedWhileLocked`場合は、キーを展開して、アクセスが制限されているインテントのクラス名を追加します。 MonkeyChat アプリの例では、次のように追加`INSendMessageIntent`したチャットメッセージを送信するためにユーザーがログインしている必要があります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents10.png "追加された INSendMessageIntent キー")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents10w.png "追加された INSendMessageIntent キー")](implementing-sirikit-images/intents10w.png#lightbox)

-----

使用可能なインテントドメインの完全な一覧については、「Apple の[インテントドメインのリファレンス](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)」を参照してください。

### <a name="configuring-the-main-class"></a>Main クラスの構成

次に、開発者は、インテント拡張機能のメインエントリポイントとして機能するメインクラスを Siri に構成する必要があります。 デリゲートに準拠するの`INExtension`サブクラスである必要があります。 `IINIntentHandler` 例えば:

```csharp
using System;
using System.Collections.Generic;

using Foundation;
using Intents;

namespace MonkeyChatIntents
{
    [Register ("IntentHandler")]
    public class IntentHandler : INExtension, IINSendMessageIntentHandling, IINSearchForMessagesIntentHandling, IINSetMessageAttributeIntentHandling
    {
        #region Constructors
        protected IntentHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override NSObject GetHandler (INIntent intent)
        {
            // This is the default implementation.  If you want different objects to handle different intents,
            // you can override this and return the handler you want for that particular intent.

            return this;
        }
        #endregion
        ...
    }
}
```

アプリがインテント拡張 main クラス ( `GetHandler`メソッド) に実装する必要がある1つの単独メソッドがあります。 このメソッドは、SiriKit によってインテントが渡されます。アプリは、指定されたインテントの種類と一致する**インテントハンドラー**を返す必要があります。

例の monkeychat アプリは1つのインテントのみを処理するため、 `GetHandler`メソッドでそれ自体を返します。 拡張機能が複数の目的を処理した場合、開発者は、各インテント型にクラスを追加し、メソッドに`Intent`渡されたに基づいて、ここでインスタンスを返します。

### <a name="handling-the-resolve-stage"></a>解決段階の処理

解決段階では、インテント拡張機能が Siri から渡されたパラメーターを明確にして検証し、ユーザーのメッセージ交換によって設定されていることを確認します。

Siri から送信される各パラメーターには、 `Resolve`メソッドがあります。 アプリは、ユーザーからの正しい回答を得るために、アプリケーションが Siri のヘルプを必要とする可能性のあるすべてのパラメーターに対して、このメソッドを実装する必要があります。

MonkeyChat アプリの例の場合、インテント拡張機能では、メッセージの送信先として1人以上の受信者が必要です。 リスト内の各受信者に対して、拡張機能は次の結果を持つ連絡先検索を実行する必要があります。

- 一致する連絡先が1つだけ見つかりました。
- 2つ以上の一致する連絡先が見つかりました。
- 一致する連絡先が見つかりません。

また、MonkeyChat では、メッセージ本文の内容が必要です。 ユーザーがこれを指定していない場合、Siri はユーザーにコンテンツの入力を求める必要があります。

インテント拡張機能では、これらの各ケースを適切に処理する必要があります。

```csharp
[Export ("resolveRecipientsForSearchForMessages:withCompletion:")]
public void ResolveRecipients (INSendMessageIntent intent, Action<INPersonResolutionResult []> completion)
{
    var recipients = intent.Recipients;
    // If no recipients were provided we'll need to prompt for a value.
    if (recipients.Length == 0) {
        completion (new INPersonResolutionResult [] { (INPersonResolutionResult)INPersonResolutionResult.NeedsValue });
        return;
    }

    var resolutionResults = new List<INPersonResolutionResult> ();

    foreach (var recipient in recipients) {
        var matchingContacts = new INPerson [] { recipient }; // Implement your contact matching logic here to create an array of matching contacts
        if (matchingContacts.Length > 1) {
            // We need Siri's help to ask user to pick one from the matches.
            resolutionResults.Add (INPersonResolutionResult.GetDisambiguation (matchingContacts));
        } else if (matchingContacts.Length == 1) {
            // We have exactly one matching contact
            resolutionResults.Add (INPersonResolutionResult.GetSuccess (recipient));
        } else {
            // We have no contacts matching the description provided
            resolutionResults.Add ((INPersonResolutionResult)INPersonResolutionResult.Unsupported);
        }
    }

    completion (resolutionResults.ToArray ());
}

[Export ("resolveContentForSendMessage:withCompletion:")]
public void ResolveContent (INSendMessageIntent intent, Action<INStringResolutionResult> completion)
{
    var text = intent.Content;
    if (!string.IsNullOrEmpty (text))
        completion (INStringResolutionResult.GetSuccess (text));
    else
        completion ((INStringResolutionResult)INStringResolutionResult.NeedsValue);
}
```

詳細については、「[解決段階のリファレンス](~/ios/platform/sirikit/understanding-sirikit.md)」および「Apple の[解決および処理インテントの参照](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1)」を参照してください。

### <a name="handling-the-confirm-stage"></a>確認段階の処理

確認段階では、インテント拡張機能によって、ユーザーの要求を満たすためにすべての情報が含まれているかどうかが確認されます。 アプリは、必要な操作であることをユーザーに確認できるように、Siri に対して何が起こっているかの詳細をすべてサポートしていることを確認します。

```csharp
[Export ("confirmSendMessage:completion:")]
public void ConfirmSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Verify user is authenticated and the app is ready to send a message.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Ready, userActivity);
    completion (response);
}
```

詳細については、「[確認段階の参照](~/ios/platform/sirikit/understanding-sirikit.md)」を参照してください。

### <a name="processing-the-intent"></a>インテントの処理

これは、インテント拡張が実際にユーザーの要求を満たすタスクを実行し、結果を Siri に渡してユーザーに通知できるようにするためのポイントです。

```csharp
public void HandleSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Implement the application logic to send a message here.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, userActivity);
    completion (response);
}

public void HandleSearchForMessages (INSearchForMessagesIntent intent, Action<INSearchForMessagesIntentResponse> completion)
{
    // Implement the application logic to find a message that matches the information in the intent.
    ...

    var userActivity = new NSUserActivity (nameof (INSearchForMessagesIntent));
    var response = new INSearchForMessagesIntentResponse (INSearchForMessagesIntentResponseCode.Success, userActivity);

    // Initialize with found message's attributes
    var sender = new INPerson (new INPersonHandle ("sarah@example.com", INPersonHandleType.EmailAddress), null, "Sarah", null, null, null);
    var recipient = new INPerson (new INPersonHandle ("+1-415-555-5555", INPersonHandleType.PhoneNumber), null, "John", null, null, null);
    var message = new INMessage ("identifier", "I am so excited about SiriKit!", NSDate.Now, sender, new INPerson [] { recipient });
    response.Messages = new INMessage [] { message };
    completion (response);
}

public void HandleSetMessageAttribute (INSetMessageAttributeIntent intent, Action<INSetMessageAttributeIntentResponse> completion)
{
    // Implement the application logic to set the message attribute here.
    ...

    var userActivity = new NSUserActivity (nameof (INSetMessageAttributeIntent));
    var response = new INSetMessageAttributeIntentResponse (INSetMessageAttributeIntentResponseCode.Success, userActivity);
    completion (response);
}
```

詳細について[は、ハンドルステージのリファレンス](~/ios/platform/sirikit/understanding-sirikit.md)を参照してください。

## <a name="adding-an-intents-ui-extension"></a>インテント UI 拡張機能の追加

省略可能なインテント UI 拡張は、アプリの UI とブランド化を Siri エクスペリエンスに導入し、ユーザーがアプリに接続できるようにする機会を提供します。 この拡張機能を使用すると、アプリは、視覚情報やその他の情報などの情報をトランスクリプトに取り込むことができます。

[![](implementing-sirikit-images/intentsui01.png "インテント UI 拡張出力の例")](implementing-sirikit-images/intentsui01.png#lightbox)

インテント拡張と同様に、開発者はインテント UI 拡張機能に対して次の手順を実行します。

- インテント UI 拡張プロジェクトを Xamarin. iOS アプリソリューションに追加します。
- インテント UI 拡張`Info.plist`ファイルを構成します。
- インテント UI 拡張の main クラスを変更します。

詳細について[は、インテント UI 拡張機能のリファレンス](~/ios/platform/sirikit/understanding-sirikit.md)と、Apple が[カスタムインターフェイス参照を提供](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1)していることを確認してください。

### <a name="creating-the-extension"></a>拡張機能の作成

インテント UI 拡張機能をソリューションに追加するには、次の手順を実行します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で**ソリューション名**を右クリックし、 **追加** > 追加 **新しいプロジェクト...** の順に選択します。
2. ダイアログボックスで [ **iOS** > **Extensions** > **インテント UI Extension** ] を選択し、 **[次へ]** ボタンをクリックします。 

    [![](implementing-sirikit-images/intents11.png "インテント UI 拡張機能の選択")](implementing-sirikit-images/intents11.png#lightbox)
3. 次に、インテント拡張機能の**名前**を入力し、 **[次へ]** ボタンをクリックします。 

    [![](implementing-sirikit-images/intents12.png "インテント拡張機能の名前を入力してください")](implementing-sirikit-images/intents12.png#lightbox)
4. 最後に、 **[作成]** ボタンをクリックして、アプリソリューションにインテント拡張を追加します。 

    [![](implementing-sirikit-images/intents13.png "アプリソリューションにインテント拡張機能を追加する")](implementing-sirikit-images/intents13.png#lightbox)
5. **ソリューションエクスプローラー**で、新しく作成されたインテント拡張機能の **[参照]** フォルダーを右クリックします。 共通の共有コードライブラリプロジェクト (上で作成したアプリ) の名前を確認し、 **[OK]** ボタンをクリックします。 

    [![](implementing-sirikit-images/intents14.png "共通の共有コードライブラリプロジェクトの名前を選択します")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で**ソリューション名**を右クリックし、 **[追加]**  > [**新しいプロジェクトの**追加] の順に選択します。
2. ダイアログボックスで [ **iOS** > **Extensions** > **インテント UI Extension** ] を選択し、 **[次へ]** ボタンをクリックします。
3. 次に、インテント拡張機能の**名前**を入力し、 **[OK]** ボタンをクリックします。
4. **ソリューションエクスプローラー**で、新しく作成されたインテント拡張機能の **[参照]** フォルダーを右クリックします。 共通の共有コードライブラリプロジェクト (上で作成したアプリ) の名前を確認し、 **[OK]** ボタンをクリックします。
    
-----

### <a name="configuring-the-infoplist"></a>情報 plist の構成

アプリで使用するインテント UI `Info.plist`拡張機能のファイルを構成します。

一般的なアプリ拡張機能と同様に、アプリにはと`NSExtension` `NSExtensionAttributes`の既存のキーがあります。 インテント拡張機能には、次のように構成する必要がある新しい属性が1つあります。

[![](implementing-sirikit-images/intents03.png "1つの新しい属性を構成する必要があります。")](implementing-sirikit-images/intents03.png#lightbox)

**Intentssupported**は必須であり、インテント拡張からアプリがサポートするインテントクラス名の配列で構成されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

インテント UI 拡張機能の`Info.plist`ファイルを構成するには、**ソリューションエクスプローラー**でそれをダブルクリックして開き、編集します。 次に、**ソース**ビューに切り替えて、エディター `NSExtension`で`NSExtensionAttributes`キーとキーを展開します。

[![](implementing-sirikit-images/intents04.png "エディターの NSExtension キーと NSExtensionAttributes キー")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

インテント UI 拡張機能の`Info.plist`ファイルを構成するには、**ソリューションエクスプローラー**でそれをダブルクリックして開き、編集します。 エディターでキー `NSExtensionAttributes`とキーを展開します。 `NSExtension`

[![](implementing-sirikit-images/intents04w.png "エディターでの t NSExtension キーと NSExtensionAttributes キー")](implementing-sirikit-images/intents04w.png#lightbox)

-----

`IntentsSupported`キーを展開し、この拡張機能がサポートするインテントクラスの名前を追加します。 MonkeyChat アプリの例では、次の`INSendMessageIntent`ものがサポートされています。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents15.png "INSendMessageIntent キー")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents15w.png "INSendMessageIntent キー")](implementing-sirikit-images/intents15w.png#lightbox)

-----

使用可能なインテントドメインの完全な一覧については、「Apple の[インテントドメインのリファレンス](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)」を参照してください。

### <a name="configuring-the-main-class"></a>Main クラスの構成

インテント UI 拡張機能のメインエントリポイントとして機能するメインクラスを Siri に構成します。 これは、 `IINUIHostedViewController`インターフェイスに準拠`UIViewController`するのサブクラスである必要があります。 例えば:

```csharp
using System;
using Foundation;
using CoreGraphics;
using Intents;
using IntentsUI;
using UIKit;

namespace MonkeyChatIntentsUI
{
    public partial class IntentViewController : UIViewController, IINUIHostedViewControlling
    {
        #region Constructors
        protected IntentViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }

        public override void DidReceiveMemoryWarning ()
        {
            // Releases the view if it doesn't have a superview.
            base.DidReceiveMemoryWarning ();

            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Public Methods
        [Export ("configureWithInteraction:context:completion:")]
        public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
        {
            // Do configuration here, including preparing views and calculating a desired size for presentation.

            if (completion != null)
                completion (DesiredSize ());
        }

        [Export ("desiredSize:")]
        public CGSize DesiredSize ()
        {
            return ExtensionContext.GetHostedViewMaximumAllowedSize ();
        }
        #endregion
    }
}
```

Siri は、インテント`INInteraction` UI 拡張機能内`Configure`の`UIViewController`インスタンスのメソッドにクラスインスタンスを渡します。

オブジェクト`INInteraction`は、拡張機能に次の3つの重要な情報を提供します。

1. 処理されるインテントオブジェクト。
2. インテント拡張のメソッド`Confirm`および`Handle`メソッドからのインテント応答オブジェクト。
3. アプリと Siri 間の相互作用の状態を定義する相互作用状態。

インスタンスは、siri との相互作用のための基本クラスであり、 `UIViewController`から継承されるため、uikit のすべての機能にアクセスできます。 `UIViewController`

Siri が`UIViewController`のメソッド`Configure`を呼び出すと、ビューコントローラーが siri Snippit または Maps カードでホストされることを示すビューコンテキストが渡されます。

Siri は、アプリの構成が完了した後に、アプリがビューの目的のサイズを返すために必要な完了ハンドラーも渡します。

### <a name="design-the-ui-in-ios-designer"></a>IOS Designer で UI を設計する

インテント UI 拡張機能のユーザーインターフェイスを iOS デザイナーでレイアウトします。 ソリューションエクスプローラー内の拡張機能の`MainInterface.storyboard`ファイルをダブルクリックして、編集用に開きます。 すべての必要な UI 要素をドラッグして、ユーザーインターフェイスを構築し、変更を保存します。

> [!IMPORTANT]
> `UIButtons`または`UITextFields` の`UIViewController`ような対話型の要素をインテント ui 拡張機能に追加することはできますが、これらは、非対話型のインテント ui として厳密には禁止されているため、ユーザーは対話できません。

### <a name="wire-up-the-user-interface"></a>ユーザーインターフェイスを接続する

IOS Designer で作成したインテント UI 拡張機能のユーザーインターフェイスを使用`UIViewController`して、サブ`Configure`クラスを編集し、次のようにメソッドをオーバーライドします。

```csharp
[Export ("configureWithInteraction:context:completion:")]
public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
{
    // Do configuration here, including preparing views and calculating a desired size for presentation.
    ...

    // Return desired size
    if (completion != null)
        completion (DesiredSize ());
}

[Export ("desiredSize:")]
public CGSize DesiredSize ()
{
    return ExtensionContext.GetHostedViewMaximumAllowedSize ();
}
```

### <a name="overriding-the-default-siri-ui"></a>既定の Siri UI のオーバーライド

インテント UI 拡張は、常に、アプリのアイコンや名前などの他の Siri コンテンツと共に表示されます。また、目的に応じて、ボタン ([送信] や [キャンセル] など) が下部に表示される場合もあります。

既定では、アプリケーションは、Siri によって表示されている情報を既定でユーザーに置き換えることができます。このような場合、アプリは既定のエクスペリエンスをアプリに合わせて置き換えることができます。

インテント ui 拡張で既定の siri UI の要素をオーバーライドする必要がある`UIViewController`場合、サブクラスは`IINUIHostedViewSiriProviding`インターフェイスを実装し、特定のインターフェイス要素を表示するためのオプトインを行う必要があります。

次のコードを`UIViewController`サブクラスに追加して、インテント UI 拡張で既にメッセージの内容が表示されていることを確認します。

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>考慮事項

Apple は、意図した UI 拡張機能を設計および実装するときに、開発者が次の考慮事項を考慮することを提案します。

- **メモリ使用量を意識する**-インテント UI 拡張は一時的であり、短時間のみ表示されるため、システムは完全なアプリで使用されるよりも多くのメモリ制約を課します。
- **ビューのサイズの最小値と最大値を考慮して**ください。すべての iOS デバイスの種類、サイズ、および向きに対してインテント UI 拡張機能が適切であることを確認してください。 さらに、アプリから Siri に返される目的のサイズを付与できない場合があります。
- **柔軟でアダプティブなレイアウトパターンを使用**して、すべてのデバイスで UI が最適に見えるようにします。

## <a name="summary"></a>Summary

この記事では、SiriKit について説明しました。また、iOS デバイスで Siri と Maps アプリを使用してユーザーがアクセスできるサービスを提供するために、その機能を Xamarin の iOS アプリに追加する方法についても説明しました。

## <a name="related-links"></a>関連リンク

- [サンプルの登録 zach](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-elizachat)
- [SiriKit プログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [インテントフレームワークリファレンス](https://developer.apple.com/reference/intents)
- [インテント UI フレームワークリファレンス](https://developer.apple.com/reference/intentsui)
- [インテントドメインのリファレンス](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
