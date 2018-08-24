---
title: 既知の問題と回避策
description: このドキュメントでは、Xamarin Workbooks の既知の問題と回避策について説明します。 これは、CultureInfo 問題や、JSON の問題について説明します。
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: d362698d2844ae6d96bba4929d509f5373742578
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350996"
---
# <a name="known-issues--workarounds"></a>既知の問題と回避策

## <a name="persistence-of-cultureinfo-across-cells"></a>すべてのセルの CultureInfo の永続化

設定`System.Threading.CurrentThread.CurrentCulture`または`System.Globalization.CultureInfo.CurrentCulture`はすべてのブックの Mono に基づくターゲット (Mac、iOS、および Android) のブックのセルは保持されないために、 [Mono のバグ`AppContext.SetSwitch` ] [ appcontext-bug]実装.

### <a name="workarounds"></a>問題回避

* アプリケーション ドメイン ローカル設定`DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* または、1.2.1 のブックを更新またはそれ以降への割り当てをリライトするされます`System.Threading.CurrentThread.CurrentCulture`と`System.Globalization.CultureInfo.CurrentCulture`(Mono のバグの回避) 目的の動作を提供します。

## <a name="unable-to-use-newtonsoftjson"></a>Newtonsoft.Json を使用することができません。

### <a name="workaround"></a>回避策

* Newtonsoft.Json 9.0.1 をインストールする 1.2.1、ブックを更新します。
  ブック 1.3、アルファ チャネルでは、現在では、10 以降のバージョンをサポートしています。

### <a name="details"></a>説明

リリースをサポートするバージョンのブックと競合するが同梱されて Microsoft.CSharp への依存性を高く Newtonsoft.Json 10`dynamic`します。 ブック 1.3 のプレビュー リリースでこの問題に対処されますが、ここで取り組んでいますこの問題を回避してピン留め Newtonsoft.Json バージョン 9.0.1 を具体的には。

NuGet パッケージ Newtonsoft.Json 10 によって明示的にまたはそれ以降は、現在ブックの 1.3 ではアルファ チャネルでサポートのみされます。

## <a name="code-tooltips-are-blank"></a>コードのツールヒントが空

[Monaco エディターのバグ][ monaco-bug]コード ツールヒントの表示テキストなしで結果を Safari/WebKit の Mac の Workbooks アプリで使用されています。

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>回避策

* ツールヒントが表示されたらをクリックすると、表示するテキストが実行されます。

* 1.2.1 ブックへの更新またはまたはそれ以降

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>SkiaSharp のレンダラーがブック 1.3 にありません。

ブック 1.3 以降、削除しましたに付属していた SkiaSharp レンダラーで SkiaSharp を使用して、自体のレンダラーを提供することを優先してブック 0.99.0、当社[SDK](~/tools/workbooks/sdk/index.md)します。

### <a name="workaround"></a>回避策

* SkiaSharp を NuGet の最新バージョンに更新します。 この記事の執筆時に、これは、1.57.1 です。

## <a name="related-links"></a>関連リンク

- [バグの報告](~/tools/workbooks/install.md#reporting-bugs)
