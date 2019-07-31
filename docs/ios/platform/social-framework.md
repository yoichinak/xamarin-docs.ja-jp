---
title: Xamarin のソーシャルフレームワーク
description: ソーシャルフレームワークは、Twitter や Facebook などのソーシャルネットワークや中国のユーザー向けの SinaWeibo と対話するための統一された API を提供します。
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 715fd408cc05671beba0277a690585fdbb558c7e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654025"
---
# <a name="social-framework-in-xamarinios"></a>Xamarin のソーシャルフレームワーク

_ソーシャルフレームワークは、Twitter や Facebook などのソーシャルネットワークや中国のユーザー向けの SinaWeibo と対話するための統一された API を提供します。_

ソーシャルフレームワークを使用すると、アプリケーションは認証を管理しなくても、1つの API からソーシャルネットワークと対話できます。 これには、投稿を作成するためのシステム提供のビューコントローラーと、HTTP で各ソーシャルネットワークの API を使用できる抽象化が含まれています。

> [!IMPORTANT]
> クロスプラットフォーム API を使用してさまざまなソーシャルネットワークに接続する方法については、Xamarin コンポーネントストアの「 [xamarin. ソーシャル](http://components.xamarin.com/view/xamarin.social/)コンポーネント」を参照してください。

## <a name="connecting-to-twitter"></a>Twitter への接続

### <a name="twitter-account-settings"></a>Twitter アカウントの設定

ソーシャルフレームワークを使用して Twitter に接続するには、次に示すように、デバイスの設定でアカウントを構成する必要があります。

 [![](social-framework-images/twitter01.png "Twitter アカウントの設定")](social-framework-images/twitter01.png#lightbox)

アカウントが Twitter によって入力され、確認されると、ソーシャルフレームワーククラスを使用して Twitter にアクセスするデバイス上のアプリケーションは、このアカウントを使用します。

### <a name="sending-tweets"></a>ツイートの送信

ソーシャルフレームワークには、ツイートを`SLComposeViewController`編集および送信するためのシステム指定のビューを提供する、というコントローラーが含まれています。 次のスクリーンショットは、このビューの例を示しています。

 [![](social-framework-images/twitter02.png "このスクリーンショットは、Slている例を示しています。")](social-framework-images/twitter02.png#lightbox)

Twitter `SLComposeViewController`でを使用するには、次に示すように、で`FromService` `SLServiceType.Twitter`メソッドを呼び出すことによって、コントローラーのインスタンスを作成する必要があります。

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

`SLComposeViewController`インスタンスが返された後、そのインスタンスを使用して、Twitter にポストするための UI を表示できます。 ただし、最初に、次のようにを呼び出し`IsAvailable`て、ソーシャルネットワーク (この場合は Twitter) の可用性を確認します。

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController`は、ユーザーの介入なしに直接ツイートを送信しません。 ただし、次のメソッドを使用して初期化することができます。

-   `SetInitialText`–ツイートに表示する最初のテキストを追加します。 
-  `AddUrl`–ツイートに Url を追加します。
-  `AddImage`–イメージをツイートに追加します。


初期化される`PresentVIewController`と、を呼び出すと`SLComposeViewController`、によって作成されたビューが表示されます。 ユーザーは必要に応じて、ツイートを編集して送信することも、送信をキャンセルすることもできます。 どちらの場合も、コントローラーはで`CompletionHandler`破棄される必要があります。この場合、次に示すように、ツイートが送信またはキャンセルされたかどうかを確認するために結果を確認することもできます。

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>ツイートの例

次のコードは、 `SLComposeViewController`を使用して、ツイートの送信に使用されるビューを表示する方法を示しています。

```csharp
using System;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _twitterComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
        #endregion

        #region Computed Properties
        public bool isTwitterAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Twitter); }
        }

        public SLComposeViewController TwitterComposer {
            get { return _twitterComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            SendTweet.Enabled = isTwitterAvailable;
        }
        #endregion

        #region Actions
        partial void SendTweet_TouchUpInside (UIButton sender)
        {
            // Set initial message
            TwitterComposer.SetInitialText ("Hello Twitter!");
            TwitterComposer.AddImage (UIImage.FromFile ("Icon.png"));
            TwitterComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (TwitterComposer, true, null);
        }
        #endregion
    }
}
```

### <a name="calling-twitter-api"></a>Twitter API の呼び出し

ソーシャルフレームワークには、ソーシャルネットワークに対する HTTP 要求の作成もサポートされています。 このメソッドは、特定の`SLRequest`ソーシャルネットワークの API を対象とするために使用されるクラスに要求をカプセル化します。

たとえば、次のコードでは、(上記のコードを展開して) パブリックタイムラインを取得するように Twitter に要求しています。

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _twitterAccount;
#endregion

#region Computed Properties
public ACAccount TwitterAccount {
    get { return _twitterAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    SendTweet.Enabled = isTwitterAvailable;
    RequestTwitterTimeline.Enabled = false;

    // Initialize Twitter Account access 
    var accountStore = new ACAccountStore ();
    var accountType = accountStore.FindAccountType (ACAccountType.Twitter);

    // Request access to Twitter account
    accountStore.RequestAccess (accountType, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestTwitterTimeline.Enabled = true;
            });
        }
    });
}
#endregion

#region Actions
partial void RequestTwitterTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
    var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = TwitterAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

このコードを詳しく見てみましょう。 まず、アカウントストアへのアクセス権を取得し、Twitter アカウントの種類を取得します。

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

次に、アプリが Twitter アカウントにアクセスできるかどうかをユーザーにたずね、アクセス権が付与されると、アカウントがメモリに読み込まれ、UI が更新されます。

```csharp
// Request access to Twitter account
accountStore.RequestAccess (accountType, (granted, error) => {
    // Allowed by user?
    if (granted) {
        // Get account
        _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
        InvokeOnMainThread (() => {
            // Update UI
            RequestTwitterTimeline.Enabled = true;
        });
    }
});
```

ユーザーが (UI のボタンをタップして) タイムラインデータを要求すると、アプリはまず Twitter からデータにアクセスするための要求を行います。

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
この例では、URL にを含める`?count=10`ことで、返される結果を最後の10個のエントリに限定しています。 最後に、要求を (上記で読み込まれた) Twitter アカウントにアタッチし、Twitter への呼び出しを実行してデータをフェッチします。

```csharp
// Request data
request.Account = TwitterAccount;
request.PerformRequest ((data, response, error) => {
    // Was there an error?
    if (error == null) {
        // Was the request successful?
        if (response.StatusCode == 200) {
            // Yes, display it
            InvokeOnMainThread (() => {
                Results.Text = data.ToString ();
            });
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", response.StatusCode);
            });
        }
    } else {
        // No, display error
        InvokeOnMainThread (() => {
            Results.Text = string.Format ("Error: {0}", error);
        });
    }
});
```

データが正常に読み込まれた場合は、次の出力例のように生の JSON データが表示されます。

[![](social-framework-images/twitter03.png "生の JSON データ表示の例")](social-framework-images/twitter03.png#lightbox)

実際のアプリでは、JSON の結果が通常どおりに解析され、結果がユーザーに表示されます。 JSON を解析する方法の詳細については、「 [Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)」を参照してください。

## <a name="connecting-to-facebook"></a>Facebook に接続しています

### <a name="facebook-account-settings"></a>Facebook アカウントの設定

ソーシャルフレームワークを使用した Facebook への接続は、上に示した Twitter で使用されるプロセスとほぼ同じです。 Facebook ユーザーアカウントは、次に示すように、デバイスの設定で構成する必要があります。

[![](social-framework-images/facebook01.png "Facebook アカウントの設定")](social-framework-images/facebook01.png#lightbox)

構成が完了すると、ソーシャルフレームワークを使用するデバイス上のアプリケーションは、このアカウントを使用して Facebook に接続します。

### <a name="posting-to-facebook"></a>Facebook への投稿

ソーシャルフレームワークは、複数のソーシャルネットワークにアクセスするように設計された統合 API であるため、使用されているソーシャルネットワークに関係なく、コードはほぼ同じままです。

たとえば、前に`SLComposeViewController`示した Twitter の例とまったく同じようにを使用できますが、唯一の違いは Facebook 固有の設定およびオプションへの切り替えです。 例えば:

```csharp
using System;
using Foundation;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _facebookComposer = SLComposeViewController.FromService (SLServiceType.Facebook);
        #endregion

        #region Computed Properties
        public bool isFacebookAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Facebook); }
        }

        public SLComposeViewController FacebookComposer {
            get { return _facebookComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            PostToFacebook.Enabled = isFacebookAvailable;
        }
        #endregion

        #region Actions
        partial void PostToFacebook_TouchUpInside (UIButton sender)
        {
            // Set initial message
            FacebookComposer.SetInitialText ("Hello Facebook!");
            FacebookComposer.AddImage (UIImage.FromFile ("Icon.png"));
            FacebookComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (FacebookComposer, true, null);
        }
        #endregion
    }
}
```

Facebook `SLComposeViewController`と共に使用すると、Twitter の例とほぼ同じ外観のビューが表示されます。この場合のタイトルとして**Facebook**が示されます。

[![](social-framework-images/facebook02.png "Slて Seviewcontroller ディスプレイ")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Facebook Graph API を呼び出しています

Twitter の例と同様に、ソーシャルフレームワークの`SLRequest`オブジェクトを Facebook の graph API と共に使用できます。 たとえば、次のコードでは、Xamarin アカウントに関する graph API から情報が返されます (上記のコードを展開することによって)。

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _facebookAccount;
#endregion

#region Computed Properties
public ACAccount FacebookAccount {
    get { return _facebookAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    PostToFacebook.Enabled = isFacebookAvailable;
    RequestFacebookTimeline.Enabled = false;

    // Initialize Facebook Account access 
    var accountStore = new ACAccountStore ();
    var options = new AccountStoreOptions ();
    var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
    accountType = accountStore.FindAccountType (ACAccountType.Facebook);

    // Request access to Facebook account
    accountStore.RequestAccess (accountType, options, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _facebookAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestFacebookTimeline.Enabled = true;
            });
        }
    });

}
#endregion

#region Actions
partial void RequestFacebookTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl ("https://graph.facebook.com/283148898401104");
    var request = SLRequest.Create (SLServiceKind.Facebook, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = FacebookAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

このコードと前述の Twitter バージョンとの唯一の実際の違いは、Facebook の要件により、要求を行うときにオプションとして設定する必要がある開発者/アプリ固有の ID (Facebook の開発者ポータルから生成可能) を取得する必要があることです。

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

このオプションを設定しなかった場合 (または無効なキーを使用した場合)、エラーが発生するか、データが返されません。

## <a name="summary"></a>まとめ

この記事では、ソーシャルフレームワークを使用して Twitter と Facebook を操作する方法について説明しました。 このチュートリアルでは、デバイス設定でソーシャルネットワークごとにアカウントを構成する場所を示しました。 また、を使用して、 `SLComposeViewController`ソーシャルネットワークに投稿するための統合ビューを提供する方法についても説明しました。 さらに、各ソーシャル`SLRequest`ネットワークの API の呼び出しに使用されるクラスを調べています。


## <a name="related-links"></a>関連リンク

- [社会 Alframeworkdemo (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/socialframeworkdemo)
- [Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)
