# DataBinding & ListView

## Binding View Properties to Objects

### 1. Data Binding

To add a DataContext that will be carried on to child elements unless told otherwise:

```xaml
<Grid DataContext="GridDataContext">
    <Border> <!-- DataContext: GridDataContext -->
        <StackPanel DataContext="StackPanelDataContext">
            <TextBlock  /> <!-- DataContext: StackPanelDataContext -->
        </StackPanel>
    </Border>
</Grid>
```

To bind element properties to DataContext properties:

#### Code-Behind

```c#
public class ViewModel
{
    public string Value => "Value in ViewModel";
}
```

```c#
public class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        DataContext = new ViewModel();
    }
}
```

#### XAML

```xaml
<MainWindow>
    <Grid>
        <TextBlock Text="{Binding Value}" /> <!-- Will appear as string 'Value in ViewModel' -->
    </Grid>
</MainWindow>
```



### 2. ListView

To create ListView in XAML:

```xaml
<ListView>
    <ListViewItem Content="" />
</ListView>
```

To assign ListView ItemsSource:

#### Code-Behind

```c#
public MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        ListViewName.ItemsSource = new List<DataModel>();
    }
}
```

#### XAML

```xaml
<MainWindow>
    <Grid>
        <ListView ItemsSource="{Binding DataModelList}" SelectedValue="{Binding DataModelItem}" />
    </Grid>
</MainWindow>
```

To override ListView ItemTemplate:

```xaml
<ListView>
    <ListView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <TextBlock Text="{Binding}" /> <!-- Where DataContext is ItemsSource item -->
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

To override ListView ItemContainerStyle:

```xaml
<ListView>
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="" Value="" />
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```