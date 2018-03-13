---
title: "SiriKit を実装します。"
description: "この記事では、Xamarin.iOS アプリで SiriKit サポートを実装するための手順について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 0e271fb78cfd225f9ccdae9a515685e89bfd7ac2
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="implementing-sirikit"></a>SiriKit を実装します。

_この記事では、Xamarin.iOS アプリで SiriKit サポートを実装するための手順について説明します。_



新しい 10、iOS に SiriKit が iOS デバイスで Siri とマップのアプリを使用してユーザーがアクセスできるサービスを提供する Xamarin.iOS アプリを許可します。 この記事では、Xamarin.iOS アプリで、必要なインテント拡張子インテント UI 拡張機能およびボキャブラリを追加することによって SiriKit サポートを実装するための手順について説明します。

Siri の動作の概念を**ドメイン**のグループが関連するタスクの操作を把握します。 Siri とされているアプリの各相互作用がとおりの既知のサービスのドメインのいずれかに分類する必要があります。

- オーディオまたはビデオ通話です。
- 素敵を予約します。
- ワークアウトを管理します。
- メッセージングです。
- 写真を検索しています。
- 送信または受信支払いを処理します。

ユーザーは、1 つのアプリ拡張機能のサービスに関連する Siri の要求を行う、SiriKit 送信、拡張機能、**インテント**サポート データと共に、ユーザーの要求を記述するオブジェクト。 アプリ拡張機能は、適切な生成**応答**オブジェクトを指定された**インテント**、拡張機能が、要求を処理する方法の詳細を示すです。

このガイドでは、既存のアプリに SiriKit サポートなどのわかりやすい例を提示します。 この例で使用する、偽の MonkeyChat アプリ。

[![](implementing-sirikit-images/monkeychat01.png "MonkeyChat アイコン")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat がユーザーの友人の連絡先独自書籍を維持、それぞれ (Bobo など) のような画面名に関連付けられているユーザーの画面名が各友人にテキスト チャットを送信することができます。

## <a name="extending-the-app-with-sirikit"></a>SiriKit を使用してアプリを拡張します。

ように、 [SiriKit 概念について](~/ios/platform/sirikit/understanding-sirikit.md)ガイドは、次の 3 つの主要な部分に含まれている SiriKit でアプリを拡張します。

[![](implementing-sirikit-images/elements01.png "SiriKit ダイアグラムを使用してアプリを拡張します。")](implementing-sirikit-images/elements01.png#lightbox)

次の設定があります。

1. **インテント拡張子**-ユーザーの応答を検証、アプリが、要求を処理し、実際には、タスクを実行して、ユーザーの要求を満たすためのことを確認します。
2. **UI 拡張機能のインテント** - *オプション*、Siri 環境で応答するためのカスタム UI を提供をユーザーのエクスペリエンスを強化する Siri にブランド化およびしてアプリの UI に取り込むことができます。
3. **アプリ**-Siri 作業を支援するために特定のボキャブラリをユーザーにアプリを提供します。 

すべてのこれらの要素と、アプリに追加する手順は、以下のセクションで詳しく説明されます。


## <a name="preparing-the-app"></a>アプリを準備します。

拡張機能をベースに構築 SiriKit、ただし、すべての拡張機能をアプリに追加する前に、いくつか、開発者がの導入により、SiriKit のために実行する必要があります。

### <a name="moving-common-shared-code"></a>一般的な共有コードの移動 

最初に、開発者は、共有される共通のコードの一部間で移動できますアプリとその拡張機能に_共有プロジェクト_、_ポータブル クラス ライブラリ (Pcl)_または_ネイティブライブラリ_です。

拡張機能は、すべてのアプリは、処理を実行できるようにする必要があります。 サンプル MonkeyChat アプリ、新規の連絡先を追加する連絡先を検索するなどの用語では、メッセージを送信し、メッセージの履歴を取得します。

この一般的なコードを共有プロジェクト、PCL またはネイティブ ライブラリに移動することによっては、1 つの一般的な場所でそのコードを保守しやすいし、により、こと、拡張機能と親アプリ uniform エクスペリエンスと機能であり、ユーザーに提供します。

例のアプリ MonkeyChat の場合に、データ モデルやネットワークとデータベースへのアクセスなどの処理コードをネイティブ ライブラリに移動される予定です。

次の手順で行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac 用 Visual Studio を起動し、MonkeyChat アプリを開きます。
2. ソリューション名を右クリックし、**ソリューション パッド**選択**追加** > **新しいプロジェクト.**: 

    [![](implementing-sirikit-images/prep01.png "新しいプロジェクトを追加します。")](implementing-sirikit-images/prep01.png#lightbox)
3. 選択**iOS** > **ライブラリ** > **クラス ライブラリ** をクリックし、**次**ボタン。 

    [![](implementing-sirikit-images/prep02.png "クラス ライブラリを選択します。")](implementing-sirikit-images/prep02.png#lightbox)
4. 入力`MonkeyChatCommon`の**名前** をクリックし、**作成**ボタン。 

    [![](implementing-sirikit-images/prep03.png "MonkeyChatCommon を名を入力します。")](implementing-sirikit-images/prep03.png#lightbox)
5. 右クリックし、**参照**でメインのアプリのフォルダー、**ソリューション エクスプ ローラー**選択**参照の編集.**.チェック、 **MonkeyChatCommon**プロジェクトし、をクリックして、 **OK**ボタン。 

    [![](implementing-sirikit-images/prep05.png "MonkeyChatCommon プロジェクトをチェック アウトします。")](implementing-sirikit-images/prep05.png#lightbox)
6. **ソリューション エクスプ ローラー**、共通の共有コードをメイン アプリケーションからネイティブ ライブラリにドラッグします。
7. MonkeyChat の場合は、ドラッグ、 **DataModels**と**プロセッサ**ネイティブ ライブラリにメインのアプリからフォルダー。 

    [![](implementing-sirikit-images/prep06.png "ソリューション エクスプ ローラーで、DataModels とプロセッサのフォルダー")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio を起動し、MonkeyChat アプリを開きます。
2. ソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクト.**.
3. 選択**Visual c#** > **共有プロジェクト** をクリックし、**次**ボタン。 

    [![](implementing-sirikit-images/prep02w.png "クラス ライブラリを選択します。")](implementing-sirikit-images/prep02w.png#lightbox)
4. 入力`MonkeyChatCommon`の**名** をクリックし、**作成**ボタンをクリックします。
5. 右クリックし、**参照**でメインのアプリのフォルダー、**ソリューション エクスプ ローラー**選択**参照の編集.**.チェック、 **MonkeyChatCommon**プロジェクトし、をクリックして、 **OK**ボタン。 

    [![](implementing-sirikit-images/prep05w.png "MonkeyChatCommon プロジェクトをチェック アウトします。")](implementing-sirikit-images/prep05w.png#lightbox)
6. **ソリューション エクスプ ローラー**、メインのアプリから共有プロジェクトに共通の共有コードをドラッグします。
7. MonkeyChat の場合は、ドラッグ、 **DataModels**と**プロセッサ**ネイティブ ライブラリにメインのアプリからのフォルダーです。

-----

すべてのネイティブ ライブラリに移動されたファイルを編集し、ライブラリの一致するように名前空間を変更します。 たとえば、変更`MonkeyChat`に`MonkeyChatCommon`:

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

次に、メイン アプリケーションに戻るし、追加、`using`ステートメント ネイティブ ライブラリの名前空間の任意の場所、アプリは使用して移動されたクラスのいずれか。

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

### <a name="architecting-the-app-for-extensions"></a>拡張機能のアプリの設計

通常、アプリは複数のインテントにサインアップし、開発者は、アプリが適切な数の目的とした拡張機能用に設計されていることを確認する必要があります。

開発者ではで、アプリが 1 つ以上の目的が必要な場合、1 つの目的とした拡張機能に配置することで、すべてのインテント処理のまたは各インテントの別個の目的とした拡張機能の作成のオプションがあります。

各目的別の目的とした拡張機能を作成する場合、開発者可能性があります最終的に大量の各拡張機能での定型的なコードを複製して、大量のプロセッサとメモリのオーバーヘッドを作成します。

2 つのオプションを選択する際かどうか、インテントのいずれかの必然的に互いを参照してください。 たとえば、オーディオおよびビデオの呼び出しを行っているアプリ両方を含めるにそれらのインテントの単一の目的とした拡張機能で同様のタスクを処理するようおよびほとんどのコードの再利用を提供します。

任意の目的または既存のグループに収まらないの目的のグループを使用して、それらを格納する、アプリのソリューション内の目的の新しい拡張機能を作成します。


### <a name="setting-the-required-entitlements"></a>必要な権利の設定

SiriKit との統合、Xamarin.iOS アプリ、する必要がありますが、正しい権利設定。 開発者は必要な権利を正しく設定しない場合は開けなくなるインストールまたは実際の iOS 10 (またはそれ以上)、ハードウェアは、iOS 10 以降に必須でアプリをテストするシミュレーター SiriKit をサポートしていません。

次の手順で行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダブルクリックして、`Entitlements.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
2. 切り替えて、**ソース**タブです。
3. 追加、 `com.apple.developer.siri` **プロパティ**、設定、**型**に`Boolean`と**値**に`Yes`: 

    [![](implementing-sirikit-images/setup01.png "Com.apple.developer.siri プロパティを追加します。")](implementing-sirikit-images/setup01.png#lightbox)
4. 変更内容をファイルに保存します。
5. ダブルクリックして、**プロジェクト ファイル**で、**ソリューション エクスプ ローラー**編集用に開きます。
6. 選択**iOS バンドル署名 ***ことを確認して、`Entitlements.plist`でファイルを選択、**カスタム権利**フィールド。 

    [![](implementing-sirikit-images/setup02.png "カスタム権利フィールドで Entitlements.plist ファイルを選択します。")](implementing-sirikit-images/setup02.png#lightbox)
7. **[OK]** ボタンをクリックして、変更を保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダブルクリックして、`Entitlements.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
3. 追加、 `com.apple.developer.siri` **プロパティ**、設定、**型**に`Boolean`と**値**に`Yes`: 

    [![](implementing-sirikit-images/setup01w.png "Com.apple.developer.siri プロパティを追加します。")](implementing-sirikit-images/setup01w.png#lightbox)
4. 変更内容をファイルに保存します。
5. ダブルクリックして、**プロジェクト ファイル**で、**ソリューション エクスプ ローラー**編集用に開きます。
6. 選択**iOS バンドル署名 ***ことを確認して、`Entitlements.plist`でファイルを選択、**カスタム権利**フィールドです。

-----

完了したら、アプリの`Entitlements.plist`ファイルは次のようになります (の外部エディターで開く)。

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

### <a name="correctly-provisioning-the-app"></a>アプリを正しくプロビジョニング

Apple が SiriKit framework、SiriKit を実装する任意の Xamarin.iOS アプリの周囲に配置される厳密なセキュリティのため_必要があります_正しいアプリ ID と権利 (前のセクションを参照してください) があり、適切なで署名する必要がありますプロファイルをプロビジョニングします。

Mac で、次の操作を行います。

1. Web ブラウザーでに移動[http://developer.apple.com](http://developer.apple.com)と自分のアカウントにログインします。
2. をクリックして**証明書**、**識別子**と**プロファイル**です。
3. 選択**プロビジョニング プロファイル**を選択し、**のアプリ Id**、をクリックして、  **+** ボタンをクリックします。
4. 入力、**名前**新しいプロファイルのです。
5. 入力、**バンドル ID**推奨設定の名前付け Apple の後です。
6. 下方向にスクロール、 **App Services**セクションで、 **SiriKit**  をクリックし、**続行**ボタン。 

    [![](implementing-sirikit-images/setup03.png "SiriKit を選択します。")](implementing-sirikit-images/setup03.png#lightbox)
7. すべての設定、し確認**送信**アプリ id。
8. 選択**プロビジョニング プロファイル** > **開発**をクリックして、  **+** ボタン、、 **Apple ID**、をクリックして**続行**です。
9. [選択] をクリックして**すべて**をクリックし、**続行**です。
10. をクリックして**すべて選択**再度 をクリックし、**続行**です。
11. 入力、**プロファイル名**Apple を使用すると、推奨事項の名前付け、をクリックして**続行**です。
12. Xcode を起動します。
13. Xcode メニューから選択**設定しています.**
14. 選択**アカウント**をクリックし、**の詳細を表示しています.** ボタンをクリックします。 

    [![](implementing-sirikit-images/setup04.png "アカウントを選択します。")](implementing-sirikit-images/setup04.png#lightbox)
15. をクリックして、**すべてのプロファイルをダウンロード**左下隅のボタンをクリックします。 

    [![](implementing-sirikit-images/setup05.png "すべてのプロファイルをダウンロードします。")](implementing-sirikit-images/setup05.png#lightbox)
16. いることを確認、**プロビジョニング プロファイル**作成以降がインストールされている Xcode でします。
17. Visual studio for mac SiriKit サポートを追加するプロジェクトを開く
18. ダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**です。
18. いることを確認、**バンドル Id** Apple の開発者ポータル上で作成されたものと一致します。 

    [![](implementing-sirikit-images/setup06.png "バンドル Id")](implementing-sirikit-images/setup06.png#lightbox)
18. **ソリューション エクスプ ローラー**、select、**プロジェクト**です。
19. プロジェクトを右クリックし **オプション**です。
21. 選択**iOS バンドル署名 ***を選択、**署名 Identity**と**プロビジョニング プロファイル**上記で作成されました。 

    [![](implementing-sirikit-images/setup07.png "署名 Id とプロビジョニング プロファイルを選択します。")](implementing-sirikit-images/setup07.png#lightbox)
22. **[OK]** ボタンをクリックして、変更を保存します。

> [!IMPORTANT]
> **注:**テスト SiriKit でのみ機能実際 iOS 10 のハードウェア デバイス、iOS 10 ではなくシミュレーター。 Xamarin.iOS アプリを実際のハードウェアが有効な問題、SiriKit をインストールする場合は、必要な権限、アプリ ID、識別子の署名とプロビジョニング プロファイルが構成されている正しく Apple の開発者ポータルと Visual Studio の両方で for mac を確認してください。

### <a name="requesting-siri-authorization"></a>Siri の承認を要求します。

を、アプリが任意のユーザーの特定のボキャブラリを追加または Siri にインテント拡張機能が接続する前に承認 Siri にアクセスするユーザーから要求する必要があります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

アプリの編集`Info.plist`ファイルに切り替え、**ソース**表示し、追加、 `NSSiriUsageDescription` Siri と新機能のアプリの使用方法を説明する文字列値を持つキーの種類のデータが送信されます。 たとえば、MonkeyChat アプリでは、「MonkeyChat 連絡先は Siri に送信する」と可能性があります。

[![](implementing-sirikit-images/request01.png "Info.plist エディターで NSSiriUsageDescription")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

アプリの編集`Info.plist`ファイルを追加、 `NSSiriUsageDescription` Siri と新機能のアプリの使用方法を説明する文字列値とキーの種類のデータが送信されます。 たとえば、MonkeyChat アプリでは、「MonkeyChat 連絡先は Siri に送信する」と可能性があります。

[![](implementing-sirikit-images/request01w.png "Info.plist エディターで NSSiriUsageDescription")](implementing-sirikit-images/request01w.png#lightbox)

-----

呼び出す、`RequestSiriAuthorization`のメソッド、`INPreferences`アプリが初めて起動したときのクラスです。 編集、`AppDelegate.cs`クラスし、作成、`FinishedLaunching`次のようなメソッドの検索。


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

初めてこのメソッドが呼び出されたとき、アラートは、アプリケーションに Siri のアクセスを許可するユーザー入力を求める表示します。 メッセージに、開発者が追加される、`NSSiriUsageDescription`上、このアラートに表示されます。 使用できるユーザーには、最初に、アクセスが拒否された場合、**設定**アプリケーションへのアクセスを許可するアプリです。

いつでも、アプリが Siri を呼び出すことによってアクセスするアプリの機能を確認できます、`SiriAuthorizationStatus`のメソッド、`INPreferences`クラスです。

### <a name="localization-and-siri"></a>ローカリゼーションと Siri

IOS デバイス、ユーザーは Siri とは異なる、システムの既定の言語を選択することです。 ローカライズされたデータを使用する場合、アプリが使用する必要があります、`SiriLanguageCode`のメソッド、`INPreferences`クラス Siri から言語コードを取得します。 例:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>ユーザー固有のボキャブラリの追加

ボキャブラリを特定のユーザーがアプリの個々 のユーザーに固有の語句を提供しようとしています。 これらは、実行時にメインのアプリ (アプリ拡張機能) から、一覧の先頭に最も重要な条件を持つ、ユーザーの最も重要な使用の優先度に並べ替え用語の順序付きセットとして提供されます。

ユーザー固有のボキャブラリは、次のカテゴリのいずれかに属する必要があります。

- 名前 (つまり、アドレス帳フレームワークで管理されていない) に問い合わせてください。
- 写真のタグ。
- フォト アルバムの名前です。
- トレーニングの名前です。

カスタム ボキャブラリとして登録する用語を選択する場合は、用語のみを選択して、アプリに慣れていないユーザーをとなります。 決してレジスタ一般的な用語「My トレーニング」または「My アルバム」などです。 たとえば、MonkeyChat アプリは、ユーザーのアドレス帳に連絡先ごとに関連付けられているニックネームを登録します。

呼び出して、アプリがユーザーの特定のボキャブラリを提供、`SetVocabularyStrings`のメソッド、`INVocabulary`クラスを渡すと、`NSOrderedSet`メインのアプリからのコレクション。 アプリは常に呼び出す必要があります、`RemoveAllVocabularyStrings`メソッド最初は、新しいジョブを追加する前に、既存の用語を削除します。 例:

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

配置でこのコードでは、その可能性があるように呼び出されます。

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
> **注:** Siri がヒントとしてカスタム ボキャブラリを扱い、できるだけ多くの可能な用語が組み込まれます。 ただし、領域のカスタムのボキャブラリは登録する重要なことが限定されて_のみ_登録されている用語の合計数を最小限に抑えるため、複雑になる可能性がある用語。

詳細についてを参照してください、[ユーザー固有のボキャブラリ リファレンス](~/ios/platform/sirikit/understanding-sirikit.md)と Apple の[カスタム ボキャブラリの参照を指定する](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)です。

### <a name="adding-app-specific-vocabulary"></a>アプリ固有の仕様のボキャブラリの追加

アプリ固有のボキャブラリは、すべての車両のタイプまたはトレーニング名などのアプリのユーザーに、特定の単語と語句を認識するを定義します。 定義されているこれらは、アプリケーションの一部であるため、`AppIntentVocabulary.plist`メインのアプリ バンドルの一部としてファイル。 さらに、これらの単語や語句をローカライズする必要があります。

特定のアプリな用語は、次のカテゴリのいずれかに属する必要があります。

- オプションをオーバーライドします。
- トレーニングの名前です。

アプリの特定ボキャブラリ ファイルには、2 つのルート レベル キーが含まれています。

- `ParameterVocabularies` **必要な**-アプリのカスタムの条項およびに適用される目的としたパラメーターを定義します。
- `IntentPhrases` **省略可能な**-例語句で定義されているカスタムの条項を使用してが含まれています、`ParameterVocabularies`です。

内の各エントリ、 `ParameterVocabularies` ID 文字列、用語に適用されるという用語は、目的を指定する必要があります。 さらに、1 つの用語は、複数の意図的に適用可能性があります。

許容される値と必要なファイルの構造の一覧については、Apple を参照してください[アプリ ボキャブラリ ファイル形式のリファレンス](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)です。

追加する、`AppIntentVocabulary.plist`アプリ プロジェクトへのファイルで、次の操作します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. プロジェクト名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいファイル.**  >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "プロパティ リストを追加します。")](implementing-sirikit-images/plist01.png#lightbox) 
2. ダブルクリックして、`AppIntentVocabulary.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
3. クリックして、  **+** キーを追加するには、次のように設定します、**名前**に`ParameterVocabularies`と**型**に`Array`:。

    [![](implementing-sirikit-images/plist02.png "ParameterVocabularies と配列の型に名前を設定します。")](implementing-sirikit-images/plist02.png#lightbox)
4. 展開`ParameterVocabularies` をクリックし、  **+** ボタンをクリックし、設定、**型**に`Dictionary`:

    [![](implementing-sirikit-images/plist03.png "型をディクショナリに設定します。")](implementing-sirikit-images/plist03.png#lightbox)
5. クリックして、  **+** を新しいキーを追加するには、次のように設定します、**名**に`ParameterNames`と**型**に`Array`:。

    [![](implementing-sirikit-images/plist04.png "ParameterNames と配列の型に名前を設定します。")](implementing-sirikit-images/plist04.png#lightbox)
6. クリックして、  **+** を持つ新しいキーを追加する、**型**の`String`ととして利用可能なパラメーター名のいずれかの値。 たとえば、 `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05.png "INStartWorkoutIntent.workoutName キー")](implementing-sirikit-images/plist05.png#lightbox)
7. 追加、`ParameterVocabulary`キーを`ParameterVocabularies`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist06.png "配列の型を持つ ParameterVocabularies キーに ParameterVocabulary キーを追加します。")](implementing-sirikit-images/plist06.png#lightbox)
8. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist07.png "型のディクショナリに新しいキーを追加します。")](implementing-sirikit-images/plist07.png#lightbox)
9. 追加、`VocabularyItemIdentifier`キーを**型**の`String`し、用語の一意の ID を指定します。

    [![](implementing-sirikit-images/plist08.png "VocabularyItemIdentifier キー文字列型を追加し、一意の ID を指定")](implementing-sirikit-images/plist08.png#lightbox)
10. 追加、`VocabularyItemSynonyms`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist09.png "型の配列の VocabularyItemSynonyms キーを追加します。")](implementing-sirikit-images/plist09.png#lightbox)
11. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist10.png "型のディクショナリに新しいキーを追加します。")](implementing-sirikit-images/plist10.png#lightbox)
12. 追加、`VocabularyItemPhrase`キーを**型**の`String`という用語は、アプリを定義するとします。

    [![](implementing-sirikit-images/plist11.png "文字列型と、アプリを定義する用語の VocabularyItemPhrase キーを追加します。")](implementing-sirikit-images/plist11.png#lightbox)
13. 追加、`VocabularyItemPronunciation`キーを**型**の`String`と用語の音声発音。

    [![](implementing-sirikit-images/plist12.png "文字列型と用語の音声発音 VocabularyItemPronunciation キーを追加します。")](implementing-sirikit-images/plist12.png#lightbox)
14. 追加、`VocabularyItemExamples`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist13.png "型の配列の VocabularyItemExamples キーを追加します。")](implementing-sirikit-images/plist13.png#lightbox)
15. いくつかの追加`String`用語の使用例を使用してキー。

    [![](implementing-sirikit-images/plist14.png "用語の使用例をいくつかの文字列のキーを追加します。")](implementing-sirikit-images/plist14.png#lightbox)
16. 任意の他のカスタムの条項を定義する必要があります、アプリを上記の手順を繰り返します。
17. 折りたたみ、`ParameterVocabularies`キー。
18. 追加、`IntentPhrases`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist15.png "型の配列の IntentPhrases キーを追加します。")](implementing-sirikit-images/plist15.png#lightbox)
19. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist16.png "型のディクショナリに新しいキーを追加します。")](implementing-sirikit-images/plist16.png#lightbox)
20. 追加、`IntentName`キーを**型**の`String`例では、インテントとします。

    [![](implementing-sirikit-images/plist17.png "たとえば、型の文字列と目的で IntentName キーを追加します。")](implementing-sirikit-images/plist17.png#lightbox)
21. 追加、`IntentExamples`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist18.png "型の配列の IntentExamples キーを追加します。")](implementing-sirikit-images/plist18.png#lightbox)
22. いくつかの追加`String`用語の使用例を使用してキー。

    [![](implementing-sirikit-images/plist19.png "用語の使用例をいくつかの文字列のキーを追加します。")](implementing-sirikit-images/plist19.png#lightbox)
23. アプリでの使用例を提供する必要があります、インテントを上記の手順を繰り返します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. プロジェクト名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいファイル.**  >  **iOS**:

    [![](implementing-sirikit-images/plist01w.png "新しい Info.plist を追加します。")](implementing-sirikit-images/plist01w.png#lightbox) 
2. ダブルクリックして、`AppIntentVocabulary.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
3. クリックして、  **+** キーを追加するには、次のように設定します、**名前**に`ParameterVocabularies`と**型**に`Array`:。

    [![](implementing-sirikit-images/plist02w.png "ParameterVocabularies と配列の型に名前を設定します。")](implementing-sirikit-images/plist02w.png#lightbox)
4. 展開`ParameterVocabularies` をクリックし、  **+** ボタンをクリックし、設定、**型**に`Dictionary`:

    [![](implementing-sirikit-images/plist03w.png "型をディクショナリに設定します。")](implementing-sirikit-images/plist03w.png#lightbox)
5. クリックして、  **+** を新しいキーを追加するには、次のように設定します、**名**に`ParameterNames`と**型**に`Array`:。

    [![](implementing-sirikit-images/plist04w.png "ParameterNames と配列の型に名前を設定します。")](implementing-sirikit-images/plist04w.png#lightbox)
6. クリックして、  **+** を持つ新しいキーを追加する、**型**の`String`ととして利用可能なパラメーター名のいずれかの値。 たとえば、 `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05w.png "INStartWorkoutIntent.workoutName キー")](implementing-sirikit-images/plist05w.png#lightbox)
7. 追加、`ParameterVocabulary`キーを`ParameterVocabularies`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist06w.png "配列の型を持つ ParameterVocabularies キーに ParameterVocabulary キーを追加します。")](implementing-sirikit-images/plist06w.png#lightbox)
8. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist07w.png "型のディクショナリに新しいキーを追加します。")](implementing-sirikit-images/plist07w.png#lightbox)
9. 追加、`VocabularyItemIdentifier`キーを**型**の`String`し、用語の一意の ID を指定します。

    [![](implementing-sirikit-images/plist08w.png "文字列型で VocabularyItemIdentifier キーを追加して、用語の一意の ID を指定")](implementing-sirikit-images/plist08w.png#lightbox)
10. 追加、`VocabularyItemSynonyms`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist09w.png "型の配列の VocabularyItemSynonyms キーを追加します。")](implementing-sirikit-images/plist09w.png#lightbox)
11. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist10w.png "型のディクショナリに新しいキーを追加します。")](implementing-sirikit-images/plist10w.png#lightbox)
12. 追加、`VocabularyItemPhrase`キーを**型**の`String`という用語は、アプリを定義するとします。

    [![](implementing-sirikit-images/plist11w.png "文字列型と、アプリを定義する用語の VocabularyItemPhrase キーを追加します。")](implementing-sirikit-images/plist11w.png#lightbox)
13. 追加、`VocabularyItemPronunciation`キーを**型**の`String`と用語の音声発音。

    [![](implementing-sirikit-images/plist12w.png "文字列型と用語の音声発音 VocabularyItemPronunciation キーを追加します。")](implementing-sirikit-images/plist12w.png#lightbox)
14. 追加、`VocabularyItemExamples`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist13w.png "型の配列の VocabularyItemExamples キーを追加します。")](implementing-sirikit-images/plist13w.png#lightbox)
15. いくつかの追加`String`用語の使用例を使用してキー。

    [![](implementing-sirikit-images/plist14w.png "用語の使用例をいくつかの文字列のキーを追加します。")](implementing-sirikit-images/plist14w.png#lightbox)
16. 任意の他のカスタムの条項を定義する必要があります、アプリを上記の手順を繰り返します。
17. 折りたたみ、`ParameterVocabularies`キー。
18. 追加、`IntentPhrases`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist15w.png "型の配列の IntentPhrases キーを追加します。")](implementing-sirikit-images/plist15w.png#lightbox)
19. 新しいキーを追加、**型**の`Dictionary`:

    [![](implementing-sirikit-images/plist16w.png "型のディクショナリに新しいキーを追加します。")](implementing-sirikit-images/plist16w.png#lightbox)
20. 追加、`IntentName`キーを**型**の`String`例では、インテントとします。

    [![](implementing-sirikit-images/plist17w.png "たとえば、型の文字列と目的で IntentName キーを追加します。")](implementing-sirikit-images/plist17w.png#lightbox)
21. 追加、`IntentExamples`キーを**型**の`Array`:

    [![](implementing-sirikit-images/plist18w.png "型の配列の IntentExamples キーを追加します。")](implementing-sirikit-images/plist18w.png#lightbox)
22. いくつかの追加`String`用語の使用例を使用してキー。

    [![](implementing-sirikit-images/plist19w.png "用語の使用例をいくつかの文字列のキーを追加します。")](implementing-sirikit-images/plist19w.png#lightbox)
23. アプリでの使用例を提供する必要があります、インテントを上記の手順を繰り返します。

-----

> [!IMPORTANT]
> **注:** 、`AppIntentVocabulary.plist`が登録される開発中にデバイスでテストに Siri をカスタムのボキャブラリを組み込む Siri の時間がかかる可能性があります。 その結果、テスト担当者が更新されたときに、アプリ固有のボキャブラリをテストする前に数分間待機する必要があります。

詳細についてを参照してください、[アプリ ボキャブラリ リファレンス](~/ios/platform/sirikit/understanding-sirikit.md)と Apple の[カスタム ボキャブラリの参照を指定する](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)です。

## <a name="adding-an-intents-extension"></a>インテント拡張機能を追加

これで、アプリが SiriKit を採用する準備されて、開発者が Siri 統合に必要な変換を処理するソリューションに 1 つ (以上) の目的の拡張機能を追加する必要があります。

それぞれの目的は、必要な拡張機能、次の操作を行います。

- Xamarin.iOS アプリ ソリューションには、目的の拡張機能プロジェクトを追加します。
- 目的の拡張機能の構成`Info.plist`ファイル。
- インテントの拡張機能の主なクラスを変更します。

詳細についてを参照してください、[インテント拡張子 Reference](~/ios/platform/sirikit/understanding-sirikit.md)と Apple の[インテントの拡張機能の参照を作成する](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1)です。

### <a name="creating-the-extension"></a>拡張機能を作成します。

ソリューションにインテント拡張機能を追加するには、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 右クリックし、**ソリューション名**で、**ソリューション パッド**選択**追加** > **新しいプロジェクトの追加.**.
2. ダイアログ ボックスから選択**iOS** > **拡張機能** > **目的とした拡張機能** をクリックし、**次**ボタンをクリックします。 

    [![](implementing-sirikit-images/intents05.png "インテントの拡張機能を選択します。")](implementing-sirikit-images/intents05.png#lightbox)
3. 次を入力、**名前**目的とした拡張機能とクリック、**次**ボタン。 

    [![](implementing-sirikit-images/intents06.png "インテントの拡張機能の名前を入力します。")](implementing-sirikit-images/intents06.png#lightbox)
4. 最後をクリックして、**作成**アプリ ソリューションを目的とした拡張機能を追加する。 

    [![](implementing-sirikit-images/intents07.png "アプリ ソリューションを目的とした拡張機能を追加します。")](implementing-sirikit-images/intents07.png#lightbox)
5. **ソリューション エクスプ ローラー**を右クリックし、**参照**新しく作成された目的とした拡張機能のフォルダーです。 (つまり、アプリは、上記で作成) 共通の共有コード ライブラリのプロジェクトの名前を確認し、をクリックして、 **OK**ボタンをクリックします。 

    [![](implementing-sirikit-images/intents08.png "共通の共有コード ライブラリのプロジェクトの名前を選択します。")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 右クリックし、**ソリューション名**で、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクトの追加.**.
2. ダイアログ ボックスから選択**iOS** > **拡張機能** > **目的とした拡張機能** をクリックし、**次**ボタンをクリックします。 

    [![](implementing-sirikit-images/intents05w.png "インテントの拡張機能を選択します。")](implementing-sirikit-images/intents05w.png#lightbox)
3. 次を入力、**名前**目的とした拡張機能とをクリックして、 **[ok]**ボタンをクリックします。
5. **ソリューション エクスプ ローラー**を右クリックし、**参照**新しく作成された目的とした拡張機能のフォルダーです。 (つまり、アプリは、上記で作成) 共通の共有コード ライブラリのプロジェクトの名前を確認し、をクリックして、 **OK**ボタンをクリックします。 

    [![](implementing-sirikit-images/intents08w.png "共通の共有コード ライブラリのプロジェクトの名前を選択します。")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

目的の拡張機能の数を次の手順を繰り返します (に基づいて[拡張機能用のアプリを設計](#Architecting-the-App-for-Extensions)前のセクション)、アプリが必要になります。

### <a name="configuring-the-infoplist"></a>Info.plist の構成

アプリのソリューションに追加した目的の拡張機能のそれぞれについてで構成する必要があります、`Info.plist`アプリを使用するファイル。

アプリがの既存のキーを持つ、一般的なアプリの拡張と同じように`NSExtension`と`NSExtensionAttributes`です。 インテント拡張では、構成する必要がある 2 つの新しい属性があります。

[![](implementing-sirikit-images/intents01.png "構成する必要がある 2 つの新しい属性")](implementing-sirikit-images/intents01.png#lightbox)

- **IntentsSupported** : は必須であり、アプリが目的とした拡張機能をサポートする目的のクラス名の配列で構成されます。
- **IntentsRestrictedWhileLocked** -アプリ拡張機能のロック画面の動作を指定するため、省略可能なキーです。 アプリが目的とした拡張機能から使用する場合にログインするユーザーが要求する目的のクラス名の配列で構成されます。

目的とした拡張機能の構成に`Info.plist`ファイルをダブルクリック、**ソリューション エクスプ ローラー**編集用に開きます。 次に、スイッチ、**ソース**を表示し、展開、`NSExtension`と`NSExtensionAttributes`エディター内のキー。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents02.png "エディターで NSExtension と NSExtensionAttributes キー")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents02w.png "エディターで NSExtension と NSExtensionAttributes キー")](implementing-sirikit-images/intents02w.png#lightbox)

-----

展開して、`IntentsSupported`キーし、この拡張機能がサポートする任意の目的としたクラスの名前を追加します。 例の MonkeyChat アプリのサポート、 `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents09.png "INSendMessageIntent キー")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents09w.png "INSendMessageIntent キー")](implementing-sirikit-images/intents09w.png#lightbox)

-----

必要に応じて、アプリでは、ユーザーが特定の目的を使用して、展開するデバイスにログオンする必要がある場合、`IntentRestrictedWhileLocked`キーし、アクセスが制限の目的のクラス名を追加します。 例 MonkeyChat アプリのユーザー ログインする必要ありますが追加されましたので、チャット メッセージを送信する`INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents10.png "追加した INSendMessageIntent キー")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents10w.png "追加した INSendMessageIntent キー")](implementing-sirikit-images/intents10w.png#lightbox)

-----


使用可能な目的としたドメインの完全な一覧を参照してください Apple の[ドメインの参照を目的とした](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)です。

### <a name="configuring-the-main-class"></a>メインのクラスを構成します。

次に、開発者は、Siri を目的とした拡張機能のメイン エントリ ポイントとして機能する主要なクラスを構成する必要があります。 サブクラスである必要があります、`INExtension`に準拠している、`IINIntentHandler`を委任します。 例:

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

アプリを目的とした拡張機能のメイン クラスに実装する必要がある 1 つの単独のメソッドが、`GetHandler`メソッドです。 このメソッドは渡しインテント SiriKit であり、アプリを返す必要があります、**インテント ハンドラー**特定の目的の型に一致します。

例の MonkeyChat アプリは、1 つの目的のみを処理するためにそれ自体を返している、`GetHandler`メソッドです。 場合は、拡張機能には、1 つ以上の目的が処理される、開発者はインテント種類ごとにクラスを追加し、ここに基づいてインスタンスを返す、`Intent`メソッドに渡されます。

### <a name="handling-the-resolve-stage"></a>解決ステージの処理

解決ステージは目的とした拡張機能が明確にし、パラメーターを検証する場所から渡された Siri とユーザーのメッセージ交換を使用して設定されています。

Siri から送信された各パラメーターの場合は、`Resolve`メソッドです。 アプリは、アプリがユーザーから正しい答えを取得する Siri のヘルプを必要がありますすべてのパラメーターに対してこのメソッドを実装する必要があります。

例の MonkeyChat アプリの場合は、目的とした拡張機能は、メッセージを送信する 1 つまたは複数の受信者を必要となります。 各受信者一覧にするには、拡張機能は、次の結果を持つことができる連絡先の検索を行う必要があります。

- 1 つだけの一致する連絡先が見つかりました。
- 2 つ以上の一致する連絡先が見つかりました。
- 一致する連絡先が見つかりませんでした。

さらに、MonkeyChat では、メッセージの本文のコンテンツが必要です。 ユーザーが指定されていないこのコンテンツのユーザー入力を求める Siri 必要があります。

目的とした拡張機能は、これらの各ケースに対処する必要があります。


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

詳細についてを参照してください、[解決ステージ Reference](~/ios/platform/sirikit/understanding-sirikit.md)と Apple の[処理の目的の参照の解決と](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1)です。

### <a name="handling-the-confirm-stage"></a>確認段階の処理

確認段階では、すべてのユーザーの要求を満たすために情報を持っていることが確認の目的とした拡張機能のチェック場所です。 アプリがに沿って確認はすべて、サポートの詳細についてはについて発生 Siri をことを確認できるユーザーを使用して目的のアクションであることを送信しようとするとします。

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

詳細についてを参照してください、[確認ステージ Reference](~/ios/platform/sirikit/understanding-sirikit.md)です。

### <a name="processing-the-intent"></a>目的の処理

これは、ここで、目的とした拡張機能実際には、タスクを実行して、ユーザーの要求を満たすため、ユーザーに通知することができますので、Siri に結果を渡しますポイントです。


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

詳細についてを参照してください、[処理ステージ Reference](~/ios/platform/sirikit/understanding-sirikit.md)です。

## <a name="adding-an-intents-ui-extension"></a>インテント UI 拡張機能を追加

省略可能なインテント UI 拡張機能では、アプリの UI と Siri エクスペリエンスにブランド化する機会を表示され、アプリに接続されていると思われるユーザーを作成します。 この拡張機能では、アプリは、ブランドだけでなく、トラン スクリプトにビジュアルと他の情報を表示できます。

[![](implementing-sirikit-images/intentsui01.png "UI 拡張機能の目的の出力例")](implementing-sirikit-images/intentsui01.png#lightbox)

目的の拡張機能と同じように、開発者はインテント UI 拡張機能の次の手順を行います。

- Xamarin.iOS アプリ ソリューションにインテント UI 拡張機能プロジェクトを追加します。
- インテント UI 拡張機能の構成`Info.plist`ファイル。
- インテント UI 拡張機能の主なクラスを変更します。

詳細についてを参照してください、 [、インテント UI 拡張機能の参照](~/ios/platform/sirikit/understanding-sirikit.md)と Apple の[カスタム インターフェイス リファレンスを提供する](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1)です。

### <a name="creating-the-extension"></a>拡張機能を作成します。

ソリューションにインテント UI 拡張機能を追加するには、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 右クリックし、**ソリューション名**で、**ソリューション パッド**選択**追加** > **新しいプロジェクトの追加.**.
2. ダイアログ ボックスから選択**iOS** > **拡張機能** > **UI 拡張機能の目的とした** をクリックし、**次**ボタンをクリックします。 

    [![](implementing-sirikit-images/intents11.png "インテント UI 拡張機能を選択します。")](implementing-sirikit-images/intents11.png#lightbox)
3. 次を入力、**名前**目的とした拡張機能とクリック、**次**ボタン。 

    [![](implementing-sirikit-images/intents12.png "インテントの拡張機能の名前を入力します。")](implementing-sirikit-images/intents12.png#lightbox)
4. 最後をクリックして、**作成**アプリ ソリューションを目的とした拡張機能を追加する。 

    [![](implementing-sirikit-images/intents13.png "アプリ ソリューションを目的とした拡張機能を追加します。")](implementing-sirikit-images/intents13.png#lightbox)
5. **ソリューション エクスプ ローラー**を右クリックし、**参照**新しく作成された目的とした拡張機能のフォルダーです。 (つまり、アプリは、上記で作成) 共通の共有コード ライブラリのプロジェクトの名前を確認し、をクリックして、 **OK**ボタンをクリックします。 

    [![](implementing-sirikit-images/intents14.png "共通の共有コード ライブラリのプロジェクトの名前を選択します。")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 右クリックし、**ソリューション名**で、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクトの追加.**
2. ダイアログ ボックスから選択**iOS** > **拡張機能** > **UI 拡張機能の目的とした** をクリックし、**次**ボタンをクリックします。
3. 次を入力、**名前**目的とした拡張機能とをクリックして、 **[ok]**ボタンをクリックします。
5. **ソリューション エクスプ ローラー**を右クリックし、**参照**新しく作成された目的とした拡張機能のフォルダーです。 (アプリは、上記で作成) を共通の共有コード ライブラリのプロジェクトの名前を確認し、 **OK**ボタンをクリックします。
    
-----

### <a name="configuring-the-infoplist"></a>Info.plist の構成

インテント UI 拡張機能の構成`Info.plist`アプリを操作するファイル。

アプリがの既存のキーを持つ、一般的なアプリの拡張と同じように`NSExtension`と`NSExtensionAttributes`です。 インテント拡張では、1 つの新しい属性を構成する必要があります。

[![](implementing-sirikit-images/intents03.png "構成する必要がある 1 つの新しい属性")](implementing-sirikit-images/intents03.png#lightbox)

**IntentsSupported**は必須であり、アプリが目的とした拡張機能をサポートする目的のクラス名の配列で構成されます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

目的の UI 拡張機能の構成に`Info.plist`ファイルをダブルクリック、**ソリューション エクスプ ローラー**編集用に開きます。 次に、スイッチ、**ソース**を表示し、展開、`NSExtension`と`NSExtensionAttributes`エディター内のキー。

[![](implementing-sirikit-images/intents04.png "エディターで NSExtension と NSExtensionAttributes キー")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

目的の UI 拡張機能の構成に`Info.plist`ファイルをダブルクリック、**ソリューション エクスプ ローラー**編集用に開きます。 展開して、`NSExtension`と`NSExtensionAttributes`エディター内のキー。

[![](implementing-sirikit-images/intents04w.png "エディター内の %t NSExtension と NSExtensionAttributes キー")](implementing-sirikit-images/intents04w.png#lightbox)

-----

展開して、`IntentsSupported`キーし、この拡張機能がサポートする任意の目的としたクラスの名前を追加します。 例の MonkeyChat アプリのサポート、 `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents15.png "INSendMessageIntent キー")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents15w.png "INSendMessageIntent キー")](implementing-sirikit-images/intents15w.png#lightbox)

-----

使用可能な目的としたドメインの完全な一覧を参照してください Apple の[ドメインの参照を目的とした](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)です。

### <a name="configuring-the-main-class"></a>メインのクラスを構成します。

Siri を目的とした UI 拡張機能のメイン エントリ ポイントとして機能する主要なクラスを構成します。 サブクラスである必要があります、`UIViewController`に準拠している`IINUIHostedViewController`インターフェイスです。 例:

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

Siri が合格、`INInteraction`クラスのインスタンス、`Configure`のメソッド、`UIViewController`インテント UI 拡張機能内でインスタンス。

`INInteraction`オブジェクトは、次の 3 つの重要な情報を拡張機能を提供します。

1. 処理中のインテント オブジェクト。
2. 目的とした応答オブジェクトから、`Confirm`と`Handle`目的とした拡張機能のメソッドです。
3. アプリと Siri 間の相互作用の状態を定義する対話状態です。

`UIViewController`インスタンスが Siri との相互作用の原則クラスから継承しているため、 `UIViewController`UIKit の機能をすべてにアクセスします。

Siri が呼び出されると、`Configure`のメソッド、`UIViewController`ビュー コント ローラーが Siri Snippit またはマップのカードでホストするかされることを示すビュー コンテキストを渡します。

Siri が、アプリがアプリでは、その構成が終了した後に、ビューの目的のサイズを返す必要がある完了ハンドラーに渡すこともできます。

### <a name="design-the-ui-in-ios-designer"></a>IOS デザイナーで、UI を設計します。

IOS デザイナーで目的の UI 拡張機能のユーザー インターフェイスをレイアウトします。 ダブルクリックして、拡張機能の`MainInterface.storyboard`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。 すべてのユーザー インターフェイスを構築し、変更を保存する必須の UI 要素にドラッグします。

> [!IMPORTANT]
> **注:**などの対話型要素を追加することができますが`UIButtons`または`UITextFields`を目的とした UI 拡張機能の`UIViewController`非対話型の目的とした UI と禁じこれら、およびユーザーをやり取りすることにすることはできません。

### <a name="wire-up-the-user-interface"></a>ネットワーク上のユーザー インターフェイス

インテント UI 拡張機能のユーザー インターフェイスが iOS デザイナーで作成、編集、`UIViewController`サブクラスと上書き、`Configure`次のようにメソッド。

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

インテント UI 拡張機能が常にする、アプリのアイコンと、UI の上部にある名などの他の Siri 内容と共に表示または、目的に基づいて、(送信やキャンセル) などのボタンが下部に表示されます。

アプリが Siri が既定では、メッセージングなどのユーザーに表示されている情報や、アプリがアプリに合わせて調整いずれかの既定のエクスペリエンスを置き換えることができます場所マップに置き換えることができます、いくつかのインスタンスが生じます。

インテント UI 拡張機能が既定の Siri UI の要素をオーバーライドする必要がある場合、`UIViewController`サブクラスを実装する必要があります、`IINUIHostedViewSiriProviding`インターフェイスと、特定のインターフェイス要素の表示にオプトインします。

次のコードを追加、`UIViewController`サブクラス Siri インテント UI 拡張機能が既にメッセージの内容を表示することを確認します。

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>注意事項

Apple では、開発者がデザインと目的の UI 拡張機能を実装するときに、次の点を考慮することを示しています。

- **重視のメモリ使用量をする**- ためのインテント UI 拡張機能は一時的なものとシステムによる制限より完全アプリとも緊密なメモリの制約がだけで、短い形式の時刻を表示します。
- **最小値と最大サイズの表示の検討**-インテント UI 拡張機能がすべての iOS デバイスの種類、サイズと方向に良いことを確認してください。 さらに、アプリが Siri に送り返す必要なサイズできないことがありますを許可します。
- **柔軟でアダプティブ レイアウト パターンを使用して**再び、UI がないすべてのデバイスに優れたいることを確認します。

## <a name="summary"></a>まとめ

この記事は、SiriKit をカバーし、iOS デバイスで Siri とマップのアプリを使用してユーザーがアクセスできるサービスを提供する Xamarin.iOS アプリに追加する方法を示すは。




## <a name="related-links"></a>関連リンク

- [ElizaChat サンプル](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [インテント Framework リファレンス](https://developer.apple.com/reference/intents)
- [目的の UI フレームワークのリファレンス](https://developer.apple.com/reference/intentsui)
- [目的のドメインの参照](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
