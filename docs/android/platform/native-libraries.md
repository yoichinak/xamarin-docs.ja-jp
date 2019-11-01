---
title: ネイティブ ライブラリの使用
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 7ef9e0415d7d1e5fe75be70e0ccf6e06a5eaf332
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027063"
---
# <a name="using-native-libraries"></a>ネイティブ ライブラリの使用

Xamarin Android では、標準の PInvoke 機構によるネイティブライブラリの使用がサポートされています。 また、OS に含まれていない追加のネイティブライブラリを apk にバンドルすることもできます。

Xamarin Android アプリケーションを使用してネイティブライブラリを配置するには、ライブラリバイナリをプロジェクトに追加し、**ビルドアクション**を**androidのライブラリ**に設定します。

Xamarin Android ライブラリプロジェクトを含むネイティブライブラリを配置するには、ライブラリバイナリをプロジェクトに追加し、**ビルドアクション**を**EmbeddedNativeLibrary**に設定します。

Android では複数のアプリケーションバイナリインターフェイス (ABIs) がサポートされているため、Xamarin では、ネイティブライブラリがどの ABI に対して構築されているかを把握している必要があります。
これを行うには 2 つの方法があります。

1. パス "スニッフィング"
1. プロジェクトファイル内の `AndroidNativeLibrary/Abi` 要素を使用する

パス スニッフィングを使用すると、ネイティブ ライブラリの親ディレクトリ名が、ライブラリがターゲットとする ABI を指定するために使用されます。 このため、プロジェクトに `lib/armeabi/libfoo.so` を追加すると、ABI は `armeabi`として "スニッフィングさ" になります。

または、プロジェクトファイルを編集して、使用する ABI を明示的に指定することもできます。

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

ネイティブライブラリの使用方法の詳細については、「[ネイティブライブラリとの相互運用](https://www.mono-project.com/docs/advanced/pinvoke/)」を参照してください。

## <a name="debugging-native-code-with-visual-studio"></a>Visual Studio を使用したネイティブコードのデバッグ

*Visual studio 2019*または*visual studio 2017*を使用している場合は、前述のようにプロジェクトファイルを変更する必要はありません。
C++ C++ **ダイナミック共有ライブラリ (android)** プロジェクトにプロジェクト参照を追加することによって、Xamarin. android ソリューション内でビルドおよびデバッグを行うことができます。

プロジェクトのネイティブC++コードをデバッグするには、次の手順を実行します。

1. プロジェクトの **[プロパティ]** をダブルクリックし、 **[Android のオプション]** ページを選択します。
2. 下にスクロールして、**デバッグオプションを選択**します。
3. **デバッガー**のドロップダウンメニューで、 **C++** (既定の **.net (Xamarin)** ではなく) を選択します。

Visual Studio C++の開発者は、 [SanAngeles_NativeDebug](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk)サンプルを参照C++して、Xamarin を使用して Visual studio 2019 または visual studio 2017 からデバッグを試すことができます。詳細については、[ブログの投稿](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/)を参照してください。

## <a name="related-links"></a>関連リンク

- [SanAngeles_NativeDebug (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk)
- [Xamarin Android ネイティブアプリケーションの開発](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
