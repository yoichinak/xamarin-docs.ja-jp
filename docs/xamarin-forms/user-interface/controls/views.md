---
title: "Xamarin.Forms ビュー"
description: "Xamarin.Forms ビューは、クロスプラット フォーム モバイル ユーザー インターフェイスのビルド ブロックです。"
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: df8c8463b2556035c5369c70cb10dbc3dc6b6743
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms ビュー

_Xamarin.Forms ビューは、クロスプラット フォーム モバイル ユーザー インターフェイスのビルド ブロックです。_

<style>.tableimg { max-width: none !important;}</style>

## <a name="views"></a>ビュー

Xamarin.Forms は、word を使用*ビュー*にボタン、ラベルまたはウィジェットのコントロールとよく呼ばれる場合があります - テキスト入力ボックスなどのビジュアル オブジェクトを参照してください。

これらの UI 要素は、通常のサブクラス[ `View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)です。

<br clear="right" />

Xamarin.Forms をサポートします。

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>型</strong>
    </th>
    <th>
      <strong>説明</strong>
    </th>
    <th style="min-width:400px">
      <strong>スクリーン ショット</strong>
    </th>

  </thead>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a>
    </td>
    <td valign="top">
何かが継続的なことを示すために使用される visual コントロールです。 このコントロールは、問題が起こっているか、その進行状況に関する情報がないことをユーザーに視覚的な手掛かりを持ちます。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ActivityIndicatorDemoPage.cs"><img src="views-images/ActivityIndicator.png" title="ActivityIndicator 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>純色の四角形を描画するために使用します。 BoxView は初期プロトタイピングを実施する際に、画像またはカスタムの要素に役立ちます代替です。 BoxView が 40 x 40 の既定のサイズの要求です。 サイズが異なる場合は、割り当て、 <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/">VisualElement.WidthRequest</a>と<a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/">VisualElement.HeightRequest</a>です。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/BoxViewDemoPage.cs"><img src="views-images/BoxView.png" title="BoxView 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">ボタン</a>
    </td>
    <td align="center" valign="top">
ボタン<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>タッチ イベントに反応をします。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ButtonDemoPage.cs"><img src="views-images/Button.png" title="ボタンの例" class="tableimg">
    </a></td>
  </tr>
  <tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>日付選択ことができます。 DatePicker のビジュアル表現は、のいずれかによく似ています<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">エントリ</a>日付を選択するための特別なコントロールがキーボードの代わりに表示される点を除いて、 </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/DatePickerDemoPage.cs"><img src="views-images/DatePicker.png" title="DatePicker 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">エディター</a>
    </td>
    <td valign="top">
複数行のテキストを編集できるコントロールです。 1 つの行エントリに、次を参照してください。<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">エントリ</a>です。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EditorDemoPage.cs"><img src="views-images/Editor.png" title="エディターの使用例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a>
    </td>
    <td valign="top">
1 行のテキストを編集できるコントロールです。 エントリは、1 つの行テキスト入力です。 不連続小さなユーザー名とパスワードなどの情報を収集するためには適しています。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="views-images/Entry.png" title="エントリの例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">イメージ</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>イメージを保持します。
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">イメージ API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/images.md">画像の操作</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageDemoPage.cs">デモ ソース</a>
    </td>
    <td>
    <img src="views-images/Image.png" title="イメージの例" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">ラベル</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>読み取り唯一の形式のテキストを表示する表示します。 ラベルを使用して、複数の行のテキスト ブロックと同様に 1 行のテキスト要素を表示できます。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/LabelDemoPage.cs"><img src="views-images/Label.png" title="ラベルの例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a>
    </td>
    <td valign="top">
<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/">ItemView</a>垂直方向の一覧としてデータのコレクションを表示します。
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/listview/index.md">ListView のドキュメント</a>
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ListViewDemoPage.cs"><img src="views-images/ListView.png" title="ListView の例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/">OpenGLView</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a> OpenGL のコンテンツを表示します。
    <ul>
      <li>IOS および Android のプロジェクト (Windows Phone はサポートされません) に対してのみ機能します。
      <li>参照が必要です、 <b>OpenTK 1.0</b> iOS および Android のプロジェクト内のアセンブリ。</li>
      <li>共有プロジェクトで使用するに最適な場合は、PCL に使用し、DependencyService も必要になります。</li>
    </ul>
    </td>
    <td>
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/"><img src="views-images/OpenGL.png" title="OpenGlView 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">ピッカー</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>コントロール リストの要素を取得します。 ピッカーのビジュアル表現がに似ていますが、<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">エントリ</a>、ピッカー コントロールがキーボードの代わりに表示されます。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/PickerDemoPage.cs"><img src="views-images/Picker.png" title="ピッカーの例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>する進行状況を示すコントロール。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ProgressBarDemoPage.cs"><img src="views-images/ProgressBar.png" title="ProgressBar の使用例のクラス ="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>検索ボックスを提供するコントロール。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SearchBarDemoPage.cs"><img src="views-images/SearchBar.png" title="SearchBar 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">スライダー</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>線形の値を入力するコントロール。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SliderDemoPage.cs"><img src="views-images/Slider.png" title="スライダーの例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">ステッパ</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>不連続の値を入力するコントロールは、範囲に制限します。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StepperDemoPage.cs"><img src="views-images/Stepper.png" title="ステッパ例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">スイッチ</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>切り替えられた値を提供するコントロール。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchDemoPage.cs"><img src="views-images/Switch.png" title="スイッチの例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>の行を保持する<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">セル</a>s。
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/tableview.md">テーブルのドキュメント</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TableViewFormDemoPage.cs">デモ ソース</a>
    </td>
    <td>
    <img src="views-images/TableViewNewest.png" title="テーブルの例" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>時間ピッキングを提供するコントロール。 TimePicker のビジュアル表現は、のいずれかによく似ています<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">エントリ</a>キーボードの代わりに、時間を取得する特殊なコントロールが表示されます。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TimePickerDemoPage.cs"><img src="views-images/TimePicker.png" title="TimePicker 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a> HTML コンテンツを表示します。
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/webview.md">WebView のドキュメント</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/WebViewDemoPage.cs">デモ ソース</a>
    </td>
    <td>
    <img src="views-images/WebView.png" title="WebView の例" class="tableimg">
    </td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms ギャラリー (サンプル)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API ドキュメント](https://developer.xamarin.com/api/root/Xamarin.Forms/)
