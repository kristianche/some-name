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
