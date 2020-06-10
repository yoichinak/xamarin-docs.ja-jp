---
title: "エンタープライズアプリでの検証" の説明: "この章では、eShopOnContainers モバイルアプリがユーザー入力の検証を実行する方法について説明します。 これには、検証規則の指定、検証のトリガー、検証エラーの表示などが含まれます。
ms. 製品: xamarin ms. assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 08/07/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="validation-in-enterprise-apps"></a>エンタープライズアプリでの検証

ユーザーからの入力を受け入れるアプリであれば、入力が有効であることを確認する必要があります。 たとえば、特定の範囲の文字のみを含む入力、特定の長さ、または特定の形式に一致する入力を確認することができます。 検証を行わないと、ユーザーはアプリを失敗させるデータを提供できます。 検証では、ビジネスルールを適用し、攻撃者が悪意のあるデータを挿入するのを防ぎます。

モデルビュービューモデル (MVVM) パターンのコンテキストでは、データ検証を実行し、ユーザーが検証エラーを修正できるようにビューの検証エラーを通知するために、ビューモデルまたはモデルが必要になることがよくあります。 EShopOnContainers モバイルアプリは、ビューモデルのプロパティの同期クライアント側検証を実行し、無効なデータが含まれているコントロールを強調表示し、データが無効である理由をユーザーに通知するエラーメッセージを表示することによって、検証エラーをユーザーに通知します。 図6-1 は、eShopOnContainers モバイルアプリで検証を実行するために必要なクラスを示しています。

[![](validation-images/validation.png "Validation classes in the eShopOnContainers mobile app")](validation-images/validation-large.png#lightbox "Validation classes in the eShopOnContainers mobile app")

**図 6-1**: eShopOnContainers モバイルアプリの検証クラス

検証を必要とするモデルのプロパティを型 `ValidatableObject<T>` として表示し `ValidatableObject<T>` ます。各インスタンスには、そのプロパティに検証規則が追加されてい `Validations` ます。 検証は、インスタンスのメソッドを呼び出すことによって、ビューモデルから呼び出され `Validate` ます。このメソッドは、 `ValidatableObject<T>` 検証規則を取得し、プロパティに対して実行し `ValidatableObject<T>` `Value` ます。 すべての検証エラーがインスタンスのプロパティに配置され、 `Errors` `ValidatableObject<T>` `IsValid` インスタンスのプロパティが更新され、 `ValidatableObject<T>` 検証が成功したか失敗したかが示されます。

プロパティの変更通知はクラスによって提供されるため、コントロールは、入力され `ExtendedBindableObject` [`Entry`](xref:Xamarin.Forms.Entry) `IsValid` `ValidatableObject<T>` たデータが有効かどうかを通知するために、ビューモデルクラスのインスタンスのプロパティにバインドできます。

## <a name="specifying-validation-rules"></a>検証規則の指定

検証規則は、 `IValidationRule<T>` 次のコード例に示すように、インターフェイスから派生するクラスを作成することによって指定します。

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

このインターフェイスは、検証規則クラスが、 `boolean` `Check` 必要な検証を実行するために使用されるメソッドと、 `ValidationMessage` 検証が失敗した場合に表示される検証エラーメッセージを値として持つプロパティを提供する必要があることを指定します。

次のコード例は、 `IsNotNullOrEmptyRule<T>` `LoginView` eShopOnContainers モバイルアプリでモックサービスを使用するときに、でユーザーが入力したユーザー名とパスワードの検証を実行するために使用される検証規則を示しています。

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

メソッドは、 `Check` `boolean` 値の引数がであるか、空であるか、 `null` または空白文字のみで構成されているかを示すを返します。

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

メソッドは、 `Check` `boolean` 値引数が有効な電子メールアドレスであるかどうかを示すを返します。 これは、コンストラクターで指定された正規表現パターンが最初に出現する値引数を検索することで実現され `Regex` ます。 入力文字列で正規表現パターンが見つかったかどうかは、オブジェクトのプロパティの値をチェックすることによって判断でき `Match` `Success` ます。

> [!NOTE]
> プロパティの検証には、依存プロパティが含まれる場合があります。 依存プロパティの例として、プロパティ A の有効な値のセットが、プロパティ B で設定されている特定の値に依存する場合があります。プロパティ A の値が、許可されている値のいずれかであることを確認するには、プロパティ B の値を取得する必要があります。さらに、プロパティ B の値が変更された場合は、プロパティ A を再検証する必要があります。

## <a name="adding-validation-rules-to-a-property"></a>プロパティへの検証規則の追加

EShopOnContainers モバイルアプリでは、検証を必要とするビューモデルプロパティが型として宣言され `ValidatableObject<T>` ます。ここで、 `T` は検証対象のデータの型です。 次のコード例は、このような2つのプロパティの例を示しています。

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

検証を実行するには、 `Validations` `ValidatableObject<T>` 次のコード例に示すように、各インスタンスのコレクションに検証規則を追加する必要があります。

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

このメソッドは、検証規則の `IsNotNullOrEmptyRule<T>` プロパティの値を指定して、各インスタンスのコレクションに検証規則を追加し、検証に `Validations` `ValidatableObject<T>` 失敗した場合に表示される `ValidationMessage` 検証エラーメッセージを指定します。

## <a name="triggering-validation"></a>検証のトリガー

EShopOnContainers モバイルアプリで使用される検証方法では、プロパティの検証を手動でトリガーし、プロパティが変更されたときに自動的に検証をトリガーできます。

### <a name="triggering-validation-manually"></a>手動による検証のトリガー

検証は、ビューモデルのプロパティに対して手動でトリガーできます。 たとえば、eShopOnContainers モバイルアプリでは、 **Login** `LoginView` モックサービスを使用しているときに、ユーザーがの [ログイン] ボタンをタップしたときに発生します。 コマンドデリゲートは、のメソッドを呼び出します。このメソッドは、 `MockSignInAsync` `LoginViewModel` 次の `Validate` コード例に示すように、メソッドを実行して検証を呼び出します。

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

メソッドは、 `Validate` `LoginView` 各インスタンスで Validate メソッドを呼び出すことによって、にユーザーが入力したユーザー名とパスワードの検証を実行し `ValidatableObject<T>` ます。 次のコード例は、クラスの Validate メソッドを示してい `ValidatableObject<T>` ます。

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

このメソッドは、コレクションをクリアし、 `Errors` オブジェクトのコレクションに追加されたすべての検証規則を取得し `Validations` ます。 `Check`取得した検証規則のメソッドが実行され、 `ValidationMessage` データの検証に失敗した検証規則のプロパティ値がインスタンスのコレクションに追加され `Errors` `ValidatableObject<T>` ます。 最後に、 `IsValid` プロパティが設定され、その値が呼び出し元のメソッドに返され、検証が成功したか失敗したかが示されます。

### <a name="triggering-validation-when-properties-change"></a>プロパティが変更したときに検証をトリガーする

バインドされたプロパティが変更されるたびに、検証をトリガーすることもできます。 たとえば、内の双方向のバインディングで `LoginView` またはプロパティを設定すると、 `UserName` `Password` 検証がトリガーされます。 この状況を示すコード例を次に示します。

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

[`Entry`](xref:Xamarin.Forms.Entry)コントロールが `UserName.Value` インスタンスのプロパティにバインドされ、 `ValidatableObject<T>` コントロールのコレクションに `Behaviors` インスタンスが `EventToCommandBehavior` 追加されます。 この動作は、の [] イベントの発生に応答してを実行し `ValidateUserNameCommand` `TextChanged` `Entry` ます。これは、のテキストが変更されたときに発生し `Entry` ます。 その後、 `ValidateUserNameCommand` デリゲートは `ValidateUserName` メソッドを実行し、 `Validate` インスタンスでメソッドを実行し `ValidatableObject<T>` ます。 したがって、ユーザーがユーザー名のコントロールに文字を入力するたびに、 `Entry` 入力されたデータの検証が行われます。

動作の詳細については、「[動作の実装](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing-behaviors)」を参照してください。

## <a name="displaying-validation-errors"></a>検証エラーの表示

EShopOnContainers モバイルアプリは、無効なデータが含まれているコントロールを赤い線で強調表示し、無効なデータを含むコントロールの下にデータが無効であることをユーザーに通知するエラーメッセージを表示することによって、検証エラーをユーザーに通知します。 無効なデータが修正されると、行が黒に変わり、エラーメッセージが削除されます。 図6-2 に、検証エラーが存在する場合の eShopOnContainers mobile アプリの LoginView を示します。

![](validation-images/validation-login.png "Displaying validation errors during login")

**図 6-2:** ログイン時の検証エラーの表示

### <a name="highlighting-a-control-that-contains-invalid-data"></a>無効なデータが含まれているコントロールの強調表示

`LineColorBehavior`接続動作は、検証エラーが発生したコントロールを強調表示するために使用され [`Entry`](xref:Xamarin.Forms.Entry) ます。 次のコード例は、アタッチされた `LineColorBehavior` 動作をコントロールにアタッチする方法を示してい `Entry` ます。

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

コントロールは、 [`Entry`](xref:Xamarin.Forms.Entry) 次のコード例に示すように、明示的なスタイルを使用します。

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

このスタイルは、 `ApplyLineColor` `LineColor` コントロールのアタッチされる動作のプロパティと添付プロパティを設定し `LineColorBehavior` [`Entry`](xref:Xamarin.Forms.Entry) ます。 スタイルについて詳しくは、[スタイル](~/xamarin-forms/user-interface/styles/index.md)に関する記事をご覧ください。

`ApplyLineColor`添付プロパティの値が設定または変更されると、アタッチされた動作によってメソッドが実行され `LineColorBehavior` `OnApplyLineColorChanged` ます。次のコード例を参照してください。

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

このメソッドのパラメーターは、動作がアタッチされるコントロールのインスタンス、および添付プロパティの新旧の値を提供し `ApplyLineColor` ます。 `EntryLineColorEffect`添付プロパティがの場合、クラスはコントロールのコレクションに追加され [`Effects`](xref:Xamarin.Forms.Element.Effects) `ApplyLineColor` ます。それ以外の場合は、 `true` コントロールのコレクションから削除され `Effects` ます。 動作の詳細については、「[動作の実装](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing-behaviors)」を参照してください。

クラスのサブクラスは、 `EntryLineColorEffect` [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) 次のコード例のようになります。

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

クラスは、プラットフォームに [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) 依存しない効果を表します。これは、プラットフォーム固有の内部効果をラップします。 これは、プラットフォーム固有のエフェクトの型情報へのコンパイル時アクセスがないため、エフェクトの削除プロセスを簡略化します。 は、 `EntryLineColorEffect` 基本クラスのコンストラクターを呼び出し、解決グループ名の連結で構成されるパラメーターと、プラットフォーム固有の各効果クラスで指定される一意の ID を渡します。

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

メソッドは、 `OnAttached` コントロールのネイティブコントロールを取得 Xamarin.Forms [`Entry`](xref:Xamarin.Forms.Entry) し、メソッドを呼び出して行の色を更新し `UpdateLineColor` ます。 オーバーライドは、 `OnElementPropertyChanged` `Entry` 添付プロパティが変更された場合に線の色を更新することによって、コントロールのバインド可能なプロパティの変更に応答し `LineColor` [`Height`](xref:Xamarin.Forms.VisualElement.Height) ます。または、のプロパティを変更 `Entry` します。 エフェクトの詳細については、[エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)に関するページを参照してください。

コントロールに有効なデータが入力されると、 [`Entry`](xref:Xamarin.Forms.Entry) コントロールの下部に黒い線が適用され、検証エラーがないことが示されます。 図6-3 は、この例を示しています。

![](validation-images/validation-blackline.png "Black line indicating no validation error")

**図 6-3**: 検証エラーがないことを示す黒い線

また、コントロールには [`Entry`](xref:Xamarin.Forms.Entry) 、 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) コレクションに追加されたがあり [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) ます。 次のコード例は、を示してい `DataTrigger` ます。

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

これにより、 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) プロパティが監視され、値がになると、が実行されます。これにより、 `UserName.IsValid` `false` [`Setter`](xref:Xamarin.Forms.Setter) `LineColor` 添付動作の添付プロパティが赤に変更され `LineColorBehavior` ます。 図6-4 は、この例を示しています。

![](validation-images/validation-redline.png "Red line indicating validation error")

**図 6-4**: 検証エラーを示す赤い線

[`Entry`](xref:Xamarin.Forms.Entry)入力したデータが無効な場合、コントロールの線は赤のままになります。それ以外の場合は、入力したデータが有効であることを示すために黒に変更されます。

トリガーの詳細については、「[トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

### <a name="displaying-error-messages"></a>エラーメッセージの表示

UI では、検証に失敗したデータを持つ各コントロールの下のラベルコントロールに検証エラーメッセージが表示されます。 次のコード例は、 [`Label`](xref:Xamarin.Forms.Label) ユーザーが有効なユーザー名を入力していない場合に検証エラーメッセージを表示するを示しています。

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

各は [`Label`](xref:Xamarin.Forms.Label) `Errors` 、検証対象のビューモデルオブジェクトのプロパティにバインドされます。 `Errors`プロパティはクラスによって提供され、 `ValidatableObject<T>` 型は `List<string>` です。 プロパティには `Errors` 複数の検証エラーを含めることができるため、インスタンスを使用して、 `FirstValidationErrorConverter` 表示するコレクションから最初のエラーを取得します。

## <a name="summary"></a>まとめ

EShopOnContainers モバイルアプリは、ビューモデルのプロパティの同期クライアント側検証を実行し、無効なデータが含まれているコントロールを強調表示し、データが無効であることをユーザーに通知するエラーメッセージを表示することによって、検証エラーをユーザーに通知します。

検証を必要とするモデルのプロパティを型 `ValidatableObject<T>` として表示し `ValidatableObject<T>` ます。各インスタンスには、そのプロパティに検証規則が追加されてい `Validations` ます。 検証は、インスタンスのメソッドを呼び出すことによって、ビューモデルから呼び出され `Validate` ます。このメソッドは、 `ValidatableObject<T>` 検証規則を取得し、プロパティに対して実行し `ValidatableObject<T>` `Value` ます。 すべての検証エラーがインスタンスのプロパティに配置され、 `Errors` `ValidatableObject<T>` `IsValid` インスタンスのプロパティが更新され、 `ValidatableObject<T>` 検証が成功したか失敗したかが示されます。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
