---
title: 既知の問題 & 回避策
description: このドキュメントでは、Xamarin Workbooks に関する既知の問題と回避策について説明します。 CultureInfo の問題、JSON の問題などについて説明します。
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: c7b9f93c2d6339ba1fd26b27742ecfc0f438c5de
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018166"
---
# <a name="known-issues--workarounds"></a>既知の問題 & 回避策

## <a name="persistence-of-cultureinfo-across-cells"></a>セル間の CultureInfo の永続化

[Mono の `AppContext.SetSwitch`実装のバグ][appcontext-bug]により、`System.Threading.CurrentThread.CurrentCulture` または `System.Globalization.CultureInfo.CurrentCulture` の設定は、mono ベースのブックターゲット (Mac、iOS、および Android) のブックセル間で保持されません。

### <a name="workarounds"></a>問題回避

- アプリケーションドメインローカル `DefaultThreadCurrentCulture`を設定します。

```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

- または、ブック1.2.1 以降に更新します。これにより、割り当てが `System.Threading.CurrentThread.CurrentCulture` に書き直され、目的の動作 (Mono のバグに対処) を実現する `System.Globalization.CultureInfo.CurrentCulture` になります。

## <a name="unable-to-use-newtonsoftjson"></a>Newtonsoft. Json を使用できません

### <a name="workaround"></a>回避策

- ブック1.2.1 に更新します。これにより、Newtonsoft がインストールされます。
  現在 alpha チャネルにあるブック1.3 では、バージョン10以降がサポートされています。

### <a name="details"></a>説明

Newtonsoft. Json 10 がリリースされました。これにより、`dynamic`をサポートするために提供されるバージョンのブックと競合する、CSharp への依存関係があります。 これについては、ブック1.3 プレビューリリースで取り上げられていますが、ここでは、9.0.1 をバージョンに明示的に固定することでこれを回避しました。

Newtonsoft. Json 10 以降に明示的に依存している NuGet パッケージは、現在は alpha チャネルにあるブック1.3 でのみサポートされています。

## <a name="code-tooltips-are-blank"></a>コードヒントが空白である

Safari/WebKit の[モナコエディターに][monaco-bug]は、Mac ブックアプリで使用される、テキストなしでコードヒントが表示されるというバグがあります。

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>回避策

- ツールヒントを表示した後にクリックすると、テキストが強制的にレンダリングされます。

- またはブック1.2.1 以降に更新します。

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>SkiaSharp レンダラーがブック1.3 にありません

ブック1.3 から、ブック0.99.0 に付属していた SkiaSharp レンダラーが削除されました。これは、microsoft の[SDK](~/tools/workbooks/sdk/index.md)を使用してレンダラー自体を提供する SkiaSharp を優先しています。

### <a name="workaround"></a>回避策

- SkiaSharp を NuGet の最新バージョンに更新します。 執筆時点では、これは1.57.1 です。

## <a name="related-links"></a>関連リンク

- [バグの報告](~/tools/workbooks/install.md#reporting-bugs)
