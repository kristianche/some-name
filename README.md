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
