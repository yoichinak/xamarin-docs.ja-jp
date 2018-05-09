---
title: サービスの作成
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 05/03/2018
ms.openlocfilehash: 00785ad161f5f05fd70b059bb0a3f1c8d6c31f97
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="creating-a-service"></a>サービスの作成

Xamarin.Android サービスは、Android のサービスの 2 つの inviolable 規則に従う必要があります。

* 拡張する必要があります、 [ `Android.App.Service`](https://developer.xamarin.com/api/type/Android.App.Service/)です。
* 使用する装飾する必要があります、 [ `Android.App.ServiceAttribute`](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)です。

Android のサービスの他の要件に登録する必要があります、 **AndroidManifest.xml**一意の名前を指定します。 Xamarin.Android は自動的に登録サービス マニフェストのビルド時に必要な XML 属性を持つ。

このコード スニペットは、これら 2 つの要件を満たす Xamarin.Android でサービスを作成する最も簡単な例を示します。  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Xamarin.Android に次の XML 要素を挿入することで、サービスを登録するコンパイル時に、 **AndroidManifest.xml** (Xamarin.Android にサービスのランダムな名前が生成されることに注意してください)。

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

によって他の Android アプリケーションとサービスを共有することは_エクスポート_ことです。 これを実現するには、`Exported`プロパティを`ServiceAttribute`です。 サービスをエクスポートするときに、`ServiceAttribute.Name`プロパティは、サービスの場合は、意味のあるパブリック名を指定するも設定する必要があります。 このスニペットは、サービスの名前をエクスポートする方法を示します。

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

**AndroidManifest.xml**このサービスの要素は、ようになります。

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

サービスには、サービスが作成されるときに呼び出されるコールバック メソッドの独自のライフ サイクルがあります。 どのメソッドが呼び出される正確には、サービスの種類によって異なります。 開始されるサービスは、ハイブリッド サービスが開始されるサービスとバインドされているサービスの両方のコールバック メソッドを実装する必要があります、バインドされているサービスよりも別のライフ サイクル メソッドを実装する必要があります。 これらのメソッドのすべてのメンバーである、`Service`クラス以外の場合は、サービスを開始する方法が決まりますどのようなライフ サイクル メソッドが呼び出されます。 これらのライフ サイクル メソッドは、後で詳しく説明されます。

既定では、サービスは、Android のアプリケーションと同じプロセスで開始されます。 設定して、独自のプロセスで、サービスを開始することは、`ServiceAttribute.IsolatedProcess`プロパティを true にします。

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

次の手順では、サービスを開始し、その後、次の 3 つの異なる種類のサービスを実装する方法を確認する方法を確認します。

> [!NOTE]
> UI スレッドで、サービスが実行されるため、どの作業がある実行 UI がブロックされる場合は、サービスが作業を実行するスレッドを使用する必要があります。

## <a name="starting-a-service"></a>サービスの開始

Android でのサービスを開始する最も基本的な方法は、ディスパッチする、`Intent`どのサービスを開始するかを識別するメタデータが含まれています。 サービスを開始するために使用するインテントの 2 つの異なるスタイルがあります。

-   **明示的な目的とした** &ndash; 、_明示的な目的とした_を特定のアクションを完了するどのようなサービスを使用する正確に識別されます。 明示的な目的はようなものを特定のアドレスを持つ文字Android では、目的を明示的に識別されるサービスにルーティングします。 このスニペットは、1 つの明示的な目的を使用して呼び出されるサービスを開始する例を`DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **暗黙的な目的とした**&ndash;の目的としたこの型は疎を識別、アクションを実行するが、その操作を完了する特定のサービスが不明です。 暗黙的なインテント見なすことができますの文字です「To Whom It が問題にならなければ…」を指定します。
    Android は、目的の内容を確認し、目的に一致する既存のサービスがないか確認します。

    _インテント フィルター_登録されているサービスと暗黙的な使用目的に合わせてのために使用します。 インテントのフィルターに追加される XML 要素である**AndroidManifest.xml**暗黙のインテントを指定してサービスを検索するために必要なメタデータが含まれています。

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Android の 1 つ以上と一致する候補、暗黙的な目的とした場合は、アクションを処理するコンポーネントを選択するユーザーを依頼可能性があります。

![あいまいさを排除ダイアログのスクリーン ショット](images/creating-a-service-01.png "あいまいさを排除ダイアログのスクリーン ショット")

> [!IMPORTANT]
> Android 5.0 (AP レベル 21) 以降では、サービスを開始する暗黙的な目的は使用できません。

可能であれば、アプリケーションは、サービスを開始する、明示的なインテントを使用してください。 暗黙的な目的は、特定のサービスを開始を要求しない&ndash;は要求を処理するには、デバイスにインストールされているいくつかのサービスを要求します。 このあいまいな要求は、要求またはその他のアプリが不必要に開始 (が増えるため、デバイス上のリソースの不足) の処理の正しくないサービスになります。

目的のディスパッチ、サービスの種類に依存し、サービスの種類ごとに特定ガイドの後半で詳しく説明されます。


### <a name="creating-an-intent-filter-for-implicit-intents"></a>暗黙的なインテントのインテントのフィルターを作成します。

暗黙的なインテントを指定して、サービスを関連付けるには、Android アプリが、サービスの機能を識別する一部のメタ データを指定してください。 このメタデータは、によって提供される_インテント フィルター_です。 目的のフィルターには、アクションであるか、サービスを開始することを目的に存在する必要があるデータの種類など、一部の情報が含まれます。 Xamarin.Android で目的のフィルターが登録されている**AndroidManifest.xml**サービスを修飾することによって、 [ `IntentFilterAttribute`](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)です。 次のコードでの関連する操作が意図的にフィルターを追加するなど、 `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

これは、結果に含まれるエントリで、 **AndroidManifest.xml**ファイル&ndash;次の例に似た方法でアプリケーションをパッケージ化されているエントリ。

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Xamarin.Android サービスを邪魔の基本知識を調べてみましょう、サービスの詳細のサブタイプが異なる。


## <a name="related-links"></a>関連リンク

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
