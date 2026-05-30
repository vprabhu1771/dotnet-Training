Your API is actually running correctly.

This line confirms it:

```text id="av69aq"
Now listening on: http://localhost:5145
```

The problem is:

```text id="q1gb4r"
http://localhost:5145/
```

returns `404 Not Found` because you do not have a route for `/`.

ASP.NET Core Web API only responds to defined endpoints like:

```text id="x4b4w6"
/api/product
/api/weatherforecast
```

---

# Test Existing Controller

If you created:

```csharp id="t1t14f"
[Route("api/[controller]")]
public class ProductController : ControllerBase
```

Then open:

```text id="6fql18"
http://localhost:5145/api/product
```

NOT:

```text id="ryh9d7"
http://localhost:5145/
```

---

# Example Working Controller

`Controllers/ProductController.cs`

```csharp id="nvix3d"
using Microsoft.AspNetCore.Mvc;

namespace MauiApp.api.Controllers;

[ApiController]
[Route("api/[controller]")]
public class ProductController : ControllerBase
{
    [HttpGet]
    public IActionResult Get()
    {
        return Ok(new List<string>
        {
            "Laptop",
            "Mobile",
            "Keyboard"
        });
    }
}
```

---

# Ensure Program.cs Has Controllers

`Program.cs`

```csharp id="i8yv98"
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

var app = builder.Build();

// app.UseHttpsRedirection();

app.MapControllers();

app.Run();
```

---

# If You Want Homepage `/`

Add this in `Program.cs`:

```csharp id="i4g6u9"
app.MapGet("/", () => "API Running...");
```

Full example:

```csharp id="q4t19j"
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

var app = builder.Build();

app.MapGet("/", () => "API Running...");

app.MapControllers();

app.Run();
```

Now:

```text id="f63m6l"
http://localhost:5145/
```

shows:

```text id="50wyqm"
API Running...
```

---

# For MAUI Android Emulator

Use:

```csharp id="7s9lcy"
http://10.0.2.2:5145/api/product
```

NOT localhost.

---

# The HTTPS Warning

This warning is harmless:

```text id="rrw1m5"
Failed to determine the https port for redirect.
```

To remove it:

```csharp id="pk1otz"
// app.UseHttpsRedirection();
```

Comment/remove that line.
