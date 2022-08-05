# RichTextBox

## Highly Customizable Textbox

### 1. Document

To get RichTextBox document text:

```c#
var text = new TextRange(RichTextBox.Document.ContentStart, RichTextBox.Document.ContentEnd).Text;
```



### 2. Selection

#### Get Property

To get PropertyValue from RichTextBox element selection:

```c#
var property = RichTextBox.Selection.GetPropertyValue(Inline.FontFamilyProperty);
```

To check if a PropertyValue is equals to a value:

```c#
var boolean = !property.Equals(DependencyProperty.UnsetValue) && property.Equals(FontWeights.Bold);
```

#### Apply Property

To apply PropertyValue to RichTextBox element selection:

```c#
RichTextBox.Selection.ApplyPropertyValue(Inline.FontFamilyProperty, Font);
```

##### TextDecorations Property

To remove TextDecorations property from selection:

```c#
TextDecorationCollection decorations;

(RichTextBox.Selection.GetPropertyValue(Inline.TextDecorationsProperty) as TextDecorationsCollection).TryRemove(TextDecoractions.Underline, out decorations);

RichTextBox.Selection.ApplyPropertyValue(Inline.TextDecorationsProperty, decorations);
```



### 3. Save/Edit File

#### Save

```c#
var path = Path.Combine(Environment.CurrentDirectory, $"{FileName}.rtf");
var stream = new FileStream(path, FileMode.Create);
var content = new TextRange(RichTextBox.Document.ContentStart, RichTextBox.Document.ContentEnd);

content.Save(stream, DataFormats.Rtf);
```

#### Edit

```c#
var stream = new FileStream(path, FileMode.Open);
var content = new TextRange(RichTextBox.Document.ContentStart, RichTextBox.Document.ContentEnd);

content.Load(stream, DataFormat.Rtf);
```

