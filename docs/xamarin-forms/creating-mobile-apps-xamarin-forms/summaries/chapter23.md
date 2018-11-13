---
title: 第 23 章の概要です。 トリガーと動作
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 23 章の概要。 トリガーと動作'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 4bfa2bed7061e031c55ccbdb7f576aa02c17581a
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563993"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>第 23 章の概要です。 トリガーと動作

トリガーと動作と同様に、両方のものをデータのバインドの使用を超える要素の相互作用を簡素化し、XAML 要素の機能を拡張する XAML ファイルで使用します。 トリガーと動作の両方は、ほぼ常に視覚的ユーザー インターフェイス オブジェクトに使用されます。

トリガーとを動作をサポートするために両方`VisualElement`と`Style`2 つのコレクション プロパティをサポートします。

- [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) [ `Style.Triggers` ](xref:Xamarin.Forms.Style.Triggers)型の `IList<TriggerBase>`
- [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) [ `Style.Behaviors` ](xref:Xamarin.Forms.Style.Behaviors)型の `IList<Behavior>`

## <a name="triggers"></a>トリガー

トリガーとは別のプロパティの変更 (いくつかのコードを実行している) 応答が返される条件 (プロパティの変更またはイベントの発生)。 `Triggers`プロパティの`VisualElement`と`Style`の種類は`IList<TriggersBase>`します。 [`TriggerBase`](xref:Xamarin.Forms.TriggerBase) 次の 4 つのシール クラスの派生元となる抽象クラスを示します。

- [`Trigger`](xref:Xamarin.Forms.Trigger) プロパティの変更に基づいて、応答の
- [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) イベントの発生に基づいて応答
- [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) データ バインドに基づく応答
- [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) 複数のトリガーに基づく応答

トリガーは、トリガーによって変更されるプロパティを持つ要素を常に設定されます。

### <a name="the-simplest-trigger"></a>最も簡単なトリガー

[ `Trigger` ](xref:Xamarin.Forms.Trigger)クラスは、プロパティ値の変化を確認し、同じ要素の別のプロパティを設定して応答します。

`Trigger` 3 つのプロパティを定義します。

- [`Property`](xref:Xamarin.Forms.Trigger.Property) 型の `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Trigger.Value) 型の `Object`
- [`Setters`](xref:Xamarin.Forms.Trigger.Setters) 型の`IList<SetterBase>`の content プロパティ `Trigger`

さらに、`Trigger`から、次のプロパティが継承される必要があります`TriggerBase`設定。

- [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType) 対象の要素の種類を示す、`Trigger`がアタッチされています。

`Property`と`Value`、条件を構成して、`Setters`コレクションが応答します。 ときに、指定された`Property`によって示される値を持つ`Value`、`Setter`内のオブジェクト、`Setters`コレクションが適用されます。 ときに、`Property`別の値を持つ、set アクセス操作子が削除されます。 `Setter` 最初の 2 つのプロパティと同じである 2 つのプロパティを定義します`Trigger`:。

- [`Property`](xref:Xamarin.Forms.Setter.Property) 型の `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Setter.Value) 型の `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop)サンプル方法、`Trigger`に適用される、`Entry`のサイズを増やすことができます、`Entry`を使用して、`Scale`プロパティと、 `IsFocused`のプロパティ、`Entry`は`true`します。

一般的ではありませんが、`Trigger`としてコードでは、設定できる、 [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode)サンプルを示します。

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers)サンプルでは、`Trigger`で設定できる、`Style`複数を適用する`Entry`要素。

### <a name="trigger-actions-and-animations"></a>トリガーの動作とアニメーション

トリガーに基づく、少しのコードを実行することもできます。 このコードは、プロパティを対象とするアニメーションにすることができます。 1 つの一般的な方法は、使用する、 [ `EventTrigger` ](xref:Xamarin.Forms.EventTrigger)、2 つのプロパティを定義します。

- [`Event`](xref:Xamarin.Forms.EventTrigger.Event) 型の`string`イベントの名前
- [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions) 型の`IList<TriggerAction>`応答で実行するアクションの一覧。

これを使用するから派生するクラスを作成する必要があります。 [ `TriggerAction<T>` ](xref:Xamarin.Forms.TriggerAction`1)、一般に`TriggerAction<VisualElement>`します。 このクラスのプロパティを定義できます。 これらは、バインド可能なプロパティではなく、プレーンな CLR プロパティのため`TriggerAction`から派生していない`BindableObject`します。 オーバーライドする必要があります、 [ `Invoke` ](xref:Xamarin.Forms.TriggerAction`1.Invoke*)アクションが呼び出されたときに呼び出されるメソッド。 引数は、ターゲット要素です。

[ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリは、例を示します。 呼び出す、`ScaleTo`アニメーション化するプロパティ、`Scale`要素のプロパティ。 型のいずれかのプロパティであるため`Easing`、 [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs)クラスでは、標準を使用できます。 `Easing` XAML の静的フィールド。

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell)サンプルを呼び出す方法を示して、`ScaleAction`から`EventTrigger`状況を監視するオブジェクト、`Focused`と`Unfocused`イベント。

[ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell)サンプルのカスタム イージング関数を定義する方法を示します`ScaleAction`分離コード ファイルにします。

使用してアクションを呼び出すこともできます、 `Trigger` (から distinguished として`EventTrigger`)。 対応している必要がありますを`TriggerBase`2 つのコレクションを定義します。

- [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) 型の `IList<TriggerAction>`
- [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) 型の `IList<TriggerAction>`

[ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell)サンプルは、これらのコレクションを使用する方法を示します。

### <a name="more-event-triggers"></a>複数のイベント トリガー

[ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ呼び出し`ScaleTo`スケール アップ/ダウンするには、2 回クリックします。 [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth)スタイルでこのサンプルで使用`EventTrigger`視覚的なフィードバックを提供するときに、`Button`が押されました。 この double アニメーションは型のコレクション内の 2 つのアクションを使用することもできます。 [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

[ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs)クラス、 **Xamarin.FormsBook.Toolkit**ライブラリには、背筋がカスタマイズ可能なアクションが定義されています。 [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo)ことを示します。

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs)クラス、 **Xamarin.FormsBook.Toolkit**ライブラリは制限されるため`Entry`要素とセット、`TextColor`プロパティを赤の場合、`Text`プロパティ`double`します。 [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation)ことを示します。

### <a name="data-triggers"></a>データ トリガー

[ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger)に似ていますが、`Trigger`プロパティの値の変更を監視するには、代わりに、データ バインディングを監視する点が異なります。 これにより、別の要素のプロパティに影響を与える 1 つの要素のプロパティ。

`DataTrigger` 3 つのプロパティを定義します。

- [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding) 型の `BindingBase`
- [`Value`](xref:Xamarin.Forms.DataTrigger.Value) 型の `Object`
- [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters) 型の `IList<SetterBase>`

[ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors)サンプルが必要です、 [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt)ライブラリと青に受講者の名前の色を設定またはピンク色がに基づいて、`Sex`プロパティ。

[![性別の色の 3 倍になるスクリーン ショット](images/ch23fg04-small.png "性別色")](images/ch23fg04-large.png#lightbox "性別の色")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler)サンプル セット、`IsEnabled`のプロパティ、`Entry`に`False`場合、`Length`のプロパティ、`Text`プロパティ、の`Entry`が 0 と等しい。 注意、`Text`プロパティは空の文字列に初期化されます。 既定では`null`、および`DataTrigger`正しく機能しません。

### <a name="combining-conditions-in-the-multitrigger"></a>条件を組み合わせることで、MultiTrigger

[ `MultiTrigger` ](xref:Xamarin.Forms.MultiTrigger)条件のコレクションです。 すべてがときに`true`、set アクセス操作子を適用します。 クラスには、2 つのプロパティを定義します。

- [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions) 型の `IList<Condition>`
- [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters) 型の `IList<Setter>`

[`Condition`](xref:Xamarin.Forms.Condition) 抽象クラスでありは子孫クラスの 2 つがあります。

- [`PropertyCondition`](xref:Xamarin.Forms.Condition)を持つ[ `Property` ](xref:Xamarin.Forms.PropertyCondition.Property)と[ `Value` ](xref:Xamarin.Forms.PropertyCondition.Value)などのプロパティ `Trigger`
- [`BindingCondition`](xref:Xamarin.Forms.BindingCondition)を持つ[ `Binding` ](xref:Xamarin.Forms.BindingCondition.Binding)と[ `Value` ](xref:Xamarin.Forms.BindingCondition.Value)などのプロパティ `DataTrigger`

[ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions)サンプルを`BoxView`と 4 つの色がのみ`Switch`要素がすべて有効にします。

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions)サンプルを作成する方法を示します、`BoxView`の色をとき*任意*4 つの`Switch`要素をオンにします。 これには、De Morgan の法律およびすべてのロジックを反転のアプリケーションが必要です。

結合 AND ロジックが簡単でないし、非表示を必要としたり`Switch`中間結果の要素。 [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions)サンプル方法、`Button`場合に有効にする 2 つの`Entry`要素では、型指定されたテキストがいくつかのテキスト入力があるどちらもない場合。

## <a name="behaviors"></a>ビヘイビアー

トリガーで実行できるでの動作を行うこともできますが、動作が常にから派生したクラスを必要と[ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1)し、次の 2 つのメソッドをオーバーライドします。

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

引数は、要素に関連付けられている動作です。 一般に、`OnAttachedTo`メソッドがいくつかのイベント ハンドラーをアタッチおよび`OnDetachingFrom`それらをデタッチします。 通常、このようなクラスは、いくつかの状態を保存、ため、通常は共有できませんで、`Style`します。

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation)サンプルは、のような**TriggerEntryValidation**ビヘイビアーを使用する点を除いて&mdash;、 [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs)クラス、[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ。

### <a name="behaviors-with-properties"></a>プロパティの動作

`Behavior<T>` 派生した`Behavior`から派生した`BindableObject`ので、動作にバインド可能なプロパティを定義することができます。 これらのプロパティはデータ バインドでアクティブにすることはできます。

これは、方法については、 [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo)は、プログラムを使用して、 [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ。 `ValidEmailBehavior` 読み取り専用のバインド可能なプロパティがあり、データ バインドでのソースとして機能します。

[ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv)サンプルでは、これと同じ動作を使用して、別の種類の電子メール アドレスが有効であるかを示すインジケーターを表示します。

[ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger)サンプルは、前の例のバリエーションの 1 つ。 ButtonGlide を使用して、`DataTrigger`組み合わせて動作します。

### <a name="toggles-and-check-boxes"></a>切り替えとチェック ボックス

などのクラスでトグル ボタンの動作をカプセル化することは[ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ、し、すべてを定義全体を XAML でトグル ボタンのビジュアル。

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel)使用して、`ToggleBehavior`で、`DataTrigger`を使用する、`Label`トグル ボタンの 2 つのテキスト文字列にします。

[ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle)サンプルでは、2 つの間で切り替えることによってこの概念が拡張され`FormattedString`オブジェクト。

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs)クラス、 **Xamarin.FormsBook.Toolkit**から派生したライブラリ`ContentView`、定義、`IsToggled`プロパティが組み込まれていると、`ToggleBehavior`トグル ボタンロジック。 こうこと、XAML でのトグル ボタンの定義を簡単に示すように、 [ **TraditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox)サンプル。

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo)が含まれています、 [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs)クラスから派生した`ToggleBase`を使用して、 [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)、Xamarin.Forms のような切り替えボタンを作成するにはクラス`Switch`します。

A [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs)で、 **Xamarin.FormsBook.Toolkit**アニメーションでアニメーションのレバーにするための提供、 [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)サンプル。

### <a name="responding-to-taps"></a>タップに応答

1 つの欠点の`EventTrigger`をアタッチできませんが、`TapGestureRecognizer`タップに応答します。 目的は、その問題を回避する[ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs)で、 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

[ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver)使用して`TapBehavior`、以前に使用する`ShiverAction`タップの`BoxView`要素。

[ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews)サンプルがカプセル化して、マークアップを削減する方法を示しています、`ShiverView`クラス。

### <a name="radio-buttons"></a>オプション ボタン

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリには、 [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs)ごとにグループ化するラジオ ボタンを作成するクラス、`string`グループ名。

[ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels)プログラムがそのラジオ ボタンのテキスト文字列を使用します。 [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle)使用して、`Style`外観 checked と unchecked のボタンの間の差をします。 [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages)サンプルでは、そのラジオ ボタンのボックス化されたイメージを使用します。

[![オプションのイメージの 3 倍になるスクリーン ショット](images/ch23fg17-small.png "ラジオ ボタン イメージ")](images/ch23fg17-large.png#lightbox "ラジオ ボタンの画像")

[ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios)サンプルは、円の内部にドットで表示されている従来のラジオ ボタンを描画します。

### <a name="fades-and-orientation"></a>フェードと印刷の向き

最後のサンプルでは、 [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders)ラジオ ボタンを使用して次の 3 つの異なる色の選択ビューの間で切り替えることができます。 3 つのビュー フェードインとフェードアウトを使用して、 [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ。

方向を縦長と横長の使用の間での変更にも対応して、プログラム、 [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs)で、 **Xamarin.FormsBook.Toolkit**ライブラリ。



## <a name="related-links"></a>関連リンク

- [第 23 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [第 23 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [トリガーの使用](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms の動作](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
