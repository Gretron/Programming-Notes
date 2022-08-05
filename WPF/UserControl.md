# UserControl

## Custom Controls

To define UserControl property using DependencyProperty:

#### Code-Behind

```c#
public partial class MyUserControl : UserControl
{
    private static readonly DependencyProperty mProperty = Dependency.Register(nameof(Property), typeof(Property), typeof(MyUserControl), 	  	new PropertyMetadata(OnPropertyChanged));
    
    private static void OnPropertyChanged(DependencyObject d, DependencyPropertyChangedEventArgs)
    {
        var control = d as MyUserControl;
        var newValue = e.NewValue as DataModel;
    }
    
    public DataModel Property
    {
        get => (DataModel)GetValue(mProperty);
        set => SetValue(mProperty, value);
    }
}
```

#### XAML

```xaml
<Grid>
    <namespace:MyUserControl Property="" />
</Grid>
```

