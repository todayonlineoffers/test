import scrapy
from scrapy.loader import ItemLoader
from scrapy import Spider
from quotes_scrapy.items import QuotesScrapyItem

class QuotesSpider(scrapy.Spider):
    name = 'quotes_scrapy'
    allowed_domains = ['quotes.toscrape.com']
    start_urls = ['http://quotes.toscrape.com/']
    
    def parse(self, response):
        #category = response.xpath('//*[@class = "tag-item"]/a/text()').extract()
        #title = response.xpath('//h1/a/text()').extract_first()
        #yield{'category': category ,'title': title}
        #l = ItemLoader(item = QuotesScrapyItem(), response = response)
        quotes = response.xpath('//*[@class="quote"]')
        
        for quote in quotes:
            #print(response.xpath('//*[@class="previous"]/a/@href').extract_first())
            page_url = response.xpath('//*[@class="previous"]/a/@href').extract_first()
            
            if page_url is not None:
                page_no = int(page_url.replace('/page/','').replace('/',''))
                page_no = page_no + 1
            else:
                page_no = 1
            text = quote.xpath('.//*[@class="text"]/text()').extract_first()
            author = quote.xpath('.//*[@itemprop="author"]/text()').extract_first()
            tag = quote.xpath('.//*[@itemprop="keywords"]/@content').extract_first()
            yield{'Page_no': page_no,'Text': text ,'Author': author, 'Tag': tag}
        
            next_page_url = response.xpath('//*[@class="next"]/a/@href').extract_first()
            absolute_next_page_url = response.urljoin(next_page_url)
            yield scrapy.Request(absolute_next_page_url)
        
        