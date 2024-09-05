using System;
using System.Collections.Generic;
using System.Xml;
using System.Net;

public class RSSFeedReader
{
    public static void Main(string[] args)
    {
        Console.WriteLine("Welcome to the RSS Feed Reader!");
        Console.Write("Enter the RSS feed URL: ");
        string feedUrl = Console.ReadLine();

        try
        {
            // Read the RSS feed
            XmlDocument xmlDoc = new XmlDocument();
            xmlDoc.Load(feedUrl);

            // Extract feed title
            string feedTitle = xmlDoc.SelectSingleNode("/rss/channel/title").InnerText;
            Console.WriteLine($"nFeed Title: {feedTitle}");

            // Extract and display feed items
            XmlNodeList items = xmlDoc.SelectNodes("/rss/channel/item");
            if (items.Count > 0)
            {
                foreach (XmlNode item in items)
                {
                    Console.WriteLine("n---------------------------------------");

                    string title = item.SelectSingleNode("title").InnerText;
                    Console.WriteLine($"Title: {title}");

                    string link = item.SelectSingleNode("link").InnerText;
                    Console.WriteLine($"Link: {link}");

                    string description = item.SelectSingleNode("description").InnerText;
                    Console.WriteLine($"Description: {description}");

                    // Get the published date (if available)
                    string pubDate = item.SelectSingleNode("pubDate").InnerText;
                    if (!string.IsNullOrEmpty(pubDate))
                    {
                        Console.WriteLine($"Published Date: {pubDate}");
                    }
                }
            }
            else
            {
                Console.WriteLine("No items found in the feed.");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error reading RSS feed: {ex.Message}");
        }

        Console.ReadKey();
    }
}
