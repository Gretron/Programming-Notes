# Resources & Styles

## Improving the UI With Resources & Styles

### 1. Styles

To make styles in WPF:

```xaml
<!-- Remove x:Key to Make Default Style for TargetType -->
<Style x:Key="..." TargetType="..." BasedOn="...">
    <Setter Property="..." Value="..." />
</Style>
```



### 2. Resources

To locate resources from files:

```xaml
<ResourceDictionary Source="..." />
```

To merge multiple resources:

```xaml
<ResourceDictionary>
    <ResourceDictionary.MergedDictionaries>
        <ResourceDictionary Source="..." />
        <ResourceDictionary Source="..." />
    </ResourceDictionary.MergedDictionaries>
</ResourceDictionary>
```

