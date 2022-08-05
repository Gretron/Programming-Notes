# File

## Select, Create, Update & Delete Files

### 1. Select File

To select file via OpenFileDialog:

```c#
OpenFileDialog dialog = new OpenFileDialog();
    
if (dialog.ShowDialog())
{
	var path = dialog.FileName;
}
```

To add filter for file types:

```c#
dialog.Filter = "Image Files (*.png, *.jpg)|*.png;*.jpg;*.jpeg|All Files (*.*)|*.*"; // (Label|Types)|(Label|Types)
```

To add initial directory of OpenFileDialog:

```c#
dialog.InitialDirectory = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
```
