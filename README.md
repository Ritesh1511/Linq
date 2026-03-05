# Linq 
## 🎮 LINQ and Lambda Expressions in C# (Game Example)

 

```
using System.Collections.Generic;
using System;
using System.Collections;
using System.Text;
using System.Threading.Channels;
using System.Diagnostics;
using System.Security.Cryptography;
using System.Security.AccessControl;
using static System.Runtime.InteropServices.JavaScript.JSType;
using System.Data.Common;
using System.Xml.Linq;

namespace game
{
    internal class Program
    {
        static void Main(string[] args)
        {
            var games = new List<Game>
            {
                new Game { Title = "The Legend of Zelda", Genre = "Adventure", ReleaseYear = 1986, Rating = 9.5, Price = 60 },
                new Game { Title = "Super Mario Bros.", Genre = "Platformer", ReleaseYear = 1985, Rating = 9.2, Price = 50 },
                new Game { Title = "Elden Ring", Genre = "RPG", ReleaseYear = 2022, Rating = 9.8, Price = 70 },
                new Game { Title = "Stardew Valley", Genre = "Simulation", ReleaseYear = 2016, Rating = 9.0, Price = 15 },
                new Game { Title = "Tetris", Genre = "Puzzle", ReleaseYear = 1984, Rating = 8.9, Price = 10 }
            };

 

// A lambda function is a shortcut to write a tiny function.
// A lambda function is a concise way to represent anonymous method in c#.It is commonly used in Linq to specify how to handle each item in a collection.
            
 
            //using normal list

            List<string> allgames = new List<string>();

            foreach (var game in games)
            {
                allgames.Add(game.Title);
            }

            foreach (var title in allgames)
            {
                Console.WriteLine(title);
            }


            //using linq in List

            List<string> list_linq = new List<string>();

            list_linq = games.Select(g => g.Title).ToList();


            foreach (var title in list_linq)
            {
                Console.WriteLine(title);
            }

            var var_linq = games.Select(g => g.Title).ToList();
            Console.WriteLine("var_linq is type of :" + var_linq.GetType()); // list type              
            var var_linq2 = games.Select(g => g.Title); // IEnumerable type
            Console.WriteLine("var_linq2 is type of :" + var_linq2.GetType()); // IEnumerable type

            //Lambda does NOT decide IEnumerable vs IQueryable
            //Source decides

            //With in-memory collections → LINQ returns IEnumerable
            //With EF Core / DbSet → LINQ returns IQueryable

            //(g => g.Title.ToString()) executes immediately in memory
            //Return type → IEnumerable<string>


            //var result = context.Games.Select(g => g.Title.ToString()) executes immediately in memory
            //Return type → IQueryable<string>

            //IEnumerable source  → IEnumerable result
            //IQueryable source   → IQueryable result


            //Here are some functions that does not affect the type of the result other than Tostring: 


            //1️ ToLower()
            //.Select(g => g.Title.ToLower())
            //2️ Trim()
            //.Select(g => g.Title.Trim())
            //3️ Substring(int start, int length)
            //.Select(g => g.Title.Substring(0, 3))
            //4️ Replace(string oldValue, string newValue)
            //.Select(g => g.Title.Replace(" ", "-"))
            //5️ Contains(string value)
            //.Where(g => g.Title.Contains("Pro"))




            //When Does the Result Type Change? 




            // ToList()
            // var result = context.Games.Select(g => g.Title).ToList();
            // Before ToList() → IQueryable<string>
            // After → List<string>




            // First() / FirstOrDefault()
            // var result = context.Games.Select(g => g.Title).First();
            // Before → IQueryable<string>
            // After → string
            // Type changed from sequence → single value.



            // Count()
            // var count = context.Games.Count();
            // Before → IQueryable<Game>
            // After → int



            // Any()
            // bool exists = context.Games.Any();
            // Returns bool.


            // AsEnumerable()
            // This one is very important.
            // var result = context.Games
            //.AsEnumerable()
            //.Select(g => MyCustomMethod(g.Title));
            // AsEnumerable() switches from:IQueryable → IEnumerable

            //Lets try some Linq functions 


            //Select(g => g.Title) → IEnumerable<string>

            var result1 = games.Select(g => g.Title);
            foreach (var res1 in result1)
            {
                Console.WriteLine($"Title: " + res1);
            }

            var result2 = games.Select(g => new { g.Title, g.Genre, g.ReleaseYear, g.Rating, g.Price });
            foreach (var res1 in result2)
            {
                Console.WriteLine($"Title: {res1.Title}, Genre: {res1.Genre}, Release Year: {res1.ReleaseYear}, Rating: {res1.Rating}, Price: {res1.Price}");
            }


            //.Where(g => g.Title) → IEnumerable<string>

            var result3 = games.Where(g => g.Rating > 8);
            foreach (var res1 in result3)
            {
                Console.WriteLine(res1);
            }  //This will print the type of the object, not the properties.

            var result4 = games.Where(g => g.Rating > 8).Select(g => g.Title);
            foreach (var res1 in result4)
            {
                Console.WriteLine(res1);
            }  //This will select only the title of the games with rating > 8 and print them.

            var result5 = games.Where(g => g.Rating > 8).Select(g => new { g.Title, g.ReleaseYear, g.Price, g.Rating });
            foreach (var res1 in result5)
            {
                Console.WriteLine(res1);
            }  //This will select multiple columns of the games with rating > 8 and print them.

            //Most used where methods  -
            //var expensiveGames = games.Where(g => g.Price > 50);
            //var topRatedCheapGames = games.Where(g => g.Rating > 8 && g.Price < 50);
            //var actionGames = games.Where(g => g.Genre == "Action");
            //var adventureGames = games.Where(g => g.Title.Contains("Adventure"));
            //We can chain Where calls too: var result = games
            //.Where(g => g.Price < 50).Where(g => g.Rating > 8);


            //.Any(g => g.Title) → Boolean

            var result6 = games.Any(g => g.Rating > 9.5);
            Console.WriteLine(result6);






            var sortedGames = games.OrderBy(g => g.Title);  // Sorts by Title (no change in the object shape)
            foreach (var game in sortedGames)
            {
                Console.WriteLine($"{game.Title} - {game.Genre}");  // Still printing full game object
            }

            var sortedGames2 = games.OrderByDescending(g => g.Title).Select(g => g.Title);  // Sorts by Title and selects only the Title

            foreach (var title in sortedGames2)
            {
                Console.WriteLine(title);  // Now printing only the Title

            }
            //Summary of Behavior:

            //Without Select: The entire dataset(full objects) will be selected.

            //With Select: You choose specific columns(properties), and only those will be returned.

            //Where: Filters the rows based on conditions.

            //OrderBy: Orders the data, but the column selection stays the same.



            // Average 
            var averagePrice = games.Average(g => g.Price);
            Console.WriteLine($"Average Price: {averagePrice}");

            // Max 
            var maxPrice = games.Max(g => g.Price);
            Console.WriteLine($"Max Price: {maxPrice}");

            //game with best rating 
            var bestRatedGame = games.Max(g => g.Rating);
            var bestRatedGameDetails = games.First(g => g.Rating == bestRatedGame);
            Console.WriteLine($"Best Rated Game: {bestRatedGameDetails.Title} with Rating: {bestRatedGameDetails.Rating}");
        }
    }
}

```
