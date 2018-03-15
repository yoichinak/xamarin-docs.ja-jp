---
title: "23 章の概要です。 トリガーと動作"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ddb76e00cfe1c19a9d31dc3e53b80a2be0697dbc
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>23 章の概要です。 トリガーと動作

トリガーと動作と同様に、これらは両方の目的はデータ バインドの使用を超える要素の相互作用を簡素化し、XAML 要素の機能を拡張する XAML ファイルで使用します。 トリガーと動作の両方は、ほぼ常にビジュアルのユーザー インターフェイス オブジェクトで使用されます。

トリガーと動作をサポートするために両方`VisualElement`と`Style`2 つのコレクションのプロパティをサポートします。

- [`VisualElement.Triggers`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) および[ `Style.Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Triggers/)型の `IList<TriggerBase>`
- [`VisualElement.Behaviors`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) および[ `Style.Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Behaviors/)型の `IList<Behavior>`

## <a name="triggers"></a>トリガー

トリガーとは別のプロパティの変更 (一部のコードを実行している) の応答に結果条件 (プロパティの変更またはイベントの発生)。 `Triggers`プロパティ`VisualElement`と`Style`の種類は`IList<TriggersBase>`します。 [`TriggerBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerBase/) 4 つのシールされたクラスの派生元となる抽象クラスを示します。

- [`Trigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) プロパティの変更に基づいて応答
- [`EventTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/) イベントの発生に基づく応答
- [`DataTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) データ バインディングに基づいて応答
- [`MultiTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) 複数のトリガーに基づく応答

トリガーは常に、トリガーによって変更されるプロパティを持つ要素に設定します。

### <a name="the-simplest-trigger"></a>最も単純なトリガー

[ `Trigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/)クラス プロパティ値の変化を確認し、同じ要素の別のプロパティを設定して応答します。

`Trigger` 3 つのプロパティを定義します。

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Property/) 型の `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Value/) 型の `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Setters/) 型の`IList<SetterBase>`の content プロパティ `Trigger`

さらに、`Trigger`から、次のプロパティが継承される必要があります`TriggerBase`を設定します。

- [`TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.TargetType/) 対象の要素の種類を示すために、`Trigger`がアタッチされています。

`Property`と`Value`、条件を構成して、`Setters`コレクション応答を示します。 ときに、指定された`Property`によって示される値を持つ`Value`、`Setter`内のオブジェクト、`Setters`コレクションが適用されます。 ときに、`Property`別の値を持つ、set アクセス操作子が削除されます。 `Setter` 最初の 2 つのプロパティと同じである 2 つのプロパティを定義`Trigger`:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) 型の `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/) 型の `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop)サンプルではどのように、`Trigger`に適用される、`Entry`のサイズを増やすことができます、`Entry`経由で、`Scale`プロパティときに、 `IsFocused`のプロパティ、`Entry`は`true`します。

一般的ではありませんが、`Trigger`としてコードでは、設定することができます、 [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode)サンプルを示します。

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers)サンプルでは、どのように`Trigger`で設定できます、`Style`複数を適用する`Entry`要素。

### <a name="trigger-actions-and-animations"></a>トリガーの動作とアニメーション

トリガーに基づく、少しのコードを実行することもできます。 このコードは、プロパティを対象とするアニメーションをすることができます。 1 つの一般的な方法を使用して、 [ `EventTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/)、2 つのプロパティを定義します。

- [`Event`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Event/) 型の`string`イベントの名前
- [`Actions`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Actions/) 型の`IList<TriggerAction>`応答で実行する操作の一覧です。

これを使用するから派生するクラスを作成する必要があります。 [ `TriggerAction<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/)、一般に`TriggerAction<VisualElement>`です。 このクラスでプロパティを定義できます。 これらは、バインド可能なプロパティではなく、標準の CLR プロパティのため`TriggerAction`から派生していない`BindableObject`です。 オーバーライドする必要があります、 [ `Invoke` ](https://developer.xamarin.com/api/member/Xamarin.Forms.TriggerAction%3CT%3E.Invoke/p/T/)アクションが呼び出されたときに呼び出されるメソッド。 引数は、ターゲット要素です。

[ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリは、例を示します。 呼び出す、`ScaleTo`アニメーション化するプロパティ、`Scale`要素のプロパティです。 型のいずれかのプロパティがあるため`Easing`、 [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs)クラスでは、標準を使用できます。 `Easing` XAML の静的フィールドです。

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell)サンプルを呼び出す方法を示します、`ScaleAction`から`EventTrigger`状況を監視するオブジェクト、`Focused`と`Unfocused`イベント。

[ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell)サンプルのカスタム イージング関数を定義する方法を示します`ScaleAction`分離コード ファイルにします。

使用してアクションを起動することも、 `Trigger` (から distinguished として`EventTrigger`)。 対応している必要がありますを`TriggerBase`2 つのコレクションを定義します。

- [`EnterActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.EnterActions/) 型の `IList<TriggerAction>`
- [`ExitActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.ExitActions/) 型の `IList<TriggerAction>`

[ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell)サンプルは、これらのコレクションを使用する方法を示します。

### <a name="more-event-triggers"></a>複数のイベント トリガー

[ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ呼び出し`ScaleTo`を上下に拡大縮小を 2 回クリックします。 [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth)サンプルでは、これを使用で、スタイル設定された`EventTrigger`視覚的なフィードバックを提供するときに、`Button`が押されました。 この 2 つのアニメーションが型のコレクションに 2 つのアクションを使用することも [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

[ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs)クラス内で、 **Xamarin.FormsBook.Toolkit**ライブラリは、カスタマイズ可能な背筋アクションを定義します。 [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo)サンプルでは、ことを示しています。

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs)クラス内で、 **Xamarin.FormsBook.Toolkit**ライブラリは制限されるため`Entry`要素およびセット、`TextColor`場合は赤にプロパティ、`Text`プロパティ`double`です。 [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation)サンプルでは、ことを示しています。

### <a name="data-triggers"></a>データのトリガー

[ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/)に似ていますが、`Trigger`の値が変更されたプロパティを監視するには、代わりに、データ バインディングを監視する点が異なります。 これにより、別の要素のプロパティに影響を与える 1 つの要素のプロパティです。

`DataTrigger` 3 つのプロパティを定義します。

- [`Binding`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Binding/) 型の `BindingBase`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Value/) 型の `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Setters/) 型の `IList<SetterBase>`

[ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors)サンプルが必要です、 [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt)ライブラリと、色を青受講者の名前のセットまたはピンクのに基づいて、`Sex`プロパティ。

[![性別色のトリプル スクリーン ショット](images/ch23fg04-small.png "性別色")](images/ch23fg04-large.png#lightbox "性別の色")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler)サンプル セット、`IsEnabled`のプロパティ、`Entry`に`False`場合、`Length`のプロパティ、`Text`プロパティ、の`Entry`0 に等しい。 注意して、`Text`プロパティは、空の文字列に初期化以外の場合は既定では`null`、および`DataTrigger`組み合わせは正しく機能します。

### <a name="combining-conditions-in-the-multitrigger"></a>MultiTrigger に条件の結合

[ `MultiTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/)条件のコレクションです。 すべて`true`、set アクセス操作子を適用します。 クラスには、2 つのプロパティを定義します。

- [`Conditions`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Conditions/) 型の `IList<Condition>`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Setters/) 型の `IList<Setter>`

[`Condition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/) 抽象クラスは、2 つの派生クラスがあります。

- [`PropertyCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/)、を持つ[ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Property/)と[ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Value/)などのプロパティ `Trigger`
- [`BindingCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingCondition/)、を持つ[ `Binding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Binding/)と[ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Value/)などのプロパティ `DataTrigger`

[ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions)サンプルについては、 `BoxView` 4 ときは、カラーのみ`Switch`要素がすべてオンにします。

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions)サンプルでは、作成する方法を示しています、`BoxView`色と*任意*4 つの`Switch`要素をオンにします。 これには、アプリケーションの De Morgan に関する法律およびすべてのロジックを反転することが必要です。

組み合わせること AND ロジックが簡単には通常、非表示が必要ですや`Switch`中間結果の要素。 [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions)サンプルでは方法、`Button`場合に有効にする 2 つの`Entry`要素、型指定されたいくつかのテキストがいない場合、どちらもいくつかのテキスト入力します。

## <a name="behaviors"></a>ビヘイビアー

トリガーで実行できるものの動作を行うこともできますが動作から派生するクラスを常に要求する[ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)し、次の 2 つのメソッドをオーバーライドします。

- [`OnAttachedTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/T/)
- [`OnDetachingFrom`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/T/)

引数は、動作にアタッチされている要素です。 一般に、`OnAttachedTo`メソッドがいくつかのイベント ハンドラーをアタッチし、`OnDetachingFrom`それらの関連付けを解除します。 通常、このようなクラスには、いくつかの状態が保存される、ため、通常では共有できない、`Style`です。

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation)サンプルはのような**TriggerEntryValidation**ビヘイビアーを使用する点を除いて&mdash;、 [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs)クラス内で、[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリです。

### <a name="behaviors-with-properties"></a>プロパティの動作

`Behavior<T>` 派生した`Behavior`から派生した`BindableObject`ので、動作にバインド可能なプロパティを定義することができます。 これらのプロパティは、データ バインドでアクティブにできます。

これに示されている、 [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo)は、プログラムを使用して、 [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリです。 `ValidEmailBehavior` 読み取り専用のバインド可能なプロパティを持ち、データのバインド内のソースとして機能します。

[ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv)サンプルでは、これと同じ動作を使用して、別の種類の電子メール アドレスが有効であることを通知するインジケーターを表示します。

[ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger)サンプルは、前のサンプルのバリエーションの 1 つです。 ButtonGlide を使用して、`DataTrigger`動作を組み合わせています。

### <a name="toggles-and-check-boxes"></a>切り替えやチェック ボックス

などのクラスでのトグル ボタンの動作をカプセル化することは[ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ、し、すべて定義XAML での切り替えのビジュアル。

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel)サンプルは、`ToggleBehavior`で、`DataTrigger`を使用する、`Label`のトグル ボタンの 2 つのテキスト文字列を使用します。

[ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle)サンプルでは、2 つの間を切り替えることによってこの概念が拡張され`FormattedString`オブジェクト。

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs)クラス内で、 **Xamarin.FormsBook.Toolkit**から派生したライブラリ`ContentView`、定義、`IsToggled`プロパティ、組み込まれています、`ToggleBehavior`トグル ボタンロジック。 これによって、XAML では、トグル ボタンを定義する簡単に示すように、 [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox)サンプルです。

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo)が含まれています、 [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs)から派生したクラス`ToggleBase`を使用して、 [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)、Xamarin.Forms のようなトグル ボタンを構築するためにクラス`Switch`です。

A [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs)で、 **Xamarin.FormsBook.Toolkit**アニメーションでアニメーションのレベルにするための提供、 [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)サンプルです。

### <a name="responding-to-taps"></a>タップへの応答

1 つの欠点`EventTrigger`にアタッチできないことが、`TapGestureRecognizer`タップに応答します。 目的は、その問題を回避する[ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs)で、 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

[ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver)サンプルは`TapBehavior`以前を使用する`ShiverAction`タップの`BoxView`要素。

[ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews)サンプルをカプセル化して、マークアップを削減する方法を示しています、`ShiverView`クラスです。

### <a name="radio-buttons"></a>オプション ボタン

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリには、 [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs)によってグループ化のオプション ボタンを作成するクラス、`string`グループの名前。

[ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels)プログラムは、ラジオ ボタンのテキスト文字列を使用します。 [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle)サンプルは、`Style`外観 checked と unchecked のボタンの間の差をします。 [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages)サンプルでは、そのラジオ ボタンのボックス化されたイメージを使用します。

[![ラジオ イメージのトリプル スクリーン ショット](images/ch23fg17-small.png "ラジオ ボタン イメージ")](images/ch23fg17-large.png#lightbox "ラジオ ボタンの画像")

[ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios)サンプルは、円で囲まれたドット付きで表示されている従来のラジオ ボタンを描画します。

### <a name="fades-and-orientation"></a>フェードや向き

最後のサンプルでは、 [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders)ラジオ ボタンを使用して 3 つの異なる色の選択ビューに切り替えることができます。 3 つのビュー フェード in と out を使用して、 [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリです。

方向を縦長と横長の使用の間での変更にも対応して、プログラム、 [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs)で、 **Xamarin.FormsBook.Toolkit**ライブラリです。



## <a name="related-links"></a>関連リンク

- [23 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [23 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [トリガーの使用](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms の動作](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
