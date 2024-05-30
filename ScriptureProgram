using System;
using System.Collections.Generic;
using System.Linq;
using System.IO;

// Class to represent the scripture reference
public class ScriptureReference
{
    public string Book { get; private set; }
    public int StartChapter { get; private set; }
    public int StartVerse { get; private set; }
    public int? EndChapter { get; private set; }
    public int? EndVerse { get; private set; }

    public ScriptureReference(string reference)
    {
        var parts = reference.Split(new[] { ' ', ':', '-' }, StringSplitOptions.RemoveEmptyEntries);
        Book = parts[0];
        StartChapter = int.Parse(parts[1]);
        StartVerse = int.Parse(parts[2]);

        if (parts.Length > 3)
        {
            if (parts.Length == 4)
            {
                EndVerse = int.Parse(parts[3]);
            }
            else
            {
                EndChapter = int.Parse(parts[3]);
                EndVerse = int.Parse(parts[4]);
            }
        }
    }

    public override string ToString()
    {
        if (EndChapter.HasValue)
        {
            return $"{Book} {StartChapter}:{StartVerse}-{EndChapter}:{EndVerse}";
        }
        else if (EndVerse.HasValue)
        {
            return $"{Book} {StartChapter}:{StartVerse}-{EndVerse}";
        }
        else
        {
            return $"{Book} {StartChapter}:{StartVerse}";
        }
    }
}

// Class to represent each word in the scripture
public class Word
{
    public string Text { get; private set; }
    public bool IsHidden { get; private set; }

    public Word(string text)
    {
        Text = text;
        IsHidden = false;
    }

    public void Hide()
    {
        IsHidden = true;
    }

    public override string ToString()
    {
        return IsHidden ? "_____" : Text;
    }
}

// Class to represent the entire scripture including its reference and text
public class Scripture
{
    public ScriptureReference Reference { get; private set; }
    private List<Word> Words;

    public Scripture(string reference, string text)
    {
        Reference = new ScriptureReference(reference);
        Words = text.Split(' ').Select(word => new Word(word)).ToList();
    }

    public void HideRandomWords(int count)
    {
        Random random = new Random();
        var wordsToHide = Words.Where(word => !word.IsHidden).OrderBy(_ => random.Next()).Take(count);
        foreach (var word in wordsToHide)
        {
            word.Hide();
        }
    }

    public bool AllWordsHidden()
    {
        return Words.All(word => word.IsHidden);
    }

    public override string ToString()
    {
        return $"{Reference}\n{string.Join(' ', Words)}";
    }
}

// Class to load and manage a library of scriptures
public class ScriptureLibrary
{
    private List<Scripture> Scriptures;

    public ScriptureLibrary(string filePath)
    {
        Scriptures = new List<Scripture>();
        LoadScriptures(filePath);
    }

    private void LoadScriptures(string filePath)
    {
        var lines = File.ReadAllLines(filePath);
        for (int i = 0; i < lines.Length; i += 2)
        {
            string reference = lines[i];
            string text = lines[i + 1];
            Scriptures.Add(new Scripture(reference, text));
        }
    }

    public Scripture GetRandomScripture()
    {
        Random random = new Random();
        int index = random.Next(Scriptures.Count);
        return Scriptures[index];
    }
}

// Main program class
class Program
{
    static void Main(string[] args)
    {
        ScriptureLibrary library = new ScriptureLibrary("scriptures.txt");
        Scripture scripture = library.GetRandomScripture();

        Console.Clear();
        Console.WriteLine(scripture);
        Console.WriteLine("\nPress Enter to hide words or type 'quit' to exit.");

        while (true)
        {
            string input = Console.ReadLine();
            if (input.ToLower() == "quit")
            {
                break;
            }

            scripture.HideRandomWords(3);
            Console.Clear();
            Console.WriteLine(scripture);

            if (scripture.AllWordsHidden())
            {
                break;
            }

            Console.WriteLine("\nPress Enter to hide words or type 'quit' to exit.");
        }
    }
}
