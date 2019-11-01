---
title: Xamarin での watchOS アイコンの使用
description: このドキュメントでは、watchOS アプリケーションに必要なさまざまなアイコンと、これらのアイコンを含むソリューションを設定する方法について説明します。
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/26/2018
ms.openlocfilehash: c8c5b8d0417fb7fd1069d2bf6fa5d9887d569453
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001569"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>Xamarin での watchOS アイコンの使用

Apple Watch ソリューションには、次の2つのアイコンセットが必要です。

- IPhone に表示される iOS アプリアイコン。
- [ウォッチ] メニューと通知画面に円で表示されるアイコンを Apple Watch します。 [アプリの監視] アイコンは、 [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS アプリにも表示されます。

## <a name="apple-watch-icons"></a>Apple Watch アイコン

| | | |
|-|-|-|
|iOS アプリアイコン|IPhone に表示され、親アプリを開始します|![iOS アプリアイコン](icons-images/icon-ios.png)|
|アプリのウォッチアイコン|Apple Watch ホーム画面に表示されます。|![watchOS アプリアイコン](icons-images/icon-home.png)|
||ウォッチ通知に表示されます|![watchOS 通知アイコン](icons-images/notification-icon.png)|
||[IOS Apple Watch アプリ](~/ios/watchos/app-fundamentals/settings.md)に表示されます|![iOS Watch アプリアイコン](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>ソリューションの構成

IOS アプリとウォッチアプリの両方で正しい名前とアイコンが表示されるようにするには、各プロジェクトに対して次の手順を実行します。

### <a name="ios-app"></a>iOS アプリ

Ios アプリのアイコンが正しく構成されていることを確認するには、 [Ios アプリケーションアイコンガイド](~/ios/app-fundamentals/images-icons/app-icons.md)を参照してください。

#### <a name="infoplist"></a>Info.plist

[Apple Watch 設定アプリ](~/ios/watchos/app-fundamentals/settings.md)で watch アプリの横に表示される文字列は、 **IOS アプリの情報 plist**で構成されます。

**Plist**に `CFBundleName` キーと値があることを確認します (注: これは `CFBundleDisplayName`とは異なり、両方を使用できます)。

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch アプリ

[親アプリ](~/ios/watchos/app-fundamentals/parent-app.md)のアイコンが構成されたら、アプリケーションアイコン asset catalog を watch アプリに追加する必要があります。

1. Watch App プロジェクトを右クリックし、[ファイル] を選択して **> 新しいファイルを追加 > ます...** アセットカタログをプロジェクトに追加するには、iOS > アセットカタログを > します。

    ![](icons-images/newasset.png "Add an asset catalog to the project")

2. **Appicons.appiconset/AppIcon**ファイルをダブルクリックします。

    ![](icons-images/xcassets-iconset-sml.png "The AppIcon contents")

3. このスクリーンショットに示されているように、すべての watchOS イメージを追加します。

    [![](icons-images/appicons-sml.png "Add all the watchOS images, as shown in this screenshot")](icons-images/appicons.png#lightbox)

    必要なサイズについては、 [Apple のアイコンのガイドライン](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/menu-icons/)を参照してください (寸法も画面に表示されます)。 これらのアイコンは、円形のレンダーに自動的にクリッピングされることに注意してください。

    アイコンの一覧は次のようになります。

    ![](icons-images/xcassets-complete-sml.png "The icon list in the Solution Explorer")

4. アセットカタログがアプリに含まれていることを確認するには、次のキーと値を**Watch アプリの情報に追加します。 plist**:

    ```xml
    <key>XSAppIconAssets</key>
    <string>Images.xcassets/AppIcon.appiconset</string>
    ```

アイコンが正しく構成されていることを確認するには、iPhone シミュレーターで[Apple Watch 設定アプリ](~/ios/watchos/app-fundamentals/settings.md)を確認するか、通知を生成して、通知画面にアイコンが表示されることを[確認します](~/ios/watchos/platform/notifications.md)。

> [!NOTE]
> アイコンにはアルファチャネルを含めることができません (アルファチャネルが存在する場合、アプリストアの送信中にアプリは拒否されます)。 [Mac OS X のプレビューアプリを使用して](~/ios/watchos/troubleshooting.md#noalpha)、アルファチャネルが存在するかどうかを確認し、削除することができます。

## <a name="related-links"></a>関連リンク

- [Apple の watchOS アイコン & イメージガイド](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/)
