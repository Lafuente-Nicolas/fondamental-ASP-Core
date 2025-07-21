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
