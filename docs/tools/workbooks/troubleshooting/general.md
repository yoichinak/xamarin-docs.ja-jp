---
title: 既知の問題 & 回避策
description: このドキュメントでは、Xamarin Workbooks に関する既知の問題と回避策について説明します。 CultureInfo の問題、JSON の問題などについて説明します。
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: aa6bbf9336acbff8558744220b9f4b8634f46421
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997229"
---
# <a name="known-issues--workarounds"></a>既知の問題 & 回避策

## <a name="persistence-of-cultureinfo-across-cells"></a>セル間の CultureInfo の永続化

`System.Threading.CurrentThread.CurrentCulture` `System.Globalization.CultureInfo.CurrentCulture` [Mono の `AppContext.SetSwitch` 実装でのバグ][appcontext-bug]のため、またはの設定は、mono ベースのブックターゲット (Mac、iOS、および Android) のブックセル間で保持されません。

### <a name="workarounds"></a>対処方法

- アプリケーションをドメインローカルに設定し `DefaultThreadCurrentCulture` ます。

```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

- または、ブック1.2.1 以降に更新します。これにより、割り当てがに書き直され、 `System.Threading.CurrentThread.CurrentCulture` `System.Globalization.CultureInfo.CurrentCulture` 目的の動作 (Mono のバグの回避) が提供されます。

## <a name="unable-to-use-newtonsoftjson"></a>Newtonsoft.Jsを使用できません

### <a name="workaround"></a>回避策

- ブック1.2.1 に更新します。これにより、9.0.1 に Newtonsoft.Jsがインストールされます。
  現在 alpha チャネルにあるブック1.3 では、バージョン10以降がサポートされています。

### <a name="details"></a>詳細

によってサポートされるバージョンのブックと競合する、10の Newtonsoft.Jsがリリースされました `dynamic` 。 これについては、ブック1.3 のプレビューリリースで取り上げられていますが、ここでは、具体的に Newtonsoft.Jsをバージョン9.0.1 に固定することで、この問題を回避しました。

10以上の Newtonsoft.Jsに明示的に依存している NuGet パッケージは、現在は alpha チャネルにあるブック1.3 でのみサポートされています。

## <a name="code-tooltips-are-blank"></a>コードヒントが空白である

Safari/WebKit の[モナコエディターに][monaco-bug]は、Mac ブックアプリで使用される、テキストなしでコードヒントが表示されるというバグがあります。

![モナコツールヒント表示 (テキストなし)](general-images/monaco-signature-help-bug.png)

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
