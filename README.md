# Song
using System;
using System.Collections.Generic;
using System.Text;

public abstract class Song
{
    private string title;

    public string Title
    {
        get
        {
            return this.title;
        }
        set
        {
            if (value.Length >= 2 && value.Length <= 100)
            {
                this.title = value;
            } else
            {
                throw new ArgumentException("Title should be between 2 and 100 characters!");
            }
        }
    }

    private int duration;

    public int Duration
    {
        get
        {
            return this.duration;
        }
        set
        {
            if (value > 0)
            {
                this.duration = value;
            } else
            {
                throw new ArgumentException("Duration should be positive!");
            }
        }
    }

    private string artist;

    public string Artist
    {
        get
        {
            return this.artist;
        }
        set
        {
            if (value.Length >= 3 && value.Length <= 50)
            {
                this.artist = value;
            } else
            {

            }
        }
    }

    private string genre;

    public string Genre
    {
        get
        {
            return this.genre;
        }
        set
        {
            this.genre = value;
        }
    }

    public Song(string title, int duration, string artist, string genre)
    {
        Title = title;
        Duration = duration;
        Artist = artist;
        Genre = genre;
    }

    public override string ToString()
    {
        string result = $"Title: {this.title}" + "\n" + $"Duration: {this.duration}" + "\n" + $"Artist: {this.artist}" + "\n" + $"Genre: {this.genre}";
        return result;
    }
}


# Single

using System;
using System.Collections.Generic;
using System.Text;

public class Single : Song
{
    private DateTime releaseDate;

    public DateTime ReleaseDate
    {
        get
        {
            return this.releaseDate;
        }
        set
        {
            this.releaseDate = value;
        }
    }

    public Single(string title, int duration, string artist, string genre, DateTime releaseDate)
        : base(title, duration, artist, genre)
    {
        ReleaseDate = releaseDate;
    }

    public override string ToString()
    {
        string result = base.ToString() + "\n" + $"Release Date: {this.releaseDate}";
        return result;
    }
}

# AlbumSong

using System;
using System.Collections.Generic;
using System.Text;

public class AlbumSong : Song
{
    private string albumName;

    public string AlbumName
    {
        get
        {
            return this.albumName;
        }
        set
        {
            if (value.Length >= 3 && value.Length <= 100) 
            { 
                this.albumName = value;
            } else
            {
                throw new ArgumentException("Album name should be between 3 and 100 characters!");
            }
        }
    }

    public AlbumSong(string title, int duration, string artist, string genre, string albumName)
        : base(title, duration, artist, genre)
    {
        AlbumName = albumName;
    }

    public override string ToString()
    {
        string result = base.ToString() + "\n" + $"Album: {this.albumName}";
        return result;
    }
}

#Playlist

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

public class Playlist
{
    private string title;

    public string Title
    {
        get
        {
            return this.title;
        }
        set
        {
            if (value.Length >= 3 && value.Length <= 50)
            {
                this.title = value;
            } else
            {
                throw new ArgumentException("Playlist title should be between 3 and 50 characters!");
            }
        }
    }

    private List<Song> songs;

    public Playlist(string title)
    {
        Title = title;
        this.songs = new List<Song>();
    }

    public void AddSong(Song song)
    {
        this.songs.Add(song);
    }

    public int TotalDuration()
    {
        int totalDuration = 0;

        foreach (Song song in this.songs)
        {
            totalDuration += song.Duration;
        }

        return totalDuration;
    }

    public List<Song> GetSongsByArtist(string artist)
    {
        return this.songs.Where(s => s.Artist == artist).OrderBy(s => s.Title).ToList();
    }

    public List<Song> GetSongsByGenre(string genre)
    {
        return this.songs.Where(s => s.Genre == genre).OrderBy(s => s.Title).ToList();
    }

    public List<Song> GetSongsAboveDuration(int duration)
    {
        return this.songs.Where(s => s.Duration >= duration).OrderByDescending(s => s.Duration).ToList();
    }

    public override string ToString()
    {
        string result = $"Playlist: {Title}" + "\n" + $"Total Songs: {this.songs.Count}";
        return result;
    }
}

#User 

using System;
using System.Collections.Generic;
using System.Linq;
using System.Xml.Schema;

public class User
{
    private string username;

    public string Username
    {
        get
        {
            return this.username;
        }
        set
        {
            if (value.Length >= 3 && value.Length <= 30)
            {
                this.username = value;
            } else
            {
                throw new ArgumentException("Username should be between 3 and 30 characters!");
            }
        }
    }

    private int age;

    public int Age
    {
        get
        {
            return this.age;
        }
        set
        {
            if (value >= 13)
            {
                this.age = value;
            } else
            {
                throw new ArgumentException("User must be at least 13 years old!");
            }
        }
    }

    private List<Playlist> playlists;

    public User(string username, int age)
    {
        Username = username;
        Age = age;
        playlists = new List<Playlist>();
    }

    public void AddPlaylist(Playlist playlist)
    {
        playlists.Add(playlist);
    }

    public Playlist GetPlaylistByTitle(string title)
    {
        return playlists.Where(p => p.Title == title).First();
    }

    public override string ToString()
    {
        string result = $"Username: ${username}" + "\n" + $"Age: {age}" + "\n" + $"Total Playlists: {this.playlists.Count}";
        return result;
    }
}

#Controller 

using Microsoft.VisualBasic;
using System;
using System.Collections.Generic;

public class Controller
{
    private Dictionary<string, User> users;

    public Controller()
    {
        users = new Dictionary<string, User>();
    }

    public string AddUser(List<string> args)
    {
        string username = args[0];
        int age = int.Parse(args[1]);

        users[username]  = new User(username, age);

        return $"Created User {username}!";
    }

    public string AddPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];

        User user = users[username];

        user.AddPlaylist(new Playlist(playlistTitle));

        return $"Created Playlist {playlistTitle} for User {username}!";
    }

    public string AddSongToPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        string songTitle = args[2];
        int duration = int.Parse(args[3]);
        string artist = args[4];
        string genre = args[5];
        string type = args[6];

        if (type == "Single")
        {
            DateTime releaseDate = DateTime.Parse(args[7]);
            Single single = new Single(songTitle, duration, artist, genre, releaseDate);
            users[username].GetPlaylistByTitle(playlistTitle).AddSong(single);

        }
        else if (type == "AlbumSong")
        {
            string albumName = args[7];
            AlbumSong albumSong = new AlbumSong(songTitle, duration, artist, genre, albumName);
            users[username].GetPlaylistByTitle(playlistTitle).AddSong(albumSong);
        }

        return $"Added song {songTitle} to Playlist {playlistTitle}!";

    }

    public string GetTotalDurationOfPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];

        int duration = users[username].GetPlaylistByTitle(playlistTitle).TotalDuration();

        return $"Total duration of {playlistTitle}: {duration} seconds";
    }

    public string GetSongsByArtistFromPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        string artist = args[2];
        string result = "";

        Playlist playlist = users[username].GetPlaylistByTitle(playlistTitle);

        foreach(Song s in playlist.GetSongsByArtist(artist))
        {
            result = result + s.ToString() + "\n";
        }

        return result;


    }

    public string GetSongsByGenreFromPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        string genre = args[2];
        string result = "";

        Playlist playlist = users[username].GetPlaylistByTitle(playlistTitle);

        foreach (Song s in playlist.GetSongsByGenre(genre))
        {
            result = result + s.ToString() + "\n";
        }

        return result;
    }

    public string GetSongsAboveDurationFromPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        int duration = int.Parse(args[2]);
        string result = "";

        Playlist playlist = users[username].GetPlaylistByTitle(playlistTitle);


        foreach (Song s in playlist.GetSongsAboveDuration(duration))
        {
            result = result + s.ToString() + "\n";
        }

        return result;
    }
}
 #End

     public string GetSongsByArtistFromPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        string artist = args[2];
        string result = "";

        Playlist playlist = users[username].GetPlaylistByTitle(playlistTitle);

        foreach(Song s in playlist.GetSongsByArtist(artist))
        {
            result = result + s.ToString() "\n";
        }

        return result;


    }

    public string GetSongsByGenreFromPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        string genre = args[2];
        string result = "";

        Playlist playlist = users[username].GetPlaylistByTitle(playlistTitle);

        foreach (Song s in playlist.GetSongsByGenre(genre))
        {
            result = result + s.ToString() "\n";
        }

        return result;
    }

    public string GetSongsAboveDurationFromPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        int duration = int.Parse(args[2]);
        string result = "";

        Playlist playlist = users[username].GetPlaylistByTitle(playlistTitle);


        foreach (Song s in playlist.GetSongsAboveDuration(duration))
        {
            result = result + s.ToString() + "\n";
        }

        return result;
    }
}

#FINAL

using Microsoft.VisualBasic;
using System;
using System.Collections.Generic;

public class Controller
{
    private Dictionary<string, User> users;

    public Controller()
    {
        users = new Dictionary<string, User>();
    }

    public string AddUser(List<string> args)
    {
        string username = args[0];
        int age = int.Parse(args[1]);

        users[username]  = new User(username, age);

        return $"Created User {username}!";
    }

    public string AddPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];

        User user = users[username];

        user.AddPlaylist(new Playlist(playlistTitle));

        return $"Created Playlist {playlistTitle} for User {username}!";
    }

    public string AddSongToPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        string songTitle = args[2];
        int duration = int.Parse(args[3]);
        string artist = args[4];
        string genre = args[5];
        string type = args[6];

        if (type == "Single")
        {
            DateTime releaseDate = DateTime.Parse(args[7]);
            Single single = new Single(songTitle, duration, artist, genre, releaseDate);
            users[username].GetPlaylistByTitle(playlistTitle).AddSong(single);

        }
        else if (type == "AlbumSong")
        {
            string albumName = args[7];
            AlbumSong albumSong = new AlbumSong(songTitle, duration, artist, genre, albumName);
            users[username].GetPlaylistByTitle(playlistTitle).AddSong(albumSong);
        }

        return $"Added song {songTitle} to Playlist {playlistTitle}!";

    }

    public string GetTotalDurationOfPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];

        int duration = users[username].GetPlaylistByTitle(playlistTitle).TotalDuration();

        return $"Total duration of {playlistTitle}: {duration} seconds";
    }

    public string GetSongsByArtistFromPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        string artist = args[2];
        string result = "";

        Playlist playlist = users[username].GetPlaylistByTitle(playlistTitle);

        foreach(Song s in playlist.GetSongsByArtist(artist))
        {
            result = result + s.ToString() + "\n";
        }

        return result;


    }

    public string GetSongsByGenreFromPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        string genre = args[2];
        string result = "";

        Playlist playlist = users[username].GetPlaylistByTitle(playlistTitle);

        foreach (Song s in playlist.GetSongsByGenre(genre))
        {
            result = result + s.ToString() + "\n";
        }

        return result;
    }

    public string GetSongsAboveDurationFromPlaylist(List<string> args)
    {
        string username = args[0];
        string playlistTitle = args[1];
        int duration = int.Parse(args[2]);
        string result = "";

        Playlist playlist = users[username].GetPlaylistByTitle(playlistTitle);


        foreach (Song s in playlist.GetSongsAboveDuration(duration))
        {
            result = result + s.ToString() + "\n";
        }

        return result;
    }
}
#Home
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp77
{
    internal class Home
    {
        private string type;

        private int numberOfRooms;

        public int NumberOfRooms
        {
            get { return this.numberOfRooms; }
            set { 
                if (value >= 0)
                {
                    this.numberOfRooms = value;
                }
            }
        }

        private bool hasYard;

        public bool hasYard
        {
            get { return this.hasYard; }
            set { this.hasYard = value;}
        }

        private double price
        {
            get { return this.price; }
            set { if (price >= 0.0) {  this.price = value; } }
        }

    }
}
#Food

        private string name;

        public string Name
        {
            get { return this.name; }
            set { this.name = value; }
        }

        private string type;

        public string Type
        {
            get { return this.type; }
            set { this.type = value; }
        }

        private double price;

        public double Price
        {
            get { return this.price; }
            set { if (value >= 0) {  this.price = value; } }
        }
#Time
public class Time
{
    private int hour;

    public int Hour { get => this.hour; set => this.hour = value; }

    private int minute;

    public int Minute { get => this.minute; set => this.minute = value; }

    private int second;

    public int Second { get => this.second; set => this.second = value; }

    private int millisecond;

    public int Millisecond { get => this.millisecond; set => this.millisecond = value; }

    public Time( int hour, int minute, int second, int millisecond)
    {
        Hour = hour;
        Minute = minute;
        Second = second;
        Millisecond = millisecond;
    }

    public override string ToString()
    {
        string result = $"{this.hour}:{this.minute}:{this.second}:{this.millisecond}" ;
        return result;
    }
#Date
public class Date
{
    private int dayOfMonth;

    public int DayOfMonth { get => this.dayOfMonth; set => this.dayOfMonth = value; }

    private string month;

    public string Month { get => this.month; set => this.month = value; }

    private string dayOfWeek;

    public string DayOfWeek { get => this.dayOfWeek; set => this.dayOfWeek = value; }

    private string year;

    public string Year { get => this.year; set => this.year = value; }

    public Date(int dayOfMonth, string month, string dayOfWeek, string year)
    {
        DayOfWeek = dayOfWeek;
        Month = month;
        DayOfWeek = dayOfWeek;
        Year = year;
    }

    public override string ToString()
    {
        return $"Day of the week: {this.dayOfMonth}, Day of the month: {this.dayOfMonth}, Month: {this.month}, Year: {this.year}";
    }
}


#Reseat
public class Reseat
{
    private Date date;

    public Date Date { get => this.date; set => this.date = value; }

    private double amount;

    public double Amount { get => this.amount; set => this.amount = value; }

    private double tax;

    public double Tax { get => this.tax; set => this.tax = value; }

    public Reseat (Date date, double amount, double tax)
    {
        Date = date;
        Amount = amount;
        Tax = tax;
    }

    public override string ToString()
    {
        return $"Date: {this.date} Amount: {this.amount} Tax: {this.tax}";
    }
}
#Final
public class Laptop
{
    private int ram;

    public int Ram { get => this.ram; set => this.ram = value; }

    private string cpuModel;

    public string CpuModel { get => this.cpuModel; set => this.cpuModel = value; }

    private string gpuModel;

    public string GpuModel { get => this.gpuModel; set => this.gpuModel = value; }

    public Laptop(int ram, string cpuModel, string gpuModel)
    {
        Ram = ram;
        CpuModel = cpuModel;
        GpuModel = gpuModel;
    }

    public override string ToString()
    {
        return $"Ram: {this.ram} CPU Model: {this.cpuModel} GPU Model: {this.gpuModel}";
    }
}

using System;

class Math
{
    public int sum(int a, int b)
    {
        return a + b;
    }
    public int sum(int a, int b, int c)
    {
        return a + b + c;
    }
    public int sum(int a, int b, int c, int d)
    {
        return a + b + c + d;
    }
    public int sum(int a, int b, int c, int d, int e)
    {
        return a + b + c + d + e;
    }
    public int product(int a, int b)
    {
        return a * b;
    }
    public int product(int a, int b, int c)
    {
        return a * b * c;
    }
    public int product(int a, int b, int c, int d)
    {
        return a * b * c * d;
    }
    public int product(int a, int b, int c, int d, int e)
    {
        return a * b * c * d * e;
    }

    public void Multiply(int count, string text)
    {
        for (int i = 0; i < count; i++)
        {
            Console.WriteLine(text);
        }
    }

    public void Multiply(int a, int b)
    {
        int result = a * b;
        Console.WriteLine($"Result of multiplication: {result}");
    }

    public void PrintPermutations(int a, int b)
    {
        Console.WriteLine($"{a} {b}");
        Console.WriteLine($"{b} {a}");
    }

    public void PrintPermutations(int a, int b, int c)
    {
        Console.WriteLine($"{a} {b} {c}");
        Console.WriteLine($"{a} {c} {b}");
        Console.WriteLine($"{b} {a} {c}");
        Console.WriteLine($"{b} {c} {a}");
        Console.WriteLine($"{c} {a} {b}");
        Console.WriteLine($"{c} {b} {a}");
    }

    public void PrintPermutations(int a, int b, int c, int d)
    {
        Console.WriteLine($"{a} {b} {c} {d}");
        Console.WriteLine($"{a} {b} {d} {c}");
        Console.WriteLine($"{a} {c} {b} {d}");
        Console.WriteLine($"{a} {c} {d} {b}");
        Console.WriteLine($"{a} {d} {b} {c}");
        Console.WriteLine($"{a} {d} {c} {b}");

        Console.WriteLine($"{b} {a} {c} {d}");
        Console.WriteLine($"{b} {a} {d} {c}");
        Console.WriteLine($"{b} {c} {a} {d}");
        Console.WriteLine($"{b} {c} {d} {a}");
        Console.WriteLine($"{b} {d} {a} {c}");
        Console.WriteLine($"{b} {d} {c} {a}");

        Console.WriteLine($"{c} {a} {b} {d}");
        Console.WriteLine($"{c} {a} {d} {b}");
        Console.WriteLine($"{c} {b} {a} {d}");
        Console.WriteLine($"{c} {b} {d} {a}");
        Console.WriteLine($"{c} {d} {a} {b}");
        Console.WriteLine($"{c} {d} {b} {a}");

        Console.WriteLine($"{d} {a} {b} {c}");
        Console.WriteLine($"{d} {a} {c} {b}");
        Console.WriteLine($"{d} {b} {a} {c}");
        Console.WriteLine($"{d} {b} {c} {a}");
        Console.WriteLine($"{d} {c} {a} {b}");
        Console.WriteLine($"{d} {c} {b} {a}");
    }
}

class Program
{
    static void Main()
    {
        Math math = new Math();

        Console.WriteLine("Sum of 2 numbers: " + math.sum(1, 2));
        Console.WriteLine("Sum of 3 numbers: " + math.sum(1, 2, 3));
        Console.WriteLine("Sum of 4 numbers: " + math.sum(1, 2, 3, 4));
        Console.WriteLine("Sum of 5 numbers: " + math.sum(1, 2, 3, 4, 5));

        Console.WriteLine("Product of 2 numbers: " + math.product(1, 2));
        Console.WriteLine("Product of 3 numbers: " + math.product(1, 2, 3));
        Console.WriteLine("Product of 4 numbers: " + math.product(1, 2, 3, 4));
        Console.WriteLine("Product of 5 numbers: " + math.product(1, 2, 3, 4, 5));

        Console.WriteLine("Calling Multiply(int, string):");
        math.Multiply(3, "Hello");

        Console.WriteLine("\nCalling Multiply(int, int):");
        math.Multiply(4, 5);

        Console.WriteLine("Permutations of 2 numbers:");
        math.PrintPermutations(1, 2);

       
        Console.WriteLine("\nPermutations of 3 numbers:");
        math.PrintPermutations(1, 2, 3);

        
        Console.WriteLine("\nPermutations of 4 numbers:");
        math.PrintPermutations(1, 2, 3, 4);
    }
}

#TASK

internal class Program
{
    static void Main(string[] args)
    {
        HashSet<string> ints = new HashSet<string>();
        Random random = new Random();
        while(ints.Count < 4)
        {
            ints.Add(random.Next(0, 10).ToString());
        }
        string correctNumber = string.Empty;
        foreach(string s in ints)
        {
            correctNumber += s;
        }
        Console.WriteLine(correctNumber);
        bool flag = true;
        while (flag)
        {
            string guess = Console.ReadLine();
            string result = CheckIfCorrect(correctNumber, guess);
            Console.WriteLine(result);
        }
        
    }

    public static string CheckIfCorrect(string secretNumber, string guess)
    {
        
        string result = string.Empty;
        int bullCount = 0;
        int cowCount = 0;
        for(int i = 0; i < secretNumber.Length; i++)
        {
            if (secretNumber[i] == guess[i])
            {
                bullCount++;
                

            }
            else if (secretNumber.Contains(guess[i]))
            {
                cowCount++;
            }
            
        }
        result = $"Bulls - {bullCount} : Cows - {cowCount}";
        if(bullCount == 4)
        {
            return "Good job";
        }
        return result;
    }
    
}

