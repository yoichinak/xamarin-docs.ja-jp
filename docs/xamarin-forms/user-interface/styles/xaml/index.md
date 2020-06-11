---
title: 「 Xamarin.Forms Xaml スタイルを使用したアプリのスタイルを設定する」 description: "このガイドでは、xaml スタイルを使用してアプリケーションの外観をカスタマイズする方法について説明 Xamarin.Forms します。"
ms. 製品: xamarin ms. assetid: 344 A34AA-B19A-4765-875D9A6B5EA8: xamarin-forms author: davidbritch BC8A: dabritch ms. date: 01/30/2019 no loc: [ Xamarin.Forms ,] を指定します。 Xamarin.Essentials
---

# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>XAML スタイルを使用して Xamarin.Forms アプリのスタイルを設定する

## <a name="introduction"></a>[はじめに](introduction.md)

Xamarin.Forms多くの場合、アプリケーションには同じ外観を持つ複数のコントロールが含まれています。 個々のコントロールの外観を設定すると、繰り返しが発生し、エラーが発生しやすくなります。 代わりに、コントロールの種類で使用可能なプロパティをグループ化および設定することによって、コントロールの外観をカスタマイズするスタイルを作成できます。

## <a name="explicit-styles"></a>[明示的なスタイル](explicit.md)

*明示的*なスタイルとは、プロパティを設定することによってコントロールに選択的に適用されるスタイルです [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 。

## <a name="implicit-styles"></a>[暗黙的なスタイル](implicit.md)

*暗黙的*なスタイルとは、 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 各コントロールがスタイルを参照する必要がなく、同じのすべてのコントロールで使用されるスタイルです。

## <a name="global-styles"></a>[グローバル スタイル](application.md)

スタイルは、アプリケーションのに追加することで、グローバルに使用でき [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。 これは、ページまたはコントロール間でスタイルが重複しないようにするために役立ちます。

## <a name="style-inheritance"></a>[スタイルの継承](inheritance.md)

スタイルを他のスタイルから継承して、重複を減らし、再利用できるようにすることができます。

## <a name="dynamic-styles"></a>[動的スタイル](dynamic.md)

スタイルは、プロパティの変更に応答せず、アプリケーションの継続中は変更されません。 ただし、アプリケーションは動的リソースを使用して、実行時に動的にスタイルの変更に応答できます。

## <a name="device-styles"></a>[デバイスのスタイル](device.md)

Xamarin.Formsには、クラスに、*デバイス*スタイルと呼ばれる6つの*動的*スタイルが含まれてい [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) ます。 6つのスタイルはすべてインスタンスにのみ適用でき [`Label`](xref:Xamarin.Forms.Label) ます。

## <a name="style-classes"></a>[スタイルクラス](style-class.md)

Xamarin.Formsスタイルクラスを使用すると、スタイルの継承を使用せずに、コントロールに複数のスタイルを適用できます。
