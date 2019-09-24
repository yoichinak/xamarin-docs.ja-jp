---
title: Xamarin の複数の Windows for iPad。 iOS
description: IPad アプリケーションに複数のウィンドウサポートを追加します。
ms.prod: xamarin
ms.assetid: 524b6f2e-dbdf-11e9-8a34-2a2ae2dbcce4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/20/2019
ms.openlocfilehash: b3f96f342679d8be6d2f8fbc0ad5962ca675d2a5
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213805"
---
# <a name="multiple-windows-for-ipad"></a>IPad 用の複数の windows

iOS 13 では、iPad で同じアプリのサイドバイサイドウィンドウがサポートされるようになりました。 これにより、新しいエクスペリエンスと、ウィンドウ間のドラッグアンドドロップ操作が可能になります。 このドキュメントでは、この機能をサポートするようにアプリケーションをセットアップする方法について説明し、これらの新機能について説明します。 

## <a name="project-configuration"></a>プロジェクトの構成

複数のウィンドウに対してプロジェクトを構成する`info.plist`には`NSUserActivityTypes` 、を修正して、アプリがこの種類のアクティビティを処理`UIApplicationSupportsMultipleScenes`することを宣言`UISceneConfigurations`し`UIApplicationSceneManifest` 、複数のウィンドウで有効にして、ストーリーボードを持つシーン。

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>com.xamarin.Gallery.openDetail</string>
</array>
<key>UIApplicationSceneManifest</key>
<dict>
    <key>UIApplicationSupportsMultipleScenes</key>
    <true/>
    <key>UISceneConfigurations</key>
    <dict>
        <key>UIWindowSceneSessionRoleApplication</key>
        <array>
            <dict>
                <key>UISceneConfigurationName</key>
                <string>Default Configuration</string>
                <key>UISceneDelegateClassName</key>
                <string>SceneDelegate</string>
                <key>UISceneStoryboardFile</key>
                <string>Main</string>
            </dict>
        </array>
    </dict>
</dict>
```

## <a name="opening-multiple-windows"></a>複数のウィンドウを開く

IPad でアプリを開いて実行すると、iOS が既定として処理するアプリの複数のウィンドウを開くためのいくつかの方法があります。

- アプリが**公開**されているときに、dock のアプリアイコンをタップして、アプリが開いている間にこのモードに移行します。
- **スライドオーバー** -実行中のアプリの上部にあるドッキングからアプリのアイコンをドラッグして、フローティングウィンドウを表示します。
- **分割画面**-ドックから iPad 画面の端にアプリアイコンをドラッグして、新しいサイドバイサイドウィンドウを作成します。

複数のウィンドウモードに入る他の対話操作は、アプリケーション内から使用できます。

**アプリからドラッグ**-アプリ内でドラッグ操作を使用して、前`NSUserActivity`の例でアプリアイコンをドラッグするのと同じように新しいを開始します。

[ドラッグアンドドロップ操作][0]を使用する場合は、を作成`NSUserActivity`し、渡されるデータを新しいウィンドウに関連付けて、iOS に開くように依頼します。

```csharp
public UIDragItem [] GetItemsForBeginningDragSession (UICollectionView collectionView, IUIDragSession session, NSIndexPath indexPath)
{
    var selectedPhoto = GetPhoto (indexPath);

    var userActivity = selectedPhoto.OpenDetailUserActivity ();
    var itemProvider = new NSItemProvider (UIImage.FromFile (selectedPhoto.Name));
    itemProvider.RegisterObject (userActivity, NSItemProviderRepresentationVisibility.All);

    var dragItem = new UIDragItem (itemProvider) {
        LocalObject = selectedPhoto
    };

    return new [] { dragItem };
}
```

上記`selectedPhoto`のコードでは、モデルオブジェクトに、呼び出さ`OpenDetailUserActivity()`れたを`NSUserActivity`返すメソッドがあります。 ドラッグジェスチャが完了`UIDragItem`すると、ではを使用してが`userActivity`表示`NSItemProvider`されます。

**明示的な操作**-ボタンまたはリンクのユーザージェスチャでは、新しいウィンドウを開くことができます。

からを呼び出す`UISceneSession` `RequestSceneSessionActivation`ことによって、新しいを開始できます。 `UIApplication` 既存のシーンが既に存在する場合は、それを使用する必要があります。 既定では、新しいシーンが作成されます。

```csharp
pubic void ItemSelected(UICollectionView collectionView, NSIndexPath indexPath)
{
    var userActivity = selectedPhoto.OpenDetailUserActivity ();

    UIApplication.SharedApplication.RequestSceneSessionActivation(
        sceneSession: null,
        userActivity: userActivity,
        options: null,
        errorHandler: null
    );
}
```

この例では、 `userActivity`は、明示的なユーザーアクション`RequestSceneSessionActivation` (この場合は`UICollectionView`の`ItemSelected`ハンドラー) に基づいてアプリケーションの新しいウィンドウを開くために、メソッドに渡される唯一の値です。

## <a name="related-links"></a>関連リンク

- [Xamarin. iOS にドラッグアンドドロップ][0]

[0]: ~/ios/platform/introduction-to-ios11/drag-and-drop.md
