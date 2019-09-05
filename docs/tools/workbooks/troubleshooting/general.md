---
title: 既知の問題 & 回避策
description: このドキュメントでは、Xamarin Workbooks に関する既知の問題と回避策について説明します。 CultureInfo の問題、JSON の問題などについて説明します。
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: b7b73e214af6a5a45426b4e2d2d7e01a436b379e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292779"
---
# <a name="known-issues--workarounds"></a>既知の問題 & 回避策

## <a name="persistence-of-cultureinfo-across-cells"></a>セル間の CultureInfo の永続化

`System.Threading.CurrentThread.CurrentCulture` [Monoの`AppContext.SetSwitch`実装でのバグ][appcontext-bug]のため、またはの設定は`System.Globalization.CultureInfo.CurrentCulture` 、mono ベースのブックターゲット (Mac、iOS、および Android) のブックセル間で保持されません。

### <a name="workarounds"></a>問題回避

- アプリケーションをドメインローカル`DefaultThreadCurrentCulture`に設定します。

```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

- または、ブック1.2.1 以降に更新します。これにより`System.Threading.CurrentThread.CurrentCulture` 、 `System.Globalization.CultureInfo.CurrentCulture`割り当てがに書き直され、目的の動作 (Mono のバグの回避) が提供されます。

## <a name="unable-to-use-newtonsoftjson"></a>Newtonsoft. Json を使用できません

### <a name="workaround"></a>回避策

- ブック1.2.1 に更新します。これにより、Newtonsoft がインストールされます。
  現在 alpha チャネルにあるブック1.3 では、バージョン10以降がサポートされています。

### <a name="details"></a>説明

Newtonsoft. Json 10 がリリースされました。これにより、サポート`dynamic`に提供されているバージョンのブックと競合する Microsoft CSharp への依存関係がバンプされました。 これについては、ブック1.3 プレビューリリースで取り上げられていますが、ここでは、9.0.1 をバージョンに明示的に固定することでこれを回避しました。

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
