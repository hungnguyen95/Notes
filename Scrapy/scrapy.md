# Scrapy basic

## Create new project scrapy:
`scrapy startproject tutorial`

## Create spider
In folder `spiders` create `quotes_spider.py`:

```python
import scrapy


class QuotesSpider(scrapy.Spider):
    name = "quotes"

    def start_requests(self):
        urls = [
            'http://quotes.toscrape.com/page/1/',
            'http://quotes.toscrape.com/page/2/',
        ]
        for url in urls:
            yield scrapy.Request(url=url, callback=self.parse)

    def parse(self, response):
        page = response.url.split("/")[-2]
        filename = 'quotes-%s.html' % page
        with open(filename, 'wb') as f:
            f.write(response.body)
        self.log('Saved file %s' % filename)
```

## Run scrapy
`scrapy crawl quotes`

## Extracting data from response
```python
response.css('title')
# This will return SelectorList, which represents a list of selector
```
Select a HTML element, simply write element name: `'title'`
```python
response.css('title').getall()
# Get content from selector, use function getall(), get()
```

To extract the text inside HTML tag (title): `'title::text'`
```python
>>> response.css('title::text').getall()
['Quotes to Scrape']
```

Besides the `getall()` and `get()` methods, you can also use the `re()` method to extract using **regular expressions**:
```python
>>> response.css('title::text').re(r'Quotes.*')
['Quotes to Scrape']
>>> response.css('title::text').re(r'Q\w+')
['Quotes']
>>> response.css('title::text').re(r'(\w+) to (\w+)')
['Quotes', 'Scrape']
```

## XPATH brief intro
Besides CSS, Scrapy selectors also support using XPath expressions:
```python
>>> response.xpath('//title')
[<Selector xpath='//title' data='<title>Quotes to Scrape</title>'>]
>>> response.xpath('//title/text()').get()
'Quotes to Scrape'
```

## Following Link
### Follow link with absolute URL
First thing is to extract the link to the page we want to follow. Examining our page, we can see there is a link to the next page with the following markup:
```html
<ul class="pager">
    <li class="next">
        <a href="/page/2/">Next <span aria-hidden="true">&rarr;</span></a>
    </li>
</ul>
```

We can try extracting it in the shell:
```python
>>> response.css('li.next a').get()
'<a href="/page/2/">Next <span aria-hidden="true">→</span></a>'
```

Select href in href attribute:

Method 1:
```python
>>> response.css('li.next a::attr(href)').get()
'/page/2/'
```
Method 2:
```python
>>> response.css('li.next a').attrib['href']
'/page/2'
```
Let’s see now our spider modified to recursively follow the link to the next page, extracting data from it:
```python
import scrapy


class QuotesSpider(scrapy.Spider):
    name = "quotes"
    start_urls = [
        'http://quotes.toscrape.com/page/1/',
    ]

    def parse(self, response):
        for quote in response.css('div.quote'):
            yield {
                'text': quote.css('span.text::text').get(),
                'author': quote.css('small.author::text').get(),
                'tags': quote.css('div.tags a.tag::text').getall(),
            }

        next_page = response.css('li.next a::attr(href)').get()
        if next_page is not None:
            next_page = response.urljoin(next_page)
            yield scrapy.Request(next_page, callback=self.parse)
```
### A shortcut for creating Requests
As a shortcut for creating Request objects you can use `response.follow`:
```python
import scrapy


class QuotesSpider(scrapy.Spider):
    name = "quotes"
    start_urls = [
        'http://quotes.toscrape.com/page/1/',
    ]

    def parse(self, response):
        for quote in response.css('div.quote'):
            yield {
                'text': quote.css('span.text::text').get(),
                'author': quote.css('span small::text').get(),
                'tags': quote.css('div.tags a.tag::text').getall(),
            }

        next_page = response.css('li.next a::attr(href)').get()
        if next_page is not None:
            yield response.follow(next_page, callback=self.parse)
```
You can also pass a selector to `response.follow` instead of a string; this selector should extract necessary attributes:
```python
for href in response.css('li.next a::attr(href)'):
    yield response.follow(href, callback=self.parse)
```

## More Example
```python
import scrapy


class AuthorSpider(scrapy.Spider):
    name = 'author'

    start_urls = ['http://quotes.toscrape.com/']

    def parse(self, response):
        # follow links to author pages
        for href in response.css('.author + a::attr(href)'):
            yield response.follow(href, self.parse_author)

        # follow pagination links
        for href in response.css('li.next a::attr(href)'):
            yield response.follow(href, self.parse)

    def parse_author(self, response):
        def extract_with_css(query):
            return response.css(query).get(default='').strip()

        yield {
            'name': extract_with_css('h3.author-title::text'),
            'birthdate': extract_with_css('.author-born-date::text'),
            'bio': extract_with_css('.author-description::text'),
        }
```
This spider will start from the main page, it will follow all the links to the authors pages calling the `parse_author` callback for each of them, and also the pagination links with the parse callback as we saw before.

Here we’re passing callbacks to `response.follow` as positional arguments to make the code shorter; it also works for `scrapy.Request`.

The `parse_author` callback defines a helper function to extract and cleanup the data from a CSS query and yields the Python dict with the author data.