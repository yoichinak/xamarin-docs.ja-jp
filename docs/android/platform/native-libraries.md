---
title: ネイティブ ライブラリの使用
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: fa0a3a75a4cc2cfd04b607f17206faa822af0474
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523646"
---
# <a name="using-native-libraries"></a>ネイティブ ライブラリの使用

Xamarin Android では、標準の PInvoke 機構によるネイティブライブラリの使用がサポートされています。 .apk に OS の一部ではないその他のネイティブ ライブラリをバンドルすることもできます。

Xamarin Android アプリケーションを使用してネイティブライブラリを配置するには、ライブラリバイナリをプロジェクトに追加し、**ビルドアクション**を**androidのライブラリ**に設定します。

Xamarin Android ライブラリプロジェクトを含むネイティブライブラリを配置するには、ライブラリバイナリをプロジェクトに追加し、**ビルドアクション**を**EmbeddedNativeLibrary**に設定します。

Android では複数のアプリケーションバイナリインターフェイス (ABIs) がサポートされているため、Xamarin では、ネイティブライブラリがどの ABI に対して構築されているかを把握している必要があります。
これを行うには 2 つの方法があります。

1. パス "スニッフィング"
1. プロジェクトファイル内`AndroidNativeLibrary/Abi`の要素を使用する


パス スニッフィングを使用すると、ネイティブ ライブラリの親ディレクトリ名が、ライブラリがターゲットとする ABI を指定するために使用されます。 したがって、をプロジェクト`lib/armeabi/libfoo.so`に追加すると、ABI はとして`armeabi`"スニッフィングさ" になります。

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

1. [プロジェクトの**プロパティ**] をダブルクリックし、[ **Android のオプション**] ページを選択します。
2. 下にスクロールして、**デバッグオプションを選択**します。
3. **デバッガー**のドロップダウンメニューで、 **C++** (既定の **.net (Xamarin)** ではなく) を選択します。

Visual Studio C++の開発者は、 [SanAngeles_NativeDebug](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk)サンプルを参照C++して、Xamarin を使用して Visual studio 2019 または visual studio 2017 からデバッグを試すことができます。詳細については、[ブログの投稿](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/)を参照してください。



## <a name="related-links"></a>関連リンク

- [SanAngeles_NativeDebug (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk)
- [Xamarin Android ネイティブアプリケーションの開発](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
