# Rest API & JSON

## Rest API Requests & JSON Serialization/Deserialization

### 1. Rest API

To send request, you must create an HttpClient:

```c#
using (HttpClient client = new HttpClient()) // Dispose() automatically at end of scope
{
    
}
```

To add custom header to request:

```c#
using (HttpClient client = new HttpClient())
{
    client.DefaultRequestHeaders.Add("Key", "Value");
}
```

To send request via GET:

```c#
using (HttpClient client = new HttpClient())
{
    var response = await client.GetAsync(url);
    var text = await response.Content.ReadAsStringAsync();
}
```

To send request via POST:

```c#
using (HttpClient client = new HttpClient())
{
   	using (var content = new ByteArrayContent(file))
    {
        content.Headers.Headers.ContentType = new MediaTypeHeaderValue(type);
        var response = await client.PostAsync(url, content);
    	var text = await response.Content.ReadAsStringAsync();
    }
}
```



### 2. JSON

To get JSON via NuGet, download the package called "**Newtonsoft.Json**":

![image-20220718214412029](..\.img\image-20220718214412029.png)

To turn sample JSON object into C# type, use tool at https://jsonutils.com

#### Deserialization

```c#
var obj = JsonConvert.DeserializeObject<DataModel>(text);
```

#### Serialization

```c#
var text = JsonConvert.SerializeObject(obj);
await File.WriteAllTextAsync(path, text);
```

