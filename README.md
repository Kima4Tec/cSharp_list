
# C# Noter

## Indholdsfortegnelse
1. [OOP (Objektorienteret programmering)](#oop-objektorienteret-programmering)
2. [Clean Code](#clean-code)
3. [Defensive Coding](#defensive-coding)
4. [Iterative Agile](#iterative-agile)
5. [API](#api)
6. [Design Patterns](#design-patterns)
7. [Domain Driven Design (DDD)](#domain-driven-design-ddd)
8. [.NET Apps](#net-apps)
9. [Klasser](#klasser)
10. [Objekt](#objekt)
11. [Entity](#entity)
12. [Forretningsobjekt](#forretningsobjekt)
13. [Separation of Concerns](#separation-of-concerns)
14. [Code First](#code-first)
15. [Encapsulation](#encapsulation)
16. [ICollection](#icollection)
17. [Repository og interface](#repository-og-interface)



---

## OOP (Objektorienteret programmering)

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
### Dependency Injection og interfaces

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
## Clean Code

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
## Defensive Coding

Skriv kode der forventer fejl og beskytter sig selv:

| Teknik                | Eksempel                        |
|----------------------|----------------------------------|
| Null-tjek            | Undgå `NullReferenceException`  |
| Input-validering     | Validér før brug                |
| Exceptions med mening| Kast relevante fejl             |
| Guard clauses        | Afbryd hurtigt hvis noget fejler|
| Fail fast            | Fejl tidligt og tydeligt        |

---

## Iterative Agile

Iterativ og Agile udvikling handler om:

- Små trin (sprints)
- Tidlig og hyppig feedback
- Konstant forbedring
- Tidlig værdilevering

**Eksempel:** Start med brugeroversigt → Tilføj søgning i næste iteration → Tilføj brugeroprettelse bagefter.

---
[Home](#indholdsfortegnelse)
## API

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
## Domain Driven Design (DDD)

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
## .NET Apps

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
## Klasser

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
## Objekt

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
## Entity

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
## Forretningsobjekt

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
## Code First


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
## ICollection
I C# er ICollection<T> en generisk interface, der repræsenterer en generel samling af objekter, som kan tilgås individuelt. Den er en del af System.Collections.Generic og er basisklasse for mange typer som List<T>, HashSet<T>, osv.

🔍 Hvad er ICollection<T>?
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
## Repository og interface
Når du laver en .NET API, er brugen af repository pattern og interfaces en god softwarearkitektur-praksis. Her er en grundig forklaring på hvorfor det er en fordel:

Hvad er en Repository og et Interface?
Repository: Et repository er en klasse, som indeholder logikken for at hente og gemme data (typisk i en database). Det fungerer som et mellemled mellem databasen og din controller/service.

Interface (IRepository): Et interface definerer hvilke metoder repositoryet skal have (fx GetAll(), Add(), Delete()), uden at fortælle hvordan de implementeres.

Fordele ved at bruge Repository og Interface
1. Adskillelse af ansvar (Separation of Concerns)
Din controller (API endpoint) skal ikke bekymre sig om hvordan data hentes fra databasen – det overlades til repositoryet.

Resultat: Koden bliver mere overskuelig og lettere at vedligeholde.

2. Lettere at teste
Du kan bruge en mock af interfacet (IRepository) i dine tests i stedet for at bruge den rigtige database.

Det gør unit tests hurtige og pålidelige.

3. Fleksibilitet og udskiftelighed
Hvis du skifter database (f.eks. fra SQL Server til MongoDB), skal du kun ændre repository-implementeringen – ikke resten af applikationen.

Du kan også bruge flere implementeringer af samme interface, fx en in-memory version til test og en database-version til produktion.

4. Genbrug og fælles logik
Repositoryet kan indeholde fælles datalogik (f.eks. søgning, filtrering, validering) som ellers ville blive gentaget i flere controllers.

5. Rydder op i controlleren
Controlleren bliver meget "ren" og fokuserer kun på at håndtere HTTP-anmodninger og svar:

```csharp
[HttpGet]
public async Task<IActionResult> GetAllMovies()
{
    var movies = await _movieRepository.GetAllAsync();
    return Ok(movies);
}
```


Simpelt eksempel:
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


Repository (implementering):
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
Opsummering
Ved at bruge repository pattern og interfaces i din API får du:

- Mere struktureret kode
- Bedre testbarhed
- Større fleksibilitet
- Mindre afhængighed af teknologivalg (f.eks. EF Core)
- Det er især en fordel i mellemstore til store projekter, eller når flere udviklere arbejder sammen.
