---
title: サービスの作成
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/03/2018
ms.openlocfilehash: 97c582ada0951f530885359112c3c7adfacc3502
ms.sourcegitcommit: bdb8ad7337931adf2ea45b10c2af81ecc4aad26a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/27/2019
ms.locfileid: "75500236"
---
# <a name="creating-a-service"></a>サービスの作成

Xamarin Android サービスは、次の2つの Android サービスの規則に従う必要があります。

- [`Android.App.Service`](xref:Android.App.Service)を拡張する必要があります。
- これらは、 [`Android.App.ServiceAttribute`](xref:Android.App.ServiceAttribute)で修飾する必要があります。

Android サービスのもう1つの要件は、ユーザーが**Androidmanifest .xml**に登録され、一意の名前が指定されていることです。 Xamarin Android は、ビルド時に必要な XML 属性を使用してサービスをマニフェストに自動的に登録します。

このコードスニペットは、次の2つの要件を満たす、Xamarin. Android でサービスを作成する最も簡単な例です。  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

コンパイル時に、Xamarin Android は、次の XML 要素を**Androidmanifest .xml**に挿入することによってサービスを登録します (xamarin android によってサービスに対してランダムな名前が生成されたことに注意してください)。

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

_エクスポート_することによって、サービスを他の Android アプリケーションと共有することができます。 これを行うには、`ServiceAttribute`の `Exported` プロパティを設定します。 サービスをエクスポートする場合は、サービスに対して意味のあるパブリック名を提供するために、`ServiceAttribute.Name` プロパティも設定する必要があります。 このスニペットは、サービスをエクスポートして名前を指定する方法を示しています。

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

このサービスの**Androidmanifest**要素は次のようになります。

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

サービスには、サービスの作成時に呼び出されるコールバックメソッドを含む独自のライフサイクルがあります。 どのメソッドが呼び出されるかは、サービスの種類によって異なります。 開始されるサービスは、バインドされたサービスとは異なるライフサイクルメソッドを実装する必要があります。一方、ハイブリッドサービスは、開始されたサービスとバインドされたサービスの両方に対してコールバックメソッドを実装する必要があります。 これらのメソッドは、`Service` クラスのすべてのメンバーです。サービスの開始方法によって、どのライフサイクルメソッドが呼び出されるかが決まります。 これらのライフサイクル方法については、後で詳しく説明します。

既定では、サービスは Android アプリケーションと同じプロセスで開始されます。 `ServiceAttribute.IsolatedProcess` プロパティを true に設定することで、独自のプロセスでサービスを開始することができます。

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

次の手順では、サービスを開始する方法を確認してから、3種類のサービスを実装する方法を確認します。

> [!NOTE]
> サービスは UI スレッドで実行されるので、UI をブロックする処理を実行する場合、サービスはスレッドを使用して作業を実行する必要があります。

## <a name="starting-a-service"></a>サービスの開始

Android でサービスを開始する最も基本的な方法は、開始するサービスを特定するのに役立つメタデータを含む `Intent` をディスパッチすることです。 サービスの開始に使用できるインテントには、次の2つの異なるスタイルがあります。

- 明示的な**インテント &ndash; 明示的に**指定すると、特定のアクションを完了するために使用する必要があるサービスが正確に_特定され_ます。 明示的なインテントは、特定のアドレスを持つ文字と考えることができます。Android は、明示的に識別されたサービスにインテントをルーティングします。 このスニペットは、明示的なインテントを使用して `DownloadService`というサービスを開始する例の1つです。

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

- **暗黙的なインテント**&ndash; この種類のインテントは、ユーザーが実行しようとしているアクションの種類を大まかに特定しますが、その操作を完了するための正確なサービスは不明です。 暗黙のインテントは、"関心のあるユーザー" に対応する文字と考えることができます。
    Android は目的の内容を調べ、目的に合った既存のサービスがあるかどうかを判断します。

    _インテントフィルター_を使用して、登録済みサービスとの暗黙的なインテントを照合します。 インテントフィルターは、サービスと暗黙的なインテントを照合するために必要なメタデータを含む**Androidmanifest .xml**に追加される xml 要素です。

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Android に暗黙的なインテントに関して複数の一致候補がある場合、そのアクションを処理するコンポーネントを選択するようにユーザーに要求することがあります。

![あいまいを解消するダイアログのスクリーンショット](images/creating-a-service-01.png "あいまいを解消するダイアログのスクリーンショット")

> [!IMPORTANT]
> Android 5.0 (AP レベル 21) 以降では、暗黙的なインテントを使用してサービスを開始することはできません。

可能であれば、アプリケーションは明示的インテントを使用してサービスを開始する必要があります。 暗黙的なインテントでは、特定のサービスの開始を要求することはありません。 &ndash;、要求を処理するためにデバイスにインストールされているサービスの要求です。 このあいまいな要求によって、要求または別のアプリを不必要に (デバイス上のリソースの負荷が増加する) サービスを正しく処理できないことがあります。

インテントのディスパッチ方法は、サービスの種類によって異なります。詳細については、各サービスの種類に固有のガイドの後半で説明します。

### <a name="creating-an-intent-filter-for-implicit-intents"></a>暗黙的インテントのインテントフィルターを作成する

サービスを暗黙的なインテントに関連付けるには、Android アプリがサービスの機能を識別するためにいくつかのメタデータを提供する必要があります。 このメタデータは、_インテントフィルター_によって提供されます。 インテントフィルターには、サービスを開始する目的で存在する必要がある、アクションやデータの種類など、いくつかの情報が含まれています。 Xamarin Android では、インテントフィルターは、サービスを[`IntentFilterAttribute`](xref:Android.App.IntentFilterAttribute)で修飾することによって、 **Androidmanifest .xml**に登録されます。 たとえば、次のコードでは、`com.xamarin.DemoService`のアクションが関連付けられたインテントフィルターを追加しています。

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

この結果、次の例のように、アプリケーションでパッケージ化されたエントリ &ndash; **Androidmanifest .xml**ファイルにエントリが追加されます。

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Xamarin. Android サービスの基本については、サービスのさまざまなサブタイプについて詳しく説明します。

## <a name="related-links"></a>関連リンク

- [Android.App.Service](xref:Android.App.Service)
- [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute)
- [Android... インテント](xref:Android.Content.Intent)
- [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute)
