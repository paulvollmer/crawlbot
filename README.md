CrawlBot
========

Crawlbot is a simple, efficient, and flexible webcrawler. Crawlbot is easy to use out-of-the-box, but also provides extensive flexibility for advanced users.

```go
package main

import (
	"fmt"
	"github.com/phayes/crawlbot"
	"log"
)

func main() {
	crawler := NewCrawler("http://cnn.com", myURLHandler, 4)
	crawler.Start()
	crawler.Wait()
}

func myURLHandler(resp *Response) {
	if resp.Error != nil {
		log.Fatal(resp.Error)
	}

	fmt.Println("Found URL at " + resp.URL)
}
```

CrawlBot provides extensive customizability for advances use cases. 

```go
package main

import (
	"fmt"
	"github.com/phayes/crawlbot"
	"log"
)

func main() {
	crawler := crawlbot.Crawler{
		URLs:       []string{"http://example.com", "http://cnn.com", "http://en.wikipedia.org"},
		NumWorkers: 12,
		Handler:    PrintTitle,
		CheckURL:   AllowEverything,
	}
	crawler.Start()
	crawler.Wait()
}

// Print the title of the page
func PrintTitle(resp *Response) {
	if resp.Error != nil {
		log.Println(err)
	}

	if resp.Doc != nil {
		title, err := resp.Doc.Search("//title")
		if err != nil {
			log.Println(err)
		}
		fmt.Printf("Title of %s is %s\n", resp.URL, title)
	} else {
		fmt.Println("HTML was not parsed for " + resp.URL)
	}
}

// Crawl everything!
func AllowEverything(crawler *Crawler, url string) bool {
	return true
}

```