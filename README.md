
# C# Noter

# Indholdsfortegnelse
1. [API](#api)  
2. [Automapper](#automapper)  
3. [Bcrypt](#bcrypt)  
4. [Clean Code](#clean-code)  
5. [Code First](#code-first)
6. [Controllers](#controllers)  
7. [Data Transfer Objects](#dto)  
8. [Defensive Coding](#defensive-coding)  
9. [Design Patterns](#design-patterns)  
10. [Domain Driven Design (DDD)](#domain-driven-design-ddd)  
11. [Entity](#entity)  
12. [Encapsulation](#encapsulation)  
13. [Fluent Api](#fluent-api)  
14. [Forretningsobjekt](#forretningsobjekt)  
15. [ICollection](#icollection)  
16. [Iterative Agile](#iterative-agile)  
17. [JWT](#jwt)  
18. [Klasser](#klasser)  
19. [Models](#models)  
20. [Objekt](#objekt)  
21. [OOP (Objektorienteret programmering)](#oop-objektorienteret-programmering)  
22. [Repository og interface](#repository-og-interface)  
23. [Separation of Concerns](#separation-of-concerns)
24. [Services](#services)  
25. [.NET Apps](#net-apps)




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

Fokuser på forretningsdomænet og bygg din applikation derfra.

| Begreb            | Forklaring                             |
|-------------------|----------------------------------------|
| Domæne            | Fx biograf, bank, webshop              |
| Entity            | Objekter med identitet                 |
| Value Object      | Kun værdi, ikke identitet              |
| Aggregate         | En gruppering af entiteter + regler    |
| Repository        | Gem og hent aggregates                 |
| Service           | Forretningslogik                       |
| Ubiquitous Language | Samme sprog hos devs og eksperter     |

**Fordele:** Klar logik, tæt kobling til virkeligheden, skalerbar kodebase.

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

---
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
En **Controller** i ASP.NET Core er en central del af **MVC (Model-View-Controller)**-designmønstret og fungerer som en **formidler** mellem brugerens input og applikationens logik. Controlleren **modtager HTTP-anmodninger**, behandler dem og returnerer **svar (response)** til brugeren, ofte i form af **HTML, JSON** eller **XML**.

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

