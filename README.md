# XitSoap
_An alternative way to do SOAP Requests without use the WSDL_

In 2015, in one of the companies that I worked, we face a very simple problem: the code generated automatically by Visual Studio to use webservices has a lot of shits. Facing this problem, I resolve to create a library that, based on you model, generates the SOAP requests, with code smells on you code.

## Download it using NuGet

```
Install-Package HodStudio.XitSoap
```

Link: https://www.nuget.org/packages/HodStudio.XitSoap

## How to use

### Possibility 1: object return

On application start, register the contract to the correct service, using the _ServiceCatalog_.
```cs
namespace ConsoleTester
{
    public static class Program
    {
        static void Main(string[] args)
        {
            ServiceCatalog.Catalog.Add("ProductContract", "http://localhost/XitSoap/ProductService.asmx");
        }
    }
}
```

After that, configure the return type of the service to use the specified contract.
```cs
namespace HodStudio.XitSoap.Tests.Model
{
    [WsContract("ProductContract")]
    public class ProductOutput : ApiResult
    {
        public List<Product> Products { get; set; }
    }
}
```

Create a _WebService_ of the return type, using the method name to call and invoke it. The property ResultObject is a typed object from the return type.
```cs
var wsCon = new WebService<ProductOutput>("GetProduct");
wsCon.Invoke();
var result = wsCon.ResultObject;
```

You also can create a _WebService_ of the return type, but without specify the method name. When invoke, inform the correct method. This way, you can reuse the _WebService_ object.
```cs
var wsCon = new WebService<ProductOutput>();
wsCon.Invoke("GetProduct");
var result = wsCon.ResultObject;
```

### Possibility 2: string return

When use this approach, the return is not from a type. So, you will have only the direct string from the return, using the property ResultXml or ResultString.
Create the _WebService_ with the url. You can provide the method name too.
```cs
var wsCon = new WebService("http://localhost/XitSoap/ProductService.asmx", "GetProduct");
wsCon.Invoke();
var actual = wsCon.ResultXML;
````
