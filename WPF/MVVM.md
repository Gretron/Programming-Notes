# MVVM

## MVVM Pattern for WPF

### 1. Model

#### Model Classes

Model classes are simply classes meant to hold data, see the following examples:

```c#
public class Area
{
    public string ID { get; set; }
    public string LocalizedName { get; set; }
}

public class Location
{
    public int Version { get; set; }
    public string Key { get; set; }
    public string Type { get; set; }
    public int Rank { get; set; }
    public string LocalizedName { get; set; }
    public Area Country { get; set; }
    public Area AdministrativeArea { get; set; }
}
```

```c#
public class Degree
{
    public double Value { get; set; }
    public string Unit { get; set; }
    public int UnitType { get; set; }
}

public class Temperature
{
    public Degree Metric { get; set; }
    public Degree Imperial { get; set; }
}

public class Conditions
{
    public DateTime LocalObservationDateTime { get; set; }
    public int EpochTime { get; set; }
    public string WeatherText { get; set; }
    public int WeatherIcon { get; set; }
    public bool HasPrecipitation { get; set; }
    public object PrecipitationType { get; set; }
    public bool IsDayTime { get; set; }
    public Temperature Temperature { get; set; }
    public string MobileLink { get; set; }
    public string Link { get; set; }
}
```

#### Model Helpers

##### Rest/JSON Helper

```c#
public class AccuWeatherHelper
{
    public const string BaseUrl = "http://dataservice.accuweather.com/";
    public const string ApiKey = "Ojy42E1cYSGQhGc6XxAdRmiyKsHXZv3b";
    public const string AutocompleteEndpoint = "locations/v1/cities/autocomplete?apikey={0}&q={1}";
    public const string CurrentConditionsEndpoint = "currentconditions/v1/{0}?apikey={1}";

    public static async Task<List<Location>> GetLocations(string query)
    {
        var locations = new List<Location>();
        var url = BaseUrl + string.Format(AutocompleteEndpoint, ApiKey, query);

        using (HttpClient client = new HttpClient())
        {
            var response = await client.GetAsync(url);
            var json = await response.Content.ReadAsStringAsync();
            
            locations = JsonConvert.DeserializeObject<List<Location>>(json);
        }

        return locations;
    }
    
    public static async Task<Conditions> GetConditions(string key)
    {
        var conditions = new Conditions();
        var url = BaseUrl + string.Format(CurrentConditionsEndpoint, key, ApiKey);

        using (HttpClient client = new HttpClient())
        {
            var response = await client.GetAsync(url);
            var json = await response.Content.ReadAsStringAsync();
            
            conditions = JsonConvert.DeserializeObject<List<Conditions>>(json).FirstOrDefault();
        }

        return conditions;
    }
}
```

##### SQLite Helper

```c#
public class DatabaseHelper
{
    private static string DatabasePath = Path.Combine(Environment.CurrentDirectory, "notes.db");

    public static bool Insert<T>(T row)
    {
        var result = false;

        using (SQLiteConnection connection = new SQLiteConnection(DatabasePath))
        {
            connection.CreateTable<T>();
            var rows = connection.Insert(row);

            if (rows != 0)
                result = true;
        }
        
        return result;
    }
    
    public static bool Update<T>(T row)
    {
        var result = false;

        using (SQLiteConnection connection = new SQLiteConnection(DatabasePath))
        {
            connection.CreateTable<T>();
            var rows = connection.Update(row);

            if (rows != 0)
                result = true;
        }
        
        return result;
    }

    public static bool Delete<T>(T row)
    {
        var result = false;

        using (SQLiteConnection connection = new SQLiteConnection(DatabasePath))
        {
            connection.CreateTable<T>();
            var rows = connection.Delete(row);

            if (rows != 0)
                result = true;
        }
        
        return result;
    }

    public static List<T> Select<T>() where T : new()
    {
        List<T> rows;

        using (SQLiteConnection connection = new SQLiteConnection(DatabasePath))
        {
            connection.CreateTable<T>();
            rows = connection.Table<T>().ToList();
        }
        
        return rows;
    }
}
```



### 2. ViewModel

To save the trouble of having to constantly call property value changed events, download the package "**PropertyChanged.Fody**" via NuGet:

![image-20220724113713823](..\.img\image-20220724113713823.png)

To start creating ViewModel classes, we must first make a base to prevent having to constantly rewrite code:

```c#
/// <summary>
/// Base ViewModel With PropertyChanged Event
/// </summary>
public class BaseViewModel : INotifyPropertyChanged
{
    /// <summary>
    /// Event Raised When Property Value Changes
    /// </summary>
    public event PropertyChangedEventHandler PropertyChanged = (sender, e) => { };
    
    /// <summary>
    /// Raise <see cref="PropertyChanged"/> Event From Code
    /// </summary>
    /// <param name="name"></param>
    public void OnPropertyChanged(string name)
    {
        PropertyChanged(this, new PropertyChangedEventArgs(name));
    }
}
```

To create a ViewModel class, inherit from our base ViewModel class:

```c#
public class ApplicationViewModel : BaseViewModel
{
}
```

To start creating bindable properties for our ViewModel, see following examples:

####  xxxxxxxxxx <ListView>    <ListView.ItemContainerStyle>        <Style TargetType="ListViewItem">            <Setter Property="" Value="" />        </Style>    </ListView.ItemContainerStyle></ListView>xaml

```c#
public class ApplicationViewModel : BaseViewModel
{
    public int Number { get; set; } = 5;
}
```

#### Lists

```c#
public class ApplicationViewModel : BaseViewModel
{
    public ObservableCollection<int> Numbers { get; set; } = new ObservableCollection<int>(); // Only add & remove elements
}
```

#### Commands

To start creating ICommand properties, we must first make a base to prevent having to constantly rewrite code:

```c#
/// <summary>
/// Command Where <see cref="CanExecute"/> Always Returns True
/// </summary>
public class RelayCommand : ICommand
{
    #region Private Members

    /// <summary>
    /// Action to Perform
    /// </summary>
    private Action mAction;

    #endregion
    
    
    #region Public Events

    /// <summary>
    /// Event Raised When <see cref="CanExecute(object)"/> Value Changes
    /// </summary>
    public virtual event EventHandler CanExecuteChanged = (sender, e) => { };

    #endregion

    
    #region Constructor

    /// <summary>
    /// Default Constructor
    /// </summary>
    /// <param name="action"> Action to Perform </param>
    public RelayCommand(Action action)
    {
        mAction = action;
    }

    #endregion
    
    
    #region Public Methods

    /// <summary>
    /// Relay Command Can Always Execute
    /// </summary>
    /// <param name="parameter"> Search Query </param>
    /// <returns></returns>
    public virtual bool CanExecute(object parameter) { return true; }

    /// <summary>
    /// Perform Command Action
    /// </summary>
    /// <param name="parameter"></param>
    public void Execute(object parameter)
    {
        mAction();
    }

    #endregion
}
```

```c#
/// <summary>
/// Command Where <see cref="CanExecute"/> Always Returns True
/// </summary>
public class ParameterizedRelayCommand : ICommand
{
    #region Private Members

    /// <summary>
    /// Action to Perform
    /// </summary>
    private Action<object> mAction;

    #endregion
    
    
    #region Public Events

    /// <summary>
    /// Event Raised When <see cref="CanExecute(object)"/> Value Changes
    /// </summary>
    public virtual event EventHandler CanExecuteChanged = (sender, e) => { };

    #endregion

    
    #region Constructor

    /// <summary>
    /// Default Constructor
    /// </summary>
    /// <param name="action"> Action to Perform </param>
    public ParameterizedRelayCommand(Action<object> action)
    {
        mAction = action;
    }

    #endregion
    
    
    #region Public Methods

    /// <summary>
    /// Relay Command Can Always Execute
    /// </summary>
    /// <param name="parameter"> Search Query </param>
    /// <returns></returns>
    public virtual bool CanExecute(object parameter) { return true; }

    /// <summary>
    /// Perform Command Action
    /// </summary>
    /// <param name="parameter"></param>
    public void Execute(object parameter)
    {
        mAction(parameter);
    }

    #endregion
}
```

To add CanExecute functionality, override virtual methods and members:

```c#
public override event EventHandler CanExecuteChanged
{
    add => CommandManager.RequerySuggested += value; 
    remove => CommandManager.RequerySuggested -= value; 
}

public override bool CanExecute(object parameter)
{
    var boolean = true;
    
    if (expression)
    	boolean = false;
    
    return boolean;
}
```

To implement ICommand properties:

```c#
public class ApplicationViewModel : BaseViewModel
{
    public ICommand Command { get; set; }
    
    public ApplicationViewModel()
    {
        Command = new RelayCommand(() => { /* Command Action */ });
    }
}
```



### 3. View

To bind WPF element properties to ViewModel properties:

```c#
public partial class ApplicationWindow : Window
{
    public ApplicationWindow()
    {
        InitializeComponent();
        
        DataContext = new ApplicationViewModel();
    }
}
```

```xaml
<Window>
    <TextBlock Text="{Binding Text}" />

	<ListView ItemSource="{Binding List}" />

	<Button Command="{Binding Command}" CommandParameter="{Binding Parameter}" } />
</Window>
```

To use design time data:

#### C#

```c#
/// <summary>
/// DesignModel for Application User Interface
/// </summary>
public class ApplicationDesignModel : MarkupExtension
{
    /// <summary>
    /// Provides Design Instance of the ViewModel
    /// </summary>
    /// <param name="serviceProvider"></param>
    /// <returns></returns>
    public override object ProvideValue(IServiceProvider serviceProvider)
    {
        var designModel = new ApplicationViewModel();
        
        return designModel;
    }
}
```

#### XAML

```xaml
<Window d:DataContext="{local:ApplicationDesignModel}">
    <Texblock Text="{Binding Text}" />
</Window>
```



### 4. Value Converter

To start creating IValueConverter classes, we must first make a base to prevent having to constantly rewrite code:

```c#
/// <summary>
/// Base Value Converter
/// </summary>
/// <typeparam name="T"> Type of the Value Converter </typeparam>
public abstract class BaseValueConverter<T> : MarkupExtension, IValueConverter
    where T : class, new()
{
  	#region Private Members

	/// <summary>
	/// Instance of the Value Converter
	/// </summary>
	private static T mConverter = null;

	#endregion
        

	#region MarkupExtension Methods

	/// <summary>
	/// Provides Static Instance of the Value Converter
	/// </summary>
	/// <param name="serviceProvider"></param>
	/// <returns></returns>
 	public override object ProvideValue(IServiceProvider serviceProvider)
  	{
    	return mConverter ?? (mConverter = new T());
  	}

   	#endregion
        

	#region ValueConverter Methods

	/// <summary>
    /// Method to Convert One Type to Another
    /// </summary>
	/// <param name="value"></param>
	/// <param name="targetType"></param>
	/// <param name="parameter"></param>
	/// <param name="culture"></param>
	/// <returns></returns>
	public abstract object Convert(object value, Type targetType, object parameter, CultureInfo culture);

	/// <summary>
	/// Method to Convert Back One Type to the Source Type
	/// </summary>
	/// <param name="value"></param>
	/// <param name="targetType"></param>
	/// <param name="parameter"></param>
	/// <param name="culture"></param>
	 /// <returns></returns>
	public abstract object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture);

	#endregion
}
```

To implement IValueConverter converters, see following example:

#### C#

```c#
[ValueConversion(typeof(string), typeof(BitmapImage))]
public class StringToImageConverter : BaseValueConverter<StringToImageConverter>
{
    public override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        var path = (string)value;
        
        return new BitmapImage(new Uri($"pack://application:,,,/{path}"));
    }

    public override object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

#### XAML

```xaml
<Image Source={Binding Text, Converter={local:StringToImageConverter}} />
```

