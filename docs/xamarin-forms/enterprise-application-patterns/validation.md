---
title: 検証
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 80c78d359761c4383f9abf9338a995e3cc486968
ms.sourcegitcommit: a69439ad4c9fd0abe759143687d3b23582573d90
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/28/2018
---
# <a name="validation"></a>検証

すべてのアプリをユーザーからの入力を受け付けるは、入力が有効であることを確認する必要があります。 アプリは、たとえば、特定の範囲内の文字だけを含む、特定の長さがまたは特定の形式に一致する入力を確認します。 検証なしで、ユーザーは、アプリが失敗する原因となるデータを指定できます。 検証は、ビジネス ルールの適用し、攻撃者が悪意のあるデータを挿入するを防ぎます。

コンテキストの ViewModel モデル (MVVM) のビュー モデルのパターンまたはモデルがデータ検証を行い、ユーザーが修正できるように、ビューに検証エラーを通知する必要多くの場合があります。 EShopOnContainers モバイル アプリは、モデルのプロパティの表示の同期のクライアント側検証を実行して、ユーザーに通知するエラー メッセージを表示したりして、無効なデータを格納しているコントロールが強調表示され、検証エラーのユーザーに通知理由のデータが正しくありません。 図 6-1 は、eShopOnContainers モバイル アプリでの検証の実行に関連するクラスを示しています。

[![](validation-images/validation.png "検証クラス eShopOnContainers モバイル アプリで")](validation-images/validation-large.png#lightbox "eShopOnContainers モバイル アプリで検証クラス")

**図 6-1**: eShopOnContainers モバイル アプリで検証クラス

型の検証が必要なモデル プロパティの表示は、 `ValidatableObject<T>`、および各`ValidatableObject<T>`インスタンスに追加の検証規則にはその`Validations`プロパティです。 ビュー モデルから呼び出すことによって検証が呼び出される、`Validate`のメソッド、`ValidatableObject<T>`検証を取得するには、インスタンスがルールし、それらに対して実行、 `ValidatableObject<T>` `Value`プロパティです。 検証エラーを配置している、`Errors`のプロパティ、`ValidatableObject<T>`インスタンス、および`IsValid`のプロパティ、`ValidatableObject<T>`インスタンスは、検証が成功したか失敗したかどうかを示すために更新されます。

プロパティの変更通知がによって提供される、`ExtendedBindableObject`クラス、しているため、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールにバインドできます、`IsValid`プロパティ`ValidatableObject<T>`通知を受けるかどうかのビュー モデル クラスのインスタンス入力されたデータは有効です。

## <a name="specifying-validation-rules"></a>検証規則を指定します。

派生するクラスを作成することで検証規則が指定されて、`IValidationRule<T>`インターフェイスで、次のコード例に示します。

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

このインターフェイスは、検証規則クラスを提供する必要がありますを指定します、 `boolean` `Check`必要な検証を実行するために使用するメソッドと`ValidationMessage`プロパティの値がある場合に表示される検証エラー メッセージ検証は失敗します。

次のコード例は、`IsNotNullOrEmptyRule<T>`ユーザー名とパスワード ユーザーが入力の検証を実行に使用される検証規則、 `LoginView` eShopOnContainers モバイル アプリでモック サービスを使用する場合。

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

`Check`メソッドを返します、`boolean`値引数があるかどうかを示す`null`、空の場合、または空白文字のみで構成されます。

EShopOnContainers モバイル アプリでも使用できません、次のコード例は、電子メール アドレスを検証するための検証規則を示します。

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

`Check`メソッドを返します、`boolean`値引数が有効な電子メール アドレスかどうかを示すです。 Value 引数で指定された正規表現パターンの最初に出現する位置を検索することでこれは、`Regex`コンス トラクターです。 値をチェックして、入力文字列で正規表現パターンが検出されてかどうかを決定できます、`Match`オブジェクトの`Success`プロパティです。

> [!NOTE]
> プロパティの検証は、依存関係プロパティを必要があることができます。 依存関係プロパティの例は、プロパティの A の有効な値のセットが B. プロパティに設定されている特定の値に依存する場合A プロパティの値が許可される値のいずれかのことを確認するには、は、B. プロパティの値を取得します。さらに、プロパティ B の値が変更されたときにプロパティ A とする必要がありますを再検証します。

## <a name="adding-validation-rules-to-a-property"></a>プロパティに検証規則の追加

型にする、eShopOnContainers モバイル アプリで、検証が必要なモデル プロパティの表示が宣言されている`ValidatableObject<T>`ここで、`T`検証に使用するデータの種類です。 次のコード例では、このような 2 つのプロパティの例を示します。

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

検証を行うには、検証ルールを追加する必要があります、`Validations`の各コレクション`ValidatableObject<T>`インスタンス、次のコード例で示したようにします。

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

このメソッドは、追加、`IsNotNullOrEmptyRule<T>`する検証規則、`Validations`の各コレクション`ValidatableObject<T>`検証規則の値を指定して、インスタンス`ValidationMessage`プロパティを場合に表示される検証エラー メッセージを指定します。検証は失敗します。

## <a name="triggering-validation"></a>検証をトリガーします。

EShopOnContainers モバイル アプリで使用される検証方法には、プロパティの検証と自動的にトリガー検証手動でをトリガーできますプロパティが変更されたとき。

### <a name="triggering-validation-manually"></a>検証を手動でトリガーします。

検証は、ビュー モデルのプロパティを手動でトリガーされたことができます。 たとえば、これが発生した eShopOnContainers モバイル アプリで、ユーザーがタップすると、**ログイン**のボタンでは、`LoginView`モック サービスを使用する場合、します。 コマンドのデリゲートの呼び出し、`MockSignInAsync`メソッドで、 `LoginViewModel`、実行することによって検証が呼び出されます`Validate`メソッドは、次のコード例に示されています。

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

`Validate`メソッド名とパスワード ユーザーが入力の検証を行い、 `LoginView`、それぞれの Validate メソッドを呼び出すことによって`ValidatableObject<T>`インスタンス。 次のコード例から検証メソッドを示しています、`ValidatableObject<T>`クラス。

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

このメソッドは、クリア、`Errors`コレクション、および、任意の検証規則は、オブジェクトの追加された取得`Validations`コレクション。 `Check`それぞれ取得された検証規則のメソッドを実行すると、および`ValidationMessage`には、データの検証に失敗する検証ルールのプロパティの値を追加、`Errors`のコレクション、`ValidatableObject<T>`インスタンス。 最後に、`IsValid`プロパティを設定して、検証が成功したか失敗したかどうかを示す、呼び出し元メソッドにその値が返されます。

### <a name="triggering-validation-when-properties-change"></a>プロパティを変更するときに検証をトリガーします。

バインドされたプロパティが変更されるたびに、検証をトリガーもできます。 たとえば、双方向のバインドで、`LoginView`設定、`UserName`または`Password`プロパティ、検証が発生します。 次のコード例では、このしくみを示しています。

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールのバインド先、`UserName.Value`のプロパティ、`ValidatableObject<T>`インスタンス、およびコントロールの`Behaviors`コレクションには、`EventToCommandBehavior`インスタンスを追加します。 この動作を実行、`ValidateUserNameCommand`への応答で、[`TextChanged`] イベントで起動、 `Entry`、どの場合に発生するが内のテキスト、`Entry`変更します。 さらに、`ValidateUserNameCommand`デリゲートは実行、`ValidateUserName`メソッドで、実行、`Validate`メソッドを`ValidatableObject<T>`インスタンス。 したがって、毎回ユーザーが入力内の文字、`Entry`ユーザー名、入力されたデータの検証コントロールを実行します。

動作に関する詳細については、次を参照してください。[を実装する動作](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)です。

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>検証エラーを表示します。

EShopOnContainers モバイル アプリを赤色の線を使用して無効なデータを含むコントロールが強調表示され、検証エラーのユーザーに通知し、理由、ユーザーに通知するエラー メッセージを表示することによって、データが無効を含むコントロールの下、無効なデータ。 無効なデータが修正されると、行が黒に変更され、エラー メッセージが削除されます。 図 6-2 では、検証エラーが存在する場合に、eShopOnContainers モバイル アプリで、LoginView を示します。

![](validation-images/validation-login.png "ログイン時に検証エラーを表示します。")

**図 6-2:** ログイン時に検証エラーを表示します。

### <a name="highlighting-a-control-that-contains-invalid-data"></a>無効なデータを含むコントロールを強調表示

`LineColorBehavior`接続されている動作を使用して、強調表示する[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールの検証エラーが発生しました。 次のコード例に示す方法、`LineColorBehavior`に関連付けられた動作が接続されている、`Entry`コントロール。

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

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールは、次のコード例に示されている明示的なスタイルを消費します。

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

このスタイルを設定、`ApplyLineColor`と`LineColor`添付プロパティの`LineColorBehavior`の動作をアタッチ、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロール。 スタイルの詳細については、次を参照してください。[スタイル](~/xamarin-forms/user-interface/styles/index.md)です。

ときの値、`ApplyLineColor`添付プロパティは、セット、または、変更、`LineColorBehavior`添付の動作を実行、`OnApplyLineColorChanged`メソッドは、次のコード例に示されています。

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

このメソッドのパラメーターを指定の動作は、関連付けられているコントロールのインスタンスと、古い値と新しい値の`ApplyLineColor`添付プロパティ。 `EntryLineColorEffect`をコントロールのクラスを追加[ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクション場合、`ApplyLineColor`添付プロパティは`true`、それ以外の場合、コントロールから削除されます`Effects`コレクション。 動作に関する詳細については、次を参照してください。[を実装する動作](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)です。

`EntryLineColorEffect`サブクラス、 [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)クラス、し、次のコード例に示します。

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)クラスは、プラットフォーム固有の内部の効果をラップするプラットフォームに依存しない効果を表します。 プラットフォーム固有の効果の種類の情報へのコンパイル時アクセスは存在しないために、効果削除プロセスが簡略化します。 `EntryLineColorEffect`解像度グループ名、および各プラットフォームに固有の効果クラスで指定されている一意の ID の連結で構成されるパラメーターを渡して、基底クラス コンス トラクターを呼び出します。

次のコード例は、 `eShopOnContainers.EntryLineColorEffect` iOS 用の実装。

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

`OnAttached`メソッドは、Xamarin.Forms のネイティブ コントロールを取得[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)制御、および線の色を呼び出すことによって更新プログラム、`UpdateLineColor`メソッドです。 `OnElementPropertyChanged`上の上書きがバインド可能なプロパティの変更に応答、`Entry`場合は、線の色を更新することによってコントロール、添付された`LineColor`プロパティの変更、または[ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) のプロパティ`Entry`変更します。 影響の詳細については、次を参照してください。[効果](~/xamarin-forms/app-fundamentals/effects/index.md)です。

有効なデータが入力されている場合、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールに適用されます黒の線を妥当性確認エラーがないことを示すために、コントロールの下部にします。 図 6-3 では、この例を示します。

![](validation-images/validation-blackline.png "黒い線の妥当性確認エラーがないことを示します。")

**図 6-3**: 黒の線が検証エラーがないことを示します。

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールもあります、 [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/)に追加されたその[ `Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/)コレクション。 次のコード例は、 `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

これは、 [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/)モニター、`UserName.IsValid`プロパティの値である場合は`false`を実行して、 [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)、変更、`LineColor`添付プロパティ、`LineColorBehavior`赤に動作をアタッチします。 図 6-4 では、この例を示します。

![](validation-images/validation-redline.png "検証エラーを示す赤い線")

**図 6-4**: 検証エラーを示す赤い線

内の行、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)入力されたデータが無効なは、コントロールが赤い残ります、入力されたデータが無効であることを示す黒に変更は、それ以外の場合。

トリガーの詳細については、次を参照してください。[トリガー](~/xamarin-forms/app-fundamentals/triggers.md)です。

### <a name="displaying-error-messages"></a>エラー メッセージを表示します。

UI では、データの検証に失敗しました各コントロールの下のラベル コントロールに検証エラー メッセージが表示されます。 次のコード例は、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)ユーザーが有効なユーザー名を入力していない場合、検証エラー メッセージを表示します。

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

各[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)にバインドされて、`Errors`検証されているビュー モデル オブジェクトのプロパティです。 `Errors`によって提供されるプロパティ、`ValidatableObject<T>`クラスと型`List<string>`です。 `Errors`プロパティは、複数の検証エラーを含めることができます、`FirstValidationErrorConverter`表示のコレクションから、最初のエラーを取得するインスタンスを使用します。

## <a name="summary"></a>まとめ

EShopOnContainers モバイル アプリは、モデルのプロパティの表示の同期のクライアント側検証を実行して、ユーザーに通知するエラー メッセージを表示したりして、無効なデータを格納しているコントロールが強調表示され、検証エラーのユーザーに通知理由のデータが正しくありません。

型の検証が必要なモデル プロパティの表示は、 `ValidatableObject<T>`、および各`ValidatableObject<T>`インスタンスに追加の検証規則にはその`Validations`プロパティです。 ビュー モデルから呼び出すことによって検証が呼び出される、`Validate`のメソッド、`ValidatableObject<T>`検証を取得するには、インスタンスがルールし、それらに対して実行、 `ValidatableObject<T>` `Value`プロパティです。 検証エラーを配置している、`Errors`のプロパティ、`ValidatableObject<T>`インスタンス、および`IsValid`のプロパティ、`ValidatableObject<T>`インスタンスは、検証が成功したか失敗したかどうかを示すために更新されます。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
