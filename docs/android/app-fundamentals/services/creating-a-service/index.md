---
title: サービスの作成
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: 8c2086025ccb5fe41b3ffddc9cd650c1e0c81fbc
ms.sourcegitcommit: f890b5ec9b7c2702875070859e1a8cbf6e870e46
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/28/2018
ms.locfileid: "53813986"
---
# <a name="creating-a-service"></a>サービスの作成

Xamarin.Android サービスは、Android のサービスの 2 つの壊れない規則に従う必要があります。

* これらを拡張する必要があります、 [ `Android.App.Service`](https://developer.xamarin.com/api/type/Android.App.Service/)します。
* 修飾する必要がありますが、 [ `Android.App.ServiceAttribute`](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)します。

Android サービスのもう 1 つの要件に登録する必要があります、 **AndroidManifest.xml**一意の名前を指定します。 Xamarin.Android は自動的に登録、サービス マニフェストで必要な XML 属性を持つビルド時にします。

このコード スニペットでは、これら 2 つの要件を満たしている Xamarin.Android でサービスの作成の最も簡単な例を示します。  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Xamarin.Android には、次の XML 要素を挿入することで、サービスを登録する、コンパイル時**AndroidManifest.xml** (Xamarin.Android にサービスのランダムな名前が生成されることに注意してください)。

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

によって、その他の Android アプリケーションとサービスを共有することは_エクスポート_こと。 これは、設定によって実現されます、`Exported`プロパティを`ServiceAttribute`します。 サービスをエクスポートするときに、`ServiceAttribute.Name`プロパティは、サービスの意味のあるパブリック名を指定するも設定する必要があります。 このスニペットでは、サービスの名前をエクスポートする方法を示します。

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

**AndroidManifest.xml**ようこのサービスの要素になりますし。

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

サービスでは、サービスが作成されると呼び出されるコールバック メソッドで、独自のライフ サイクルがあります。 どのメソッドが呼び出される正確には、サービスの種類によって異なります。 開始されるサービスは、ハイブリッド サービスが開始されるサービスとバインドされているサービスの両方のコールバック メソッドを実装する必要があります、バインドされているサービスより別のライフ サイクル メソッドを実装する必要があります。 これらのメソッドのすべてのメンバーである、`Service`クラスは、サービスを開始する方法が決まりますがどのようなライフ サイクル メソッドが呼び出されます。 これらのライフ サイクル メソッドは、後で詳しく説明されます。

既定では、サービスは、Android アプリケーションと同じプロセスで開始されます。 設定して、独自のプロセスでサービスを開始することは、`ServiceAttribute.IsolatedProcess`プロパティを true にします。

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

次の手順では、サービスを起動し、次の 3 つの異なる種類のサービスを実装する方法を確認する移動する方法について説明します。

> [!NOTE]
> UI スレッドで実行しているサービス、すべての作業が UI をブロックするを実行する場合は、サービスが作業を実行するスレッドを使用する必要があります。

## <a name="starting-a-service"></a>サービスの開始

Android でサービスを開始する最も基本的な方法は、ディスパッチする、`Intent`を含むサービスを開始するかを識別するメタデータ。 サービスを開始するために使用するインテントの 2 つの異なるスタイルがあります。

-   **明示的インテント** &ndash; 、_明示的インテント_特定のアクションを完了するどのようなサービスを使用する正確に識別されます。 明示的なインテントを特定のアドレスを持つ文字として考えることができます。Android では、目的を明示的に識別されるサービスにルーティングします。 このスニペットは、1 つの例と呼ばれるサービスを開始する明示的なインテントを使用する`DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **暗黙的インテント**&ndash;疎の目的は、この型を識別の操作を実行するが、そのアクションを完了する正確なサービスが不明です。 暗黙的インテントは、ある文字が「To Whom It May 問題…」をアドレス指定、考えることができます。
    Android は、目的の内容を確認し、目的に一致する既存のサービスがあるかどうかを判断します。

    _インテント フィルター_登録されているサービスの暗黙的な目的を検索するために使用します。 インテントのフィルターに追加される XML 要素は、 **AndroidManifest.xml**暗黙的インテントによるサービスを検索するために必要なメタ データを格納します。

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Android の暗黙的なインテントに一致する 1 つ以上の場合は、ユーザー アクションを処理するためにコンポーネントを選択を求める場合があります。

![曖昧性除去ダイアログのスクリーン ショット](images/creating-a-service-01.png "曖昧性除去ダイアログのスクリーン ショット")

> [!IMPORTANT]
> Android 5.0 (AP レベル 21) 以降では、サービスを開始する暗黙的なインテントは使用できません。

可能であれば、アプリケーションは、サービスを開始するのに明示的なインテントを使用する必要があります。 暗黙的インテントが特定のサービスの開始を要求しない&ndash;は要求を処理するには、デバイスにインストールされているいくつかのサービスの要求。 このあいまいな要求と、要求または別のアプリが不必要に開始 (これは、デバイス上のリソースの負荷が大きくなります) を処理する不適切なサービスがあります。

目的をディスパッチする方法と、サービスの種類に依存し、サービスの種類ごとに固有のガイドで後で詳しく説明されます。


### <a name="creating-an-intent-filter-for-implicit-intents"></a>暗黙的インテントのインテントのフィルターを作成します。

暗黙的インテントに関連付けるサービスには、Android のアプリは、サービスの機能を識別するいくつかのメタ データを提供する必要があります。 このメタデータがによって提供される_インテント フィルター_します。 インテント フィルターには、アクションまたはサービスを開始するインテントに存在する必要がある、データの型など、いくつかの情報が含まれます。 Xamarin.Android では、インテント フィルターが登録されている**AndroidManifest.xml**サービスを修飾することによって、 [ `IntentFilterAttribute`](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)します。 たとえば、次のコードは、インテント フィルターとの関連付けられているアクションを追加します`com.xamarin.DemoService`:。

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

これは、結果に含まれるエントリ、 **AndroidManifest.xml**ファイル&ndash;次の例に似ていますようにアプリケーションをパッケージ化されているエントリ。

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Xamarin.Android のサービスの基礎、サービスの詳細のさまざまなサブタイプを調べてみましょう。


## <a name="related-links"></a>関連リンク

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
