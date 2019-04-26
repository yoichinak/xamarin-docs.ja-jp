---
title: ネイティブ ライブラリの使用
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 1b0771a0ccc2597ebd800468b82044e4020d9d94
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153313"
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

## <a name="debugging-native-code-with-visual-studio"></a>Visual Studio を使用したネイティブ コードのデバッグ

使用している場合*Visual Studio 2019*または*Visual Studio 2017*、前述のように、プロジェクト ファイルを変更する必要はありません。
ビルドして、C++ への参照をプロジェクトに追加することで、Xamarin.Android ソリューション内で C++ をデバッグ**ダイナミック共有ライブラリ (Android)** プロジェクト。

プロジェクトでネイティブの C++ コードをデバッグするには、次の手順を実行します。

1. プロジェクトをダブルクリックして**プロパティ**を選択し、 **Android オプション**ページ。
2. 下へスクロールして**デバッグ オプション**します。
3. **デバッガー**ドロップダウン メニューで、 **C++** (既定ではなく **.Net (Xamarin)**)。

Visual StudioC++開発者を参照してください、 [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)デバッグのサンプルC++から Visual Studio 2019 または Visual Studio 2017 と Xamarin; を参照してください、[ブログの投稿](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/)詳細情報。



## <a name="related-links"></a>関連リンク

- [SanAngeles_NativeDebug (サンプル)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
- [Xamarin Android のネイティブ アプリケーションの開発](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
