---
title: ネイティブ ライブラリの使用
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 1195685db9e85e7fba006272ef300e22d47d1fa6
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666373"
---
# <a name="using-native-libraries"></a>ネイティブ ライブラリの使用

Xamarin.Android では、標準の PInvoke メカニズムを使用してネイティブ ライブラリの使用をサポートします。 .Apk に OS の一部ではないその他のネイティブ ライブラリをバンドルすることもできます。

Xamarin.Android アプリケーションでネイティブ ライブラリを展開するには、バイナリのライブラリをプロジェクトに追加し、設定、**ビルド アクション**に**AndroidNativeLibrary**します。

Xamarin.Android ライブラリ プロジェクトでネイティブ ライブラリを展開するには、バイナリのライブラリをプロジェクトに追加し、設定、**ビルド アクション**に**EmbeddedNativeLibrary**します。

Android では、複数のアプリケーション バイナリ インターフェイス (Abi) をサポートするため Xamarin.Android によって、ネイティブ ライブラリが用にビルドされたどの ABI 知る必要がありますに注意してください。
これを行うには 2 つの方法があります。

1.  パス「スニッフィング」
1.  使用して、`AndroidNativeLibrary/Abi`プロジェクト ファイル内の要素


パス スニッフィングを使用すると、ネイティブ ライブラリの親ディレクトリ名が、ライブラリがターゲットとする ABI を指定するために使用されます。 そのため、追加する場合`lib/armeabi/libfoo.so`をプロジェクトにし、ABI は「スニッフィング」されますとして`armeabi`します。

または、明示的に使用する ABI を指定するプロジェクト ファイルを編集することができます。

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

ネイティブ ライブラリの使用に関する詳細については、次を参照してください。[ネイティブ ライブラリとの相互運用](https://www.mono-project.com/docs/advanced/pinvoke/)します。

## <a name="debugging-native-code-with-visual-studio-2017"></a>Visual Studio 2017 でのネイティブ コードのデバッグ

使用している場合*Visual Studio 2017*以上で、前述のように、プロジェクト ファイルを変更する必要はありません。
ビルドして、C++ への参照をプロジェクトに追加することで、Xamarin.Android ソリューション内で C++ をデバッグ**ダイナミック共有ライブラリ (Android)** プロジェクト。 

プロジェクトでネイティブの C++ コードをデバッグするには、次の手順を実行します。

1. プロジェクトをダブルクリックして**プロパティ**を選択し、 **Android オプション**ページ。
2. 下へスクロールして**デバッグ オプション**します。
3. **デバッガー**ドロップダウン メニューで、 **C++** (既定ではなく **.Net (Xamarin)**)。

Visual Studio の C++ 開発者を参照してください、 [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)サンプルを Visual Studio 2017 と Xamarin; から C のデバッグを試すしを参照してください、[ブログの投稿](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/)詳細についてはします。



## <a name="related-links"></a>関連リンク

- [SanAngeles_NativeDebug (サンプル)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
- [Xamarin Android のネイティブ アプリケーションの開発](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
