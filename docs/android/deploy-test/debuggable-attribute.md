---
title: デバッグ可能な属性
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 12/13/2019
ms.openlocfilehash: 6e54dba0c832596818c5163dc4e14ad1e659e040
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75487972"
---
# <a name="debuggable-attribute"></a>デバッグ可能な属性

デバッグできるようにするために、Android では Java Debug Wire Protocol (JDWP) がサポートされます。 これは、JVM と通信する ADB などのツールを許可するテクノロジです。 JDWP は開発時には重要ですが、アプリケーションを公開する前に無効にする必要があります。

JDWP は、Android アプリケーションの `android:debuggable` 属性の値によって構成できます。 Xamarin.Android でこの属性を設定するには、次の 3 つの方法の "_いずれか_" を選択します。

## <a name="androidmanifestxml"></a>AndroidManifest.xml

`AndroidManifext.xml` ファイルを作成するか開き、そこで `android:debuggable` 属性を設定します。 デバッグを有効にしたリリース ビルドを提供しないように、特に注意してください。

## <a name="add-an-application-class-attribute"></a>Application クラス属性を追加する

Xamarin.Android アプリに `[Application]` 属性を持つクラスがある場合は、その属性を `[Application(Debuggable = true)]` に更新します。 無効にするには、これを `false` に設定します。

## <a name="add-an-assembly-attribute"></a>アセンブリ属性を追加する

Xamarin.Android アプリに `[Application]` クラス属性がまだない場合は、C# ファイルにアセンブリ レベル属性 `[assembly: Application(Debuggable=true)]` を追加します。 無効にするには、これを `false` に設定します。

## <a name="summary"></a>まとめ

`AndroidManifest.xml` と `ApplicationAttribute` の両方が存在する場合は、`AndroidManifest.xml` の内容が、`ApplicationAttribute` で指定されたものより優先されます。

クラス属性とアセンブリ属性の "_両方_" を追加すると、コンパイラ エラーが発生します。

```error
"Error The "GenerateJavaStubs" task failed unexpectedly.
System.InvalidOperationException: Application cannot have both a type with an [Application] attribute and an [assembly:Application] attribute."
```

既定では (`AndroidManifest.xml` と `ApplicationAttribute` のいずれも存在しない場合)、`android:debuggable` 属性の値は、デバッグ シンボルが生成されているかどうかによって異なります。 デバッグ シンボルが存在する場合は、Xamarin.Android によって `android:debuggable` 属性が自動的に `true` に設定されます。

> [!WARNING]
> `android:debuggable` 属性の値は、必ずしもビルド構成によって異なるわけではありません。 リリース ビルドでは、`android:debuggable` 属性を true に設定することができます。 属性を使用してこの値を設定する場合は、コンパイラ ディレクティブで属性をラップすることを選択できます。
> 
> ```csharp
> #if DEBUG
> [Application(Debuggable = true)]
> #else
> [Application(Debuggable = false)]
> #endif
> ```

## <a name="related-links"></a>関連リンク

- [Android マーケットにおけるデバッグ可能なアプリ](https://labs.f-secure.com/archive/debuggable-apps-in-android-market/)
