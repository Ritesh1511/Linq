// 🎮 LINQ and Lambda Expressions in C# (Game Example)
// This single file demonstrates:
// - Lambda functions
// - IEnumerable vs List
// - Select, Where, Any, OrderBy
// - Console output examples (with comments)

using System;
using System.Collections.Generic;
using System.Linq;

namespace game
{
    // Game model
    public class Game
    {
        public string Title { get; set; }
        public string Genre { get; set; }
        public int ReleaseYear { get; set; }
        public double Rating { get; set; }
        public double Price { get; set; }

        public override string ToString()
        {
            return $"{Title} ({Genre}) - {ReleaseYear} | Rating: {Rating} | Price: ${Price}";
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            // Sample in-memory collection (List<T> → IEnumerable source)
            var games = new List<Game>
            {
                new Game { Title = "The Legend of Zelda", Genre = "Adventure", ReleaseYear = 1986, Rating = 9.5, Price = 60 },
                new Game { Title = "Super Mario Bros.", Genre = "Platformer", ReleaseYear = 1985, Rating = 9.2, Price = 50 },
                new Game { Title = "Elden Ring", Genre = "RPG", ReleaseYear = 2022, Rating = 9.8, Price = 70 },
                new Game { Title = "Stardew Valley", Genre = "Simulation", ReleaseYear = 2016, Rating = 9.0, Price = 15 },
                new Game { Title = "Tetris", Genre = "Puzzle", ReleaseYear = 1984, Rating = 8.9, Price = 10 }
            };

 
            // ⚡ A lambda function is a concise way to write an anonymous method.
            // Example: g => g.Title (takes a Game and returns its Title)

            Console.WriteLine("=== Using Normal List (Without LINQ) ===");

            // Using normal loop to extract titles
            List<string> allgames = new List<string>();

            foreach (var game in games)
            {
                allgames.Add(game.Title);
            }

            foreach (var title in allgames)
            {
                Console.WriteLine(title);
            }

            // Expected Console Output:
            // The Legend of Zelda
            // Super Mario Bros.
            // Elden Ring
            // Stardew Valley
            // Tetris


            Console.WriteLine("\n=== Using LINQ Select ===");

            // Using LINQ Select to get titles (IEnumerable<string>)
            List<string> list_linq = games.Select(g => g.Title).ToList();

            foreach (var title in list_linq)
            {
                Console.WriteLine(title);
            }

            // Output:
            // The Legend of Zelda
            // Super Mario Bros.
            // Elden Ring
            // Stardew Valley
            // Tetris


            Console.WriteLine("\n=== Understanding var and Return Types ===");

            // ToList() forces execution and returns List<string>
            var var_linq = games.Select(g => g.Title).ToList();
            Console.WriteLine("var_linq type: " + var_linq.GetType());

            // Without ToList() → Deferred execution → IEnumerable<string>
            var var_linq2 = games.Select(g => g.Title);
            Console.WriteLine("var_linq2 type: " + var_linq2.GetType());

            // Example Output:
            // var_linq type: System.Collections.Generic.List`1[System.String]
            // var_linq2 type: System.Linq.Enumerable+SelectListIterator`2[game.Game,System.String]

            // IMPORTANT:
            // Lambda does NOT decide IEnumerable vs IQueryable
            // Source decides:
            // List<T> → IEnumerable
            // DbSet (EF Core) → IQueryable


            Console.WriteLine("\n=== LINQ Select (Anonymous Object) ===");

            // Selecting multiple properties using anonymous object
            var result2 = games.Select(g => new { g.Title, g.Genre, g.ReleaseYear, g.Rating, g.Price });

            foreach (var item in result2)
            {
                Console.WriteLine($"Title: {item.Title}, Genre: {item.Genre}, Year: {item.ReleaseYear}, Rating: {item.Rating}, Price: {item.Price}");
            }

            // Output:
            // Title: The Legend of Zelda, Genre: Adventure, Year: 1986, Rating: 9.5, Price: 60
            // Title: Super Mario Bros., Genre: Platformer, Year: 1985, Rating: 9.2, Price: 50
            // ...


            Console.WriteLine("\n=== LINQ Where (Filter) ===");

            // Filtering games with rating > 8 (returns IEnumerable<Game>)
            var result3 = games.Where(g => g.Rating > 8);

            foreach (var game in result3)
            {
                Console.WriteLine(game); // Uses ToString() override
            }

            // Output:
            // The Legend of Zelda (Adventure) - 1986 | Rating: 9.5 | Price: $60
            // Super Mario Bros. (Platformer) - 1985 | Rating: 9.2 | Price: $50
            // Elden Ring (RPG) - 2022 | Rating: 9.8 | Price: $70
            // Stardew Valley (Simulation) - 2016 | Rating: 9.0 | Price: $15
            // Tetris (Puzzle) - 1984 | Rating: 8.9 | Price: $10


            Console.WriteLine("\n=== Where + Select (Only Titles of High Rated Games) ===");

            // Filter + Select specific column
            var result4 = games
                .Where(g => g.Rating > 8)
                .Select(g => g.Title);

            foreach (var title in result4)
            {
                Console.WriteLine(title);
            }

            // Output:
            // The Legend of Zelda
            // Super Mario Bros.
            // Elden Ring
            // Stardew Valley
            // Tetris


            Console.WriteLine("\n=== Any() Example ===");

            // Checks if any game has rating > 9.5 (returns bool)
            var result6 = games.Any(g => g.Rating > 9.5);
            Console.WriteLine("Any game with rating > 9.5: " + result6);

            // Output:
            // Any game with rating > 9.5: True


            Console.WriteLine("\n=== OrderBy (Sort by Title - Full Objects) ===");

            // Sorts by Title (object shape remains same)
            var sortedGames = games.OrderBy(g => g.Title);

            foreach (var game in sortedGames)
            {
                Console.WriteLine($"{game.Title} - {game.Genre}");
            }

            // Output (Alphabetical):
            // Elden Ring - RPG
            // Stardew Valley - Simulation
            // Super Mario Bros. - Platformer
            // Tetris - Puzzle
            // The Legend of Zelda - Adventure


            Console.WriteLine("\n=== OrderByDescending + Select (Only Titles) ===");

            // Sort descending and select only titles
            var sortedGames2 = games
                .OrderByDescending(g => g.Title)
                .Select(g => g.Title);

            foreach (var title in sortedGames2)
            {
                Console.WriteLine(title);
            }

            // Output (Reverse Alphabetical):
            // The Legend of Zelda
            // Tetris
            // Super Mario Bros.
            // Stardew Valley
            // Elden Ring


            // 📌 FINAL SUMMARY:
            // - Without Select → returns full objects
            // - With Select → returns only chosen properties
            // - Where → filters data (rows)
            // - OrderBy → sorts data but keeps object shape
            // - ToList() → converts IEnumerable to List (immediate execution)
            // - Any() → returns bool
        }
    }
}
