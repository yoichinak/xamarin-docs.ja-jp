---
title: エンタープライズアプリでの検証
description: この章では、eShopOnContainers モバイルアプリがユーザー入力の検証を実行する方法について説明します。 これには、検証規則の指定、検証のトリガー、検証エラーの表示が含まれます。
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: de5728710a408b8e0c7c68dc89c7e6484cbcc3ce
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760163"
---
# <a name="validation-in-enterprise-apps"></a>エンタープライズアプリでの検証

ユーザーからの入力を受け入れるアプリであれば、入力が有効であることを確認する必要があります。 たとえば、特定の範囲の文字のみを含む入力、特定の長さ、または特定の形式に一致する入力を確認することができます。 検証を行わないと、ユーザーはアプリを失敗させるデータを提供できます。 検証では、ビジネスルールを適用し、攻撃者が悪意のあるデータを挿入するのを防ぎます。

モデルビュービューモデル (MVVM) パターンのコンテキストでは、データ検証を実行し、ユーザーが検証エラーを修正できるようにビューの検証エラーを通知するために、ビューモデルまたはモデルが必要になることがよくあります。 EShopOnContainers モバイルアプリは、ビューモデルのプロパティの同期クライアント側検証を実行し、無効なデータが含まれているコントロールを強調表示し、ユーザーに通知するエラーメッセージを表示することによって、検証エラーをユーザーに通知します。データが無効である理由。 図6-1 は、eShopOnContainers モバイルアプリで検証を実行するために必要なクラスを示しています。

[EShopOnContainers モバイルアプリの検証クラス![(validation-images/validation.png " ")]](validation-images/validation-large.png#lightbox "EShopOnContainers モバイルアプリの検証クラス")

**図 6-1**:EShopOnContainers モバイルアプリの検証クラス

検証を必要とするモデルのプロパティを`ValidatableObject<T>`型とし`ValidatableObject<T>`て表示します。各インスタンス`Validations`には、そのプロパティに検証規則が追加されています。 検証`Validate`は、 `ValidatableObject<T>`インスタンスのメソッドを呼び出すことによって、ビューモデルから呼び出されます。このメソッドは、 `ValidatableObject<T>`検証規則を取得`Value`し、プロパティに対して実行します。 すべて`Errors`の検証エラーが`ValidatableObject<T>`インスタンス`IsValid`のプロパティに配置され、 `ValidatableObject<T>`インスタンスのプロパティが更新され、検証が成功したか失敗したかが示されます。

プロパティ`ExtendedBindableObject`の変更通知はクラスによって提供さ[`Entry`](xref:Xamarin.Forms.Entry)れるため、 `IsValid`コントロールは、入力さ`ValidatableObject<T>`れたデータが有効かどうかを通知するために、ビューモデルクラスのインスタンスのプロパティにバインドできます。

## <a name="specifying-validation-rules"></a>検証規則の指定

検証規則は、次のコード例に示すように`IValidationRule<T>` 、インターフェイスから派生するクラスを作成することによって指定します。

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

このインターフェイスは、検証規則クラスが、必要な`boolean`検証を実行するために使用されるメソッドを`Check`提供`ValidationMessage`する必要があることを指定します。また、次の場合に表示される検証エラーメッセージを値とするプロパティを指定します。検証は失敗します。

次のコード例は、 `IsNotNullOrEmptyRule<T>` eShopOnContainers モバイルアプリでモックサービスを使用するときに、 `LoginView`でユーザーが入力したユーザー名とパスワードの検証を実行するために使用される検証規則を示しています。

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

メソッド`Check`は、値`boolean`の引数がであるか`null`、空であるか、または空白文字のみで構成されているかを示すを返します。

EShopOnContainers モバイルアプリでは使用されませんが、次のコード例は、電子メールアドレスを検証するための検証規則を示しています。

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

メソッド`Check`は、値`boolean`引数が有効な電子メールアドレスであるかどうかを示すを返します。 これは、 `Regex`コンストラクターで指定された正規表現パターンが最初に出現する値引数を検索することで実現されます。 入力文字列で正規表現パターンが見つかったかどうかは、 `Match`オブジェクトの`Success`プロパティの値をチェックすることによって判断できます。

> [!NOTE]
> プロパティの検証には、依存プロパティが含まれる場合があります。 依存プロパティの例として、プロパティ A の有効な値のセットが、プロパティ B で設定されている特定の値に依存する場合があります。プロパティ A の値が、許可されている値のいずれかであることを確認するには、プロパティ B の値を取得する必要があります。さらに、プロパティ B の値が変更された場合は、プロパティ A を再検証する必要があります。

## <a name="adding-validation-rules-to-a-property"></a>プロパティへの検証規則の追加

EShopOnContainers モバイルアプリでは、検証を必要とするビューモデルプロパティが型`ValidatableObject<T>`として宣言されます。ここ`T`で、は検証対象のデータの型です。 次のコード例は、このような2つのプロパティの例を示しています。

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

検証を実行するには、次のコード例に`Validations`示すように`ValidatableObject<T>` 、各インスタンスのコレクションに検証規則を追加する必要があります。

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

このメソッドは、 `IsNotNullOrEmptyRule<T>`検証規則を各`Validations` `ValidatableObject<T>`インスタンスのコレクションに追加します。検証規則の`ValidationMessage`プロパティの値を指定します。これは、次の場合に表示される検証エラーメッセージを指定します。検証は失敗します。

## <a name="triggering-validation"></a>検証のトリガー

EShopOnContainers モバイルアプリで使用される検証方法では、プロパティの検証を手動でトリガーし、プロパティが変更されたときに自動的に検証をトリガーできます。

### <a name="triggering-validation-manually"></a>手動による検証のトリガー

検証は、ビューモデルのプロパティに対して手動でトリガーできます。 たとえば、eShopOnContainers モバイルアプリでは、モックサービスを使用しているときに、ユーザー `LoginView`がの [ログイン] ボタンをタップしたときに発生します。 コマンドデリゲートは、の`MockSignInAsync` `LoginViewModel`メソッドを呼び出します。このメソッドは、次`Validate`のコード例に示すように、メソッドを実行して検証を呼び出します。

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

メソッド`Validate`は、 `LoginView`各`ValidatableObject<T>`インスタンスで Validate メソッドを呼び出すことによって、にユーザーが入力したユーザー名とパスワードの検証を実行します。 次のコード例は、クラスの`ValidatableObject<T>` Validate メソッドを示しています。

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

このメソッドは`Errors` 、コレクションをクリアし、オブジェクトの`Validations`コレクションに追加されたすべての検証規則を取得します。 取得した検証規則の`ValidatableObject<T>` `Errors` `Check`メソッドが実行され、データの検証に失敗した検証規則のプロパティ値がインスタンスのコレクションに追加されます。`ValidationMessage` 最後に`IsValid` 、プロパティが設定され、その値が呼び出し元のメソッドに返され、検証が成功したか失敗したかが示されます。

### <a name="triggering-validation-when-properties-change"></a>プロパティが変更したときに検証をトリガーする

バインドされたプロパティが変更されるたびに、検証をトリガーすることもできます。 たとえば、内の双方向のバインディングで`LoginView` `UserName`または`Password`プロパティを設定すると、検証がトリガーされます。 この状況を示すコード例を次に示します。

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

コントロール[`Entry`](xref:Xamarin.Forms.Entry) `UserName.Value`がインスタンスの`ValidatableObject<T>`プロパティにバインドされ、コントロールの`Behaviors`コレクション`EventToCommandBehavior`にインスタンスが追加されます。 この動作は、 `ValidateUserNameCommand`の [`TextChanged` `Entry`] イベントの発生に応答してを実行します。これは、 `Entry`のテキストが変更されたときに発生します。 その後、デリゲート`ValidateUserNameCommand`は`ValidateUserName` `Validate`メソッドを実行し、 `ValidatableObject<T>`インスタンスでメソッドを実行します。 したがって、ユーザーがユーザー名の`Entry`コントロールに文字を入力するたびに、入力されたデータの検証が行われます。

動作の詳細については、「[動作の実装](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)」を参照してください。

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>検証エラーの表示

EShopOnContainers モバイルアプリは、無効なデータが含まれているコントロールを赤色の線で強調表示し、ユーザーに対して、データが無効であることをユーザーに通知するエラーメッセージを表示することによって、検証エラーをユーザーに通知します。データが無効です。 無効なデータが修正されると、行が黒に変わり、エラーメッセージが削除されます。 図6-2 に、検証エラーが存在する場合の eShopOnContainers mobile アプリの LoginView を示します。

![](validation-images/validation-login.png "ログイン時の検証エラーの表示")

**図 6-2:** ログイン時の検証エラーの表示

### <a name="highlighting-a-control-that-contains-invalid-data"></a>無効なデータが含まれているコントロールの強調表示

接続`LineColorBehavior`動作は、検証エラーが[`Entry`](xref:Xamarin.Forms.Entry)発生したコントロールを強調表示するために使用されます。 次のコード例は、アタッチ`LineColorBehavior`された動作を`Entry`コントロールにアタッチする方法を示しています。

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

コントロール[`Entry`](xref:Xamarin.Forms.Entry)は、次のコード例に示すように、明示的なスタイルを使用します。

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

このスタイルは、 `ApplyLineColor` [`Entry`](xref:Xamarin.Forms.Entry)コントロール`LineColor`の`LineColorBehavior`アタッチされる動作のプロパティと添付プロパティを設定します。 スタイルについて詳しくは、[スタイル](~/xamarin-forms/user-interface/styles/index.md)に関する記事をご覧ください。

`ApplyLineColor`添付プロパティの値が設定または変更されると、アタッチ`LineColorBehavior`された動作`OnApplyLineColorChanged`によってメソッドが実行されます。次のコード例を参照してください。

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

このメソッドのパラメーターは、動作がアタッチされるコントロールのインスタンス、および`ApplyLineColor`添付プロパティの新旧の値を提供します。 `ApplyLineColor` 添付プロパティ`Effects`が[`Effects`](xref:Xamarin.Forms.Element.Effects) `EntryLineColorEffect` の場合、クラスはコントロールのコレクションに追加されます。それ以外の場合は、コントロールのコレクションから削除`true`されます。 動作の詳細については、「[動作の実装](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)」を参照してください。

[クラスの`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect)サブクラスは、次のコード例のようになります。 `EntryLineColorEffect`

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

クラス[`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect)は、プラットフォームに依存しない効果を表します。これは、プラットフォーム固有の内部効果をラップします。 これは、プラットフォーム固有の効果の型情報へのコンパイル時アクセスがないため、効果の削除プロセスを簡略化します。 は`EntryLineColorEffect` 、基本クラスのコンストラクターを呼び出し、解決グループ名の連結で構成されるパラメーターと、プラットフォーム固有の各効果クラスで指定される一意の ID を渡します。

次のコード例は、 `eShopOnContainers.EntryLineColorEffect` iOS の実装を示しています。

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

メソッド`OnAttached`は、Xamarin. Forms [`Entry`](xref:Xamarin.Forms.Entry)コントロールのネイティブコントロールを取得し、 `UpdateLineColor`メソッドを呼び出して行の色を更新します。 オーバーライド`OnElementPropertyChanged`は、添付プロパティが変更さ`LineColor`れ`Entry`た場合に線の色を更新することによって、 [`Height`](xref:Xamarin.Forms.VisualElement.Height)コントロールのバインド`Entry`可能なプロパティの変更に応答します。または、のプロパティを変更します。 エフェクトの詳細については、[エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)に関するページを参照してください。

[`Entry`](xref:Xamarin.Forms.Entry)コントロールに有効なデータが入力されると、コントロールの下部に黒い線が適用され、検証エラーがないことが示されます。 図6-3 は、この例を示しています。

![](validation-images/validation-blackline.png "検証エラーがないことを示す黒い線")

**図 6-3**:検証エラーがないことを示す黒い線

また、 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) [コントロールに`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers)は、コレクションに追加されたがあります。 [`Entry`](xref:Xamarin.Forms.Entry) 次のコード例は、 `DataTrigger`を示しています。

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

これ[`DataTrigger`](xref:Xamarin.Forms.DataTrigger)により`UserName.IsValid` 、プロパティが監視`LineColorBehavior`され、値`false`がになると[`Setter`](xref:Xamarin.Forms.Setter)、が実行さ`LineColor`れます。これにより、添付動作の添付プロパティが赤に変更されます。 図6-4 は、この例を示しています。

![](validation-images/validation-redline.png "検証エラーを示す赤い線")

**図 6-4**:検証エラーを示す赤い線

入力したデータ[`Entry`](xref:Xamarin.Forms.Entry)が無効な場合、コントロールの線は赤のままになります。それ以外の場合は、入力したデータが有効であることを示すために黒に変更されます。

トリガーの詳細については、「[トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

### <a name="displaying-error-messages"></a>エラーメッセージの表示

UI では、検証に失敗したデータを持つ各コントロールの下のラベルコントロールに検証エラーメッセージが表示されます。 次のコード例は、 [`Label`](xref:Xamarin.Forms.Label)ユーザーが有効なユーザー名を入力していない場合に検証エラーメッセージを表示するを示しています。

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

各[`Label`](xref:Xamarin.Forms.Label)は、検証`Errors`対象のビューモデルオブジェクトのプロパティにバインドされます。 プロパティは`ValidatableObject<T>`クラスによって提供され、型`List<string>`はです。 `Errors` プロパティに`Errors`は複数の検証エラーを含めること`FirstValidationErrorConverter`ができるため、インスタンスを使用して、表示するコレクションから最初のエラーを取得します。

## <a name="summary"></a>まとめ

EShopOnContainers モバイルアプリは、ビューモデルのプロパティの同期クライアント側検証を実行し、無効なデータが含まれているコントロールを強調表示し、ユーザーに通知するエラーメッセージを表示することによって、検証エラーをユーザーに通知します。データが無効である理由。

検証を必要とするモデルのプロパティを`ValidatableObject<T>`型とし`ValidatableObject<T>`て表示します。各インスタンス`Validations`には、そのプロパティに検証規則が追加されています。 検証`Validate`は、 `ValidatableObject<T>`インスタンスのメソッドを呼び出すことによって、ビューモデルから呼び出されます。このメソッドは、 `ValidatableObject<T>`検証規則を取得`Value`し、プロパティに対して実行します。 すべて`Errors`の検証エラーが`ValidatableObject<T>`インスタンス`IsValid`のプロパティに配置され、 `ValidatableObject<T>`インスタンスのプロパティが更新され、検証が成功したか失敗したかが示されます。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
