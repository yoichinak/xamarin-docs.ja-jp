---
title: ネイティブ ライブラリを使用します。
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 0fa66f3a16047c18af19cb7257c778b498bc0c9b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774812"
---
# <a name="using-native-libraries"></a>ネイティブ ライブラリを使用します。

Xamarin.Android には、標準の PInvoke メカニズムを使用してネイティブ ライブラリの使用がサポートされています。 .Apk に OS の一部ではないその他のネイティブ ライブラリをバンドルすることもできます。

Xamarin.Android アプリケーションと共にネイティブ ライブラリを展開するバイナリのライブラリをプロジェクトに追加し、設定、**ビルド アクション**に**AndroidNativeLibrary**です。

Xamarin.Android ライブラリ プロジェクトでネイティブ ライブラリを展開するバイナリのライブラリをプロジェクトに追加し、設定、**ビルド アクション**に**EmbeddedNativeLibrary**です。

Android では、複数のアプリケーション バイナリ インターフェイス (ABIs) をサポートするため Xamarin.Android によってのネイティブ ライブラリをビルドする ABI 知る必要がありますに注意してください。
これを行うには 2 つの方法があります。

1.  パス「スニッフィング」
1.  使用して、`AndroidNativeLibrary/Abi`プロジェクト ファイル内の要素


パス スニッフィングを使用すると、ネイティブ ライブラリの親ディレクトリ名が、ライブラリがターゲットとする ABI を指定するために使用されます。 したがって、追加した場合`lib/armeabi/libfoo.so`をプロジェクトにし、ABI がある「見つけ出す」として`armeabi`です。

または、使用する ABI を明示的に指定するプロジェクト ファイルを編集することができます。

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

詳細については、ネイティブ ライブラリを使用して、次を参照してください。[ネイティブ ライブラリとの相互運用](http://www.mono-project.com/docs/advanced/pinvoke/)です。

## <a name="debugging-native-code-with-visual-studio-2015"></a>Visual Studio 2015 でのネイティブ コードのデバッグ

使用する場合*Visual Studio 2015*、(前述のように)、プロジェクト ファイルを変更する必要はありません。
ビルドして、C++ へのプロジェクト参照を追加するだけで、Xamarin.Android ソリューション内の C++ をデバッグ**ダイナミック共有ライブラリ (Android)** プロジェクト。

Visual Studio C 開発者できますを参照してください、 [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) Xamarin; を使用した Visual Studio 2015 から C++ のデバッグを実行するサンプルを参照してください、[ブログの投稿](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/)詳細についてはします。



## <a name="related-links"></a>関連リンク

- [SanAngeles_NativeDebug (サンプル)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
