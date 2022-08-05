# Window Template & Window Chrome

## Overriding Default Window Template & Stylizing Using Window Chrome Features

### 1. Standard Window Template

To override the standard WPF window template, use the following example:

```xaml
<Window x:Class="Pomodoroctivity.Wpf.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:Pomodoroctivity.Wpf.Views"
        mc:Ignorable="d"
        Title="MainWindow" 
        Width="1920" 
        Height="1080"
        MinWidth="320"
        MinHeight="320">
    <Window.Resources>
        <Style TargetType="{x:Type local:MainWindow}">
            <Setter Property="WindowChrome.WindowChrome">
                <Setter.Value>
                    <WindowChrome ResizeBorderThickness="20"
                                  CaptionHeight="24"
                                  GlassFrameThickness="0"
                                  CornerRadius="0" />
                </Setter.Value>
            </Setter>
            
            <Setter Property="WindowStyle" Value="None" />
            
            <Setter Property="AllowsTransparency" Value="True" />
            
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="{x:Type Window}">
                        <Border Padding="12">
                            <Grid>
                                <!-- Drop Shadow -->
                                <Border Background="Black" CornerRadius="8">
                                    <Border.Effect>
                                        <DropShadowEffect ShadowDepth="0" Opacity="0.4" />
                                    </Border.Effect>
                                </Border>
                                
                                <!-- Opacity Mask -->
                                <Border x:Name="Mask" Background="Black" CornerRadius="8">
                                    <Grid>
                                        <Grid.ColumnDefinitions>
                                            <ColumnDefinition Width="*" />
                                            <ColumnDefinition Width="*" />
                                        </Grid.ColumnDefinitions>
                                        
                                        <Grid.RowDefinitions>
                                            <RowDefinition Height="*" />
                                            <RowDefinition Height="*" />
                                        </Grid.RowDefinitions>
                                        
                                        <Border Background="Black" 
                                                CornerRadius="8 0 0 0"
                                                BorderThickness="0" />
                                        
                                        <Border Background="Black" 
                                                CornerRadius="0 8 0 0" 
                                                BorderThickness="0"
                                                Grid.Column="1" />
                                        
                                        <Border Background="Black" 
                                                CornerRadius="0 0 0 8"
                                                Grid.Row="1" />
                                        
                                        <Border Background="Black" 
                                                CornerRadius="0 0 8 0" 
                                                Grid.Column="1" 
                                                Grid.Row="1" />
                                    </Grid>
                                </Border>
                                
                                <Grid>
                                    <Grid.OpacityMask>
                                        <VisualBrush Visual="{Binding ElementName=Mask}" />
                                    </Grid.OpacityMask>
                                    
                                    <Grid.RowDefinitions>
                                        <RowDefinition Height="32" /> 
                                        <RowDefinition Height="*" /> 
                                    </Grid.RowDefinitions>
                                    
                                    <!-- Title Bar -->
                                    <Grid Panel.ZIndex="1">
                                    </Grid>
                                    
                                    <!-- Content -->
                                    <Border Grid.Row="1">
                                        <ContentPresenter Content="{TemplateBinding Content}" />
                                    </Border>
                                </Grid>
                            </Grid>
                        </Border>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>    
    </Window.Resources>
    
    <Grid>
        <!-- Actual Content -->
    </Grid>
</Window>

```

