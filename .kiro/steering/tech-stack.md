# Tech Stack Reference

## Backend Stack

### Python 3.12
- Docs: https://docs.python.org/3.12/
- Gunakan type hints modern (PEP 695): `def func[T](x: T) -> T`
- Gunakan `match` statement untuk pattern matching
- F-string dengan `=` untuk debugging: `f"{var=}"`

### FastAPI
- Docs: https://fastapi.tiangolo.com/
- Version: Latest stable
- Patterns:
  - Dependency Injection untuk database session
  - Pydantic v2 untuk validation (bukan v1)
  - `Annotated` untuk dependencies: `Annotated[Session, Depends(get_db)]`
  - Background tasks dengan `BackgroundTasks`
  - Async endpoints untuk I/O operations

```python
# Correct FastAPI pattern
from typing import Annotated
from fastapi import Depends, FastAPI
from sqlmodel import Session

def get_session():
    with Session(engine) as session:
        yield session

SessionDep = Annotated[Session, Depends(get_session)]

@app.get("/items/{id}")
async def get_item(id: int, session: SessionDep):
    return session.get(Item, id)
```

### SQLModel
- Docs: https://sqlmodel.tiangolo.com/
- Kombinasi SQLAlchemy + Pydantic
- PENTING: Gunakan `SQLModel` bukan `BaseModel` untuk database models
- Relationship pattern:

```python
from sqlmodel import SQLModel, Field, Relationship

class Team(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str
    heroes: list["Hero"] = Relationship(back_populates="team")

class Hero(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str
    team_id: int | None = Field(default=None, foreign_key="team.id")
    team: Team | None = Relationship(back_populates="heroes")
```

### Pandas + OpenPyXL
- Pandas docs: https://pandas.pydata.org/docs/
- OpenPyXL docs: https://openpyxl.readthedocs.io/
- Untuk Excel operations, gunakan `engine='openpyxl'`

```python
import pandas as pd

# Read Excel
df = pd.read_excel("file.xlsx", engine="openpyxl")

# Write Excel
df.to_excel("output.xlsx", engine="openpyxl", index=False)
```

### PostgreSQL 16 (Docker)
- Docs: https://www.postgresql.org/docs/16/
- Connection string: `postgresql://user:pass@localhost:5432/dbname`
- Async driver: `asyncpg` (bukan `psycopg2`)

---

## Frontend Stack (WPF .NET 8)

### .NET 8 / C# 12
- Docs: https://learn.microsoft.com/en-us/dotnet/
- Gunakan primary constructors
- Gunakan collection expressions: `[1, 2, 3]`
- `required` modifier untuk required properties

### WPF dengan MVVM
- Docs: https://learn.microsoft.com/en-us/dotnet/desktop/wpf/
- Pattern: MVVM (Model-View-ViewModel)
- Data binding dengan `{Binding PropertyName}`
- Commands untuk user actions

### MVVM CommunityToolkit
- Docs: https://learn.microsoft.com/en-us/dotnet/communitytoolkit/mvvm/
- NuGet: `CommunityToolkit.Mvvm`
- PENTING: Gunakan source generators (partial class)

```csharp
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;

public partial class MainViewModel : ObservableObject
{
    [ObservableProperty]
    private string _name = string.Empty;

    [ObservableProperty]
    [NotifyCanExecuteChangedFor(nameof(SaveCommand))]
    private bool _isValid;

    [RelayCommand(CanExecute = nameof(IsValid))]
    private async Task SaveAsync()
    {
        // Implementation
    }
}
```

### MaterialDesignInXaml
- Docs: https://github.com/MaterialDesignInXAML/MaterialDesignInXamlToolkit/wiki
- NuGet: `MaterialDesignThemes`
- Setup di App.xaml:

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <materialDesign:BundledTheme BaseTheme="Light" PrimaryColor="DeepPurple" SecondaryColor="Lime" />
            <ResourceDictionary Source="pack://application:,,,/MaterialDesignThemes.Wpf;component/Themes/MaterialDesign2.Defaults.xaml" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

---

## Network & Resilience

### Refit
- Docs: https://github.com/reactiveui/refit
- NuGet: `Refit.HttpClientFactory`
- Define interface untuk API:

```csharp
public interface IMyApi
{
    [Get("/users/{id}")]
    Task<User> GetUserAsync(int id);

    [Post("/users")]
    Task<User> CreateUserAsync([Body] CreateUserRequest request);

    [Multipart]
    [Post("/upload")]
    Task UploadFileAsync([AliasAs("file")] StreamPart stream);
}

// Registration
services.AddRefitClient<IMyApi>()
    .ConfigureHttpClient(c => c.BaseAddress = new Uri("https://api.example.com"));
```

### Polly (v8+)
- Docs: https://www.pollydocs.org/
- NuGet: `Microsoft.Extensions.Http.Polly` atau `Polly.Extensions`
- PENTING: Polly v8 syntax berbeda dari v7!

```csharp
// Polly v8 pattern
using Polly;

var retryPipeline = new ResiliencePipelineBuilder()
    .AddRetry(new RetryStrategyOptions
    {
        MaxRetryAttempts = 3,
        Delay = TimeSpan.FromSeconds(1),
        BackoffType = DelayBackoffType.Exponential
    })
    .Build();

// With Refit
services.AddRefitClient<IMyApi>()
    .ConfigureHttpClient(c => c.BaseAddress = new Uri(baseUrl))
    .AddResilienceHandler("retry", builder =>
    {
        builder.AddRetry(new HttpRetryStrategyOptions
        {
            MaxRetryAttempts = 3,
            BackoffType = DelayBackoffType.Exponential
        });
    });
```

---

## Reporting & Utilities

### QuestPDF
- Docs: https://www.questpdf.com/
- NuGet: `QuestPDF`
- Fluent API untuk PDF generation:

```csharp
using QuestPDF.Fluent;
using QuestPDF.Helpers;
using QuestPDF.Infrastructure;

QuestPDF.Settings.License = LicenseType.Community;

Document.Create(container =>
{
    container.Page(page =>
    {
        page.Size(PageSizes.A4);
        page.Margin(2, Unit.Centimetre);
        
        page.Header().Text("Report Title").FontSize(20).Bold();
        
        page.Content().Table(table =>
        {
            table.ColumnsDefinition(columns =>
            {
                columns.RelativeColumn();
                columns.RelativeColumn();
            });
            
            table.Cell().Text("Column 1");
            table.Cell().Text("Column 2");
        });
        
        page.Footer().AlignCenter().Text(x =>
        {
            x.Span("Page ");
            x.CurrentPageNumber();
        });
    });
}).GeneratePdf("report.pdf");
```

### ClosedXML
- Docs: https://closedxml.github.io/ClosedXML/
- NuGet: `ClosedXML`
- Excel manipulation:

```csharp
using ClosedXML.Excel;

using var workbook = new XLWorkbook();
var worksheet = workbook.Worksheets.Add("Sheet1");

worksheet.Cell("A1").Value = "Header";
worksheet.Cell("A1").Style.Font.Bold = true;
worksheet.Cell("A1").Style.Fill.BackgroundColor = XLColor.LightBlue;

worksheet.Range("A1:D1").Style.Border.OutsideBorder = XLBorderStyleValues.Thin;

workbook.SaveAs("output.xlsx");
```

---

## Auto-Update

### Velopack
- Docs: https://velopack.io/docs/
- NuGet: `Velopack`
- Modern replacement untuk Squirrel:

```csharp
using Velopack;

// In App startup
VelopackApp.Build().Run();

// Check for updates
var mgr = new UpdateManager("https://releases.example.com");
var newVersion = await mgr.CheckForUpdatesAsync();
if (newVersion != null)
{
    await mgr.DownloadUpdatesAsync(newVersion);
    mgr.ApplyUpdatesAndRestart(newVersion);
}
```

---

## Aturan Umum

1. **Selalu gunakan versi terbaru** dari syntax dan API
2. **Async/await** untuk semua I/O operations
3. **Dependency Injection** untuk semua services
4. **Nullable reference types** enabled (`<Nullable>enable</Nullable>`)
5. **Jangan campur** Polly v7 dan v8 syntax
6. **SQLModel** untuk FastAPI, bukan raw SQLAlchemy
7. **Source generators** untuk MVVM CommunityToolkit
