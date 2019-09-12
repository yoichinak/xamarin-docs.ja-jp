---
title: カスタム リンカーの構成
description: このドキュメントでは、必要なコードがリンクされているアプリケーションから削除されないことを明示的に確認し、リンカーを構成するために使用できる XML ファイルについて説明します。
ms.prod: xamarin
ms.assetid: F8A99E3F-2197-4399-AC81-F1DBAB5729C9
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: 230fe0f168b5718c2bc91cff6dbdc078b0e6834d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765927"
---
# <a name="custom-linker-configuration"></a>カスタム リンカーの構成

既定のオプションのセットでは不十分な場合、リンカーからの必要なものについて記述した XML ファイルを使用してリンク プロセスを促進することができます。

型、メソッド、フィールドがアプリケーションから削除されないように、追加の定義をリンカーに指定できます。 独自のコードの場合、推奨される方法は `[Preserve]` カスタム属性を使用することです。詳細は、「[iOS でのリンク](~/ios/deploy-test/linker.md)」ガイドと「[Android でのリンク](~/android/deploy-test/linker.md)」ガイドにあります。
ただし、SDK またはプロダクト アセンブリからの定義をいくつか必要とする場合、(必要な要素がリンカーによって削除されないコードの追加よりも) XML ファイルの利用が最適な解決策となることがあります。

これを行うには、最上位要素 `<linker>` で XML ファイルを定義します。この要素には*アセンブリ* ノードが含まれ、アセンブリ ノードには*型*ノードが含まれ、型ノードには*メソッド* ノードと*フィールド* ノードが含まれます。

このリンカー記述ファイルが与えられたら、それをプロジェクトに追加し、次の操作を行います。

- **Android の場合**: **[ビルド アクション]** を **LinkDescription** に設定します
- **iOS の場合**: **[ビルド アクション]** を **LinkDescription** に設定します

次の例では、XML ファイルがどのようなものであるかを確認できます。

```xml
<linker>
        <assembly fullname="mscorlib">
                <type fullname="System.Environment">
                        <field name="mono_corlib_version" />
                        <method name="get_StackTrace" />
                </type>
        </assembly>
        <assembly fullname="My.Own.Assembly">
                <type fullname="Foo" preserve="fields">
                        <method name=".ctor" />
                </type>
                <type fullname="Bar">
                        <method signature="System.Void .ctor(System.String)" />
                        <field signature="System.String _blah" />
                </type>
                <namespace fullname="My.Own.Namespace" />
                <type fullname="My.Other*" />
        </assembly>
</linker>
```

上の例では、リンカーが指示を読み取り、`mscorlib.dll` (Mono for Android で出荷) アセンブリと `My.Own.Assembly` (ユーザー コード) アセンブリに適用します。

最初のセクションは `mscorlib.dll` の定義ですが、これによって `System.Environment` 型はフィールド `mono_corlib_version` とメソッド `get_StackTrace` を保持します。
リンカーは IL で動作し、C# プロパティを認識しないため、getter または setter メソッドの名前を利用する必要があります。

2 番目のセクションは `My.Own.Assembly.dll` の定義ですが、これによって型 `Foo` はすべてのフィールド (すなわち、`preserve="fields"` 属性) とすべてのコンストラクター (すなわち、IL のすべての `.ctor` メソッド) を保持します。 型 `Bar` は 1 つのコンストラクター (文字列パラメーターを 1 つ受け取る) のためと特定の文字列フィールド `_blah` のために特定の署名 (名前ではなく) を保持します。
名前空間 `My.Own.Namespace` はそれに含まれているすべての型を保持します。
最後になりますが、完全名 (名前空間を含む) がワイルドカード パターン "My.Other\*" に一致する型はすべて、そのすべてのフィールドとメソッドを保持します。 ワイルドカード文字 `*` は "type fullname" パターン内に複数回含めることができます。

## <a name="related-links"></a>関連リンク

- [iOS でのリンク](~/ios/deploy-test/linker.md)
- [Android でのリンク](~/android/deploy-test/linker.md)
