# Events

## Events on WPF Elements

### 1. Bind Events to Commands

To bind element events to commands, download the package called "**Microsoft.Xaml.Behaviors.Wpf**":

![image-20220724220433353](..\.img\image-20220724220433353.png)

To utilize in XAML code:

```xaml
<Window xmlns:i="http://schemas.microsoft.com/xaml/behaviors">
    <TextBox>
        <i:Interaction.Triggers>
            <i:EventTrigger EventName="">
                <i:InvokeCommandAction Command="{Binding Command}" CommandParameter="{Binding CommandParameter}" />
            </i:EventTrigger>
        </i:Interaction.Triggers>
    </TextBox>
</Window>
```

