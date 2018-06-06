---
title: 既知の問題と回避策
description: このドキュメントでは、Xamarin のブックの既知の問題と回避策について説明します。 これは、CultureInfo 問題や、JSON の問題について説明します。
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: topgenorth
ms.author: toopge
ms.openlocfilehash: b6dc3b119d3e85369a71638f2519b2ef0c85446c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794034"
---
# <a name="known-issues--workarounds"></a>既知の問題と回避策

## <a name="persistence-of-cultureinfo-across-cells"></a>すべてのセルの CultureInfo の永続化

設定`System.Threading.CurrentThread.CurrentCulture`または`System.Globalization.CultureInfo.CurrentCulture`はすべてのブックの Mono ベース ターゲット (Mac、iOS、および Android) のブックのセルは保持されないために、[モノラルのバグ`AppContext.SetSwitch` ] [ appcontext-bug]の実装.

### <a name="workarounds"></a>問題回避

* アプリケーション ドメイン ローカル設定`DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* または、更新するブック 1.2.1 またはそれ以降への割り当てを書き直してくださいは`System.Threading.CurrentThread.CurrentCulture`と`System.Globalization.CultureInfo.CurrentCulture`(モノのバグに対処) 目的の動作を提供します。

## <a name="unable-to-use-newtonsoftjson"></a>Newtonsoft.Json を使用できません。

### <a name="workaround"></a>回避策

* Newtonsoft.Json 9.0.1 をインストール、1.2.1、ブックを更新します。
  ブック 1.3、現在、アルファ チャネルには、バージョン 10 以降をサポートしています。

### <a name="details"></a>説明

Newtonsoft.Json 10 がリリースされたバージョンのブックと競合していますがサポートするために付属 Microsoft.CSharp への依存性を高く`dynamic`です。 ブック 1.3 プレビュー リリースでこの問題に対処がここではおがの作業ではこの問題を回避バージョン 9.0.1 を具体的には固定 Newtonsoft.Json でします。

Newtonsoft.Json 10 によって明示的にまたはそれ以降の NuGet パッケージは、現在ブック 1.3 ではアルファ チャネルでサポートのみされます。

## <a name="code-tooltips-are-blank"></a>コードのツールヒントが空白

[Monaco エディターでのバグ][ monaco-bug] Safari Mac ブック アプリで使用する WebKit の結果生成されたコードのツールヒントの表示テキストなしでします。

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>回避策

* クリックすると、ツールヒントに表示されると、表示するためにテキストが実行されます。

* 1.2.1 ブックへの更新またはまたはそれ以降

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>ブック 1.3. で SkiaSharp レンダラーが表示されません。

ブック 1.3 以降、取り除きました SkiaSharp レンダラーは同梱されているブック 0.99.0、SkiaSharp 自体、レンダラーを提供することを使用して、優先するために、 [SDK](~/tools/workbooks/sdk/index.md)です。

### <a name="workaround"></a>回避策

* SkiaSharp を NuGet の最新バージョンに更新します。 ドキュメントの執筆時に、これは、1.57.1 です。

## <a name="related-links"></a>関連リンク

- [バグの報告](~/tools/workbooks/install.md#reporting-bugs)
