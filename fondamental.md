# Fondamental 

## program.cs

Program.cs est un fichier dans __ASP.NET__ application MVC Core est un __point d’entrée__ d’une application. Il contient une logique permettant de __démarrer le serveur__, d’écouter les demandes, ainsi que de configurer l’application.

## les middelwares

Un middleware est __un logiciel__ qui est assemblé dans __un pipeline__ d’application pour gérer __les requêtes__ et __les réponses.__ 

Chaque composant :

- Choisit de passer la requête au composant suivant dans le pipeline.
- Peut travailler avant et après le composant suivant dans le pipeline.

### roles :

 les Pages Razor et MVC __traitent les requêtes__ du navigateur sur le serveur avec le middleware.

 ### exemple :

 ```csharp
app.UseRouting();
```
## L'injection de dépendance 

- L’injection de dépendances (DI) est un design pattern.
- Elle permet de donner à une classe ce dont elle a besoin (ses dépendances) sans qu’elle ait à les créer elle-même.
- Cela rend ton code plus propre, plus modifiable et plus facile à tester.

```csharp
public interface IEmailService
{
    void Send(string to, string message);
}

public class EmailService : IEmailService
{
    public void Send(string to, string message)
    {
        Console.WriteLine($"Email envoyé à {to} : {message}");
    }
}
```
```csharp
builder.Services.AddScoped<IEmailService, EmailService>();
```
## 1️ Transient

 **Quoi ?**  
Une nouvelle instance du service est créée à chaque fois qu’on en a besoin.

 **Quand l’utiliser ?**  
Quand ton service n’a pas besoin de garder de l’état entre les appels.  
Exemple : un service qui fait juste un calcul ou une opération courte.

 **Exemple :**

```csharp
builder.Services.AddTransient<IMyService, MyService>();
``` 
## 2️ Scoped

 **Quoi ?**  
Une seule instance est créée pour **chaque requête HTTP**.

 **Quand l’utiliser ?**  
Quand tu as besoin de partager des infos pendant toute la requête  
(par exemple, pour gérer une connexion à la base de données).

 **Exemple :**

```csharp
builder.Services.AddScoped<IMyService, MyService>();
```
## 3️ Singleton

 **Quoi ?**  
Une seule et unique instance est créée pour **toute la durée de vie de l’application**.

 **Quand l’utiliser ?**  
Quand ton service :  
- Ne dépend d’**aucune donnée spécifique à une requête**.  
- Est **thread-safe** (peut être utilisé par plusieurs requêtes en même temps).  
- A besoin de **mémoriser des choses globales** (ex : config, cache).

 **Exemple :**

```csharp
builder.Services.AddSingleton<IMyService, MyService>();
``` 
##  Contrôleur, Vue et Modèle (MVC)

 **Quoi ?**

MVC veut dire **Model - View - Controller**.  
C’est un **design pattern** utilisé pour **séparer** la logique de ton application web.

---

###  ** Modèle (Model)**

- Contient les **données** et la **logique métier**.  
- Exemple : classes C# qui représentent ta base de données (`User`, `Product`, etc.).

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
}
``` 
### Contrôleur (Controller)

- Reçoit les requêtes HTTP.  
- Récupère ou envoie les données au modèle.  
- Renvoie une Vue ou une réponse JSON.

```csharp
public class ProductController : Controller
{
    public IActionResult Index()
    {
        var products = new List<Product>(); // Exemple
        return View(products);
    }
}
```
### Vue (View)

- C’est le template HTML qui affiche les données.  
- Les vues sont dans le dossier `/Views`.  
- Elles utilisent Razor (`.cshtml`).

```html
@model List<Product>

<h1>Produits</h1>
<ul>
@foreach(var p in Model)
{
    <li>@p.Name</li>
}
</ul>
``` 
##  HTML Helper

 **Quoi ?**  
Les **HTML Helpers** sont des **méthodes C#** qui t’aident à **générer du HTML** plus facilement dans une Vue Razor.

Au lieu d’écrire tout le HTML à la main, tu peux utiliser ces helpers pour :  
- Créer des balises `<form>`, `<input>`, `<label>`, etc.
- Lier automatiquement des champs à ton **Model**.

---
#### exemple : 
```html
@model Product

@using (Html.BeginForm())
{
    @Html.LabelFor(m => m.Name)
    @Html.TextBoxFor(m => m.Name)
    <input type="submit" value="Envoyer" />
}
``` 
##  Tag Helper

 **Quoi ?**  
Les **Tag Helpers** sont une **nouvelle façon** (plus moderne que les HTML Helpers) pour **générer du HTML dynamique** dans Razor.  
Ils ressemblent à du HTML **normal**, mais ajoutent de la logique côté serveur.

---

###  **Exemple**

**Sans Tag Helper (avec HTML Helper) :**

```html
@model Product

<form asp-action="Create" method="post">
    <label asp-for="Name"></label>
    <input asp-for="Name" />
    <button type="submit">Envoyer</button>
</form>
```
 Ici :
```
asp-action="Create" dit au <form> d’envoyer les données à l’action Create du contrôleur.

asp-for="Name" lie directement le champ au modèle (Product).`
```
##  Authentification & Autorisation

### **Authentification**

 **Quoi ?**  
**Savoir QUI tu es.**  
L’authentification vérifie l’**identité** de l’utilisateur (login, mot de passe, token, cookie…).

---

###  **Exemple :**
- `Identity` (librairie ASP.NET Core) gère l’enregistrement, la connexion, le mot de passe oublié…
- Peut aussi se faire avec JWT pour une **API**.

```csharp
// Exemple de middleware pour activer l'authentification
app.UseAuthentication();
``` 
##  Autorisation

 **Quoi ?**  
Savoir **CE QUE tu peux faire**.  
L’autorisation décide si l’utilisateur a le **droit d’accéder** à une ressource ou une action.

---

###  Exemple

```csharp
[Authorize] // Seuls les utilisateurs connectés peuvent accéder à cette action
public IActionResult SecurePage()
{
    return View();
}
``` 

##  Web API

 **Quoi ?**  
Une **Web API** permet à ton application de **communiquer** avec d’autres applications via **HTTP** (souvent en JSON).

---

##  Récupération et réponse dans une API

###  **Récupération des données**

Dans une API, les données sont généralement récupérées via les paramètres de la requête :  
- **Route parameters** (ex : `/api/products/5`)  
- **Query string** (ex : `/api/products?category=books`)  
- **Corps de la requête** (pour les POST, PUT)

```csharp
[HttpGet("{id}")]
public ActionResult<Product> GetById(int id)
{
    var product = _service.GetProductById(id);
    if (product == null)
        return NotFound(); // 404 si pas trouvé
    return Ok(product); // 200 + produit JSON
}
```
## Réponse

Utiliser les méthodes d’aide comme `Ok()`, `NotFound()`, `BadRequest()` pour renvoyer des réponses HTTP claires.

Le contenu renvoyé est généralement du **JSON**.

```csharp
[HttpPost]
public IActionResult Create(Product product)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState); // 400 si erreur de validation

    _service.AddProduct(product);
    return CreatedAtAction(nameof(GetById), new { id = product.Id }, product); // 201 + URI du nouvel élément
}
``` 
##  Résumé

| Action           | Résultat HTTP   | Description                  |
|------------------|-----------------|------------------------------|
| `Ok(object)`     | 200 OK          | Succès avec données           |
| `CreatedAtAction`| 201 Created     | Ressource créée + URL         |
| `BadRequest`     | 400 Bad Request | Erreur côté client            |
| `NotFound`       | 404 Not Found   | Ressource non trouvée         |

##  Entity Framework Core (EF Core)

 **Quoi ?**  
EF Core est un **ORM** (Object-Relational Mapper) pour .NET.  
Il te permet de **manipuler ta base de données avec du code C#** au lieu d’écrire du SQL à la main.

---

###  **Principes**

- Tu définis tes **modèles** (classes C#) qui représentent tes tables en base de données.
- EF Core s’occupe de générer les requêtes SQL et de faire la liaison entre ta base et ton code.
- Tu peux faire des opérations **CRUD** facilement.
