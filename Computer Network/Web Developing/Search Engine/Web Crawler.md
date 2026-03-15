# Set up
```Haskell
pip install scrapy scrapy-splash
scrapy startproject <project-name>
cd <project-name> && scrapy genspider <spidername> <url>
```
# Develop Steps
1. Explore the selectors/xpaths
```Shell
scrapy shell "https://quotes.toscrape.com/page/1/"
response.css("title::text").getall()
response.xpath("/html/body/div/div[2]/div[1]/div[1]/span[1]/text()").getall()
view(response)
```

> use with the browser extension _SelectorGadget_ for CSS selectors or xpaths
1. Write the spider and include selectors/xpaths into the `parse` method
2. Run web crawler
```Shell
scrapy crawl <spider_name> -o data.json
# -O overwrite the existing file; -o append to the existing file
```
  
[https://scrapy-cookbook.readthedocs.io/zh-cn/latest/scrapy-11.html](https://scrapy-cookbook.readthedocs.io/zh-cn/latest/scrapy-11.html)
# Scrapy on JS heavy pages
Need browser-based automation tools, such as: **Playwright (not working on windows), Puppeteer, Splash (need docker)**
