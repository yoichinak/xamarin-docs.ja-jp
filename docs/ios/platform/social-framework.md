---
title: Xamarin.iOS でソーシャル フレームワーク
description: ソーシャル フレームワークは、中国でのユーザーの Twitter および Facebook、ほか SinaWeibo をなどのソーシャル ネットワークと対話するための統一された API を提供します。
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 08ccd5b5ac78e82bf745764d70e59d2db9ec6776
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115706"
---
# <a name="social-framework-in-xamarinios"></a>Xamarin.iOS でソーシャル フレームワーク

_ソーシャル フレームワークは、中国でのユーザーの Twitter および Facebook、ほか SinaWeibo をなどのソーシャル ネットワークと対話するための統一された API を提供します。_

ソーシャル フレームワークを使用すると、アプリケーションが認証を管理することがなく、単一の API からのソーシャル ネットワークと対話します。 これには、HTTP 経由で各ソーシャル ネットワークの API を使用できるようにする抽象化と同様に投稿を作成するためのビュー コント ローラーが提供されているシステムが含まれます。

> [!IMPORTANT]
> さまざまなソーシャル ネットワークに接続するためのクロス プラットフォーム API を参照してください、 [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) Xamarin コンポーネント ストア コンポーネント。

## <a name="connecting-to-twitter"></a>Twitter に接続します。

### <a name="twitter-account-settings"></a>Twitter アカウントの設定

ソーシャル フレームワークを使用して Twitter に接続するには、アカウントを次に示すように、デバイス設定で構成する必要があります。

 [![](social-framework-images/twitter01.png "Twitter アカウントの設定")](social-framework-images/twitter01.png#lightbox)

アカウントを入力して、Twitter で検証されると、ソーシャル フレームワーク クラスを使用して、Twitter にアクセスするデバイス上の任意のアプリケーションはこのアカウントを使用します。

### <a name="sending-tweets"></a>ツイートを送信します。

ソーシャル フレームワークにはと呼ばれるコント ローラーが含まれています`SLComposeViewController`を編集し、ツイートの送信に提供されるシステム ビューを表示します。 次のスクリーン ショットでは、このビューの例を示します。

 [![](social-framework-images/twitter02.png "このスクリーン ショットは、SLComposeViewController の例を示しています。")](social-framework-images/twitter02.png#lightbox)

使用する、 `SLComposeViewController` twitter で呼び出すことによって、コント ローラーのインスタンスを作成する必要があります、`FromService`メソッド`SLServiceType.Twitter`次に示すよう。

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

後に、`SLComposeViewController`インスタンスが返されます、Twitter に投稿するための UI を提示するために使用できます。 ただし、最初には、呼び出すことで、この場合は、Twitter、ソーシャル ネットワークの可用性を確認する`IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` ユーザーの介入なしで直接、ツイートを送信しません。 ただし、次のメソッドで初期化できます。

-   `SetInitialText` – ツイートに表示するのには、最初のテキストを追加します。 
-  `AddUrl` – ツイートに Url を追加します。
-  `AddImage` – ツイートにイメージを追加します。


初期化されると、呼び出す`PresentVIewController`によって作成されたビューが表示されます、`SLComposeViewController`します。 ユーザーし、必要に応じて編集し、ツイートを送信したり送信をキャンセルできます。 どちらの場合に、コント ローラーを破棄する必要があります、 `CompletionHandler`、場所、結果チェックすることもに、次に示すように、ツイートの送信または取り消された場合を参照してください。

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>ツイートの例

次のコード例を使用して、`SLComposeViewController`ツイートの送信に使用されるビューに表示します。

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

### <a name="calling-twitter-api"></a>Twitter API を呼び出す

ソーシャル フレームワークには、ソーシャル ネットワークに HTTP 要求を行うためのサポートも含まれています。 要求をカプセル化、`SLRequest`ソーシャル ネットワークの特定の API をターゲットに使用されるクラスです。

たとえば、次のコードは、(上記のコードを拡張する) をパブリックのタイムラインを取得するのには Twitter への要求に。

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

このコードの詳細を見てみましょう。 最初に、アカウント ストアにアクセスし、Twitter アカウントの種類を取得します。

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

次に、自分の Twitter アカウントに、アプリにアクセスできるし、メモリと、UI の更新にアカウントが読み込まれる場合は、アクセスが許可かどうか、ユーザーが求められます。

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

(UI のボタンをタップして) をユーザーがタイムラインのデータを要求すると、アプリは、Twitter からデータにアクセスする要求を最初にフォームします。

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
この例を含めることで、最後の 10 個のエントリに返される結果を制限する`?count=10`URL にします。 最後に、(上記読み込まれた)、Twitter アカウントに、要求をアタッチし、データをフェッチする Twitter への呼び出しを実行します。

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

データの読み込みが成功した場合は (次の例の出力) のように、未加工の JSON データが表示されます。

[![](social-framework-images/twitter03.png "未加工の JSON データの表示の例")](social-framework-images/twitter03.png#lightbox)

実際のアプリで JSON の結果を標準と、ユーザーに表示される結果として解析し、でした。 参照してください[Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)JSON を解析する方法についてはします。

## <a name="connecting-to-facebook"></a>Facebook への接続

### <a name="facebook-account-settings"></a>Facebook アカウントの設定

ソーシャル フレームワークを使用して Facebook への接続は、前に示した Twitter を使用するプロセスとほぼ同じです。 Facebook ユーザー アカウントは、次に示すよう、デバイスの設定で構成する必要があります。

[![](social-framework-images/facebook01.png "Facebook アカウントの設定")](social-framework-images/facebook01.png#lightbox)

構成されると、ソーシャル フレームワークを使用するデバイス上の任意のアプリケーションは Facebook に接続するこのアカウントを使用します。

### <a name="posting-to-facebook"></a>Facebook に投稿します。

ソーシャル フレームワークは、複数のソーシャル ネットワークにアクセスするように設計 unified API は、コードは、ソーシャル ネットワークが使用されているに関係なくほぼ同じです。

たとえば、`SLComposeViewController`点だけが異なりますが、Facebook に固有の設定とオプションに切り替える前に示した例では、Twitter のように正確に使用することができます。 例えば:

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

Facebook では、使用すると、`SLComposeViewController`次を示す、Twitter の例とほぼ同じなビューが表示されます**Facebook**ここでは、タイトルとして。

[![](social-framework-images/facebook02.png "SLComposeViewController 表示")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Facebook Graph API の呼び出し

例のように、Twitter、ソーシャル フレームワークの`SLRequest`Facebook の graph API を使用してオブジェクトを使用できます。 たとえば、次のコードは、(上記のコードを拡張する) を Xamarin アカウントの詳細については、graph API から情報を返します。

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

このコードと、上で示した Twitter バージョンの唯一の違いは、開発者/特定するアプリ ID (Facebook の開発者ポータルから生成することができます)、要求を行うときに、オプションとして設定する必要がありますをするための Facebook の要件を示します。

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

このオプション (または、無効なキーを使用して) の設定に失敗したは、エラーまたは返されるデータなしのいずれかになります。

## <a name="summary"></a>まとめ

この記事では、Twitter および Facebook と対話するソーシャル フレームワークを使用する方法を示しました。 デバイス設定でソーシャル ネットワークごとにアカウントを構成する場所を示しました。 使用する方法についても説明、`SLComposeViewController`ソーシャル ネットワークへの投稿に対する統一されたビューに表示します。 さらに、その調査、`SLRequest`ソーシャル ネットワークの API の呼び出しに使用されるクラス。


## <a name="related-links"></a>関連リンク

- [SocialFrameworkDemo (サンプル)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)
