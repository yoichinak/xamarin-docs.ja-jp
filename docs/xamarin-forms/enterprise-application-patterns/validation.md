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
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306393"
---
# <a name="validation-in-enterprise-apps"></a>エンタープライズアプリでの検証

ユーザーからの入力を受け入れるアプリであれば、入力が有効であることを確認する必要があります。 たとえば、特定の範囲の文字のみを含む入力、特定の長さ、または特定の形式に一致する入力を確認することができます。 検証を行わないと、ユーザーはアプリを失敗させるデータを提供できます。 検証では、ビジネスルールを適用し、攻撃者が悪意のあるデータを挿入するのを防ぎます。

モデルビュービューモデル (MVVM) パターンのコンテキストでは、データ検証を実行し、ユーザーが検証エラーを修正できるようにビューの検証エラーを通知するために、ビューモデルまたはモデルが必要になることがよくあります。 EShopOnContainers モバイルアプリは、ビューモデルのプロパティの同期クライアント側検証を実行し、無効なデータが含まれているコントロールを強調表示し、ユーザーに通知するエラーメッセージを表示することによって、検証エラーをユーザーに通知します。データが無効である理由。 図6-1 は、eShopOnContainers モバイルアプリで検証を実行するために必要なクラスを示しています。

[![](validation-images/validation.png "Validation classes in the eShopOnContainers mobile app")](validation-images/validation-large.png#lightbox "Validation classes in the eShopOnContainers mobile app")

**図 6-1**: eShopOnContainers モバイルアプリの検証クラス

検証を必要とするビューモデルプロパティは `ValidatableObject<T>`型で、各 `ValidatableObject<T>` インスタンスには `Validations` プロパティに追加された検証規則があります。 検証は、`ValidatableObject<T>` インスタンスの `Validate` メソッドを呼び出すことによって、ビューモデルから呼び出されます。このメソッドは検証規則を取得し、`ValidatableObject<T>` `Value` プロパティに対して実行します。 検証エラーは、`ValidatableObject<T>` インスタンスの `Errors` プロパティに配置され、`ValidatableObject<T>` インスタンスの `IsValid` プロパティが更新され、検証が成功したか失敗したかが示されます。

プロパティの変更通知は `ExtendedBindableObject` クラスによって提供されるため、 [`Entry`](xref:Xamarin.Forms.Entry)コントロールは、入力されたデータが有効かどうかを通知するために、ビューモデルクラスの `ValidatableObject<T>` インスタンスの `IsValid` プロパティにバインドできます。

## <a name="specifying-validation-rules"></a>検証規則の指定

検証規則は、次のコード例に示すように、`IValidationRule<T>` インターフェイスから派生するクラスを作成することによって指定します。

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

このインターフェイスは、検証規則クラスが、必要な検証を実行するために使用される `boolean` `Check` メソッドと、検証が失敗した場合に表示される検証エラーメッセージを値とする `ValidationMessage` プロパティを提供する必要があることを指定します。

次のコード例は、eShopOnContainers モバイルアプリでモックサービスを使用するときに、ユーザーが `LoginView` に入力したユーザー名とパスワードの検証を実行するために使用される `IsNotNullOrEmptyRule<T>` 検証規則を示しています。

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

`Check` メソッドは、値の引数が `null`、空、または空白文字だけで構成されているかどうかを示す `boolean` を返します。

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

`Check` メソッドは、値引数が有効な電子メールアドレスであるかどうかを示す `boolean` を返します。 これは、`Regex` コンストラクターで指定された正規表現パターンが最初に出現する値引数を検索することで実現されます。 入力文字列で正規表現パターンが見つかったかどうかを判断するには、`Match` オブジェクトの `Success` プロパティの値を確認します。

> [!NOTE]
> プロパティの検証には、依存プロパティが含まれる場合があります。 依存プロパティの例として、プロパティ A の有効な値のセットが、プロパティ B で設定されている特定の値に依存する場合があります。プロパティ A の値が、許可されている値のいずれかであることを確認するには、プロパティ B の値を取得する必要があります。さらに、プロパティ B の値が変更された場合は、プロパティ A を再検証する必要があります。

## <a name="adding-validation-rules-to-a-property"></a>プロパティへの検証規則の追加

EShopOnContainers モバイルアプリでは、検証を必要とするビューモデルプロパティが `ValidatableObject<T>`型として宣言されています。 `T` は検証対象のデータの型です。 次のコード例は、このような2つのプロパティの例を示しています。

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

検証を実行するには、次のコード例に示すように、各 `ValidatableObject<T>` インスタンスの `Validations` コレクションに検証規則を追加する必要があります。

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

このメソッドは、検証規則の `ValidationMessage` プロパティの値を指定して、各 `ValidatableObject<T>` インスタンスの `Validations` コレクションに `IsNotNullOrEmptyRule<T>` 検証規則を追加します。検証が失敗した場合に表示される検証エラーメッセージを指定します。

## <a name="triggering-validation"></a>検証のトリガー

EShopOnContainers モバイルアプリで使用される検証方法では、プロパティの検証を手動でトリガーし、プロパティが変更されたときに自動的に検証をトリガーできます。

### <a name="triggering-validation-manually"></a>手動による検証のトリガー

検証は、ビューモデルのプロパティに対して手動でトリガーできます。 たとえば、eShopOnContainers モバイルアプリでは、モックサービスを使用しているときに、ユーザーが `LoginView`の **[ログイン]** ボタンをタップしたときに、このような処理が行われます。 コマンドデリゲートは、`LoginViewModel`内の `MockSignInAsync` メソッドを呼び出します。このメソッドは、次のコード例に示すように、`Validate` メソッドを実行して検証を呼び出します。

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

`Validate` メソッドは、各 `ValidatableObject<T>` インスタンスで Validate メソッドを呼び出すことによって、`LoginView`にユーザーが入力したユーザー名とパスワードの検証を実行します。 次のコード例は、`ValidatableObject<T>` クラスの Validate メソッドを示しています。

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

このメソッドは `Errors` コレクションをクリアし、オブジェクトの `Validations` コレクションに追加されたすべての検証規則を取得します。 取得した検証規則ごとに `Check` メソッドが実行され、データの検証に失敗した検証規則の `ValidationMessage` プロパティ値が `ValidatableObject<T>` インスタンスの `Errors` コレクションに追加されます。 最後に、`IsValid` プロパティが設定され、その値が呼び出し元のメソッドに返され、検証が成功したか失敗したかが示されます。

### <a name="triggering-validation-when-properties-change"></a>プロパティが変更したときに検証をトリガーする

バインドされたプロパティが変更されるたびに、検証をトリガーすることもできます。 たとえば、`LoginView` の双方向のバインドが `UserName` または `Password` プロパティを設定すると、検証がトリガーされます。 この状況を示すコード例を次に示します。

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

[`Entry`](xref:Xamarin.Forms.Entry)コントロールは `ValidatableObject<T>` インスタンスの `UserName.Value` プロパティにバインドされます。コントロールの `Behaviors` コレクションには、`EventToCommandBehavior` インスタンスが追加されます。 この動作により、`Entry`での [`TextChanged`] イベントの発生に応じて `ValidateUserNameCommand` が実行されます。これは、`Entry` 内のテキストが変更されたときに発生します。 さらに、`ValidateUserNameCommand` デリゲートは、`ValidatableObject<T>` インスタンスで `Validate` メソッドを実行する `ValidateUserName` メソッドを実行します。 そのため、ユーザーがユーザー名の `Entry` コントロールに文字を入力するたびに、入力されたデータの検証が実行されます。

動作の詳細については、「[動作の実装](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)」を参照してください。

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>検証エラーの表示

EShopOnContainers モバイルアプリは、無効なデータが含まれているコントロールを赤色の線で強調表示し、ユーザーに対して、データが無効であることをユーザーに通知するエラーメッセージを表示することによって、検証エラーをユーザーに通知します。データが無効です。 無効なデータが修正されると、行が黒に変わり、エラーメッセージが削除されます。 図6-2 に、検証エラーが存在する場合の eShopOnContainers mobile アプリの LoginView を示します。

![](validation-images/validation-login.png "Displaying validation errors during login")

**図 6-2:** ログイン時の検証エラーの表示

### <a name="highlighting-a-control-that-contains-invalid-data"></a>無効なデータが含まれているコントロールの強調表示

`LineColorBehavior` アタッチされる動作は、検証エラーが発生した[`Entry`](xref:Xamarin.Forms.Entry)コントロールを強調表示するために使用されます。 次のコード例は、`LineColorBehavior` のアタッチ動作を `Entry` コントロールにアタッチする方法を示しています。

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

[`Entry`](xref:Xamarin.Forms.Entry)コントロールは、次のコード例に示すように、明示的なスタイルを使用します。

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

このスタイルでは、 [`Entry`](xref:Xamarin.Forms.Entry)コントロールの `LineColorBehavior` アタッチされる動作の `ApplyLineColor` および `LineColor` 添付プロパティを設定します。 スタイルについて詳しくは、「[Styles](~/xamarin-forms/user-interface/styles/index.md)」(スタイル) をご覧ください。

`ApplyLineColor` 添付プロパティの値が設定または変更されると、`LineColorBehavior` のアタッチされた動作によって `OnApplyLineColorChanged` メソッドが実行されます。次のコード例を参照してください。

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

このメソッドのパラメーターは、動作がアタッチされるコントロールのインスタンス、および `ApplyLineColor` 添付プロパティの新旧の値を提供します。 `ApplyLineColor` 添付プロパティが `true`されている場合は、コントロールの[`Effects`](xref:Xamarin.Forms.Element.Effects)コレクションに `EntryLineColorEffect` クラスが追加されます。それ以外の場合は、コントロールの `Effects` コレクションから削除されます。 動作の詳細については、「[動作の実装](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)」を参照してください。

`EntryLineColorEffect` サブクラス[`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect)クラスを次のコード例に示します。

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect)クラスは、プラットフォームに依存しない効果を表します。これは、プラットフォーム固有の内部効果をラップします。 これは、プラットフォーム固有のエフェクトの型情報へのコンパイル時アクセスがないため、エフェクトの削除プロセスを簡略化します。 `EntryLineColorEffect` は、基本クラスのコンストラクターを呼び出し、解決グループ名の連結で構成されるパラメーターと、プラットフォーム固有の各効果クラスで指定される一意の ID を渡します。

次のコード例は、iOS の `eShopOnContainers.EntryLineColorEffect` の実装を示しています。

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

`OnAttached` メソッドは、Xamarin. Forms [`Entry`](xref:Xamarin.Forms.Entry)コントロールのネイティブコントロールを取得し、`UpdateLineColor` メソッドを呼び出して行の色を更新します。 `OnElementPropertyChanged` オーバーライドは、`Entry` コントロールのバインド可能なプロパティの変更に応答します。添付 `LineColor` プロパティが変更された場合、または `Entry` の[`Height`](xref:Xamarin.Forms.VisualElement.Height)プロパティが変更された場合は、線の色を更新します。 エフェクトの詳細については、[エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)に関するページを参照してください。

有効なデータが[`Entry`](xref:Xamarin.Forms.Entry)コントロールに入力されると、コントロールの下部に黒い線が適用され、検証エラーがないことが示されます。 図6-3 は、この例を示しています。

![](validation-images/validation-blackline.png "Black line indicating no validation error")

**図 6-3**: 検証エラーがないことを示す黒い線

[`Entry`](xref:Xamarin.Forms.Entry)コントロールには、その[`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers)コレクションにも[`DataTrigger`](xref:Xamarin.Forms.DataTrigger)が追加されています。 `DataTrigger`を次のコード例に示します。

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

この[`DataTrigger`](xref:Xamarin.Forms.DataTrigger)は `UserName.IsValid` プロパティを監視し、値が `false`になると、 [`Setter`](xref:Xamarin.Forms.Setter)を実行して、`LineColor` のアタッチされた動作の `LineColorBehavior` 添付プロパティを赤に変更します。 図6-4 は、この例を示しています。

![](validation-images/validation-redline.png "Red line indicating validation error")

**図 6-4**: 検証エラーを示す赤い線

入力したデータが無効な場合、 [`Entry`](xref:Xamarin.Forms.Entry)コントロールの線は赤のままになります。それ以外の場合は、入力したデータが有効であることを示すために黒に変更されます。

トリガーの詳細については、「[トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

### <a name="displaying-error-messages"></a>エラーメッセージの表示

UI では、検証に失敗したデータを持つ各コントロールの下のラベルコントロールに検証エラーメッセージが表示されます。 次のコード例は、ユーザーが有効なユーザー名を入力していない場合に検証エラーメッセージを表示する[`Label`](xref:Xamarin.Forms.Label)を示しています。

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

各[`Label`](xref:Xamarin.Forms.Label)は、検証対象のビューモデルオブジェクトの `Errors` プロパティにバインドされます。 `Errors` プロパティは、`ValidatableObject<T>` クラスによって提供され、`List<string>`型です。 `Errors` プロパティには複数の検証エラーを含めることができるため、`FirstValidationErrorConverter` インスタンスを使用して、コレクションから表示する最初のエラーを取得します。

## <a name="summary"></a>要約

EShopOnContainers モバイルアプリは、ビューモデルのプロパティの同期クライアント側検証を実行し、無効なデータが含まれているコントロールを強調表示し、ユーザーに通知するエラーメッセージを表示することによって、検証エラーをユーザーに通知します。データが無効である理由。

検証を必要とするビューモデルプロパティは `ValidatableObject<T>`型で、各 `ValidatableObject<T>` インスタンスには `Validations` プロパティに追加された検証規則があります。 検証は、`ValidatableObject<T>` インスタンスの `Validate` メソッドを呼び出すことによって、ビューモデルから呼び出されます。このメソッドは検証規則を取得し、`ValidatableObject<T>` `Value` プロパティに対して実行します。 検証エラーは、`ValidatableObject<T>`インスタンスの `Errors` プロパティに配置され、`ValidatableObject<T>` インスタンスの `IsValid` プロパティが更新され、検証が成功したか失敗したかが示されます。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
