---
title: "アイコンの使用"
ms.topic: article
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 2961eb4726b9f313d01f8bc075e5ca362d708e92
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-icons"></a>アイコンの使用

Apple Watch ソリューションには、2 つのアイコンのセットが必要です。

* IPhone で表示される iOS アプリのアイコン。
* Apple Watch と通知の画面で、円で囲んだ [ウォッチ] メニューに表示されるアイコン。 Watch アプリのアイコンにも表示されます、 [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS アプリ。

## <a name="apple-watch-icons"></a>Apple Watch アイコン

| | | |
|-|-|-|
|iOS アプリのアイコン|IPhone で表示され、親アプリケーションを起動します|![](icons-images/icon-ios.png)|
|[ウォッチ アプリ] アイコン|Apple Watch のホーム画面に表示されます。|![](icons-images/icon-home.png)|
||ウォッチ通知に表示されます。|![](icons-images/notification-icon.png)|
||表示されます、 [iOS Apple Watch アプリ](~/ios/watchos/app-fundamentals/settings.md)|![](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>ソリューションを構成します。

IOS アプリと watch アプリは、アイコンと正しい名前を表示するためには、各プロジェクトに次の手順に従います。

### <a name="ios-app"></a>iOS アプリ

参照してください、 [iOS アプリケーションのアイコンのガイド](~/ios/app-fundamentals/images-icons/app-icons.md)に iOS アプリのアイコンが正しく構成されていることを確認します。

#### <a name="infoplist"></a>Info.plist

ウォッチのアプリケーションの横に表示される文字列、 [Apple Watch 設定アプリ](~/ios/watchos/app-fundamentals/settings.md)で構成された、 **iOS アプリの Info.plist**です。

いることを確認、 **Info.plist**が、`CFBundleName`キーと値 (注: これは、異なる、 `CFBundleDisplayName`、両方を持つことができます)。

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch App

1 回、[親アプリ](~/ios/watchos/app-fundamentals/parent-app.md)watch アプリにアプリケーション アイコン資産カタログを追加する必要がありますが、アイコンで構成されています。

1. Watch アプリ プロジェクトを右クリックし、選択**ファイル > 追加 > 新しい File… > iOS > アセット カタログ**資産カタログをプロジェクトに追加します。

 ![](icons-images/newasset.png "資産カタログをプロジェクトに追加します。")

2. ダブルクリックして、 **AppIcons.appiconset/Contents.json**ファイル

  ![](icons-images/xcassets-iconset-sml.png "AppIcons 内容")

3. このスクリーン ショットに示すように、すべての watchOS イメージを追加します。

  [![](icons-images/appicons-sml.png "このスクリーン ショットに示すように、すべての watchOS イメージを追加します。")](icons-images/appicons.png#lightbox)

  参照してください[Apple のアイコンのガイドライン](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)(サイズは画面には表示も) 必要なサイズにします。 円で表示するためにこれらのアイコンを自動的にクリップすることに注意してください。

  アイコン リストは、次のようになります。

  ![](icons-images/xcassets-complete-sml.png "ソリューション エクスプ ローラーでアイコンのリスト")

4. アプリで資産カタログが含まれていることを確認するには、次のキーを追加し、値を**Watch アプリの Info.plist**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcons.appiconset</string>
```

チェックして、アイコンが正しく構成されてことを確認することができます、 [Apple Watch 設定アプリ](~/ios/watchos/app-fundamentals/settings.md)の iPhone シミュレーター、または生成、[通知](~/ios/watchos/platform/notifications.md)通知に表示されるアイコンを確認します。画面。

> [!NOTE]
> アイコン (アプリは拒否アプリ ストアの送信中に、アルファ チャネルが存在する場合)、アルファ チャネルことはできません。 アルファ チャネルが存在し、それを削除するかどうかにチェックすることができます[プレビュー アプリを使用して、Mac OS X で](~/ios/watchos/troubleshooting.md#noalpha)です。


## <a name="related-links"></a>関連リンク

- [Apple のアイコンとイメージを説明します。](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)
