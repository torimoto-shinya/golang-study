# Exercise: Web Crawler

## URL
https://tour.golang.org/concurrency/10

## Answer
```
package main

import (
	"fmt"
	"sync"
)

type Fetcher interface {
	// Fetch returns the body of URL and
	// a slice of URLs found on that page.
	Fetch(url string) (body string, urls []string, err error)
}

type SafeCache struct {
	b   map[string]bool
	mux sync.Mutex
}

type FetchResult struct {
	url   string
	body  string
	err   error
}

func (c *SafeCache) Visited(url string) {
	c.mux.Lock()
	c.b[url] = true
	c.mux.Unlock()
}

func (c *SafeCache) IsVisited(url string) bool {
	c.mux.Lock()
	defer c.mux.Unlock()
	return c.b[url]
}

// Crawl uses fetcher to recursively crawl
// pages starting with url, to a maximum of depth.
func Crawl(url string, depth int, fetcher Fetcher) {

	// mapの読み書きを保証するために排他制御を行う
	cache := SafeCache{b: make(map[string]bool)}

	var crawl func(url string, depth int, fetcher Fetcher, ch chan FetchResult)
	crawl = func(url string, depth int, fetcher Fetcher, ch chan FetchResult) {

		var f func(url string, depth int, fetcher Fetcher, ch chan FetchResult)
		f = func(url string, depth int, fetcher Fetcher, ch chan FetchResult) {
			if depth <= 0 {
				return
			}
			body, urls, err := fetcher.Fetch(url)
			cache.Visited(url)

			if err != nil {
				fmt.Println(err)
				return
			}
			// チャネルに送信
			var result FetchResult
			result.url  = url
			result.body = body
			result.err  = err
			ch <- result

			for _, u := range urls {
				//fmt.Printf("url: %s\n", u)
				if !cache.IsVisited(u) {
					f(u, depth-1, fetcher, ch)
				}
			}
		}
		f(url, depth, fetcher, ch)
		close(ch)
	}

	ch := make(chan FetchResult)

	go crawl(url, depth, fetcher, ch)
	for r := range ch {
		fmt.Printf("found: %s %q\n", r.url, r.body)
	}

}

func main() {
	Crawl("https://golang.org/", 4, fetcher)
}

// fakeFetcher is Fetcher that returns canned results.
type fakeFetcher map[string]*fakeResult

type fakeResult struct {
	body string
	urls []string
}


func (f fakeFetcher) Fetch(url string) (string, []string, error) {
	if res, ok := f[url]; ok {
		return res.body, res.urls, nil
	}
	return "", nil, fmt.Errorf("not found: %s", url)
}

// fetcher is a populated fakeFetcher.
var fetcher = fakeFetcher{
	"https://golang.org/": &fakeResult{
		"The Go Programming Language",
		[]string{
			"https://golang.org/pkg/",
			"https://golang.org/cmd/",
		},
	},
	"https://golang.org/pkg/": &fakeResult{
		"Packages",
		[]string{
			"https://golang.org/",
			"https://golang.org/cmd/",
			"https://golang.org/pkg/fmt/",
			"https://golang.org/pkg/os/",
		},
	},
	"https://golang.org/pkg/fmt/": &fakeResult{
		"Package fmt",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
	"https://golang.org/pkg/os/": &fakeResult{
		"Package os",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
}
```
