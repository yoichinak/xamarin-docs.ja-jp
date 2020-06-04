---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 23. Triggers and behaviors''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9a0206354254f79756e29f834c85837240736eca
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136657"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>第 23 章の概要。 トリガーと動作

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)

トリガーと動作は、両方とも XAML ファイルで使用して、データ バインディングの使用を超えた要素の相互作用を簡略化し、XAML 要素の機能を拡張することが意図されているという点で似ています。 トリガーと動作は、ほとんどの場合、ビジュアル ユーザーインターフェイス オブジェクトと共に使用されます。

トリガーと動作をサポートするために、`VisualElement` と `Style` の両方で 2 つのコレクション プロパティがサポートされます。

- `IList<TriggerBase>` 型の [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) および [`Style.Triggers`](xref:Xamarin.Forms.Style.Triggers)
- `IList<Behavior>` 型の [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) および [`Style.Behaviors`](xref:Xamarin.Forms.Style.Behaviors)

## <a name="triggers"></a>トリガー

トリガーとは、応答 (別のプロパティの変更または一部のコードの実行) を引き起こす条件 (プロパティの変更またはイベントの起動) です。 `VisualElement` および `Style` の `Triggers` プロパティは、`IList<TriggersBase>` 型です。 [`TriggerBase`](xref:Xamarin.Forms.TriggerBase) は、4 つのシール クラスの派生元となる抽象クラスです。

- プロパティの変更に基づいた応答の [`Trigger`](xref:Xamarin.Forms.Trigger)
- イベント起動に基づいた応答の [`EventTrigger`](xref:Xamarin.Forms.EventTrigger)
- データ バインディングに基づいた応答の [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)
- 複数のトリガーに基づいた応答の [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger)

トリガーは常に、プロパティがトリガーによって変更されている要素に対して設定されます。

### <a name="the-simplest-trigger"></a>最もシンプルなトリガー

[`Trigger`](xref:Xamarin.Forms.Trigger) クラスは、プロパティ値の変更を確認し、同じ要素の別のプロパティを設定することによって応答します。

`Trigger` では、3 つのプロパティが定義されます。

- `BindableProperty` 型の [`Property`](xref:Xamarin.Forms.Trigger.Property)
- `Object` 型の [`Value`](xref:Xamarin.Forms.Trigger.Value)
- 型 `IList<SetterBase>` の [`Setters`](xref:Xamarin.Forms.Trigger.Setters)、`Trigger` のコンテンツ プロパティ

また、`Trigger` では、`TriggerBase` から継承された次のプロパティが設定されている必要があります。

- [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType)。`Trigger` がアタッチされている要素の型を示します。

`Property` と `Value` により条件が構成され、`Setters` コレクションは応答です。 指定された `Property` に `Value` によって指定された値がある場合、`Setters` コレクションの `Setter` オブジェクトが適用されます。 `Property` の値が異なる場合、setters が削除されます。 `Setter` では、`Trigger` の最初の 2 つのプロパティと同じ 2 つのプロパティが定義されます。

- `BindableProperty` 型の [`Property`](xref:Xamarin.Forms.Setter.Property)
- `Object` 型の [`Value`](xref:Xamarin.Forms.Setter.Value)

[**EntryPop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) サンプルは、`Entry` の `IsFocused` プロパティが `true` の場合に、`Entry` に適用される `Trigger` により `Scale` プロパティを使用して `Entry` のサイズを増やす方法を示します。

これは一般的ではありませんが、[**EntryPopCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) サンプルで示されているように、`Trigger` をコードで設定できます。

[**StyledTriggers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) サンプルは、`Trigger` を `Style` に設定して複数の `Entry` 要素に適用する方法を示しています。

### <a name="trigger-actions-and-animations"></a>トリガー アクションとアニメーション

また、トリガーに基づいてわずかなコードを実行することもできます。 このコードは、プロパティを対象とするアニメーションにすることができます。 一般的な方法の 1 つとして、2 つのプロパティを定義する [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) を使用する方法があります。

- `string` 型の [`Event`](xref:Xamarin.Forms.EventTrigger.Event)。イベントの名前
- `IList<TriggerAction>` 型の [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions)。応答として実行するアクションの一覧

これを使用するには、[`TriggerAction<T>`](xref:Xamarin.Forms.TriggerAction`1) から派生したクラスを記述する必要があります (通常は `TriggerAction<VisualElement>`)。 このクラスでは、プロパティを定義できます。 これらは、`TriggerAction` が `BindableObject` から派生しないため、連結可能なプロパティではなく、プレーンな CLR プロパティです。 アクションが呼び出されたときに呼び出される [`Invoke`](xref:Xamarin.Forms.TriggerAction`1.Invoke*) メソッドをオーバーライドする必要があります。 引数は対象の要素です。

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリの [`ScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) クラスが 1 つの例です。 これにより `ScaleTo` プロパティが呼び出され、要素の `Scale` プロパティがアニメーション化されます。 このプロパティの 1 つは `Easing` 型であるため、[`EasingConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) クラスを使用すると、XAML で標準の `Easing` 静的フィールドを使用できます。

[**EntrySwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) サンプルは、`Focused`イベントと `Unfocused` イベントを監視する `EventTrigger` オブジェクトから `ScaleAction` を呼び出す方法を示します。

[**CustomEasingSwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) サンプルは、分離コード ファイル内の `ScaleAction` に対してカスタム イージング関数を定義する方法を示しています。

(`EventTrigger` ではなく) `Trigger` を使ってアクションを呼び出すこともできます。 これには、`TriggerBase` によって 2 つのコレクションが定義されることを認識している必要があります。

- `IList<TriggerAction>` 型の [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions)
- `IList<TriggerAction>` 型の [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions)

[**EnterExitSwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) サンプルでは、これらのコレクションの使用方法を示します。

### <a name="more-event-triggers"></a>その他のイベント トリガー

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリの [`ScaleUpAndDownAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) クラスでは、スケールアップおよびスケールダウンのため `ScaleTo` が 2 回呼び出されます。 [**ButtonGrowth**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) サンプルでは、スタイル設定された `EventTrigger` 内でこれを使用して、`Button` が押されたときに視覚的なフィードバックが提供されます。 このダブル アニメーションは、型 [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs) のコレクション内の 2 つのアクションを使用して実現することもできます。

**Xamarin.FormsBook.Toolkit** ライブラリ内の [`ShiverAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) クラスでは、カスタマイズ可能な Shiver アクションが定義されます。 これは、[**ShiverButtonDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) サンプルで示されています。

**Xamarin.FormsBook.Toolkit** ライブラリの [`NumericValidationAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) クラスは `Entry` 要素に制限されており、`Text` プロパティが `double` でない場合に `TextColor` プロパティが赤に設定されます。 これは、[**TriggerEntryValidation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) サンプルで示されています。

### <a name="data-triggers"></a>データ トリガー

[`DataTrigger`](xref:Xamarin.Forms.DataTrigger) は `Trigger` に似ています。ただし、値の変更のプロパティが監視されるのではなく、データ バインディングが監視されます。 これにより、1 つの要素内の 1 つのプロパティによって、他の要素内の 1 つのプロパティに影響が及ぼされます。

`DataTrigger` では、3 つのプロパティが定義されます。

- `BindingBase` 型の [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding)
- `Object` 型の [`Value`](xref:Xamarin.Forms.DataTrigger.Value)
- `IList<SetterBase>` 型の [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters)

[**GenderColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) サンプルでは [**SchoolOfFineArt**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) ライブラリが必要で、学生の名前の色が `Sex` プロパティに基づいて、青またはピンクに設定されます。

[![性別の色のトリプル スクリーンショット](images/ch23fg04-small.png "性別の色")](images/ch23fg04-large.png#lightbox "性別の色")

[**ButtonEnabler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) サンプルでは、`Entry` の `Text` プロパティの `Length` プロパティが 0 の場合、`Entry` の `IsEnabled` プロパティが `False` に設定されます。 `Text` プロパティが空の文字列に初期化されていることに注意してください。既定ではこれは `null` で、`DataTrigger` は正しく機能しません。

### <a name="combining-conditions-in-the-multitrigger"></a>MultiTrigger での条件の結合

[`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) は、条件のコレクションです。 これらがすべて `true` の場合、setter が適用されます。 このクラスでは、2 つのプロパティが定義されます。

- `IList<Condition>` 型の [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions)
- `IList<Setter>` 型の [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters)

[`Condition`](xref:Xamarin.Forms.Condition) は抽象クラスであり、次の 2 つの子孫クラスを持ちます。

- [`PropertyCondition`](xref:Xamarin.Forms.Condition)。[`Property`](xref:Xamarin.Forms.PropertyCondition.Property) および [`Value`](xref:Xamarin.Forms.PropertyCondition.Value) プロパティ (`Trigger` など) を持ちます
- [`BindingCondition`](xref:Xamarin.Forms.BindingCondition)。[`Binding`](xref:Xamarin.Forms.BindingCondition.Binding) および [`Value`](xref:Xamarin.Forms.BindingCondition.Value) プロパティ (`DataTrigger` など) を持ちます

[**AndConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) サンプルでは、4 つの `Switch` 要素がすべて有効になっている場合にのみ、`BoxView` が色分けされます。

[**OrConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) サンプルでは、4 つの `Switch` 要素の "*いずれか*" が有効になっている場合に `BoxView` に色を付ける方法を示します。 これには、De Morgan の法則を適用し、すべてのロジックを逆にする必要があります。

AND と OR の組み合わせはそれほど簡単ではなく、通常、途中結果の非表示の `Switch` 要素が必要です。 [**XorConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) サンプルでは、2 つの `Entry` 要素のいずれかにテキストが入力されている場合に `Button` を有効にする方法を示します (ただし、両方の要素にテキストが入力されている場合は無効になります)。

## <a name="behaviors"></a>ビヘイビアー

トリガーで行えることはすべて動作でも行えますが、動作は常に [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) から派生するクラスを必要とし、次の 2 つのメソッドをオーバーライドします。

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

引数は、動作のアタッチ先の要素です。 一般に、`OnAttachedTo` メソッドにより一部のイベント ハンドラーがアタッチされ、`OnDetachingFrom` によってそれらがデタッチされます。 このようなクラスにより一部の状態が保存されるため、通常、これを `Style` で共有することはできません。

[**BehaviorEntryValidation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) サンプルは **TriggerEntryValidation** に似ていますが、ここでは動作 &mdash; が使用されます ([**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリの [`NumericValidationBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) クラス)。

### <a name="behaviors-with-properties"></a>プロパティを持つ動作

`Behavior<T>` は (`BindableObject` から派生する) `Behavior` から派生するため、連結可能なプロパティを動作に対して定義できます。 これらのプロパティは、データ バインディングでアクティブにできます。

これは、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリで [`ValidEmailBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) クラスを利用する [**EmailValidationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) プログラムに示されています。 `ValidEmailBehavior` には、読み取り専用の連結可能なプロパティがあります。また、これはデータ バインディングでのソースとしての役割を果たします。

[**EmailValidationConv**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) サンプルでは、この同じ動作を使用して電子メール アドレスが有効であることを知らせる別の種類のインジケーターが表示されます。

[**EmailValidationTrigger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) サンプルは、前のサンプルを変更したものです。 ButtonGlide では、その動作と組み合わせて `DataTrigger` が使用されます。

### <a name="toggles-and-check-boxes"></a>トグルとチェック ボックス

トグル ボタンの動作を [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリ内の [`ToggleBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) などのクラスにカプセル化して、すべてのビジュアルを XAML での完全なトグル用に定義できます。

[**ToggleLabel**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) サンプルでは、`ToggleBehavior` を `DataTrigger` と共に使用して、トグル用に 2 つのテキスト文字列を持つ `Label` を使用します。

[**FormattedTextToggle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) サンプルでは、2 つの `FormattedString` オブジェクト間を切り替えることによって、この概念を拡張します。

**Xamarin.FormsBook.Toolkit** ライブラリの [`ToggleBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) クラスは `ContentView` から派生し、`IsToggled` プロパティを定義し、トグル ロジック用に `ToggleBehavior` を組み込みます。 これにより、[**TraditionalCheckBox**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) サンプルで示されるように、XAML のトグル ボタンを簡単に定義できます。

[**SwitchCloneDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) には `ToggleBase` から派生した [`SwitchClone`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) クラスが含まれていて、[`TranslateAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs) クラスを使用して、Xamarin.Forms `Switch` に似たトグル ボタンが構築されます。

**Xamarin.FormsBook.Toolkit** の [`RotateAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) により、[**LeverToggle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle) サンプルでアニメーション化されたレバーを作成するために使用されるアニメーションが提供されます。

### <a name="responding-to-taps"></a>タップへの応答

`EventTrigger` の欠点の 1 つは、これを `TapGestureRecognizer` にアタッチしてタップに応答できないことです。 この問題を回避することが、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) の [`TapBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) の目的です。

[**BoxViewTapShiver**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) サンプルでは `TapBehavior` を使用して、タップされた `BoxView` 要素用に前の `ShiverAction` を使用します。

[**ShiverViews**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) サンプルでは、`ShiverView` クラスをカプセル化することによってマークアップを減らす方法を示します。

### <a name="radio-buttons"></a>ラジオ ボタン

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリには、`string` グループ名でグループ化されたラジオ ボタンを作成するための [`RadioBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) クラスもあります。

[**RadioLabels**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) プログラムでは、ラジオ ボタン用にテキスト文字列が使用されます。 [**RadioStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) サンプルでは、checked ボタンと unchecked ボタンの外観の違いに `Style` を使用します。 [**RadioImages**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) サンプルでは、ラジオ ボタン用にボックス化されたイメージが使用されます。

[![ラジオ イメージのトリプル スクリーンショット](images/ch23fg17-small.png "ラジオ ボタンのイメージ")](images/ch23fg17-large.png#lightbox "ラジオ ボタンのイメージ")

[**TraditionalRadios**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) サンプルでは、円の内側にドットのある、従来表示されるラジオ ボタンを描画します。

### <a name="fades-and-orientation"></a>フェードと向き

最後のサンプル [**MultiColorSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) を使用すると、ラジオ ボタンを使用して、3 つの異なる色選択ビュー間を切り替えることができます。 これら 3 つのビューは、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリの [`FadeEnableAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) を使用してフェードインおよびフェードアウトします。

また、プログラムは、**Xamarin.FormsBook.Toolkit** ライブラリの [`GridOrientationBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) を使用して、縦と横の向きの変化にも対応します。

## <a name="related-links"></a>関連リンク

- [第 23 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [第 23 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [トリガーの使用](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms のビヘイビアー](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
