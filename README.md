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
