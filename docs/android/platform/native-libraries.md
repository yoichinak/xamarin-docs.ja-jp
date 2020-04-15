---
title: ネイティブ ライブラリの使用
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: b7d69e99327aa3d3e3e1f5e5dbc61697d1fb9b71
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "75489168"
---
# <a name="using-native-libraries"></a>ネイティブ ライブラリの使用

Xamarin.Android では、標準の PInvoke メカニズムによるネイティブ ライブラリの使用がサポートされています。 また、OS に含まれていない追加のネイティブ ライブラリを .apk にバンドルすることもできます。

Xamarin.Android アプリケーションでネイティブ ライブラリをデプロイするには、ライブラリ バイナリをプロジェクトに追加し、その**ビルド アクション**を **AndroidNativeLibrary** に設定します。

Xamarin.Android ライブラリ プロジェクトでネイティブ ライブラリをデプロイするには、ライブラリ バイナリをプロジェクトに追加し、その**ビルド アクション**を **EmbeddedNativeLibrary** に設定します。

Android では、複数のアプリケーション バイナリ インターフェイス (ABI) をサポートしているため、ネイティブ ライブラリがどの ABI に対してビルドされているかが Xamarin.Android で認識される必要があります。
これを行うには 2 つの方法があります。

1. パス "スニッフィング"
1. プロジェクト ファイル内で `AndroidNativeLibrary/Abi` 要素を使用する

パス スニッフィングを使用すると、ネイティブ ライブラリの親ディレクトリ名が、ライブラリがターゲットとする ABI を指定するために使用されます。 したがって、プロジェクトに `lib/armeabi/libfoo.so` を追加すると、ABI は `armeabi` として "スニッフィング" されます。

または、プロジェクト ファイルを編集して、使用する ABI を明示的に指定することもできます。

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

ネイティブ ライブラリの使用に関する詳細については、[ネイティブ ライブラリとの相互運用性](https://www.mono-project.com/docs/advanced/pinvoke/)に関するページを参照してください。

## <a name="debugging-native-code-with-visual-studio"></a>Visual Studio を使用したネイティブ コードのデバッグ

*Visual Studio 2019* または *Visual Studio 2017* を使用している場合は、前述のように、プロジェクト ファイルを変更する必要はありません。
プロジェクト参照を C++ **ダイナミック共有ライブラリ (Android)** プロジェクトに追加することによって、Xamarin.Android ソリューション内の C++ をビルドおよびデバッグできます。

プロジェクト内のネイティブ C++ コードをデバッグするには、次の手順に従います。

1. プロジェクトの **[プロパティ]** をダブルクリックし、 **[Android オプション]** ページを選択します。
2. **[デバッグ オプション]** まで下にスクロールします。
3. **[デバッガー]** ドロップダウン メニューで、 **[C++]** (既定値の **[.NET (Xamarin)]** ではなく) を選択します。

Visual Studio C++ 開発者は、[SanAngeles_NativeDebug](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk) サンプルを参照して、Xamarin を使用して Visual Studio2019 または Visual Studio2017 から C++ をデバッグすることができます。詳細については、Microsoft の[ブログ投稿](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/)を参照してください。

## <a name="related-links"></a>関連リンク

- [SanAngeles_NativeDebug (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk)
- [Xamarin Android ネイティブ アプリケーションの開発](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
