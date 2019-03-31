---
title: Xamarin.iOS で SiriKit の実装
description: このドキュメントでは、Xamarin.iOS アプリで SiriKit のサポートを実装するために必要な手順について説明します。 Intents の拡張機能および Intents UI 拡張機能について説明します。
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/03/2018
ms.openlocfilehash: 2c3bddc89348b46c9bba277580071cb8ac3d6943
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2019
ms.locfileid: "58678055"
---
# <a name="implementing-sirikit-in-xamarinios"></a>Xamarin.iOS で SiriKit の実装

_この記事では、Xamarin.iOS アプリで SiriKit のサポートを実装するために必要な手順について説明します。_

新しい ios 10 では、SiriKit により、Xamarin.iOS アプリを iOS デバイスで Siri とマップ アプリを使用してユーザーにアクセスできるサービスを提供します。 この記事では、必要な Intents 拡張機能、Intents UI 拡張機能およびボキャブラリを追加することで Xamarin.iOS アプリで SiriKit のサポートを実装するために必要な手順について説明します。

Siri の動作の概念を**ドメイン**のグループが関連するタスクの操作を把握します。 Siri アプリに含まれる各操作は必要がありますには、次のようにその既知のサービスのドメインのいずれかに分類されます。

- オーディオまたはビデオ通話。
- 素敵を予約します。
- トレーニングを管理します。
- メッセージング。
- 写真を検索します。
- 送信または支払いを受信します。

SiriKit 送信、拡張機能で、ユーザーがアプリの拡張機能のサービスのいずれかに関連する siri 要求を行うと、**インテント**の関連データと共に、ユーザーの要求を記述するオブジェクト。 アプリ拡張機能は、適切な生成**応答**オブジェクトを指定された**インテント**、拡張機能が要求を処理する方法の詳細を示します。

このガイドでは、既存のアプリケーションを SiriKit のサポートなどの簡単な例を紹介します。 この例で使用する偽 MonkeyChat アプリ。

[![](implementing-sirikit-images/monkeychat01.png "MonkeyChat アイコン")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat ユーザーの友人の連絡先独自書籍を保持する、各 (Bobo など) のような画面の名前に関連付けられたを画面の名前で各友人にテキストのチャットを送信できます。

## <a name="extending-the-app-with-sirikit"></a>SiriKit のアプリを拡張します。

ように、 [SiriKit の概念について](~/ios/platform/sirikit/understanding-sirikit.md)ガイド、SiriKit を使用してアプリを拡張に関連する 3 つの主な部分があります。

[![](implementing-sirikit-images/elements01.png "SiriKit のダイアグラムを含むアプリを拡張します。")](implementing-sirikit-images/elements01.png#lightbox)

不足している機能には次が含まれます。

1. **Intents の拡張機能**-ユーザーの応答を検証、ことを確認、アプリは、要求を処理でき、実際には、ユーザーの要求を満たすためにタスクを実行します。
2. **Intents UI 拡張機能** - *(省略可能)*、Siri 環境で応答するためのカスタム UI を提供します。 ユーザーのエクスペリエンスを強化するための Siri にブランド化およびとアプリの UI を取り込むことができます。
3. **アプリ**-Siri が作業を支援するために特定のボキャブラリをユーザーにアプリを提供します。 

すべてのこれらの要素と、アプリに含める手順は、以下のセクションで詳しく説明されます。


## <a name="preparing-the-app"></a>アプリを準備します。

SiriKit は拡張機能に基づいて、ただし、すべての拡張機能をアプリに追加する前に、いくつか SiriKit の導入を支援するために、開発者が必要があります。

### <a name="moving-common-shared-code"></a>共通の共有コードの移動 

最初に、開発者が共有される共通のコードの一部間で移動できますアプリと拡張機能に_共有プロジェクト_、_ポータブル クラス ライブラリ (Pcl)_ または_ネイティブライブラリ_します。

拡張機能は、すべてのアプリは、の操作を実行できるようにする必要があります。 サンプル MonkeyChat アプリで新しい連絡先を追加する連絡先の検索などの用語では、メッセージを送信し、メッセージの履歴を取得します。

共有プロジェクト、PCL、またはネイティブ ライブラリに、この一般的なコードを移動することは、簡単に 1 つの一般的な場所でそのコードを維持することと、により、拡張機能と親アプリケーションを提供統一されたエクスペリエンスと機能のユーザー。

MonkeyChat 例アプリの場合に、データ モデルとネットワークおよびデータベース アクセスなどの処理コードをネイティブ ライブラリに移動するされます。

次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac の Visual Studio を起動し、MonkeyChat アプリを開きます。
2. ソリューション名を右クリックし、 **Solution Pad**選択**追加** > **新しいプロジェクト.**: 

    [![](implementing-sirikit-images/prep01.png "新しいプロジェクトを追加します。")](implementing-sirikit-images/prep01.png#lightbox)
3. 選択**iOS** > **ライブラリ** > **クラス ライブラリ** をクリックし、**次**ボタン。 

    [![](implementing-sirikit-images/prep02.png "クラス ライブラリを選択します。")](implementing-sirikit-images/prep02.png#lightbox)
4. 入力`MonkeyChatCommon`の**名前** をクリックし、**作成**ボタン。 

    [![](implementing-sirikit-images/prep03.png "名前の MonkeyChatCommon を入力します。")](implementing-sirikit-images/prep03.png#lightbox)
5. 右クリックし、**参照**でメイン アプリケーションのフォルダー、**ソリューション エクスプ ローラー**選択**参照の編集.**.チェック、 **MonkeyChatCommon**プロジェクトをクリックして、 **OK**ボタン。 

    [![](implementing-sirikit-images/prep05.png "MonkeyChatCommon プロジェクトを確認してください。")](implementing-sirikit-images/prep05.png#lightbox)
6. **ソリューション エクスプ ローラー**、共通の共有コードをメイン アプリケーションからネイティブ ライブラリにドラッグします。
7. MonkeyChat の場合は、ドラッグ、 **DataModels**と**プロセッサ**ネイティブ ライブラリには、メインのアプリケーション フォルダー。 

    [![](implementing-sirikit-images/prep06.png "ソリューション エクスプ ローラーで、DataModels とプロセッサのフォルダー")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio を起動し、MonkeyChat アプリを開きます。
2. ソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクト.**.
3. 選択**Visual C#**   > **共有プロジェクト** をクリックし、**次**ボタン。 

    [![](implementing-sirikit-images/prep02.w157-sml.png "クラス ライブラリを選択します。")](implementing-sirikit-images/prep02.w157.png#lightbox)
4. 入力`MonkeyChatCommon`の**名前** をクリックし、**作成**ボタン。
5. 右クリックし、**参照**でメイン アプリケーションのフォルダー、**ソリューション エクスプ ローラー**選択**参照の編集.**.チェック、 **MonkeyChatCommon**プロジェクトをクリックして、 **OK**ボタン。 

    [![](implementing-sirikit-images/prep05w.png "MonkeyChatCommon プロジェクトを確認してください。")](implementing-sirikit-images/prep05w.png#lightbox)
6. **ソリューション エクスプ ローラー**、共通の共有コードをメイン アプリケーションから共有プロジェクトにドラッグします。
7. MonkeyChat の場合は、ドラッグ、 **DataModels**と**プロセッサ**ネイティブ ライブラリには、メインのアプリケーションのフォルダー。

-----

ネイティブ ライブラリに移動されたファイルのいずれかを編集し、ライブラリの一致するように名前空間を変更します。 たとえば、変更`MonkeyChat`に`MonkeyChatCommon`:

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

次に、メイン アプリケーションに戻るし、追加、`using`ステートメント ネイティブ ライブラリの名前空間の任意の場所、アプリで使用して移動されたクラスのいずれか。

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

### <a name="architecting-the-app-for-extensions"></a>アプリの拡張機能の設計

通常、アプリは複数のインテントにサインアップし、開発者が目的とした拡張機能の適切な数のアプリを設計することを確認する必要があります。

開発者では、アプリが 1 つ以上のインテントを必要とする場合、すべてそのインテントの処理の 1 つの目的とした拡張機能を配置するか、各インテントの別個の目的とした拡張機能の作成のオプションがあります。

各インテントの別個の目的とした拡張機能を作成する場合、開発者は最終的に大量の各拡張機能の定型コードを複製し、大量のプロセッサとメモリのオーバーヘッドを作成可能性があります。

2 つのオプションを選択するには、かどうか、インテントの必然的に属するを参照してください。 たとえば、オーディオとビデオの呼び出しを行っているアプリは同様のタスクを処理するように、単一目的の拡張機能でこれらのインテントの両方を追加したいし、ほとんどのコードの再利用を提供できます。

任意の目的または既存のグループに収まらないインテントのグループを使用して、それらを格納する、アプリのソリューションで目的の新しい拡張機能を作成します。


### <a name="setting-the-required-entitlements"></a>必要な権利の設定

SiriKit の統合を含むすべての Xamarin.iOS アプリする必要がありますが適切な権利を設定します。 開発者はこれらの必要な権利を正しく設定されていない場合、できなくインストールまたはハードウェア上で実際の iOS 10 (またはそれ以上)、これは iOS 10 以降の要件でもアプリをテストするシミュレーター SiriKit をサポートしていません。

次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. ダブルクリックして、`Entitlements.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
2. 切り替えて、**ソース**タブ。
3. 追加、 `com.apple.developer.siri` **プロパティ**、設定、**型**に`Boolean`と**値**に`Yes`: 

    [![](implementing-sirikit-images/setup01.png "Com.apple.developer.siri プロパティを追加します。")](implementing-sirikit-images/setup01.png#lightbox)
4. 変更内容をファイルに保存します。
5. ダブルクリックして、**プロジェクト ファイル**で、**ソリューション エクスプ ローラー**編集用に開きます。
6. 選択**iOS バンドル署名**いることを確認し、`Entitlements.plist`でファイルを選択、**カスタム権利**フィールド。 

    [![](implementing-sirikit-images/setup02.png "カスタム権利フィールド Entitlements.plist ファイルを選択します。")](implementing-sirikit-images/setup02.png#lightbox)
7. **[OK]** ボタンをクリックして、変更を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. ダブルクリックして、`Entitlements.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
3. 追加、 `com.apple.developer.siri` **プロパティ**、設定、**型**に`Boolean`と**値**に`Yes`: 

    [![](implementing-sirikit-images/setup01w.png "Com.apple.developer.siri プロパティを追加します。")](implementing-sirikit-images/setup01w.png#lightbox)
4. 変更内容をファイルに保存します。
5. ダブルクリックして、**プロジェクト ファイル**で、**ソリューション エクスプ ローラー**編集用に開きます。
6. 選択**iOS バンドル署名**いることを確認し、`Entitlements.plist`でファイルを選択、**カスタム権利**フィールド。

-----

完了したら、アプリの`Entitlements.plist`ファイルは次のようになります (で外部のエディターで開く)。

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

### <a name="correctly-provisioning-the-app"></a>アプリが正しくプロビジョニング

Apple は、SiriKit のフレームワーク、SiriKit を実装するすべての Xamarin.iOS アプリの周囲に配置が厳密なセキュリティのため_する必要があります_適切なアプリ ID と (前のセクションを参照してください) の利用資格があるし、適切なで署名する必要がありますプロビジョニング プロファイル。

Mac では、次の操作を行います。

1. Web ブラウザーでに移動します。 [ https://developer.apple.com ](https://developer.apple.com)と自分のアカウントにログインします。
2. をクリックして**証明書**、**識別子**と**プロファイル**します。
3. 選択**Provisioning Profiles**選択と**アプリ Id**、順にクリックして、 **+** ボタン。
4. 入力、**名前**新しいプロファイルの。
5. 入力、**バンドル ID**推奨事項の名前付けに従って Apple します。
6. 下へスクロールして、 **App Services**セクションで、 **SiriKit**  をクリックし、**続行**ボタン。 

    [![](implementing-sirikit-images/setup03.png "SiriKit を選択します。")](implementing-sirikit-images/setup03.png#lightbox)
7. すべての設定、し確認**送信**アプリ id。
8. 選択**Provisioning Profiles** > **開発**、クリックして、 **+** ボタンを選択、 **Apple ID**、クリックして**続行**します。
9. 選択 をクリックして**すべて**、 をクリックし、**続行**。
10. をクリックして**すべて選択**再度 をクリックし、**続行**します。
11. 入力、**プロファイル名**Apple を使用すると、推奨事項の名前付け、をクリックし、**続行**します。
12. Xcode を起動します。
13. Xcode メニュー から選択**設定しています.**
14. 選択**アカウント**、 をクリックし、**の詳細を表示しています.** ボタンをクリックします。 

    [![](implementing-sirikit-images/setup04.png "アカウントを選択します。")](implementing-sirikit-images/setup04.png#lightbox)
15. をクリックして、**すべてのプロファイルのダウンロード**左下隅のボタンをクリックします。 

    [![](implementing-sirikit-images/setup05.png "すべてのプロファイルをダウンロードします。")](implementing-sirikit-images/setup05.png#lightbox)
16. いることを確認、**プロビジョニング プロファイル**作成以降がインストールされている Xcode でします。
17. Visual studio for mac。 SiriKit サポートを追加するプロジェクトを開く
18. ダブルクリックして、`Info.plist`ファイル、**ソリューション エクスプ ローラー**します。
18. いることを確認、**バンドル識別子**上記の Apple の開発者ポータルで作成したものと一致します。 

    [![](implementing-sirikit-images/setup06.png "バンドル識別子")](implementing-sirikit-images/setup06.png#lightbox)
18. **ソリューション エクスプ ローラー**を選択、**プロジェクト**します。
19. プロジェクトを右クリックして**オプション**します。
21. 選択**iOS バンドル署名**を選択、**署名 Id**と**プロビジョニング プロファイル**上記で作成しました。 

    [![](implementing-sirikit-images/setup07.png "署名 Id とプロビジョニング プロファイルを選択します。")](implementing-sirikit-images/setup07.png#lightbox)
22. **[OK]** ボタンをクリックして、変更を保存します。

> [!IMPORTANT]
> SiriKit のテストでのみ機能し、実際の iOS 10 のハードウェア デバイス、iOS 10 ではなくシミュレーター。 Xamarin.iOS アプリを実際のハードウェアが有効な問題、SiriKit をインストールする場合は、必要な権利をアプリ ID、識別子の署名とプロビジョニング プロファイルが構成されている正しく Apple の開発者ポータルと Visual Studio の両方で for mac。 ことを確認します。

### <a name="requesting-siri-authorization"></a>Siri の承認を要求します。

アプリは、ユーザーの特定の語彙を追加または Siri に接続する Intents 拡張機能を前に Siri にアクセスするユーザーから承認を要求する必要があります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

アプリの編集`Info.plist`に切り替え、ファイル、**ソース**表示し、追加、 `NSSiriUsageDescription` Siri と何のアプリの使用方法を説明する文字列値を持つキーの種類のデータが送信されます。 たとえば、MonkeyChat アプリでは、「MonkeyChat 連絡先は Siri に送信する」と可能性があります。

[![](implementing-sirikit-images/request01.png "Info.plist エディターで NSSiriUsageDescription")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

アプリの編集`Info.plist`追加ファイルを開き、 `NSSiriUsageDescription` Siri と何のアプリの使用方法を説明する文字列値を持つキーの種類のデータが送信されます。 たとえば、MonkeyChat アプリでは、「MonkeyChat 連絡先は Siri に送信する」と可能性があります。

[![](implementing-sirikit-images/request01w.png "Info.plist エディターで NSSiriUsageDescription")](implementing-sirikit-images/request01w.png#lightbox)

-----

呼び出す、`RequestSiriAuthorization`のメソッド、`INPreferences`クラスのアプリを初めて起動したとき。 編集、`AppDelegate.cs`クラスを作成、`FinishedLaunching`メソッドの次のようになります。


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

このメソッドが呼び出されると、最初に、アプリケーションに Siri のアクセスを許可するように求めるアラートは表示します。 メッセージに、開発者が追加、`NSSiriUsageDescription`上、このアラートに表示されます。 使用できる、ユーザーには、最初に、アクセスが拒否された場合、**設定**アプリへのアクセスを許可するアプリです。

アプリが Siri を呼び出すことによってアクセスするアプリの機能の確認時に、いつでも、`SiriAuthorizationStatus`のメソッド、`INPreferences`クラス。

### <a name="localization-and-siri"></a>ローカリゼーションと Siri

IOS デバイスで、ユーザーは Siri とは異なるシステムの既定の言語を選択できます。 ローカライズされたデータを使用する場合、アプリが使用する必要があります、`SiriLanguageCode`のメソッド、 `INPreferences` Siri から言語のコードを取得するクラス。 例:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>特定の語彙のユーザーを追加します。

特定の語彙をユーザーがアプリの個々 のユーザーに固有の語句を提供しようとしています。 これらは、メイン アプリ (アプリ拡張機能) から実行時に、リストの先頭にある最も重要な用語を持つ、ユーザーの最も重要な使用の優先度で順序付け、用語の順序付けされたセットとして提供されます。

特定の語彙のユーザーは、次のカテゴリのいずれかに属する必要があります。

- 名前 (つまり、連絡先のフレームワークで管理されていない) にお問い合わせください。
- 写真のタグ。
- フォト アルバムの名前。
- トレーニングの名前。

カスタム語彙として登録する用語を選択する場合は、用語のみを選択して、アプリに慣れていないユーザーを誤解される可能性があります。 決して登録する一般的な用語「My トレーニング」または「My アルバム」など。 たとえば、MonkeyChat アプリは、各連絡先ユーザーのアドレス帳に関連付けられているニックネームを登録します。

アプリが呼び出すことによってユーザーの特定の語彙を提供します、`SetVocabularyStrings`のメソッド、`INVocabulary`クラスを渡して、`NSOrderedSet`メイン アプリから収集します。 アプリは常に呼び出す必要があります、`RemoveAllVocabularyStrings`メソッド最初は、新しいものを追加する前に、既存の用語を削除します。 例:

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

このコードで可能性があります呼び出すことが、次のように。

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
> Siri は、ヒントとしてカスタム語彙を処理し、可能な限り用語の多くが組み込まれます。 ただし、カスタム語彙がように登録する重要な制限のための領域_のみ_登録されている用語の合計数を最小限に抑えるため、複雑になる可能性がある用語。

詳細についてを参照してください、[ユーザー固有のボキャブラリ リファレンス](~/ios/platform/sirikit/understanding-sirikit.md)と Apple の[カスタム ボキャブラリの参照を指定する](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)します。

### <a name="adding-app-specific-vocabulary"></a>アプリ固有のボキャブラリの追加

アプリの特定のボキャブラリは、すべての車両のタイプやトレーニングの名前など、アプリのユーザーに、特定の単語や語句を認識するを定義します。 定義されているこれらは、アプリケーションの一部であるため、`AppIntentVocabulary.plist`メイン アプリ バンドルの一環としてファイル。 さらに、これらの単語や語句をローカライズする必要があります。

アプリ固有の用語は、次のカテゴリのいずれかに属する必要があります。

- オプションをオーバーライドします。
- トレーニングの名前。

アプリの特定の語彙ファイルには、2 つのルート レベルのキーが含まれています。

- `ParameterVocabularies` **必要な**-アプリのカスタムの条項とに適用される目的のパラメーターを定義します。
- `IntentPhrases` **省略可能な**-で定義されているカスタムの用語を使用して例のフレーズを含む、`ParameterVocabularies`します。

各エントリで、 `ParameterVocabularies` ID 文字列、用語と用語が適用される目的を指定する必要があります。 さらに、1 つの用語は、複数のインテントに適用可能性があります。

使用可能な値と必要なファイル構造の一覧については、Apple を参照してください[アプリ ボキャブラリ ファイル形式のリファレンス](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)します。

追加する、`AppIntentVocabulary.plist`アプリ プロジェクトにファイルで、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. プロジェクト名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいファイル.**  >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "プロパティ リストを追加します。")](implementing-sirikit-images/plist01.png#lightbox)
2. ダブルクリックして、`AppIntentVocabulary.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
3. をクリックして、 **+** キーを追加するには、設定、**名前**に`ParameterVocabularies`と**型**に`Array`:

    [![](implementing-sirikit-images/plist02.png "ParameterVocabularies と配列の型に名前を設定します。")](implementing-sirikit-images/plist02.png#lightbox)
4. 展開`ParameterVocabularies` をクリックし、 **+** ボタンをクリックし、設定、**型**に`Dictionary`:

    [![](implementing-sirikit-images/plist03.png "ディクショナリに、型を設定します。")](implementing-sirikit-images/plist03.png#lightbox)
5. をクリックして、 **+** 新しいキーを追加するには、設定、**名前**に`ParameterNames`と**型**に`Array`:

    [![](implementing-sirikit-images/plist04.png "ParameterNames と配列の型に名前を設定します。")](implementing-sirikit-images/plist04.png#lightbox)
6. をクリックして、 **+** を持つ新しいキーを追加する、**型**の`String`と利用可能なパラメーターの名前の 1 つとして値。 たとえば、 `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05.png "INStartWorkoutIntent.workoutName キー")](implementing-sirikit-images/plist05.png#lightbox)
7. 追加、`ParameterVocabulary`キーを`ParameterVocabularies`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist06.png "ParameterVocabulary キー型の配列で ParameterVocabularies キーを追加します。")](implementing-sirikit-images/plist06.png#lightbox)
8. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist07.png "ディクショナリの型を持つ新しいキーを追加します。")](implementing-sirikit-images/plist07.png#lightbox)
9. 追加、`VocabularyItemIdentifier`キーを**型**の`String`用語の一意の ID を指定。

    [![](implementing-sirikit-images/plist08.png "VocabularyItemIdentifier キー文字列の型を追加し、一意の ID を指定")](implementing-sirikit-images/plist08.png#lightbox)
10. 追加、`VocabularyItemSynonyms`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist09.png "VocabularyItemSynonyms キー型の配列を追加します。")](implementing-sirikit-images/plist09.png#lightbox)
11. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist10.png "ディクショナリの型を持つ新しいキーを追加します。")](implementing-sirikit-images/plist10.png#lightbox)
12. 追加、`VocabularyItemPhrase`キーを**型**の`String`と、アプリの定義は、用語。

    [![](implementing-sirikit-images/plist11.png "VocabularyItemPhrase キー文字列の型と、アプリを定義する用語を追加します。")](implementing-sirikit-images/plist11.png#lightbox)
13. 追加、`VocabularyItemPronunciation`キーを**型**の`String`と用語のふりがな。

    [![](implementing-sirikit-images/plist12.png "文字列型と、用語のふりがな VocabularyItemPronunciation キーを追加します。")](implementing-sirikit-images/plist12.png#lightbox)
14. 追加、`VocabularyItemExamples`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist13.png "VocabularyItemExamples キー型の配列を追加します。")](implementing-sirikit-images/plist13.png#lightbox)
15. いくつかの追加`String`という用語の使用例を使用してキー。

    [![](implementing-sirikit-images/plist14.png "用語の使用例をいくつかの文字列キーを追加します。")](implementing-sirikit-images/plist14.png#lightbox)
16. 上記の手順を定義する必要があるアプリのカスタム条件を繰り返します。
17. 折りたたみ、`ParameterVocabularies`キー。
18. 追加、`IntentPhrases`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist15.png "IntentPhrases キー型の配列を追加します。")](implementing-sirikit-images/plist15.png#lightbox)
19. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist16.png "ディクショナリの型を持つ新しいキーを追加します。")](implementing-sirikit-images/plist16.png#lightbox)
20. 追加、`IntentName`キーを**型**の`String`の例では、インテントと。

    [![](implementing-sirikit-images/plist17.png "例では、型の文字列と目的の IntentName キーを追加します。")](implementing-sirikit-images/plist17.png#lightbox)
21. 追加、`IntentExamples`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist18.png "IntentExamples キー型の配列を追加します。")](implementing-sirikit-images/plist18.png#lightbox)
22. いくつかの追加`String`という用語の使用例を使用してキー。

    [![](implementing-sirikit-images/plist19.png "用語の使用例をいくつかの文字列キーを追加します。")](implementing-sirikit-images/plist19.png#lightbox)
23. 上記の手順、アプリの使用例を提供する必要は任意のインテントを繰り返します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. プロジェクト名を右クリックし、**ソリューション エクスプ ローラー**選択**追加 > 新しい項目... > Apple > プロパティの一覧 > Info.plist**:

    [![](implementing-sirikit-images/plist01.w157-sml.png "新しい Info.plist を追加します。")](implementing-sirikit-images/plist01.w157.png#lightbox)

2. ダブルクリックして、`AppIntentVocabulary.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
3. をクリックして、 **+** キーを追加するには、設定、**名前**に`ParameterVocabularies`と**型**に`Array`:

    [![](implementing-sirikit-images/plist02w.png "ParameterVocabularies と配列の型に名前を設定します。")](implementing-sirikit-images/plist02w.png#lightbox)
4. 展開`ParameterVocabularies` をクリックし、 **+** ボタンをクリックし、設定、**型**に`Dictionary`:

    [![](implementing-sirikit-images/plist03w.png "ディクショナリに、型を設定します。")](implementing-sirikit-images/plist03w.png#lightbox)
5. をクリックして、 **+** 新しいキーを追加するには、設定、**名前**に`ParameterNames`と**型**に`Array`:

    [![](implementing-sirikit-images/plist04w.png "ParameterNames と配列の型に名前を設定します。")](implementing-sirikit-images/plist04w.png#lightbox)
6. をクリックして、 **+** を持つ新しいキーを追加する、**型**の`String`と利用可能なパラメーターの名前の 1 つとして値。 たとえば、 `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05w.png "INStartWorkoutIntent.workoutName キー")](implementing-sirikit-images/plist05w.png#lightbox)
7. 追加、`ParameterVocabulary`キーを`ParameterVocabularies`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist06w.png "ParameterVocabulary キー型の配列で ParameterVocabularies キーを追加します。")](implementing-sirikit-images/plist06w.png#lightbox)
8. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist07w.png "ディクショナリの型を持つ新しいキーを追加します。")](implementing-sirikit-images/plist07w.png#lightbox)
9. 追加、`VocabularyItemIdentifier`キーを**型**の`String`用語の一意の ID を指定。

    [![](implementing-sirikit-images/plist08w.png "VocabularyItemIdentifier キー文字列の型を追加して、用語の一意の ID を指定")](implementing-sirikit-images/plist08w.png#lightbox)
10. 追加、`VocabularyItemSynonyms`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist09w.png "VocabularyItemSynonyms キー型の配列を追加します。")](implementing-sirikit-images/plist09w.png#lightbox)
11. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist10w.png "ディクショナリの型を持つ新しいキーを追加します。")](implementing-sirikit-images/plist10w.png#lightbox)
12. 追加、`VocabularyItemPhrase`キーを**型**の`String`と、アプリの定義は、用語。

    [![](implementing-sirikit-images/plist11w.png "VocabularyItemPhrase キー文字列の型と、アプリを定義する用語を追加します。")](implementing-sirikit-images/plist11w.png#lightbox)
13. 追加、`VocabularyItemPronunciation`キーを**型**の`String`と用語のふりがな。

    [![](implementing-sirikit-images/plist12w.png "文字列型と、用語のふりがな VocabularyItemPronunciation キーを追加します。")](implementing-sirikit-images/plist12w.png#lightbox)
14. 追加、`VocabularyItemExamples`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist13w.png "VocabularyItemExamples キー型の配列を追加します。")](implementing-sirikit-images/plist13w.png#lightbox)
15. いくつかの追加`String`という用語の使用例を使用してキー。

    [![](implementing-sirikit-images/plist14w.png "用語の使用例をいくつかの文字列キーを追加します。")](implementing-sirikit-images/plist14w.png#lightbox)
16. 上記の手順を定義する必要があるアプリのカスタム条件を繰り返します。
17. 折りたたみ、`ParameterVocabularies`キー。
18. 追加、`IntentPhrases`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist15w.png "IntentPhrases キー型の配列を追加します。")](implementing-sirikit-images/plist15w.png#lightbox)
19. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist16w.png "ディクショナリの型を持つ新しいキーを追加します。")](implementing-sirikit-images/plist16w.png#lightbox)
20. 追加、`IntentName`キーを**型**の`String`の例では、インテントと。

    [![](implementing-sirikit-images/plist17w.png "例では、型の文字列と目的の IntentName キーを追加します。")](implementing-sirikit-images/plist17w.png#lightbox)
21. 追加、`IntentExamples`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist18w.png "IntentExamples キー型の配列を追加します。")](implementing-sirikit-images/plist18w.png#lightbox)
22. いくつかの追加`String`という用語の使用例を使用してキー。

    [![](implementing-sirikit-images/plist19w.png "用語の使用例をいくつかの文字列キーを追加します。")](implementing-sirikit-images/plist19w.png#lightbox)
23. 上記の手順、アプリの使用例を提供する必要は任意のインテントを繰り返します。

-----

> [!IMPORTANT]
> `AppIntentVocabulary.plist`登録は、開発中にデバイスのテストで Siri とカスタム語彙を反映するための Siri の時間がかかる場合があります。 その結果、テスト担当者が更新されたときに、アプリの特定の語彙をテストする前に数分間待機する必要があります。

詳細についてを参照してください、[アプリ固有のボキャブラリ リファレンス](~/ios/platform/sirikit/understanding-sirikit.md)と Apple の[カスタム ボキャブラリの参照を指定する](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)します。

## <a name="adding-an-intents-extension"></a>Intents の拡張機能を追加します。

SiriKit を採用するアプリケーションが準備できたので、開発者は Siri の統合に必要なインテントを処理するためにソリューションに 1 つ (以上) の Intents 拡張機能を追加する必要があります。

Intents 拡張機能が必要な各、次の操作を行います。

- Intents の拡張機能プロジェクトを Xamarin.iOS アプリのソリューションに追加します。
- Intents の拡張機能を構成する`Info.plist`ファイル。
- Intents の拡張機能の主なクラスを変更します。

詳細についてを参照してください、 [、Intents 拡張機能の参照](~/ios/platform/sirikit/understanding-sirikit.md)と Apple の[Intents の拡張機能の参照を作成する](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1)します。

### <a name="creating-the-extension"></a>拡張機能を作成します。

Intents の拡張機能をソリューションに追加するには、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 右クリックし、**ソリューション名**で、 **Solution Pad**選択**追加** > **新しいプロジェクトの追加.**.
2. ダイアログ ボックスから選択**iOS** > **拡張機能** > **目的とした拡張機能** をクリックし、**次**ボタンをクリックします。 

    [![](implementing-sirikit-images/intents05.png "インテントの拡張機能を選択します。")](implementing-sirikit-images/intents05.png#lightbox)
3. 次を入力、**名前**目的とした拡張機能をクリックして、**次**ボタン。 

    [![](implementing-sirikit-images/intents06.png "インテントの拡張機能の名前を入力します。")](implementing-sirikit-images/intents06.png#lightbox)
4. 最後に、クリックして、**作成**アプリ ソリューションに、目的の拡張機能を追加するボタン。 

    [![](implementing-sirikit-images/intents07.png "目的の拡張機能をアプリ ソリューションに追加します。")](implementing-sirikit-images/intents07.png#lightbox)
5. **ソリューション エクスプ ローラー**を右クリックし、**参照**新しく作成した目的の拡張機能のフォルダー。 (上記で作成したアプリ) を共通の共有コード ライブラリ プロジェクトの名前を確認し、 **OK**ボタン。 

    [![](implementing-sirikit-images/intents08.png "共通の共有コード ライブラリ プロジェクトの名前を選択します。")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 右クリックし、**ソリューション名**で、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクトの追加.**.
2. ダイアログ ボックスから選択**Visual C# > iOS 拡張機能 > 目的の拡張機能** をクリックし、 **次へ**ボタン。

    [![](implementing-sirikit-images/intents05.w157-sml.png "インテントの拡張機能を選択します。")](implementing-sirikit-images/intents05.w157.png#lightbox)
3. 次を入力、**名前**目的とした拡張機能をクリックして、 **OK**ボタン。
1. **ソリューション エクスプ ローラー**を右クリックし、**参照**フォルダーを新しく作成された Intents 拡張機能の選択と**追加 > 参照**。 (上記で作成したアプリ) を共通の共有コード ライブラリ プロジェクトの名前を確認し、 **OK**ボタン。

    [![](implementing-sirikit-images/intents08w.png "共通の共有コード ライブラリ プロジェクトの名前を選択します。")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

目的の拡張機能の数を次の手順を繰り返します (に基づいて[拡張機能用のアプリを設計](#architecting-the-app-for-extensions)前のセクション)、アプリが必要になります。

### <a name="configuring-the-infoplist"></a>Info.plist の構成

アプリのソリューションに追加する Intents 拡張機能のそれぞれについてで構成する必要があります、`Info.plist`ファイルをアプリで動作します。

一般的なアプリ拡張機能と同じようにアプリの既存のキーには`NSExtension`と`NSExtensionAttributes`します。 Intents の拡張機能には、構成する必要がある 2 つの新しい属性があります。

[![](implementing-sirikit-images/intents01.png "2 つの新しい属性を構成する必要があります。")](implementing-sirikit-images/intents01.png#lightbox)

- **IntentsSupported** - は必須であり、アプリが目的とした拡張機能をサポートする目的のクラス名の配列で構成されます。
- **IntentsRestrictedWhileLocked** -拡張機能のロック画面の動作を指定する、アプリの省略可能なキーします。 目的とした拡張機能から使用する場合にログインするユーザーを必要とするアプリの必要を目的としたクラス名の配列で構成されます。

目的の拡張機能の構成に`Info.plist`ファイルをダブルクリックします、**ソリューション エクスプ ローラー**編集用に開きます。 次に、切り替える、**ソース**を表示し、展開、`NSExtension`と`NSExtensionAttributes`エディター内のキー。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents02.png "エディターで NSExtension と NSExtensionAttributes キー")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents02w.png "エディターで NSExtension と NSExtensionAttributes キー")](implementing-sirikit-images/intents02w.png#lightbox)

-----

展開、`IntentsSupported`キーし、はこの拡張機能をサポートする任意の目的としたクラスの名前を追加します。 例 MonkeyChat アプリでは、サポート、 `INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents09.png "INSendMessageIntent キー")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents09w.png "INSendMessageIntent キー")](implementing-sirikit-images/intents09w.png#lightbox)

-----

必要に応じて、アプリでは、ユーザーが特定の目的を使用して、展開するデバイスにログオンする必要がある場合、`IntentRestrictedWhileLocked`キーし、アクセスが制限されたインテントのクラス名を追加します。 例 MonkeyChat アプリでは、ユーザーでログインするありますが追加されましたので、チャット メッセージを送信する`INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents10.png "追加した INSendMessageIntent キー")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents10w.png "追加した INSendMessageIntent キー")](implementing-sirikit-images/intents10w.png#lightbox)

-----


目的の使用可能なドメインの一覧は、Apple を参照してください[ドメインの参照を目的とした](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)します。

### <a name="configuring-the-main-class"></a>メイン クラスを構成します。

次に、開発者は Siri を目的とした拡張機能のメイン エントリ ポイントとして機能するメイン クラスを構成する必要があります。 サブクラス化する必要があります`INExtension`に準拠している`IINIntentHandler`を委任します。 例:

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

アプリを目的とした拡張機能の主なクラスで実装する必要があります 1 つの群れメソッドは、`GetHandler`メソッド。 このメソッドは、SiriKit によってインテントを渡され、アプリに返す必要があります、**インテント ハンドラー**目的は、特定の種類に対応します。

MonkeyChat アプリの例は、1 つのインテントのみを処理するためにそれ自体を返している、`GetHandler`メソッド。 開発者はインテントの種類ごとにクラスを追加およびここに基づいてインスタンスを返す場合は、拡張機能は、1 つ以上のインテントを処理、`Intent`メソッドに渡されます。

### <a name="handling-the-resolve-stage"></a>解決の段階の処理

解決ステージが目的とした拡張機能が明確にし、パラメーターを検証する場所で渡され Siri から、ユーザーのメッセージ交換を使用して設定されています。

Siri から送信される各パラメーターには、`Resolve`メソッド。 アプリは、アプリが、ユーザーから正しい解答を取得する Siri のヘルプを必要がありますすべてのパラメーターに対してこのメソッドを実装する必要があります。

例 MonkeyChat アプリの場合、目的とした拡張機能は、メッセージを送信する 1 つまたは複数の受信者を必要となります。 各受信者の一覧で、拡張機能は、次の結果を持つことができますが連絡先の検索を実行する必要があります。

- 1 つだけ一致する連絡先が見つかりました。
- 2 つ以上の一致する連絡先が見つかりました。
- 一致する連絡先が見つかりませんでした。

さらに、MonkeyChat では、メッセージの本文のコンテンツが必要です。 場合は、ユーザーが指定されていないこの Siri をコンテンツのユーザーに確認する必要があります。

目的の拡張機能は、これらの各ケースを適切に処理する必要があります。


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

詳細についてを参照してください、[解決ステージの参照を](~/ios/platform/sirikit/understanding-sirikit.md)と Apple の[インテントの処理の参照の解決と](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1)します。

### <a name="handling-the-confirm-stage"></a>確認段階の処理

確認段階は、目的の拡張機能がすべてのユーザーの要求を満たすために情報を持っていることを確認します。 アプリがに沿って確認は、内容の補足詳細情報をすべてが行われる Siri をことを確認できるユーザーと目的のアクションは、このようにを送信しようとするとします。

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

詳細についてを参照してください、[確認段階の参照を](~/ios/platform/sirikit/understanding-sirikit.md)します。

### <a name="processing-the-intent"></a>目的の処理

これは、目的とした拡張機能をユーザーの要求を満たすため、ユーザーの情報を把握できるように、Siri に結果を渡すタスクが実行します実際にはポイントです。


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

詳細についてを参照してください、[処理ステージの参照を](~/ios/platform/sirikit/understanding-sirikit.md)します。

## <a name="adding-an-intents-ui-extension"></a>Intents UI 拡張機能を追加します。

省略可能な Intents UI 拡張機能は、アプリの UI と Siri エクスペリエンスにブランド化する機会を表示します。 してアプリに接続されていると感じるユーザーです。 この拡張機能では、アプリはブランドとトラン スクリプトにビジュアルと他の情報を表示できます。

[![](implementing-sirikit-images/intentsui01.png "Intents UI 拡張機能の出力例")](implementing-sirikit-images/intentsui01.png#lightbox)

Intents の拡張機能と同様、開発者は、Intents UI 拡張機能を次の手順を実行します。

- Intents UI 拡張機能プロジェクトを Xamarin.iOS アプリのソリューションに追加します。
- Intents UI 拡張機能を構成する`Info.plist`ファイル。
- Intents UI 拡張機能の主なクラスを変更します。

詳細についてを参照してください、 [Intents UI 拡張機能の参照を](~/ios/platform/sirikit/understanding-sirikit.md)と Apple の[カスタム インターフェイスのリファレンスを提供する](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1)します。

### <a name="creating-the-extension"></a>拡張機能を作成します。

Intents UI 拡張機能をソリューションに追加するには、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 右クリックし、**ソリューション名**で、 **Solution Pad**選択**追加** > **新しいプロジェクトの追加.**.
2. ダイアログ ボックスから選択**iOS** > **拡張機能** > **UI 拡張機能を目的とした** をクリックし、 **次へ**ボタンをクリックします。 

    [![](implementing-sirikit-images/intents11.png "インテント UI 拡張機能を選択します。")](implementing-sirikit-images/intents11.png#lightbox)
3. 次を入力、**名前**目的とした拡張機能をクリックして、**次**ボタン。 

    [![](implementing-sirikit-images/intents12.png "インテントの拡張機能の名前を入力します。")](implementing-sirikit-images/intents12.png#lightbox)
4. 最後に、クリックして、**作成**アプリ ソリューションに、目的の拡張機能を追加するボタン。 

    [![](implementing-sirikit-images/intents13.png "目的の拡張機能をアプリ ソリューションに追加します。")](implementing-sirikit-images/intents13.png#lightbox)
5. **ソリューション エクスプ ローラー**を右クリックし、**参照**新しく作成した目的の拡張機能のフォルダー。 (上記で作成したアプリ) を共通の共有コード ライブラリ プロジェクトの名前を確認し、 **OK**ボタン。 

    [![](implementing-sirikit-images/intents14.png "共通の共有コード ライブラリ プロジェクトの名前を選択します。")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 右クリックし、**ソリューション名**で、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクトの追加.**
2. ダイアログ ボックスから選択**iOS** > **拡張機能** > **UI 拡張機能を目的とした** をクリックし、 **次へ**ボタンをクリックします。
3. 次を入力、**名前**目的とした拡張機能をクリックして、 **OK**ボタン。
5. **ソリューション エクスプ ローラー**を右クリックし、**参照**新しく作成した目的の拡張機能のフォルダー。 (上記で作成したアプリ) を共通の共有コード ライブラリ プロジェクトの名前を確認し、 **OK**ボタンをクリックします。
    
-----

### <a name="configuring-the-infoplist"></a>Info.plist の構成

Intents UI 拡張機能の構成`Info.plist`ファイルをアプリで動作します。

一般的なアプリ拡張機能と同じようにアプリの既存のキーには`NSExtension`と`NSExtensionAttributes`します。 Intents の拡張機能を構成する必要がある 1 つの新しい属性があります。

[![](implementing-sirikit-images/intents03.png "1 つの新しい属性を構成する必要があります。")](implementing-sirikit-images/intents03.png#lightbox)

**IntentsSupported**は必須であり、アプリが目的とした拡張機能をサポートする目的のクラス名の配列で構成されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

目的の UI 拡張機能の構成に`Info.plist`ファイルをダブルクリックします、**ソリューション エクスプ ローラー**編集用に開きます。 次に、切り替える、**ソース**を表示し、展開、`NSExtension`と`NSExtensionAttributes`エディター内のキー。

[![](implementing-sirikit-images/intents04.png "エディターで NSExtension と NSExtensionAttributes キー")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

目的の UI 拡張機能の構成に`Info.plist`ファイルをダブルクリックします、**ソリューション エクスプ ローラー**編集用に開きます。 展開、`NSExtension`と`NSExtensionAttributes`エディター内のキー。

[![](implementing-sirikit-images/intents04w.png "エディターで %t NSExtension と NSExtensionAttributes キー")](implementing-sirikit-images/intents04w.png#lightbox)

-----

展開、`IntentsSupported`キーし、はこの拡張機能をサポートする任意の目的としたクラスの名前を追加します。 例 MonkeyChat アプリでは、サポート、 `INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents15.png "INSendMessageIntent キー")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents15w.png "INSendMessageIntent キー")](implementing-sirikit-images/intents15w.png#lightbox)

-----

目的の使用可能なドメインの一覧は、Apple を参照してください[ドメインの参照を目的とした](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)します。

### <a name="configuring-the-main-class"></a>メイン クラスを構成します。

Siri を目的とした UI の拡張機能のメイン エントリ ポイントとして機能するメイン クラスを構成します。 サブクラス化する必要があります`UIViewController`に準拠している`IINUIHostedViewController`インターフェイス。 例:

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

Siri を渡す、`INInteraction`クラスのインスタンス、`Configure`のメソッド、`UIViewController`インテント UI 拡張機能内のインスタンス。

`INInteraction`オブジェクトは 3 つの重要な拡張機能に情報を提供します。

1. 処理対象のインテント オブジェクト。
2. 目的の応答オブジェクトから、`Confirm`と`Handle`目的とした拡張機能のメソッド。
3. アプリと Siri の間の対話の状態を定義する対話状態。

`UIViewController`インスタンスは Siri と対話するための原則クラスから継承するため、 `UIViewController`、すべての UIKit の機能にアクセスします。

Siri を呼び出すと、`Configure`のメソッド、 `UIViewController` Snippit の Siri または Maps のカードでビュー コント ローラーのアプリケーションがホストされるか、ことを示すビュー コンテキストを渡します。

Siri は、アプリがアプリの構成が完了すると、ビューの目的のサイズを返す必要がある完了ハンドラーに渡すこともできます。

### <a name="design-the-ui-in-ios-designer"></a>IOS Designer の UI の設計

IOS Designer で Intents UI 拡張機能のユーザー インターフェイスのレイアウト。 拡張機能のダブルクリック`MainInterface.storyboard`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。 すべてのユーザー インターフェイスを構築し、変更を保存する必要な UI 要素にドラッグします。

> [!IMPORTANT]
> などの対話型要素を追加することはできますが`UIButtons`または`UITextFields`を目的とした UI 拡張機能の`UIViewController`、非対話型で目的の UI として禁じこれらと、ユーザーは、対話機能を使用できません。

### <a name="wire-up-the-user-interface"></a>ユーザー インターフェイスに接続

Intents UI 拡張機能のユーザー インターフェイスを iOS Designer で作成、編集、`UIViewController`サブクラス化とオーバーライド、`Configure`メソッドとして、次のとおりです。

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

### <a name="overriding-the-default-siri-ui"></a>既定の Siri UI をオーバーライドします。

Intents UI 拡張機能は常にする UI の上部にある名前とアプリのアイコンなどの他の Siri コンテンツと共に表示されますかに基づく目的、下部にある (送信またはキャンセル) などのボタンが表示されます。

アプリケーションが Siri が既定では、メッセージングなど、ユーザーに表示されている情報や、アプリがアプリに合わせていずれかの既定のエクスペリエンスを置き換えることができます、マップに置き換えることがいくつかのインスタンスがあります。

Intents UI 拡張機能は、既定の Siri の UI の要素をオーバーライドする必要がある場合、`UIViewController`サブクラスを実装する必要があります、`IINUIHostedViewSiriProviding`インターフェイスと特定のインターフェイス要素を表示するときにオプトインします。

次のコードを追加、 `UIViewController` Siri インテント UI 拡張機能が既にメッセージの内容を表示することを通知するサブクラスです。

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>注意事項

Apple では、開発者が設計と目的の UI 拡張機能を実装するときに、以下の点を実行することを示しています。

- **意識のメモリ使用量をする**- ため、目的の UI 拡張機能は一時的なものと、システムより完全なアプリとも緊密なメモリの制約では、しばらくの間、表示のみ。
- **最小値と最大のビューのサイズを検討してください**-目的の UI 拡張機能がすべての iOS デバイスの種類、サイズと向きを気に入ったことを確認します。 さらに、目的のサイズ、アプリから Siri に返される可能性がありますを許可することができません。
- **優れた柔軟性とアダプティブのレイアウトのパターンを使用して、** - もう一度、UI がないすべてのデバイスで優れたいることを確認します。

## <a name="summary"></a>まとめ

この記事では、SiriKit をカバーし、iOS デバイスで Siri とマップ アプリを使用してユーザーがアクセスできるサービスを提供する Xamarin.iOS アプリに追加する方法を示すは。




## <a name="related-links"></a>関連リンク

- [ElizaChat サンプル](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [インテントのフレームワーク参照](https://developer.apple.com/reference/intents)
- [Intents UI フレームワークの参照](https://developer.apple.com/reference/intentsui)
- [インテントのドメインの参照](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
