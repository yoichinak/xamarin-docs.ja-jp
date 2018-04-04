---
title: ソーシャル フレームワーク
description: ソーシャル フレームワークでは、Twitter、Facebook、とともに SinaWeibo を中国でのユーザーなどのソーシャル ネットワークと対話するため、統合 API を提供します。
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 334e05ad653d766b48f7f6028a1e98b0a0548c0c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="social-framework"></a>ソーシャル フレームワーク

_ソーシャル フレームワークでは、Twitter、Facebook、とともに SinaWeibo を中国でのユーザーなどのソーシャル ネットワークと対話するため、統合 API を提供します。_


認証を管理することがなく、単一の API からソーシャル ネットワークと対話するアプリケーションをソーシャル フレームワークを使用できます。 これには、投稿だけでなく HTTP を介した各ソーシャル ネットワークの API を使用できるようにする抽象化を作成するためのビューのコント ローラーを用意されたシステムが含まれます。

> [!IMPORTANT]
> クロス プラットフォーム API さまざまなソーシャル ネットワークに接続するには、次を参照してください。、 [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) Xamarin コンポーネント ストア コンポーネントです。

## <a name="connecting-to-twitter"></a>Twitter への接続

### <a name="twitter-account-settings"></a>Twitter アカウントの設定

Twitter のソーシャル フレームワークを使用してに接続するには、アカウントを次に示すように、デバイスの設定で構成する必要があります。

 [![](social-framework-images/twitter01.png "Twitter アカウントの設定")](social-framework-images/twitter01.png#lightbox)

アカウントが入力され、Twitter で検証されて、Twitter のアクセスに、ソーシャル Framework のクラスを使用するデバイス上のアプリケーションはこのアカウントを使用します。

### <a name="sending-tweets"></a>ツイートの送信

ソーシャル フレームワークと呼ばれるコント ローラーが含まれています。`SLComposeViewController`を編集し、ツイートを送信する用意されたシステム ビューを表示します。 次のスクリーン ショットは、このビューの例を示します。

 [![](social-framework-images/twitter02.png "このスクリーン ショットは、SLComposeViewController の例を示しています。")](social-framework-images/twitter02.png#lightbox)

使用する、 `SLComposeViewController` Twitter とには、呼び出すことによって、コント ローラーのインスタンスを作成、`FromService`メソッドを`SLServiceType.Twitter`次のようにします。

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

後に、`SLComposeViewController`インスタンスが返されます、Twitter に投稿する UI を表示するために使用できます。 最初にすることが呼び出すことによってこのケースでは、Twitter、ソーシャル ネットワークの可用性をチェックする`IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` ユーザーの介入なしで直接ツイートを送信しません。 ただし、以下の方法で初期化することができます。

-   `SetInitialText` –、ツイートに表示するのには、最初のテキストを追加します。 
-  `AddUrl` –、ツイートに Url を追加します。
-  `AddImage` –、ツイートにイメージを追加します。


初期化されると、呼び出す`PresentVIewController`によって作成されたビューが表示されます、`SLComposeViewController`です。 ユーザーことができますし、必要に応じて編集、ツイートの送信や取り消しの送信。 どちらの場合に、コント ローラーを破棄する必要があります、`CompletionHandler`場所結果ことができますも確認する、ツイートの送信または取り消された場合、次のように、します。

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>ツイートの例

次のコード例を使用して、`SLComposeViewController`ツイートの送信に使用されるビューを提供します。

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

ソーシャル フレームワークには、ソーシャル ネットワークへの HTTP 要求を行うためのサポートも含まれています。 要求をカプセル化、`SLRequest`ソーシャル ネットワークの特定の API を対象に使用されるクラスです。

たとえば、次のコードは、Twitter によって取得するパブリック タイムライン (上記のコードを拡張する) への要求に。

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

このコードの詳細を見てみましょう。 最初に、アカウント ストアにアクセスして、Twitter アカウントの種類を取得します。

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

次に、アプリの Twitter アカウントにアクセスすることが、アクセスが付与されるアカウントに読み込まれ、メモリと UI の更新、ユーザーが要求します。

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

ユーザー (ボタンをタップした UI で) タイムライン データの要求、アプリはまず Twitter からデータにアクセスする要求を形成します。

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
この例は、最後の 10 個のエントリに返される結果を制限するを含めることによって、 `?count=10` URL にします。 最後に、(上記読み込まれた)、Twitter アカウントに要求をアタッチし、データをフェッチする Twitter への呼び出しを実行します。

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

データが正常に読み込まれた、(次の例の出力) のように、生の JSON データが表示されます。

[![](social-framework-images/twitter03.png "生の JSON データの表示の例")](social-framework-images/twitter03.png#lightbox)

実際のアプリに JSON の結果が、標準と、ユーザーに対して表示される結果として解析できませんでした。 参照してください[Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)JSON を解析する方法についてはします。

## <a name="connecting-to-facebook"></a>Facebook に接続します。

### <a name="facebook-account-settings"></a>Facebook アカウントの設定

ソーシャル フレームワークと Facebook への接続は、前に示した Twitter を使用するプロセスとほぼ同じです。 Facebook ユーザー アカウントは、次に示すよう、デバイスの設定で構成する必要があります。

[![](social-framework-images/facebook01.png "Facebook アカウントの設定")](social-framework-images/facebook01.png#lightbox)

構成されると、ソーシャル Framework を使用するデバイス上の任意のアプリケーションは Facebook に接続するこのアカウントを使用します。

### <a name="posting-to-facebook"></a>Facebook に投稿します。

ソーシャル フレームワークは、統合 API の複数のソーシャル ネットワークにアクセスするよう設計されていますが、コードは使用されている、ソーシャル ネットワークに関係なくとほぼ同じです。

たとえば、`SLComposeViewController`のみ異なるが、Facebook 固有の設定とオプションに切り替える前に示した Twitter 例と同様に正確に使用することができます。 例えば:

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

Facebook で使用する場合、`SLComposeViewController`を示す、Twitter 例とほぼ同じ検索をビューを表示**Facebook**ここでは、タイトルとして。

[![](social-framework-images/facebook02.png "SLComposeViewController 表示")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Facebook Graph API の呼び出し

例のように、Twitter、ソーシャル フレームワークの`SLRequest`Facebook の graph API を使用してオブジェクトを使用できます。 たとえば、次のコードは、(上記のコードを拡張する) で Xamarin アカウントについての graph API から情報を返します。

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

このコードと、上に示した Twitter バージョンの唯一の違いは、ID を取得する開発者/アプリケーション固有 (Facebook の開発者ポータルから生成することができます) を要求を行うときにオプションとして設定する必要があります Facebook の要件を示します。

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

このオプション (または、無効なキーを使用して) の設定に失敗したが、エラーまたは返されるデータはありません発生します。

## <a name="summary"></a>まとめ

この記事では、ソーシャル フレームワークを使用して Twitter および Facebook と対話する方法を示しました。 デバイスの設定でソーシャル ネットワークごとにアカウントを構成する場所を示しました。 使用する方法についても説明、`SLComposeViewController`ソーシャル ネットワークへの投稿の統一されたビューを提供します。 調査がさらに、`SLRequest`ソーシャル ネットワークごとの API の呼び出しに使用されるクラスです。


## <a name="related-links"></a>関連リンク

- [SocialFrameworkDemo (sample)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)
