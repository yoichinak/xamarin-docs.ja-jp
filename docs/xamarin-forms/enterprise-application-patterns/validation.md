---
title: エンタープライズ アプリの検証
description: この章では、eShopOnContainers のモバイル アプリがユーザー入力の検証を実行する方法について説明します。 これには、検証規則を指定して、検証をトリガーする、検証エラーを表示するが含まれます。
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 22b5fe703486f0ded3a5b91241e3fe5ce41bbc98
ms.sourcegitcommit: a7170494e1975f0f1be547a45444752fd8e57819
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/27/2019
ms.locfileid: "58507098"
---
# <a name="validation-in-enterprise-apps"></a>エンタープライズ アプリの検証

ユーザーからの入力を受け取るすべてのアプリでは、入力が有効なことを確認してください。 アプリはできますが、たとえば、特定の範囲内の文字だけを含む、特定の長さが、または特定の形式に一致する入力に対して確認します。 検証なしでは、ユーザーは、アプリが失敗する原因となったデータを指定できます。 検証は、ビジネス ルールを強制し、攻撃者が悪意のあるデータを挿入することを防止します。

モデル-ビュー-ビューモデル (MVVM) のコンテキストでのパターンをビュー モデルまたはモデルがデータ検証を実行し、ユーザーは、それを解決できるように、ビューに検証エラーを通知する必要な多くの場合は。 EShopOnContainers のモバイル アプリがビュー モデルのプロパティの同期クライアント側の検証を実行しを無効なデータを格納しているコントロールを強調表示し、ユーザーに通知するエラー メッセージを表示することによって検証エラーのユーザーに通知します理由のデータが無効です。 図 6-1 は、eShopOnContainers のモバイル アプリで検証の実行に関連するクラスを示します。

[![](validation-images/validation.png "EShopOnContainers のモバイル アプリで検証クラス")](validation-images/validation-large.png#lightbox "eShopOnContainers のモバイル アプリで検証クラス")

**図 6-1**:EShopOnContainers のモバイル アプリで検証クラス

検証が必要なビュー モデルのプロパティが型`ValidatableObject<T>`、および各`ValidatableObject<T>`インスタンスに追加する検証規則にはその`Validations`プロパティ。 ビュー モデルから呼び出すことによって検証が呼び出される、`Validate`のメソッド、`ValidatableObject<T>`インスタンスで、検証を取得しますがルールし、それらに対して実行される、 `ValidatableObject<T>` `Value`プロパティ。 検証エラーが配置されます、`Errors`のプロパティ、`ValidatableObject<T>`インスタンス、および`IsValid`のプロパティ、`ValidatableObject<T>`インスタンスは、検証が成功したか失敗したかどうかを示すために更新されます。

プロパティの変更通知がによって提供される、`ExtendedBindableObject`クラスなど、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールにバインドできます、`IsValid`プロパティの`ValidatableObject<T>`かどうかを通知するビュー モデル クラスのインスタンス入力されたデータは有効です。

## <a name="specifying-validation-rules"></a>検証規則を指定します。

派生したクラスを作成して検証規則が指定されて、`IValidationRule<T>`インターフェイスで、次のコード例に示します。

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

このインターフェイスは、検証規則クラスを提供する必要がありますを指定します、 `boolean` `Check` 、必要な検証を実行するために使用するメソッドと`ValidationMessage`プロパティ値を持つ場合に表示される検証エラー メッセージ検証は失敗します。

次のコード例は、`IsNotNullOrEmptyRule<T>`検証規則は、ユーザー名とで、ユーザーが入力したパスワードの検証を実行するために使用、 `LoginView` eShopOnContainers のモバイル アプリでモック サービスを使用する場合。

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

`Check`メソッドを返します。 を`boolean`値引数は、かどうかを示す`null`、空の場合、または空白文字のみで構成されます。

EShopOnContainers のモバイル アプリで使用しないが、次のコード例は、電子メール アドレスを検証するための検証規則を示します。

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

`Check`メソッドを返します。 を`boolean`値の引数が有効な電子メール アドレスかどうかを示します。 これは、値の引数で指定された正規表現パターンの最初に出現する位置を検索することにより実現されます、`Regex`コンス トラクター。 値をチェックして、入力文字列で正規表現パターンが検出されたかどうかを決定できます、`Match`オブジェクトの`Success`プロパティ。

> [!NOTE]
> プロパティの検証では、依存プロパティは必要があることができます。 依存プロパティの例は、プロパティの有効な値のセットは、B. プロパティが設定されている特定の値に依存する場合A プロパティの値は、許可されている値のいずれかを確認するのには、B. プロパティの値を取得さらに、プロパティ B の値が変更されたときにプロパティ A が再検証が必要です。

## <a name="adding-validation-rules-to-a-property"></a>プロパティに検証規則の追加

型にする eShopOnContainers のモバイル アプリで検証が必要なビュー モデルのプロパティが宣言されている`ValidatableObject<T>`ここで、`T`を検証するデータの種類です。 次のコード例では、このような 2 つのプロパティの例を示します。

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

検証を行うには、検証規則を追加する必要があります、`Validations`の各コレクション`ValidatableObject<T>`の次のコード例に示すように、インスタンスします。

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

このメソッドを追加、`IsNotNullOrEmptyRule<T>`に検証規則、`Validations`の各コレクション`ValidatableObject<T>`検証規則の値を指定するインスタンス`ValidationMessage`プロパティで場合に、表示される検証エラー メッセージを指定します検証は失敗します。

## <a name="triggering-validation"></a>検証をトリガーします。

EShopOnContainers のモバイル アプリで使用される検証アプローチには、プロパティの検証と、自動的にトリガー検証手動でをトリガーできますプロパティが変更されたとき。

### <a name="triggering-validation-manually"></a>検証を手動でトリガー

ビュー モデルのプロパティの検証を手動でトリガーすることができます。 たとえば、これが発生します eShopOnContainers のモバイル アプリで、ユーザーがタップすると、**ログイン**のボタンでは、`LoginView`モック サービスを使用する場合。 コマンドのデリゲートの呼び出し、`MockSignInAsync`メソッドで、`LoginViewModel`を実行して検証を呼び出します`Validate`メソッドは、次のコード例に示されています。

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

`Validate`メソッドは、ユーザー名とで、ユーザーが入力したパスワードの検証を実行、 `LoginView`、それぞれの Validate メソッドを呼び出すことによって`ValidatableObject<T>`インスタンス。 次のコード例からの Validate メソッドを示しています、`ValidatableObject<T>`クラス。

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

このメソッドはクリア、 `Errors` 、コレクションのオブジェクトの追加されたルールのいずれかの検証を取得して`Validations`コレクション。 `Check`各取得した検証規則のメソッドを実行すると、および`ValidationMessage`には、データの検証に失敗する検証ルールのプロパティの値を追加、`Errors`のコレクション、`ValidatableObject<T>`インスタンス。 最後に、`IsValid`プロパティを設定して、検証が成功したか失敗したかどうかを示す、呼び出し元メソッドにその値が返されます。

### <a name="triggering-validation-when-properties-change"></a>プロパティを変更するときに検証をトリガーします。

バインドされたプロパティが変更されるたびに、検証をトリガーこともできます。 場合に双方向のバインドなど、`LoginView`設定、`UserName`または`Password`プロパティ、検証がトリガーされます。 次のコード例では、このしくみを示しています。

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

[ `Entry` ](xref:Xamarin.Forms.Entry)コントロールがバインド、`UserName.Value`のプロパティ、`ValidatableObject<T>`インスタンス、およびコントロールの`Behaviors`コレクションには、`EventToCommandBehavior`インスタンスを追加します。 この動作を実行、`ValidateUserNameCommand`への応答で、[`TextChanged`] イベントの発生、`Entry`を発生する状況内のテキスト、`Entry`変更。 さらに、`ValidateUserNameCommand`デリゲートの実行、`ValidateUserName`メソッドで、実行、`Validate`メソッドを`ValidatableObject<T>`インスタンス。 そのため、毎回ユーザーが入力内の文字、`Entry`ユーザー名、入力されたデータの検証コントロールを実行します。

動作の詳細については、[動作を実装する](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)を参照してください。

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>検証エラーを表示します。

EShopOnContainers のモバイル アプリを赤色の線で無効なデータを格納しているコントロールを強調表示して検証エラーのユーザーに通知し、なぜユーザーに通知するエラー メッセージを表示して、データが無効を含むコントロールの下、データが無効です。 無効なデータが修正される、行が黒に変更され、エラー メッセージが削除されます。 図 6-2 では、検証エラーが存在する場合に、eShopOnContainers のモバイル アプリで、LoginView を示します。

![](validation-images/validation-login.png "ログイン時に検証エラーを表示します。")

**図 6-2:** ログイン時に検証エラーを表示します。

### <a name="highlighting-a-control-that-contains-invalid-data"></a>無効なデータを格納しているコントロールを強調表示

`LineColorBehavior`接続されている動作を使用して、強調表示する[ `Entry` ](xref:Xamarin.Forms.Entry)検証エラーが発生したコントロール。 次のコード例に示す方法、`LineColorBehavior`に関連付けられた動作が接続されている、`Entry`コントロール。

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

[ `Entry` ](xref:Xamarin.Forms.Entry)コントロールは、明示的なスタイルを次のコード例に示したを消費します。

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

このスタイル設定、`ApplyLineColor`と`LineColor`添付プロパティの`LineColorBehavior`の動作をアタッチ、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロール。 スタイルについて詳しくは、[スタイル](~/xamarin-forms/user-interface/styles/index.md)に関する記事をご覧ください。

ときの値、`ApplyLineColor`添付プロパティは、セット、または、変更、`LineColorBehavior`添付の動作を実行、`OnApplyLineColorChanged`メソッドは、次のコード例に示されています。

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

このメソッドのパラメーターは、動作は、関連付けられているコントロールのインスタンスとの新旧の値を指定、`ApplyLineColor`添付プロパティ。 `EntryLineColorEffect`をコントロールのクラスを追加[ `Effects` ](xref:Xamarin.Forms.Element.Effects)コレクション場合、`ApplyLineColor`添付プロパティは`true`、それ以外の場合、コントロールから削除されます`Effects`コレクション。 動作の詳細については、[動作を実装する](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)を参照してください。

`EntryLineColorEffect`サブクラス、 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect)クラスし、次のコード例に示します。

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect)クラスは、プラットフォーム固有の内部の効果をラップするプラットフォームに依存しない効果を表します。 これは、プラットフォーム固有の効果の型情報へのコンパイル時アクセスがないため、効果の削除プロセスを簡略化します。 `EntryLineColorEffect`解像度のグループ名、および各プラットフォームに固有のエフェクト クラスで指定されている一意の ID の連結で構成されるパラメーターを渡して基底クラスのコンス トラクターを呼び出します。

次のコード例は、 `eShopOnContainers.EntryLineColorEffect` iOS 用の実装。

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

`OnAttached`メソッドは、Xamarin.Forms のネイティブ コントロールを取得[ `Entry` ](xref:Xamarin.Forms.Entry)制御、および線の色を呼び出すことによって更新、`UpdateLineColor`メソッド。 `OnElementPropertyChanged`上、上書きがバインド可能なプロパティの変更に応答、`Entry`場合は、線の色を更新することでコントロール、添付された`LineColor`プロパティの変更、または[ `Height` ](xref:Xamarin.Forms.VisualElement.Height) のプロパティ`Entry`変更します。 エフェクトの詳細については、[エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)に関するページを参照してください。

有効なデータが入力された場合、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロール、検証エラーがないことを示すために、コントロールの下部に黒の線が適用されます。 図 6-3 は、この例を示します。

![](validation-images/validation-blackline.png "黒の線の検証エラーがないことを示します。")

**図 6-3**:黒の線の検証エラーがないことを示します。

[ `Entry` ](xref:Xamarin.Forms.Entry)コントロールもあります、 [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger)に追加されたその[ `Triggers` ](xref:Xamarin.Forms.VisualElement.Triggers)コレクション。 次のコード例は、 `DataTrigger`:

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

これは、 [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger)モニター、`UserName.IsValid`プロパティが値の場合になります`false`、実行、 [ `Setter` ](xref:Xamarin.Forms.Setter)、変更、`LineColor`アタッチされています。プロパティ、`LineColorBehavior`赤の動作をアタッチします。 図 6-4 は、この例を示します。

![](validation-images/validation-redline.png "検証エラーを示す赤色の線")

**図 6-4**:検証エラーを示す赤色の線

内の行、 [ `Entry` ](xref:Xamarin.Forms.Entry)入力されたデータが有効な間に、コントロールが赤いまま、それ以外の場合、入力したデータが有効であることを示す黒に変更されます。

トリガーの詳細については、[トリガー](~/xamarin-forms/app-fundamentals/triggers.md)を参照してください。

### <a name="displaying-error-messages"></a>エラー メッセージを表示します。

UI では、データの検証に失敗しました、各コントロールの下にあるラベル コントロールで検証エラー メッセージが表示されます。 次のコード例は、 [ `Label` ](xref:Xamarin.Forms.Label)ユーザーが有効なユーザー名を入力していない場合、検証エラー メッセージを表示します。

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

各[ `Label` ](xref:Xamarin.Forms.Label)にバインドする、`Errors`検証されているビュー モデル オブジェクトのプロパティ。 `Errors`によって提供されるプロパティ、`ValidatableObject<T>`クラスと型`List<string>`します。 `Errors`プロパティは、複数の検証エラーを含めることができます、`FirstValidationErrorConverter`インスタンスを使用して、表示するためのコレクションから最初のエラーを取得します。

## <a name="summary"></a>まとめ

EShopOnContainers のモバイル アプリがビュー モデルのプロパティの同期クライアント側の検証を実行しを無効なデータを格納しているコントロールを強調表示し、ユーザーに通知するエラー メッセージを表示することによって検証エラーのユーザーに通知しますなぜ、データが無効です。

検証が必要なビュー モデルのプロパティが型`ValidatableObject<T>`、および各`ValidatableObject<T>`インスタンスに追加する検証規則にはその`Validations`プロパティ。 ビュー モデルから呼び出すことによって検証が呼び出される、`Validate`のメソッド、`ValidatableObject<T>`インスタンスで、検証を取得しますがルールし、それらに対して実行される、 `ValidatableObject<T>` `Value`プロパティ。 検証エラーが配置されます、`Errors`のプロパティ、`ValidatableObject<T>`インスタンス、および`IsValid`のプロパティ、`ValidatableObject<T>`インスタンスは、検証が成功したか失敗したかどうかを示すために更新されます。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
