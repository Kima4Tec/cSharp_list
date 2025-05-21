
# C# Noter

# Indholdsfortegnelse
1. [ActionResult](#actionresult)
2. [API](#api)
3. [Arkitektur: Models, DTOs, Controllers, Interfaces, Repositories, Services og AutoMapper](#arkitektur)  
4. [Arv (Inheritance)](#arv)  
5. [Automapper](#automapper)  
6. [Bcrypt](#bcrypt)
7. [ChangeTracker i EF](#changetracker)
8. [Clean Code](#clean-code)  
9. [Code First](#code-first)
10. [Controllers](#controllers)
11. [CORS (Cross-Origin Resource Sharing)](#cors)
12. [Data Transfer Objects](#dto)  
13. [Defensive Coding](#defensive-coding)
14. [Dependency Injection](#dependency-injection-og-interfaces)
15. [Design Patterns](#design-patterns)  
16. [Domain Driven Design (DDD)](#domain-driven-design-ddd)  
17. [Entity](#entity)  
18. [Encapsulation](#encapsulation)
19. [FAQ](#faq)  
20. [Fluent Api](#fluent-api)  
21. [Forretningsobjekt](#forretningsobjekt)
22. [FromBody](#frombody)  
23. [ICollection](#icollection)
24. [IEnumerable<T>](#ienumerable)
25. [Iterative Agile](#iterative-agile)  
26. [JWT](#jwt)  
27. [Klasser](#klasser)
28. [Lagene og deres ansvar](#lagstruktur)
29. [Models](#models)  
30. [Objekt](#objekt)  
31. [OOP (Objektorienteret programmering)](#oop-objektorienteret-programmering)  
32. [Repository og interface](#repository-og-interface)
33. [Scalar](#scalar)  
34. [Separation of Concerns](#separation-of-concerns)
35. [Services](#services)  
36. [.NET Apps](#net-apps)




---

# OOP (Objektorienteret programmering)

C# OOP er en programmeringsparadigme, hvor kode organiseres omkring objekter. OOP i C# bygger på fire grundlæggende principper:

### 🔑 De fire grundprincipper i OOP
1. **Indkapsling** – Skjul interne detaljer og vis kun nødvendige dele.
2. **Arv** – Genbrug og udvid funktionalitet.
3. **Polymorfi** – Samme metode-navn med forskellig funktionalitet.
4. **Abstraktion** – Skjul kompleksitet bag en simpel grænseflade.

### Eksempel

```csharp
public class Animal
{
    public string Name { get; set; }

    public Animal(string name)
    {
        Name = name;
    }

    public virtual void Speak()
    {
        Console.WriteLine($"{Name} laver en lyd.");
    }
}

public class Dog : Animal
{
    public Dog(string name) : base(name) { }

    public override void Speak()
    {
        Console.WriteLine($"{Name} siger: Vuf!");
    }
}

public class Cat : Animal
{
    public Cat(string name) : base(name) { }

    public override void Speak()
    {
        Console.WriteLine($"{Name} siger: Miau!");
    }
}
```
[Home](#indholdsfortegnelse)
# Dependency Injection og interfaces

```csharp
public interface IMoveStrategy
{
    void Move(string name);
}

public abstract class Animal
{
    protected string Name;
    protected IMoveStrategy MoveStrategy;

    public Animal(string name, IMoveStrategy moveStrategy)
    {
        Name = name;
        MoveStrategy = moveStrategy;
    }

    public abstract void Speak();

    public void Move()
    {
        MoveStrategy.Move(Name);
    }
}
```

### 1. Hvad er Dependency Injection?

Dependency Injection (DI) er en designmønster, hvor et objekt får (injectes) sine afhængigheder udefra i stedet for selv at oprette dem. Det gør koden mere fleksibel, testbar og løs koblet.

**Eksempel:**

Hvis en klasse `Animal` skal bruge en `IMoveStrategy` til bevægelse, så skal den ikke selv oprette en konkret `MoveStrategy`. I stedet får den strategien **indsprøjtet** udefra.

### 2. Hvad er et Interface?

Et interface definerer en kontrakt – altså hvilke metoder en klasse SKAL implementere.

Eksempel:

```csharp
public interface IMoveStrategy
{
    void Move(string name);
}
```

Her siger vi: "Enhver, der implementerer IMoveStrategy, skal kunne udføre `Move(string name)`."

### 3. Implementering af interface

En konkret klasse implementerer interface, fx:

```csharp
public class WalkStrategy : IMoveStrategy
{
    public void Move(string name)
    {
        Console.WriteLine($"{name} går.");
    }
}
```

Du kan have mange strategier – fx `FlyStrategy`, `SwimStrategy` osv.

### 4. Konfiguration af DI i `Program.cs`

For at bruge DI i ASP.NET Core, registrerer du dine services i `Program.cs` i containeren:

```csharp
var builder = WebApplication.CreateBuilder(args);

// Registrer IMoveStrategy og dens implementation
builder.Services.AddTransient<IMoveStrategy, WalkStrategy>();

var app = builder.Build();

app.Run();
```

- `AddTransient<IMoveStrategy, WalkStrategy>()` betyder, at hver gang `IMoveStrategy` efterspørges, så får man en ny instans af `WalkStrategy`.
- Du kan også bruge `AddScoped` (per HTTP-request) eller `AddSingleton` (en instans for hele appens levetid).

### 5. Brug af DI i dine klasser

Hvis du har en controller eller service, hvor du skal bruge `IMoveStrategy`, får du den ind via constructor:

```csharp
public class AnimalService
{
    private readonly IMoveStrategy _moveStrategy;

    public AnimalService(IMoveStrategy moveStrategy)
    {
        _moveStrategy = moveStrategy;
    }

    public void MakeAnimalMove(string name)
    {
        _moveStrategy.Move(name);
    }
}
```

ASP.NET Core DI-container sørger for at sende den rigtige implementering (`WalkStrategy`) ind automatisk.

### 6. Eksempel med `Animal`-klasse

Hvis du har din abstrakte `Animal`-klasse som tidligere:

```csharp
public abstract class Animal
{
    protected string Name;
    protected IMoveStrategy MoveStrategy;

    public Animal(string name, IMoveStrategy moveStrategy)
    {
        Name = name;
        MoveStrategy = moveStrategy;
    }

    public abstract void Speak();

    public void Move()
    {
        MoveStrategy.Move(Name);
    }
}
```

Når du opretter en konkret `Animal` (fx `Dog`), skal du give den en `IMoveStrategy` – den kan fås via DI.

### Opsummering

| Begreb                  | Forklaring                                                             |
|-------------------------|------------------------------------------------------------------------|
| **Interface**            | Definerer en kontrakt for metoder.                                    |
| **Implementering**       | En konkret klasse, der opfylder kontrakten fra interface.              |
| **Dependency Injection** | Måden hvorpå konkrete implementationer gives til objekter udefra.     |
| **Registrering i Program.cs** | Gør DI-containeren opmærksom på hvilke interfaces der matcher hvilke konkrete klasser. |  


---
[Home](#indholdsfortegnelse)
# Clean Code

Principper for læsbar og vedligeholdelsesvenlig kode:

| Princip                 | Forklaring |
|------------------------|------------|
| Meningsfulde navne     | Beskriv hvad koden gør |
| Små funktioner         | Gør én ting godt |
| Enkelhed               | Undgå unødig kompleksitet |
| Ingen duplikeret kode  | DRY (Don't Repeat Yourself) |
| Brug af `var` med omtanke | Kun når det er klart hvilken type |
| Kommentarer bruges klogt | Forklar hvorfor, ikke hvad |
| Single Responsibility  | En klasse/metode = én opgave |

---
[Home](#indholdsfortegnelse)
# Defensive Coding

Skriv kode der forventer fejl og beskytter sig selv:

| Teknik                | Eksempel                        |
|----------------------|----------------------------------|
| Null-tjek            | Undgå `NullReferenceException`  |
| Input-validering     | Validér før brug                |
| Exceptions med mening| Kast relevante fejl             |
| Guard clauses        | Afbryd hurtigt hvis noget fejler|
| Fail fast            | Fejl tidligt og tydeligt        |

---

# Iterative Agile

Iterativ og Agile udvikling handler om:

- Små trin (sprints)
- Tidlig og hyppig feedback
- Konstant forbedring
- Tidlig værdilevering

**Eksempel:** Start med brugeroversigt → Tilføj søgning i næste iteration → Tilføj brugeroprettelse bagefter.

---
[Home](#indholdsfortegnelse)
# API

Et C# API (typisk ASP.NET Core) udstiller funktionalitet via HTTP:

### Hvad er et API?

- Modtager HTTP-anmodninger (GET, POST, osv.)
- Arbejder med data (fx via Entity Framework)
- Returnerer fx JSON-data

### Teknologi

- ASP.NET Core Web API
- Høj ydeevne, moderne, sikkerhed (JWT, OAuth)
- Nem integration med Angular, React, Flutter

---

## Design Patterns

Genanvendelige løsninger på velkendte problemer:

### Typer

| Kategori    | Eksempler                    |
|-------------|------------------------------|
| Creational  | Singleton, Factory, Builder  |
| Structural  | Adapter, Decorator, Composite|
| Behavioral  | Strategy, Observer, Command  |

### Eksempler

- **Singleton:** Én delt instans
- **Factory:** Opret objekter uden at kende præcis type
- **Strategy:** Udskift adfærd dynamisk
- **Observer:** Abonner på ændringer
- **Repository:** Abstraktion over databaseadgang

---
[Home](#indholdsfortegnelse)
# Domain Driven Design (DDD)
Domain-Driven Design (DDD) er en tilgang til softwareudvikling, der fokuserer på at designe systemet ud fra forretningens kerneområde (domæne) og dets logik. Ideen er at skabe et modeldrevet system, hvor både udviklere og domæneeksperter samarbejder om at bygge en fælles forståelse og et fælles sprog.

**Et domæne er det område eller den forretningskontekst, som systemet skal understøtte. F.eks.:**
- Et bibliotekssystem har domæner som "Bøger", "Udlån", "Brugere".
- En bankapplikation har domæner som "Konti", "Transaktioner", "Kreditvurdering".
  

| Begreb            | Forklaring                             |
|-------------------|----------------------------------------|
| Domæne            | Fx biograf, bank, webshop              |
| Entity            | Objekter med identitet                 |
| Value Object      | Kun værdi, ikke identitet              |
| Aggregate         | En gruppering af entiteter + regler    |
| Repository        | Gem og hent aggregates                 |
| Service           | Forretningslogik                       |
| Ubiquitous Language | Samme sprog hos devs og eksperter     |

###  Ubiquitous Language (fælles sprog)
Et fælles sprog, der bruges af både udviklere og domæneeksperter. Dette sprog afspejles i kode (klassernavne, metoder, osv.) og hjælper med at undgå misforståelser.

### Entity (Entitet)
En genstand med en identitet, som forbliver den samme gennem tid.  
**Eksempel:** En Bruger med `UserId`.

### Value Object (Værdiobjekt)
En genstand uden identitet. Den er defineret af sine attributter.  
**Eksempel:** En Adresse (gade, postnummer, by).

### Aggregate
En gruppe af entiteter og værdiobjekter med en root, der styrer al adgang.  
**Eksempel:** En Ordre kan være en aggregate med Ordrelinjer som indre objekter.

### Repository
En abstraktion til at hente og gemme aggregates fra en datakilde, uden at eksponere den underliggende database.

### Service
Indeholder forretningslogik, der ikke naturligt hører hjemme i en entitet eller et værdiobjekt.

### Domain Event
Noget vigtigt, der er sket i domænet.  
**Eksempel:** `BookReturned` eller `UserRegistered`.

### Bounded Context
En klart afgrænset del af systemet med sit eget sprog og model.  
Et stort system deles typisk op i flere bounded contexts.

---

### Et eksempel på mappestruktur
```bash
/Domain
  /Entities
  /ValueObjects
  /Interfaces (f.eks. IBookRepository)
  /Services
/Application
  /DTOs
  /UseCases eller Commands
/Infrastructure
  /Repositories
  /Database
  /ExternalServices
/API eller UI
```
### Fordele ved DDD
Klar logik, tæt kobling til virkeligheden, skalerbar kodebase.
- Fokus på forretningens behov
- Klar adskillelse mellem logik og infrastruktur
- Lettere vedligeholdelse og testbarhed
- Forbedret kommunikation mellem udviklere og domæneeksperter

### Hvornår bør man ikke bruge DDD?
DDD er ikke nødvendigt i små eller simple projekter, hvor kompleks forretningslogik ikke findes. Det giver først mening, når forretningsregler og begreber bliver komplekse.

---
[Home](#indholdsfortegnelse)
# .NET Apps

Typer af .NET apps:

| Type         | Beskrivelse                     | Teknologi         |
|--------------|----------------------------------|-------------------|
| Web Apps     | Dynamiske web/API’er             | ASP.NET Core      |
| Desktop Apps | Windows-programmer               | WPF, WinForms     |
| Console Apps | CLI værktøjer                    | .NET Console      |
| Mobile Apps  | Cross-platform apps              | .NET MAUI, Xamarin|
| Blazor Apps  | Web apps med C#                  | Blazor WebAssembly|
| Cloud Apps   | Kør i skyen                      | Azure + .NET      |

[Home](#indholdsfortegnelse)
# Klasser

En klasse i C# har:

- **Felter:** Data
- **Metoder:** Adfærd
- **Konstruktør:** Initialisering

### Eksempel

```csharp
public class Person
{
    public string Navn;
    public int Alder;

    public Person(string navn, int alder)
    {
        Navn = navn;
        Alder = alder;
    }

    public void SigHej()
    {
        Console.WriteLine($"Hej, mit navn er {Navn} og jeg er {Alder} år gammel.");
    }
}
```

### Oprettelse af et objekt af klassen Person

```csharp
class Program
{
    static void Main()
    {
        Person p = new Person("Anna", 30);
        p.SigHej();
    }
}
```

📖 Vigtige elementer i klasser
1.	Felter (Properties/Fields)
Data, som et objekt indeholder.
Eksempel: public string Navn;
2.	Metoder
Funktioner, der definerer objektets adfærd.


Opsummering
-	En klasse er en skabelon for objekter.
-	Den definerer egenskaber og metoder, som objekterne vil have.
-	Konstruktøren bruges til at initialisere objekter.
-	Klassen bliver til en type
- Forretningsobjekt betyder den aktuelle klasse

---  

[Home](#indholdsfortegnelse)
# Objekt

I C# er et objekt en instans af en klasse – det vil sige, at det er et konkret stykke data, der er oprettet ud fra en skabelon (klassen). Objekter indeholder felter (data) og metoder (funktioner), som man kan bruge til at repræsentere og arbejde med virkelige ting.

## Klasse og objekt
- Klasse = Skabelon  
- Objekt = Et faktisk "eksemplar" af skabelonen

### Eksempel: Bil-klasse og bil1-objekt

```csharp
// Klasse (skabelon)
public class Bil
{
    public string Mærke;
    public string Model;
    public int År;

    public void Start()
    {
        Console.WriteLine($"{Mærke} {Model} starter...");
    }
}
```

### Opret objekt (instans) af klassen

```csharp
class Program
{
    static void Main()
    {
        Bil bil1 = new Bil();
        bil1.Mærke = "Toyota";
        bil1.Model = "Corolla";
        bil1.År = 2020;
        bil1.Start();  // Output: "Toyota Corolla starter..."
    }
}
```

### Hvad kan et objekt?

| Objektkomponent | Eksempel |
|-----------------|----------|
| Data (felter)   | `bil1.Mærke = "Toyota"` |
| Metoder         | `bil1.Start()` |
| Skabt med `new` | `new Bil()` |

### Opsummering

- Et objekt i C# er en instans af en klasse
- Det indeholder både data og funktionalitet
- Du opretter objekter med `new`
- Objekter bruges til at repræsentere virkelige ting i koden

---
[Home](#indholdsfortegnelse)
# Entity

En Entity er en objektorienteret repræsentation af et forretningsobjekt. Det har typisk en unik identitet og bruges ofte i forbindelse med ORM som Entity Framework.

### Hvad kendetegner en Entity?
- Unik identitet
- Levetid (kan oprettes, ændres og slettes)
- Forretningslogik

### Eksempel: Customer entity

```csharp
public class Customer
{
    public int CustomerId { get; private set; }
    public string Name { get; set; }
    public string Email { get; set; }

    public Customer(int customerId, string name, string email)
    {
        CustomerId = customerId;
        Name = name;
        Email = email;
    }

    public bool IsEmailValid()
    {
        return Email.Contains("@");
    }

    public void DisplayCustomerInfo()
    {
        Console.WriteLine($"Kunde ID: {CustomerId}, Navn: {Name}, Email: {Email}");
    }
}
```

### Brug

```csharp
Customer customer1 = new Customer(1, "Alice", "alice@example.com");
customer1.DisplayCustomerInfo();
bool isEmailValid = customer1.IsEmailValid();
```

### Relationer

```csharp
public class Order
{
    public int OrderId { get; set; }
    public DateTime OrderDate { get; set; }
    public int CustomerId { get; set; }
    public Customer Customer { get; set; }
}
```

### Opsummering
- Entity er en forretningsenhed med unik identitet
- Bruges med ORM som Entity Framework


---

[Home](#indholdsfortegnelse)
# Forretningsobjekt

Et forretningsobjekt i C# er en klasse, der indeholder både data og forretningslogik og bruges til at repræsentere virkelige begreber som kunder og ordrer.

### Hvad er det?
- Indeholder relevante data
- Har forretningslogik
- Repræsenterer en enhed i systemet


---
[Home](#indholdsfortegnelse)
## Separation of Concerns

Et designprincip, hvor hver sektion i koden har ét ansvar. Det gør koden nemmere at vedligeholde og teste.

### Principper
- Enkel ansvar (Single Responsibility)
- Modularisering
- Loose coupling
- High cohesion


---
[Home](#indholdsfortegnelse)
# Code First


En tilgang i Entity Framework, hvor du starter med C#-klasser og genererer databasen derfra.

### Fordele
- Fuld kontrol over kode og modeller
- Let at ændre struktur og migrere


---
[Home](#indholdsfortegnelse)
## Encapsulation

Indkapsling betyder at beskytte data og give adgang gennem offentlige metoder.

### Uden encapsulation

```csharp
public class Person
{
    public string Name;
    public int Age;
}
```

### Med encapsulation

```csharp
public class Person
{
    private string name;
    private int age;

    public string Name
    {
        get { return name; }
        set { if (!string.IsNullOrWhiteSpace(value)) name = value; }
    }

    public int Age
    {
        get { return age; }
        set { if (value >= 0) age = value; }
    }
}
```

### Fordele

| Fordel               | Forklaring |
|----------------------|------------|
| Beskytter data       | Forhindrer ugyldige værdier |
| Skjuler kompleksitet | Intern logik skjules |
| Forbedrer vedligeholdelse | Ændringer sker ét sted |
| Muliggør validering  | Du kan kontrollere, hvad der bliver sat |

[Home](#indholdsfortegnelse)
# ICollection
I C# er ICollection<T> en generisk interface, der repræsenterer en generel samling af objekter, som kan tilgås individuelt. Den er en del af System.Collections.Generic og er basisklasse for mange typer som List<T>, HashSet<T>, osv.

 Hvad er ICollection<T>?
ICollection<T> giver dig mulighed for at:
- Tilføje elementer (Add)
- Fjerne elementer (Remove)
- Tjekke om et element findes (Contains)
- Tælle antal elementer (Count)
- Rydde hele samlingen (Clear)
- Iterere over elementer (foreach, fordi den arver IEnumerable<T>)


Hvad bruger man det til?
Typisk bruger man ICollection<T> som property type i modeller, især i Entity Framework, når der er relationer mellem objekter.

Eksempel:
```csharp
public class Author
{
    public int Id { get; set; }
    public string Name { get; set; }

    // En forfatter kan have mange bøger
    public ICollection<Book> Books { get; set; }
}
```


```csharp
public class Book
{
    public int Id { get; set; }
    public string Title { get; set; }

    public int AuthorId { get; set; }
    public Author Author { get; set; }
}
```

Her bruges ICollection<Book> til at vise, at en Author kan have flere Book-objekter.

Hvorfor ikke bare List<T>?
Abstraktion: ICollection<T> giver fleksibilitet – man er ikke bundet til én konkret type som List<T>.

Testbarhed: Det er lettere at udskifte implementation i tests.

Entity Framework bruger typisk ICollection<T> til navigation properties, men mapper det bag kulisserne til f.eks. HashSet<T>.

[Home](#indholdsfortegnelse)
# Repository og interface
Når du laver en .NET API, er brugen af repository pattern og interfaces en god softwarearkitektur-praksis. Her er en grundig forklaring på hvorfor det er en fordel:

### Hvad er en Repository og et Interface?
**Repository**: Et repository er en klasse, som indeholder logikken for at hente og gemme data (typisk i en database). Det fungerer som et mellemled mellem databasen og din controller/service.

**Interface** (IRepository): Et interface definerer hvilke metoder repositoryet skal have (fx GetAll(), Add(), Delete()), uden at fortælle hvordan de implementeres.

### Fordele ved at bruge Repository og Interface
**1. Adskillelse af ansvar (Separation of Concerns)**
Din controller (API endpoint) skal ikke bekymre sig om hvordan data hentes fra databasen – det overlades til repositoryet.

Resultat: Koden bliver mere overskuelig og lettere at vedligeholde.

**2. Lettere at teste**
Du kan bruge en mock af interfacet (IRepository) i dine tests i stedet for at bruge den rigtige database.

Det gør unit tests hurtige og pålidelige.

**3. Fleksibilitet og udskiftelighed**
Hvis du skifter database (f.eks. fra SQL Server til MongoDB), skal du kun ændre repository-implementeringen – ikke resten af applikationen.

Du kan også bruge flere implementeringer af samme interface, fx en in-memory version til test og en database-version til produktion.

**4. Genbrug og fælles logik**
Repositoryet kan indeholde fælles datalogik (f.eks. søgning, filtrering, validering) som ellers ville blive gentaget i flere controllers.

**5. Rydder op i controlleren**
Controlleren bliver meget "ren" og fokuserer kun på at håndtere HTTP-anmodninger og svar:

```csharp
[HttpGet]
public async Task<IActionResult> GetAllMovies()
{
    var movies = await _movieRepository.GetAllAsync();
    return Ok(movies);
}
```


### Simpelt eksempel:
Interface:
```csharp
public interface IRepository<T>
{
    Task<IEnumerable<T>> GetAllAsync();
    Task<T> GetByIdAsync(int id);
    Task AddAsync(T entity);
    Task DeleteAsync(int id);
}
```


### Repository (implementering):
```csharp
public class MovieRepository : IRepository<Movie>
{
    private readonly AppDbContext _context;

    public MovieRepository(AppDbContext context)
    {
        _context = context;
    }

    public async Task<IEnumerable<Movie>> GetAllAsync() => await _context.Movies.ToListAsync();

    public async Task<Movie> GetByIdAsync(int id) => await _context.Movies.FindAsync(id);

    public async Task AddAsync(Movie movie)
    {
        await _context.Movies.AddAsync(movie);
        await _context.SaveChangesAsync();
    }

    public async Task DeleteAsync(int id)
    {
        var movie = await _context.Movies.FindAsync(id);
        if (movie != null)
        {
            _context.Movies.Remove(movie);
            await _context.SaveChangesAsync();
        }
    }
}
```
### Opsummering
Ved at bruge repository pattern og interfaces i din API får du:

- Mere struktureret kode
- Bedre testbarhed
- Større fleksibilitet
- Mindre afhængighed af teknologivalg (f.eks. EF Core)
- Det er især en fordel i mellemstore til store projekter, eller når flere udviklere arbejder sammen.


---

[Home](#indholdsfortegnelse)
# DTO

Når du laver en API, er det en rigtig god idé at bruge DTO'er – Data Transfer Objects. Her får du en forklaring på, hvad en DTO er, hvorfor du bruger den, og hvordan du implementerer det i en .NET API.

DTO står for **Data Transfer Object**, og det er en speciel klasse, du bruger til at sende data mellem din API og klienten (fx Angular frontend).

### 🔹 DTO’er er ikke dine database-modeller
DTO’er bruges til at repræsentere de data, som skal sendes ud eller ind via API'et.

De kan være et udsnit af data, sammenkædede data eller tilpassede visninger, som passer til frontendens behov.

## Hvorfor bruge DTO’er?

### 1. Sikkerhed
Du eksponerer kun de data, du ønsker – fx skal adgangskoder og interne felter ikke sendes til frontend.

```csharp
public class User
{
    public int Id { get; set; }
    public string Username { get; set; }
    public string PasswordHash { get; set; } // Skal ikke ud!
}
```
DTO:
```csharp
public class UserDto
{
    public int Id { get; set; }
    public string Username { get; set; }
}
```

### 2. Fleksibilitet
Du kan lave flere forskellige DTO’er til samme model – fx en kort version og en detaljeret.

### 3. Uafhængighed af database-modellen
Hvis du ændrer i databasen (f.eks. tilføjer nye felter), behøver du ikke ændre API'et eller frontend – DTO'en beskytter mod det.

### 4. Validering og inputkontrol
Når du modtager data (POST/PUT), kan du bruge en DTO til input, som indeholder kun de felter, brugeren skal angive – og du kan tilføje `[Required]`, `[MaxLength]` osv.

## Eksempel på brug

### Model (fx i database):
```csharp
public class Movie
{
    public int Id { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public decimal Budget { get; set; }
    public bool IsDeleted { get; set; }  // Intern brug
}
```

### DTO (output til klienten):
```csharp
public class MovieDto
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string ReleaseYear { get; set; }  // Kun år, ikke dato
}
```

### DTO (input fra klienten):
```csharp
public class CreateMovieDto
{
    [Required]
    public string Title { get; set; }

    [Required]
    public DateTime ReleaseDate { get; set; }

    public decimal Budget { get; set; }
}
```

## 🛠Hvordan bruger man DTO'er i praksis?

Typisk i controlleren:
```csharp
[HttpPost]
public async Task<IActionResult> CreateMovie(CreateMovieDto dto)
{
    var movie = new Movie
    {
        Title = dto.Title,
        ReleaseDate = dto.ReleaseDate,
        Budget = dto.Budget
    };

    await _movieRepository.AddAsync(movie);
    return Ok();
}
```

Til output:
```csharp
[HttpGet("{id}")]
public async Task<ActionResult<MovieDto>> GetMovie(int id)
{
    var movie = await _movieRepository.GetByIdAsync(id);
    if (movie == null) return NotFound();

    var dto = new MovieDto
    {
        Id = movie.Id,
        Title = movie.Title,
        ReleaseYear = movie.ReleaseDate.Year.ToString()
    };

    return Ok(dto);
}
```

## Bonus: Brug AutoMapper
Hvis du har mange felter og mange DTO’er, kan du bruge [AutoMapper](https://automapper.org/) til automatisk at konvertere mellem model og DTO:
```csharp
var dto = _mapper.Map<MovieDto>(movie);
```

## Opsummering

| Fordel        | Beskrivelse                                      |
|---------------|--------------------------------------------------|
| **Sikkerhed** | Skjul interne felter som passwords, IsDeleted    |
| **Fleksibilitet** | Returnér kun de data, som frontend har brug for |
| **Validering** | DTO’er kan bruges til at validere input         |
| **Fremtidssikret** | Din API kan ændres uden at frontend går i stykker |


---

[Home](#indholdsfortegnelse)
# AutoMapper

**AutoMapper** er et populært bibliotek i .NET, der hjælper dig med at **automatisk konvertere mellem to objekter**, typisk mellem:

- **Model** (din database- eller domæneklasse)  
- **DTO** (Data Transfer Object til brug i API)

---

Når du har mange felter, og du skal **oversætte data frem og tilbage** mellem fx `Movie` og `MovieDto`, bliver det hurtigt kedeligt og fejlbehæftet at skrive dette manuelt:

```csharp
var dto = new MovieDto
{
    Id = movie.Id,
    Title = movie.Title,
    ReleaseYear = movie.ReleaseDate.Year.ToString()
};
```

AutoMapper kan gøre det for dig automatisk – med én linje:

```csharp
var dto = _mapper.Map<MovieDto>(movie);
```

---

## Sådan bruger du AutoMapper

### 1. Installer AutoMapper

Installer pakken i dit .NET API-projekt:

```bash
dotnet add package AutoMapper.Extensions.Microsoft.DependencyInjection
```

---

### 2. Opret en Mapping-profil

```csharp
using AutoMapper;

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Movie, MovieDto>()
            .ForMember(dest => dest.ReleaseYear, opt => opt.MapFrom(src => src.ReleaseDate.Year.ToString()));

        CreateMap<CreateMovieDto, Movie>();
    }
}
```

---

### 3. Registrér AutoMapper i `Program.cs`

```csharp
builder.Services.AddAutoMapper(typeof(Program));
```

---

### 4. Brug det i din controller eller service

```csharp
public class MovieController : ControllerBase
{
    private readonly IMapper _mapper;

    public MovieController(IMapper mapper)
    {
        _mapper = mapper;
    }

    [HttpGet("{id}")]
    public async Task<ActionResult<MovieDto>> GetMovie(int id)
    {
        var movie = await _movieRepository.GetByIdAsync(id);
        if (movie == null) return NotFound();

        var dto = _mapper.Map<MovieDto>(movie);
        return Ok(dto);
    }
}
```

---

## Fordele ved AutoMapper

| Fordel              | Beskrivelse                                               |
|---------------------|-----------------------------------------------------------|
| **Reducerer kode**  | Slipper for at skrive og vedligeholde ensformet kode     |
| **Minimerer fejl**  | Undgår manuelle tastefejl i mapping                      |
| **Nem at vedligeholde** | Ændringer i model eller DTO kræver kun ændring ét sted |
| **Støtter avanceret mapping** | Kan håndtere konverteringer, betingelser, navneændringer osv. |


---  


## Simpelt eksempel på brug af automapper:


**Mapping C# data with AutoMapper - Complete tutorial for beginners**


af tutorialsEU - C#


[Mapping C# data with AutoMapper](https://youtu.be/Ijw0Q5EOqmE?si=yJwzY79JFnzoNy_K)


---
[Home](#indholdsfortegnelse)
# JWT?

JWT står for **JSON Web Token**. Det er en standard til at sende oplysninger mellem to parter som et **signeret token** i JSON-format. Det bruges typisk til **autentificering og autorisation** i web-API'er.

---

## Struktur af et JWT-token

Et JWT-token består af tre dele, adskilt af punktummer:

header.payload.signature

### Eksempel:

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJ1c2VySWQiOiIxMjM0IiwibmFtZSI6IkFkbWluIn0.
s3cr3tS1gn4tur3

Her kan du tjekke JWT: 
[JSON Web Token (JWT) Debugger](https://jwt.io/)

---

## Hvordan virker JWT i et API?

1. **Login**:
   - Klienten sender brugernavn og adgangskode til API'et.
   - Hvis login er korrekt, genererer serveren et JWT-token og returnerer det til klienten.

2. **Brug af token**:
   - Klienten gemmer JWT-token (typisk i lokal storage).
   - Ved efterfølgende API-kald sendes token med som header:
     ```
     Authorization: Bearer <token>
     ```

3. **Validering**:
   - Serveren validerer signaturen og evt. udløbstid.
   - Hvis token er gyldigt, får klienten adgang.

---

## Fordele ved JWT

- **Stateless**: Serveren behøver ikke gemme sessionsdata.
- **Skalerbart**: Let at bruge i distribuerede systemer.
- **Sikkert**: Brug af hemmeligheder og digitale signaturer.

---

## Eksempel på brug i .NET

```csharp
// Generering af token i AuthService
var tokenHandler = new JwtSecurityTokenHandler();
var key = Encoding.ASCII.GetBytes("your_secret_key");
var tokenDescriptor = new SecurityTokenDescriptor
{
    Subject = new ClaimsIdentity(new Claim[]
    {
        new Claim(ClaimTypes.Name, user.UserName),
        new Claim(ClaimTypes.Role, "Admin")
    }),
    Expires = DateTime.UtcNow.AddHours(1),
    SigningCredentials = new SigningCredentials(new SymmetricSecurityKey(key), SecurityAlgorithms.HmacSha256Signature)
};

var token = tokenHandler.CreateToken(tokenDescriptor);
return tokenHandler.WriteToken(token);

```
JWT er en effektiv og sikker måde at håndtere autentificering i moderne web-API'er. Det giver en enkel metode til at overføre brugerinformation på en pålidelig måde.


## I denne video vises, hvordan man kan opsætte JWT:

JWT Authentication with .NET 9 🚀 Full Course with Roles, JSON Web Tokens & Refresh Tokens


af Patrick God


[JWT Authentication with .NET 9](https://youtu.be/6EEltKS8AwA?si=HVy8LPn0cKQq6Aqy)


---
[Home](#indholdsfortegnelse)
# Bcrypt
Bcrypt – Sikker adgangskode-hashning

**Bcrypt** er en populær og sikker metode til at hashe adgangskoder. I stedet for at gemme brugerens adgangskode i klartekst i databasen, konverteres den til en hash-værdi, som ikke kan dekrypteres tilbage til den oprindelige adgangskode.

### Hvorfor bruge Bcrypt?

- Beskytter brugernes adgangskoder, selv hvis databasen bliver lækket.
- Tilføjer en **salt** (tilfældig værdi) automatisk, som gør hver hash unik – også hvis to brugere har samme adgangskode.
- Er langsom med vilje for at modvirke brute-force-angreb.

### Hvordan virker det i et API?

1. **Registrering**:
   - Brugeren sender en adgangskode.
   - API'et hasher adgangskoden med `bcrypt`.
   - Den hashede værdi gemmes i databasen.

2. **Login**:
   - Brugeren indtaster adgangskode.
   - API'et henter den gemte hash fra databasen.
   - `bcrypt` sammenligner input med hash og validerer adgangskoden.

### Eksempel i C# (.NET)

```csharp
using BCrypt.Net;

// Hash en adgangskode
string hashedPassword = BCrypt.Net.BCrypt.HashPassword("MinSikreKode123!");

// Sammenlign input med hash
bool isValid = BCrypt.Net.BCrypt.Verify("MinSikreKode123!", hashedPassword);
```

### Installation (NuGet)
dotnet add package BCrypt.Net-Next

### Link for at hente Bcrypt
[Bcrypt.Net-Next på NuGet](https://www.nuget.org/packages/BCrypt.Net-Next)

Link til video, der forklarer om lagring af passwords:

## Password Storage Tier List: encryption, hashing, salting, bcrypt, and beyond

af Studying With Alex

[Encryption, hashing, salting, bcrypt](https://youtu.be/qgpsIBLvrGY?si=zRK5TiXdCTYSV9Sn)

---
[Home](#indholdsfortegnelse)
# Models

## Ental og flertal i `ApplicationDbContext`

Det er vigtigt at forstå navngivningskonventionerne for **ental og flertal** i forhold til **modeller (klasser)** og **DbSet-properties**.

---

### Ental: Modellens navn

Modeller (classes) repræsenterer **én enkelt enhed** – altså **ental**.

**Eksempler:**

- `Artist` = én kunstner  
- `Cover` = ét cover  
- `Author` = én forfatter  
- `Book` = én bog  
- `User` = én bruger  

Disse klasser definerer, hvilke felter (properties) der findes på en enkelt post i databasen.

---

## Flertal: DbSet-navnet

`DbSet<T>` repræsenterer en **samling** af entiteter – altså **flertal**. Derfor navngives disse typisk i flertal.

**Eksempler:**

- `DbSet<Artist> Artists` = en samling af mange kunstnere  
- `DbSet<Cover> Covers` = en samling af mange covers  
- `DbSet<Book> Books` = en samling af mange bøger  

Navnet bruges også af Entity Framework til at navngive tabeller i databasen – *medmindre du angiver andet*.

---

### Kort opsummering

| Klasse (Ental) | DbSet (Flertal) | Beskrivelse                          |
|----------------|------------------|--------------------------------------|
| `Artist`       | `Artists`        | Én kunstner vs. mange kunstnere     |
| `Cover`        | `Covers`         | Ét cover vs. mange covers           |
| `Book`         | `Books`          | Én bog vs. mange bøger              |
| `Author`       | `Authors`        | Én forfatter vs. mange forfattere   |
| `User`         | `Users`          | Én bruger vs. mange brugere         |

---

### God praksis

- **Entalsnavn** til klasserne (fordi det er én entitet).  
- **Flertalsnavn** til `DbSet`-egenskaber (fordi det er en samling).

### Modelbuilder
Man kan også tilpasse tabelnavnene direkte ved at bruge modelBuilder i OnModelCreating
```csharp
public class ApplicationDbContext : DbContext
{
    public DbSet<Artist> Artists { get; set; }
    public DbSet<Cover> Covers { get; set; }
    public DbSet<ArtistCover> ArtistCovers { get; set; }
    public DbSet<Book> Books { get; set; }
    public DbSet<Author> Authors { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        // Her overskriver vi tabelnavnene
        modelBuilder.Entity<Artist>().ToTable("Artist");
        modelBuilder.Entity<Cover>().ToTable("Cover");
        modelBuilder.Entity<ArtistCover>().ToTable("ArtistCover");
        modelBuilder.Entity<Book>().ToTable("Book");
        modelBuilder.Entity<Author>().ToTable("Author");
        modelBuilder.Entity<User>().ToTable("User");
    }
}
```

---

[Home](#indholdsfortegnelse)
# Fluent API
**Fluent API** er en måde at konfigurere din datamodel i Entity Framework Core ved hjælp af metodekædning (method chaining) i `OnModelCreating`-metoden i din `DbContext`.

Det er et alternativ (og supplement) til at bruge **data annotations** (som f.eks. `[Required]`, `[Key]`, `[MaxLength]` osv. direkte på modelklasser).

Eksempel på Fluent API:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Book>()
        .HasKey(b => b.Id);

    modelBuilder.Entity<Book>()
        .Property(b => b.Title)
        .IsRequired()
        .HasMaxLength(100);
}
```

---

### Hvorfor bruge Fluent API?

### 1. **Avanceret konfiguration**
Nogle konfigurationer kan **kun** laves med Fluent API, fx:
- Komplekse relationer (f.eks. mange-til-mange uden eksplicit join-entity)
- Shadow properties
- Konfiguration af indekser
- Tabelnavne, kolonnenavne og skemaer

### 2. **Central konfiguration**
Alt model-relateret konfiguration samles ét sted (`OnModelCreating`). Det giver et klart overblik over hele databasens struktur uden at skulle åbne hver enkelt modelklasse.

### 3. **Adskillelse af domænemodel og databasekonfiguration**
Med Fluent API undgår du at blande databaserelateret logik (som `[Column("navn")]`) direkte i dine modelklasser.

---

### Fluent API vs Data Annotations

| Funktion                  | Data Annotation | Fluent API   |
|---------------------------|------------------|---------------|
| Simpel konfiguration       | ✅               | ✅             |
| Kompleks relationer        | ❌               | ✅             |
| Bedre overblik i ét sted   | ❌               | ✅             |
| Eksterne mapper-filer (separation) | ❌ | ✅             |
| Shadow properties          | ❌               | ✅             |

---

### God praksis

- Brug **data annotations** til simple krav (som `[Required]`, `[MaxLength]`).
- Brug **Fluent API** til komplekse regler, relationer, constraints og konventioner.
- Overvej at opdele Fluent-konfiguration i separate konfigurationsklasser (med `IEntityTypeConfiguration<T>`), hvis projektet vokser.

---

### Eksempel på relation med Fluent API

```csharp
modelBuilder.Entity<Author>()
    .HasMany(a => a.Books)
    .WithOne(b => b.Author)
    .HasForeignKey(b => b.AuthorId);
```

Dette definerer en én-til-mange-relation mellem `Author` og `Book`.

---

### Konklusion

Fluent API er nødvendigt, når du skal have **fuld kontrol** over, hvordan dine entiteter kortlægges til databasen. Det er kraftfuldt, fleksibelt og ofte uundværligt i større projekter eller avancerede databasedesigns.

---
[Home](#indholdsfortegnelse)
# Controllers
En **Controller** i ASP.NET Core fungerer som en **formidler** mellem brugerens input og applikationens logik. Controlleren **modtager HTTP-anmodninger**, behandler dem og returnerer **svar (response)** til brugeren, ofte i form af **HTML, JSON** eller **XML**.

### Rolle og Ansvar

### 1. **Modtager HTTP-anmodninger**
Controlleren håndterer indgående HTTP-anmodninger fra klienter (for eksempel browsere). Den bestemmer, hvordan anmodningen skal behandles baseret på ruten og den anmodede URL.

### 2. **Behandler Anmodningen**
Controlleren kan interagere med **services**, **repositories** eller **databasen** for at hente eller opdatere data. Den kan også validere input og anvende forretningslogik.

### 3. **Sender et Svar (Response)**
Controlleren sender et **svar** tilbage til klienten. Dette svar kan være:
- En **HTML-side** (i en MVC-applikation)
- Et **JSON-objekt** (i en API-applikation)
- En **fil**, f.eks. en CSV eller et billede.

### Struktur af en Controller

En controller består typisk af:
- **Action Methods**: Metoder, der svarer på HTTP-anmodninger som GET, POST, PUT, DELETE osv.
- **Dependency Injection**: Controlleren bruger typisk afhængigheder (services, repositories) via dependency injection for at undgå tæt kobling.
- **Attributter**: Brug af attributter som `[HttpGet]`, `[HttpPost]`, `[Route]` hjælper med at specificere, hvordan controlleren håndterer anmodninger.

### Eksempel på Controller

Her er et simpelt eksempel på en controller i en API-applikation:

```csharp
using Microsoft.AspNetCore.Mvc;
using MyApplication.Models;
using MyApplication.Repositories;

namespace MyApplication.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class BooksController : ControllerBase
    {
        private readonly IBookRepository _bookRepository;

        // Constructor Injection
        public BooksController(IBookRepository bookRepository)
        {
            _bookRepository = bookRepository;
        }

        // GET api/books
        [HttpGet]
        public ActionResult<IEnumerable<Book>> GetBooks()
        {
            var books = _bookRepository.GetAllBooks();
            return Ok(books);
        }

        // POST api/books
        [HttpPost]
        public ActionResult CreateBook([FromBody] Book newBook)
        {
            _bookRepository.AddBook(newBook);
            return CreatedAtAction(nameof(GetBooks), new { id = newBook.BookId }, newBook);
        }
    }
}
```

### Forklaring:
- **`[Route("api/[controller]")]`**: Definerer, hvordan ruten til controlleren ser ud. `[controller]` erstattes med controllerens navn (i dette tilfælde `Books`).
- **`[ApiController]`**: Angiver, at controlleren er en API-controller og automatisk håndterer nogle almindelige opgaver som automatisk modelbinding og validering.
- **`ActionResult`**: En type, der definerer svaret på anmodningen, f.eks. `Ok()`, `CreatedAtAction()` eller `NotFound()`.

### Best Practices

1. **Adskillelse af ansvar**: Controlleren bør ikke indeholde forretningslogik. Den bør bruge services eller repositories til at udføre forretningslogik.
2. **Slim controllers**: Hold controlleren så enkel som muligt. Brug tjenester (services) til at håndtere komplekse operationer.
3. **Standard HTTP-statuskoder**: Sørg for, at din controller returnerer passende HTTP-statuskoder som 200 (OK), 201 (Created), 400 (Bad Request), 404 (Not Found), 500 (Internal Server Error).
4. **Afhængighedsinjektion**: Brug afhængighedsinjektion til at få adgang til de nødvendige tjenester, så controlleren er løs koblet og lettere at teste.



## Gennemgang af Web API Controller i ASP.NET Core

```csharp
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
```
---

### 🔹 `[ApiController]` – Attribut for Web API-funktionalitet

### Formål:
Markerer klassen som en **Web API-controller**, hvilket aktiverer en række forbedringer og konventioner i ASP.NET Core.

### Effekter af `[ApiController]`:

1. **Model binding og validering automatisk:**
   - Hvis du fx har en `[FromBody] MyDto dto`-parameter, og modellen ikke er gyldig, returnerer ASP.NET Core automatisk `400 Bad Request` med valideringsfejl.

2. **Konventioner for parameterkilder:**
   - Du behøver ofte ikke angive `[FromBody]` – det antages automatisk baseret på parameterens type.

3. **ProblemDetails-respons:**
   - Ved valideringsfejl returneres en struktureret JSON-respons i [RFC 7807 ProblemDetails](https://tools.ietf.org/html/rfc7807)-format.

4. **Automatisk håndtering af binding errors.**

Det er stærkt anbefalet at bruge `[ApiController]` i alle Web API-controllere.

---

### `[Route("[controller]")]` – Routing-attribut

### Formål:
Angiver **basis-ruten** (URL-stien) for controllerens endpoints.

### Hvad betyder `"[controller]"`?
Det er en **route-token** – en placeholder, der automatisk bliver erstattet med controllerens navn **uden "Controller"**-suffikset.

> Eksempel: `WeatherForecastController` bliver til ruten:
```
/WeatherForecast
```

### Sammen med `[HttpGet]` eller `[HttpPost]`:
```csharp
[HttpGet] // GET /WeatherForecast
public IEnumerable<WeatherForecast> Get() { ... }

[HttpPost("create")] // POST /WeatherForecast/create
public IActionResult Create([FromBody] WeatherForecast item) { ... }
```

---

### 🔹 `public class WeatherForecastController : ControllerBase`

### Det er din Web API-controllerklasse

- **`public`**: Klassen skal være tilgængelig for routing-mekanismen.
- **`WeatherForecastController`**: Konventionelt navngivet, slutter på `Controller`.
- **`: ControllerBase`**: Arver fra `ControllerBase`, som er basisklassen for Web API-controllere.

### Hvad får du med `ControllerBase`?
- `Ok()`, `NotFound()`, `BadRequest()` m.fl. – metoder til at returnere HTTP-responskoder og data.
- Model binding.
- Routing-integration.

> Web API-controllere arver fra `ControllerBase`, mens MVC-controllere arver fra `Controller`.

---

## Samlet eksempel

```csharp
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        // Kaldes ved GET /WeatherForecast
    }
}
```

Når du kalder `/WeatherForecast` med en GET-request, håndteres det af `Get()`-metoden. `[ApiController]` sikrer automatisk validering og passende svar.

### Sammenfatning

- En **Controller** håndterer **HTTP-anmodninger**, interagerer med **services** eller **databasen** og sender et **svar** tilbage til klienten.
- I en **API**-applikation er controlleren ansvarlig for at returnere **data** i form af JSON eller andre formater.
- Det er vigtigt at følge **best practices** for at holde controlleren **slank** og ansvarlig kun for routing og simpel datahåndtering.


---
[Home](#indholdsfortegnelse)
# Services
En **service** i et API er en klasse, der indeholder **forretningslogik** – altså den kode, som bestemmer "hvordan ting fungerer" i applikationen. Den adskiller sig fra en controller, som kun håndterer HTTP-forespørgsler og -svar. Services sikrer en ren **Separation of Concerns**, hvor controlleren er tynd, og logikken er samlet ét sted.

## Eksempel: `AuthService`

```csharp
public class AuthService
{
    private readonly IUserRepository _userRepository;

    public AuthService(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }

    // Eksempel på metode:
    public User? Authenticate(string username, string password)
    {
        var user = _userRepository.GetUserByUsername(username);
        if (user == null) return null;

        // Her kunne man tjekke password med bcrypt, fx:
        // if (!BCrypt.Net.BCrypt.Verify(password, user.Password)) return null;

        return user;
    }
}

```

### Hvad gør denne service?
AuthService er en klasse, som står for autentificering (logik for login).

Den bruger et IUserRepository til at tilgå data – den kommunikerer med datalaget (database).

Den kunne også håndtere ting som token-generering (JWT), adgangsrettigheder og hashing af passwords.

Hvorfor bruge services?
- Genbrug: Logik genbruges flere steder i koden (fx fra både controller og tests).
-  Testbarhed: Du kan let enhedsteste logik uden at involvere HTTP eller databasen direkte.
-  Vedligeholdelse: Koden er lettere at læse, ændre og udvide.
-  Adskillelse af ansvar: Controller håndterer HTTP, service håndterer logik, repository håndterer data.

### Typisk struktur i en ASP.NET Core API
**Controller:** Modtager HTTP-request → kalder service

**Service:** Indeholder forretningslogik → kalder repository

**Repository:** Henter eller gemmer data i databasen

### Eksempel på flow:
```csharp
[HTTP Request] → Controller → Service → Repository → Database
```

### Opsummering
Lag	Ansvar
Controller	HTTP request/response
Service	Forretningslogik
Repository	Dataadgang (database-CRUD)

Det gør din API robust, fleksibel og nemmere at teste og vedligeholde.


---
[Home](#indholdsfortegnelse)
# Scalar

`Scalar.AspNetCore` er et letvægtsalternativ til Swagger UI i ASP.NET Core. Det genererer en statisk HTML-dokumentation for dine endpoints og fungerer godt med både Controllers og Minimal APIs.

---

### Hvad er Scalar?

- Et .NET-værktøj til dokumentation af Web APIs
- Understøtter Controllers og Minimal APIs
- Hurtig og statisk HTML – ingen JavaScript eller Swagger UI kræves
- API-udtræk i OpenAPI JSON/YAML og HTML
- Brugervenligt og let at tilføje

---

### Installation

Tilføj pakken til dit projekt:

```bash
dotnet add package Scalar.AspNetCore
```

### Opsætning i Program.cs

```csharp
using Scalar.AspNetCore;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddOpenApi(); // Aktiverer Scalar metadata

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();              // Tilgængelig på /openapi.json og /openapi.yaml
    app.MapScalarApiReference();   // Dokumentation på /scalar/v1
}

app.MapControllers();
app.Run();

```

**Kør applikationen og åbn browseren til:**
```csharp
https://localhost:5001/scalar/v1
```

**Du kan filtrere dokumentationen via tags:**
```csharp
/scalar/v1#Movies

```

### Eksempelcontroller med tag

```csharp
using Microsoft.AspNetCore.Mvc;

namespace MyApi.Controllers
{
    [ApiController]
    [Route("movies")]
    [Tags("Movies")] // <- Vigtigt for Scalar
    public class MoviesController : ControllerBase
    {
        [HttpGet]
        public IActionResult GetAll() => Ok(new[] { "Movie 1", "Movie 2" });

        [HttpPost]
        public IActionResult Create(string title) => Ok($"Created: {title}");
    }
}

```

### Opsummering
- Scalar er et moderne alternativ til Swagger
- Brug [Tags("Navn")] for at organisere endpoints
- Tilgå /scalar/v1#<tag> for direkte dokumentationslinks
- Fungerer godt til udviklingsmiljøer og automatiseret dokumentation




---
[Home](#indholdsfortegnelse)
# CORS
CORS i ASP.NET Core API
**CORS (Cross-Origin Resource Sharing)** er en sikkerhedsmekanisme i browsere, der begrænser hvilke domæner der må foretage HTTP-anmodninger til din API fra en anden origin (domæne, port, protokol).


### Eksempel:
Et Angular-frontend på `http://localhost:4200` vil normalt få **CORS-fejl**, hvis det forsøger at kalde en API på `https://localhost:7031`, medmindre du tillader det via CORS-politik.

---

### Opsætning af CORS i `Program.cs`

1. **Definér en CORS-politik** i `builder.Services`
2. **Aktivér den** i pipeline med `UseCors`

```csharp
var builder = WebApplication.CreateBuilder(args);

// 1️⃣ Registrer CORS-politik
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowFrontend", policy =>
    {
        policy.WithOrigins("http://localhost:4200") // Tilpas til din frontend
              .AllowAnyHeader()
              .AllowAnyMethod();
    });
});

builder.Services.AddControllers();
builder.Services.AddOpenApi(); // (valgfrit, fx til Scalar eller Swagger)

var app = builder.Build();

// 2️⃣ Brug CORS-politikken i middleware-pipelinen
app.UseCors("AllowFrontend");

if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
    app.MapScalarApiReference();
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();
app.Run();
```

---

### Typiske CORS-politikker

| Situation                        | Konfiguration                                                    |
|----------------------------------|------------------------------------------------------------------|
| Udvikling med Angular eller React | `WithOrigins("http://localhost:4200")`                         |
| Tillad alle domæner (ikke anbefalet i produktion) | `.AllowAnyOrigin()`                                  |
| Begrænsede HTTP-metoder          | `.WithMethods("GET", "POST")`                                   |
| Kun visse headers                | `.WithHeaders("Content-Type", "Authorization")`                 |

---

### CORS fejl du kan møde

| Fejlmeddelelse i browseren                  | Forklaring |
|---------------------------------------------|------------|
| `has been blocked by CORS policy`          | Din API tillader ikke anmodningen fra den oprindelige origin |
| `Response to preflight request doesn't pass access control check` | Preflight-anmodning (OPTIONS) mangler tilladelse |

---

### Test med frontend

Når din frontend kører fra fx `http://localhost:4200` og kalder en endpoint på API’et (`https://localhost:7031/api/books`), skal API’en returnere CORS headers som:

```
Access-Control-Allow-Origin: http://localhost:4200
```

Dette sker kun, hvis `UseCors()` er korrekt konfigureret.

---

###  Anbefalinger

- Brug kun `AllowAnyOrigin()` i udvikling
- Brug navngivne politikker til at kontrollere CORS-strategier
- Log fejlsvar og statuskoder i din browser og backend under debugging

---

### Ressourcer

- [Microsoft Docs: Enable CORS](https://learn.microsoft.com/aspnet/core/security/cors)
- [MDN Web Docs: CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)



###  Konklusion

CORS er afgørende for at tillade frontend-applikationer at kommunikere sikkert med din backend. Med korrekt konfiguration i `Program.cs` undgår du fejlagtige blokeringer og gør din API klar til brug fra moderne SPAs som Angular, React og Vue.


---
[Home](#indholdsfortegnelse)
# Arv
Arv (Inheritance) i C#
Arv er et fundamentalt koncept i objektorienteret programmering, hvor en klasse kan arve egenskaber og metoder fra en anden klasse. Den klasse, der arver, kaldes en **afledt klasse** eller **child class**, og den klasse, der arves fra, kaldes en **baseklasse** eller **parent class**.

Dette gør det muligt at genbruge kode og skabe en hierarkisk struktur af klasser.

### Grundlæggende syntaks

```csharp
// Baseklasse
public class Animal
{
    public void Eat()
    {
        Console.WriteLine("Animal is eating.");
    }
}

// Afledt klasse
public class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("Dog barks.");
    }
}
```

Her arver `Dog` klassen fra `Animal` klassen. Det betyder, at en `Dog` kan både `Eat()` (arvet fra `Animal`) og `Bark()` (sin egen metode).

Brug af arv
```csharp
Dog myDog = new Dog();
myDog.Eat();  // Metode fra baseklassen Animal
myDog.Bark(); // Metode fra den afledte klasse Dog
```

Output:
```csharp
Animal is eating.
Dog barks
```

Overriding (polymorfi)
Hvis baseklassen har en metode, som du vil ændre i den afledte klasse, kan du bruge virtual og override:

```csharp
public class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Animal makes a sound.");
    }
}

public class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Dog barks.");
    }
}
```


### Eksempel med Customer

Her er en baseklasse `Customer` med egenskaber og en metode `Validate()`. Derefter fire afledte kundetyper, som kan have deres egne implementationsdetaljer:

```csharp
public abstract class Customer
{
    public string Name { get; set; }
    public string Email { get; set; }

    public Customer(string name, string email)
    {
        Name = name;
        Email = email;
    }

    // Metode der kan overskrives i afledte klasser
    public abstract bool Validate();
}

public class BusinessCustomer : Customer
{
    public string CompanyName { get; set; }

    public BusinessCustomer(string name, string email, string companyName)
        : base(name, email)
    {
        CompanyName = companyName;
    }

    public override bool Validate()
    {
        // Eksempel: tjek at firma og email ikke er tomme
        return !string.IsNullOrEmpty(Email) && !string.IsNullOrEmpty(CompanyName);
    }
}

public class ResidentialCustomer : Customer
{
    public ResidentialCustomer(string name, string email) : base(name, email) { }

    public override bool Validate()
    {
        // Eksempel: tjek at email er udfyldt
        return !string.IsNullOrEmpty(Email);
    }
}

public class GovernmentCustomer : Customer
{
    public string GovernmentAgency { get; set; }

    public GovernmentCustomer(string name, string email, string agency)
        : base(name, email)
    {
        GovernmentAgency = agency;
    }

    public override bool Validate()
    {
        // Eksempel: tjek at agency og email er udfyldt
        return !string.IsNullOrEmpty(Email) && !string.IsNullOrEmpty(GovernmentAgency);
    }
}

public class EducatorCustomer : Customer
{
    public string InstitutionName { get; set; }

    public EducatorCustomer(string name, string email, string institution)
        : base(name, email)
    {
        InstitutionName = institution;
    }

    public override bool Validate()
    {
        // Eksempel: tjek at institution og email er udfyldt
        return !string.IsNullOrEmpty(Email) && !string.IsNullOrEmpty(InstitutionName);
    }
}
```

### Opsummering

| Begreb      | Beskrivelse                                         |
|-------------|----------------------------------------------------|
| **Baseklasse**   | Klassen som andre klasser arver fra.              |
| **Afledt klasse** | Klassen der arver fra baseklassen.               |
| **Arv**         | Genbrug af egenskaber og metoder fra baseklasse. |
| **Abstract klasse** | Klasse der ikke kan instantieres, men skal arves. |
| **Abstract metode** | Metode uden implementering, som afledte klasser SKAL implementere. |

---
[Home](#indholdsfortegnelse)
# Arkitektur   
**Models, DTOs, Controllers, Interfaces, Repositories, Services og AutoMapper**

###  Overblik

En typisk .NET Core-applikation bruger en lagdelt arkitektur for at adskille ansvar og forbedre vedligeholdelse.

* Models
* DTOs (Data Transfer Objects)
* Controllers
* Interfaces
* Repositories
* Services
* AutoMapper

---

### 1. Model (Domain Model / Entity)

**Hvad:**

* Repræsenterer database-objekter
* Bruges af Entity Framework Core til at binde til databasen

**Eksempel:**

```csharp
public class Book
{
    public int BookId { get; set; }
    public string Title { get; set; }
    public Author Author { get; set; }
    public Cover? Cover { get; set; }
}
```

---

### 2. DTO (Data Transfer Object)

**Hvad:**

* Brugerdefineret dataformat til frontend
* Indeholder kun relevante felter
* Beskytter domænemodellen og letter versionering

**Eksempel:**

```csharp
public class BookDto
{
    public string Title { get; set; }
    public string AuthorFullName { get; set; }
}
```

---

### 3. AutoMapper

**Hvad:**

* Automatiserer konvertering mellem Models og DTOs

**Eksempel:**

```csharp
CreateMap<Book, BookDto>()
    .ForMember(dest => dest.AuthorFullName,
               opt => opt.MapFrom(src => src.Author.FirstName + " " + src.Author.LastName));
```

---

### 4. Repository (Data Access Layer)

**Hvad:**

* Håndterer databasekommunikation
* Returnerer models
* Bruger interfaces for testbarhed

**Eksempel:**

```csharp
public interface IBookRepository
{
    Task<List<Book>> GetAllAsync();
    Task<Book?> GetByIdAsync(int id);
}
```

---

### 5. Service (Business Logic Layer)

**Hvad:**

* Indeholder forretningsregler
* Kalder repositories og bruger AutoMapper

**Eksempel:**

```csharp
public class BookService : IBookService
{
    private readonly IBookRepository _repo;
    private readonly IMapper _mapper;

    public async Task<List<BookDto>> GetAllBooksAsync()
    {
        var books = await _repo.GetAllAsync();
        return _mapper.Map<List<BookDto>>(books);
    }
}
```

---

### 6. Controller (API Layer)

**Hvad:**

* Ekspornerer endpoints til frontend
* Kalder services og returnerer DTOs som JSON

**Eksempel:**

```csharp
[ApiController]
[Route("api/[controller]")]
public class BooksController : ControllerBase
{
    private readonly IBookService _service;

    [HttpGet]
    public async Task<ActionResult<List<BookDto>>> GetAll()
    {
        var books = await _service.GetAllBooksAsync();
        return Ok(books);
    }
}
```

---

### Sammenhæng mellem lag

```
[Frontend]
   ↓ (HTTP Request)
[Controller]             <--- Kalder services
   ↓
[Service]                <--- Indeholder logik og bruger AutoMapper
   ↓
[Repository]             <--- Kommunikerer med database
   ↓
[DbContext]              <--- EF Core
   ↑
[Model]                  <--- Domænemodel

   ↓
[AutoMapper]             <--- Mapper Model → DTO
   ↓
[DTO]                    <--- Returneres som JSON til frontend
```

---

### Fordele

* Let at teste og vedligeholde
* DTO beskytter intern struktur
* Services samler forretningslogik
* Repositories kan mockes i tests


---


**Eksempel**

**Endpoint i controller**
```csharp
[HttpGet]
public async Task<ActionResult<IEnumerable<BookDto>>> GetAllBooks()
{
    var books = await _bookService.GetAllBooksAsync();
    return Ok(books);
}

```
**Flowchart**
```bash
[Client: Angular App]
        │
        ▼
HTTP GET Request: /api/books
        │
        ▼
[BooksController.cs] ───────────────────────┐
    public async GetAllBooks()              │
        │                                   │
        ▼                                   │
[IBookService.cs / BookService.cs]          │
    public async GetAllBooksAsync()         │
        │                                   │
        ▼                                   │
[IBookRepository.cs / BookRepository.cs]    │
    public async GetAllAsync()              │
        │                                   │
        ▼                                   │
[DbContext (EF Core)]                       │
    Books.ToListAsync()                     │
        │                                   │
        ▼                                   │
[Database: SQL Server / SQLite / etc.]      │
    -- Henter alle bøger --                 │
        │                                   │
        ▼                                   │
[DbContext] ← Resultater                    │
        │                                   │
        ▼                                   │
[BookRepository] ← Entity list              │
        │                                   │
        ▼                                   │
[BookService] ← Entity list                 │
        │                                   │
        ▼                                   │
DTO Mapping (f.eks. AutoMapper)             │
        │                                   │
        ▼                                   │
[BooksController] ← DTO list                │
        │                                   │
        ▼                                   │
JSON Response ←-----------------------------┘
        │
        ▼
[Client: Angular] ← Vis liste over bøger

```

| Lag            | Beskrivelse                                 |
| -------------- | ------------------------------------------- |
| **Client**     | Angular frontend, sender HTTP GET-anmodning |
| **Controller** | Håndterer HTTP og kalder service-lag        |
| **Service**    | Forretningslogik, abstraherer repository    |
| **Repository** | Kommunikation med databasen via EF Core     |
| **DbContext**  | Entity Framework adgang til database        |
| **Database**   | Gemmer entiteter, fx i SQL Server           |



---
[Home](#indholdsfortegnelse)



# IEnumerable

### Hvad er `IEnumerable<T>`?

`IEnumerable` er et interface i C#, som repræsenterer en sekvens af elementer, som du kan iterere over med fx `foreach`.

```csharp
public interface IEnumerable
{
    IEnumerator GetEnumerator();
}
```

Men ofte bruges den **generiske version**:

```csharp
public interface IEnumerable<T>
{
    IEnumerator<T> GetEnumerator();
}
```

---

### Hvorfor bruge `IEnumerable<T>`?

- Returner en samling **uden at afsløre konkret type** (f.eks. `List<T>`, `T[]`, etc.)
- Understøtter **lazy evaluation** (`yield return`)
- Bruges med `foreach`, LINQ og mere

---

### `<T>` står T for Type Parameter
Det betyder, at T er en generisk type, som bruges som en pladsholder for den type, du vil arbejde med. Når du bruger en generisk klasse eller metode, kan du erstatte T med hvilken som helst datatype, fx int, string, eller en brugerdefineret klasse.

---

### Eksempel

```csharp
public IEnumerable<string> GetNames()
{
    yield return "Alice";
    yield return "Bob";
}
```

Dette bruger `yield return`, så elementer leveres én ad gangen ved behov.

---

### Anvendelse i Repositories

```csharp
public interface IBookRepository
{
    IEnumerable<Book> GetAllBooks();
}
```

```csharp
public IEnumerable<Book> GetAllBooks()
{
    return _context.Books.ToList();
}
```

---

### Sammenligning

| Type             | Beskrivelse                                                  |
|------------------|--------------------------------------------------------------|
| `IEnumerable<T>` | Itererbare datastrukturer, henter data i hukommelsen         |
| `IQueryable<T>`  | Tillader LINQ-forespørgsler, oversættes til SQL              |
| `List<T>`        | Konkrete datastrukturer med mulighed for tilføj/slet direkte |

---

### Opsummering

| Egenskab          | Beskrivelse                                |
|-------------------|---------------------------------------------|
| `IEnumerable<T>`  | Interface til iteration                     |
| Bruges til        | `foreach`, LINQ, abstraktion                |
| Understøtter      | Lazy loading med `yield return`             |
| Implementeres af  | `List<T>`, `Array`, `DbSet<T>`, osv.        |



---
[Home](#indholdsfortegnelse)


# ActionResult
ActionResult er en baseklasse, som mange andre specifikke resultater (som OkResult, NotFoundResult, ViewResult, JsonResult, osv.) arver fra.

### Eksempel – typiske brugsscenarier

```csharp
public class BookController : ControllerBase
{
    // Returnerer 200 OK med data
    [HttpGet("{id}")]
    public ActionResult<BookDto> GetBook(int id)
    {
        var book = _bookService.GetBookById(id);
        if (book == null)
            return NotFound(); // returnerer 404
        return Ok(book);      // returnerer 200 + book i JSON
    }

    // Returnerer 201 Created
    [HttpPost]
    public ActionResult<BookDto> CreateBook(BookDto book)
    {
        var created = _bookService.Create(book);
        return CreatedAtAction(nameof(GetBook), new { id = created.Id }, created);
    }
}
```

### Typer af ActionResult
Her er nogle ofte brugte typer:

| Resultat-type           | Beskrivelse                                   |
|-------------------------|-----------------------------------------------|
| `Ok()`                  | 200 OK uden data                              |
| `Ok(object)`            | 200 OK med JSON-data                          |
| `NotFound()`            | 404 Not Found                                 |
| `BadRequest()`          | 400 Bad Request                               |
| `Unauthorized()`        | 401 Unauthorized                              |
| `NoContent()`           | 204 No Content (ofte ved DELETE)              |
| `CreatedAtAction(...)`  | 201 Created med placering af ressource        |
| `Forbid()`              | 403 Forbidden                                 |


### ActionResult<T>
Når du returnerer både et objekt og et HTTP-statuskode-resultat, bruger man typisk ActionResult<T> – som i ActionResult<BookDto>. Det gør det muligt at:
- Returnere objektet (BookDto) med status 200
- Eller returnere fx NotFound() hvis objektet ikke eksisterer
- Det giver mere fleksibilitet end bare at returnere BookDto.

### Hvornår skal jeg bruge ActionResult?
**Du skal bruge det, når du vil...	Eksempel**
- Returnere forskellige HTTP-statuskoder	200 OK, 404 Not Found, 400 Bad Request osv.
- Returnere data i JSON	return Ok(data);
- Returnere fejlbeskeder	return BadRequest("Invalid input");
- Returnere ressourcer med placering (POST)	CreatedAtAction(...)

### Test af ActionResult<T>
Når du returnerer en **ActionResult<T>** i ASP.NET Core – så har resultatet to mulige veje:

```csharp
public async Task<ActionResult<Book>> CreateBook(BookDto bookDto)
```

Når denne metode returnerer noget, har ActionResult<Book> to felter:

| Ejendom  | Sæt når …                           | Typisk brugt med                                |
| -------- | ----------------------------------- | ----------------------------------------------- |
| `Value`  | du returnerer bare objektet direkte | `return book;`                                  |
| `Result` | du returnerer et IActionResult      | `return Ok(book);`, `return BadRequest();` osv. |

```csharp
// 1. Value bliver sat, Result er null:
return book;

// 2. Result bliver sat (OkObjectResult), Value er null:
return Ok(book);
```
### Eksempel 1 – Value er IKKE null:
```csharp
[HttpPost]
public ActionResult<Book> CreateBook(BookDto bookDto)
{
    var book = new Book { Title = bookDto.Title, BasePrice = bookDto.BasePrice };
    return book; // <- direkte objekt-returnering
}
```
### Eksempel 2 – Value er null:
```csharp
[HttpPost]
public ActionResult<Book> CreateBook(BookDto bookDto)
{
    var book = new Book { Title = bookDto.Title, BasePrice = bookDto.BasePrice };
    return Ok(book); // <- wrapped i IActionResult
}
```

**Du bør kun bruge Ok(book), hvis du skal returnere noget andet end 200 OK afhængig af kontekst (f.eks. BadRequest() hvis noget gik galt).**


---
[Home](#indholdsfortegnelse)

# FromBody
[FromBody] fortæller ASP.NET Core, at parameterens værdi skal læses fra selve HTTP-requestens body – typisk som JSON.

🔹 Eksempel
```csharp
[HttpPost]
public IActionResult CreateBook([FromBody] BookDto book)
{
    // book er nu deserialiseret fra JSON i request body
    _bookService.Create(book);
    return Ok();
}
```
### Hvad sker der bag kulissen?
Når klienten sender en POST-request med JSON-data:

```json
{
  "title": "C# for Beginners",
  "publishYear": 2024
}
```

Så vil ASP.NET Core:
- Læse body'en i HTTP-requesten
- Bruge JSON-deserialisering (via System.Text.Json eller Newtonsoft.Json)
- Lægge resultatet ind i BookDto book

### Hvornår bruger man [FromBody]?
**Brug [FromBody] når ...	Eksempel**
- Du sender JSON-data i POST, PUT eller PATCH	fx fra en frontend, Postman, JavaScript
- Du vil læse et objekt (fx en model eller DTO)	public IActionResult Add([FromBody] BookDto dto)

### Kontrast: [FromQuery] og [FromRoute]
Attribut	Henter data fra ...	Typisk brug
[FromQuery]	Query string (i URL)	GET med filtre/søgning -> fortæller ASP.NET Core, at værdien til parameteren skal læses fra URL'ens query string.
[FromRoute]	Route-del af URL	/api/books/42
[FromBody]	JSON i request body	POST, PUT, PATCH
[FromHeader]	HTTP headers	Custom headers
[FromForm]	Form-data (fx multipart)	Fil-upload

**Eksempel med flere attributter**
```csharp
[HttpPost("{categoryId}")]
public IActionResult AddBook(int categoryId, [FromBody] BookDto book)
{
    // categoryId kommer fra URL (FromRoute implicit)
    // book kommer fra JSON body (FromBody)
    ...
}
```

### TL;DR
[FromBody]: Bruger JSON fra request body

Bruges især ved POST/PUT/PATCH

Gør det muligt at automatisk binde JSON til en C#-model


# FAQ

### Kørsel af VS application

´´´csharp

Fejl: "System.Security.Cryptography.ProtectedData.dll" ... Exceeded retry count of 10. Failed.

´´´

Kan forekomme, hvis Visual Studio køres fra OneDrive


---
[Home](#indholdsfortegnelse)

# ChangeTracker

`ChangeTracker` er en del af EF's `DbContext`, og den holder øje med entiteters tilstand i konteksten:

-  Hvilke entiteter er tilføjet (`Added`)
-  Hvilke er blevet ændret (`Modified`)
-  Hvilke skal slettes (`Deleted`)
-  Hvilke er uændret (`Unchanged`)

---

### Typisk brug

```csharp
var changedEntities = context.ChangeTracker.Entries()
    .Where(e => e.State == EntityState.Modified)
    .ToList();
```

Dette filtrerer alle entiteter i DbContext, der er markeret som ændrede (f.eks. hvis en bruger har opdateret et navn i et objekt).

```csharp
var book = await context.Books.FindAsync(1);
book.Title = "Ny titel";

var entries = context.ChangeTracker.Entries();

foreach (var entry in entries)
{
    Console.WriteLine($"Entity: {entry.Entity.GetType().Name}, State: {entry.State}");
}
```

```yaml
Entity: Book, State: Modified
```

| State       | Forklaring                          |
| ----------- | ----------------------------------- |
| `Added`     | En ny entitet er oprettet           |
| `Modified`  | En eksisterende entitet er ændret   |
| `Deleted`   | En entitet er markeret til sletning |
| `Unchanged` | Ingen ændringer er registreret      |
| `Detached`  | Ikke spores længere af `DbContext`  |


### Avanceret: Ændringer på felt-niveau
```csharp
var entry = context.Entry(book);

foreach (var prop in entry.OriginalValues.Properties)
{
    var original = entry.OriginalValues[prop]?.ToString();
    var current = entry.CurrentValues[prop]?.ToString();

    if (original != current)
    {
        Console.WriteLine($"{prop.Name} changed from '{original}' to '{current}'");
    }
}
```

### Hvornår er det nyttigt?
- Logging af ændringer
- Auditing (hvem ændrede hvad og hvornår)
- Validere ændringer før de gemmes
- Debugging og fejlsøgning


---
[Home](#indholdsfortegnelse)



# Lagstruktur
### Lagene og deres ansvar

### 1. Api (præsentationslaget / WebAPI-projektet)
Det eneste lag der ved noget om HTTP og Web.  
Her hører følgende hjemme:

- ✅ **Controllers**
- ✅ **DTO'er**
- ✅ **Filters** (f.eks. `ActionFilter`, `ExceptionFilter`)
- ✅ **Model binders**
- ✅ **Input validation attributes** (evt. FluentValidation + integration her)
- 🔁 Mapper DTO’er til domain-objekter og omvendt (AutoMapper eller manuelt)

👉 *Tænk på dette lag som et interface mod brugeren eller klienten.*

---

### 2. Domain (forretningslaget / core logic)
Hjertet af din applikation – *domænemodeller og regler*.  
Her hører følgende hjemme:

- ✅ **Entities / Models**
- ✅ **Value Objects**
- ✅ **Interfaces** for fx `IRepository`, `IService` – ingen implementeringer!
- ✅ **Domain Services** (f.eks. regler der ikke naturligt hører til én entitet)
- ✅ **Enums**, **Exceptions**, **Business Rules**

💡 *Det skal være 100% uafhængigt af data/adgang og frameworks.*

---

### 3. Data (infrastruktur / persistence-lag)
Alt det, der har med lagring og dataadgang at gøre.  
Her hører følgende hjemme:

- ✅ **DbContext**
- ✅ **EF Core konfigurationer** (`OnModelCreating`, `EntityTypeConfiguration`)
- ✅ **Repository-implementeringer**
- ✅ **Migreringer** (valgfrit – kan også være i Api)
- ❌ **Ingen domænelogik eller DTO-håndtering**

---

### 4. (Valgfrit) Application-lag (ofte mellem Domain og Api)
Bruges typisk i Clean Architecture / DDD for at adskille *use cases* fra præsentation og domain.

- ✅ **Services** (som orchestration/brugsscenarier, fx `CreateOrderService`)
- ✅ **Command/query classes** (fx med MediatR)
- ✅ **Interfaces til services**
- ✅ **Input/output modeller**
- 🚫 **Ingen EF Core / DbContext**

*Hvis du ikke har et Application-lag endnu, kan du godt lægge services i Api eller oprette det senere.*

---

### Hvor skal hvad ligge?

| Komponent         | Typisk placering               | Forklaring                      |
|------------------|--------------------------------|----------------------------------|
| `BookController` | `Api/Controllers`              | Web API-kald                     |
| `BookDto`        | `Api/DTOs`                     | Data til/fra klient              |
| `IBookService`   | `Domain/Interfaces`            | Kontrakt for logik               |
| `BookService`    | `Data/Services` (eller `Application`) | Implementering            |
| `IBookRepository`| `Domain/Interfaces`            | Abstraktion for dataadgang       |
| `BookRepository` | `Data/Repositories`            | EF Core-implementering           |
| `Book`           | `Domain/Entities`              | Domænemodel                      |
| `AppDbContext`   | `Data`                         | DbContext for EF Core            |
| `Validation`     | `Api/Validation` (eller `Application`) | FluentValidation eller custom logic |
| `ActionFilter`   | `Api/Filters`                  | Fx logging, exceptions           |

---

### Hvad er Domain-Driven Design (DDD)?

DDD handler om at bygge software omkring det **forretningsdomæne**, systemet modellerer.

- **Entities**: Objekter med identitet over tid (f.eks. `Book`, `Order`)
- **Value Objects**: Objekter uden identitet (f.eks. `Money`, `Address`)
- **Aggregates**: Grupper af entiteter der behandles som én enhed
- **Repositories**: Abstraktion over datatilgang
- **Domain Services**: Forretningslogik uden naturlig placering i en entitet

---

### Typisk løsningstruktur i DDD + lagdeling

```plaintext
/MyProject.sln
  /MyProject.Api
    /Controllers
    /DTOs
    /Filters
    /Validation
  /MyProject.Domain
    /Entities
    /Interfaces
    /ValueObjects
    /Enums
  /MyProject.Data
    /Repositories
    /Services
    /EFConfigurations
    AppDbContext.cs
  /MyProject.Application (valgfrit)
    /Services
    /Commands
    /Queries
```

---
[Home](#indholdsfortegnelse)
