Writing a new spider
=========================
Before starting the development of a scraper for a new city, please carefully read the following guidelines necessary to maintain the project more consistent and ensure that your contribution is more easily accepted.

Reading the code of existing scrapers will also help you better understand how the project is organized and how to start development.


Basic rules for your spider:
------------------------------

- As a rule, all spiders should inherit from `BaseGazetteSpider`.
- The spider class name should follow the following pattern `<StateAbbreviation><CityName>Spider` (for example, `SpSaoPauloSpider` or `MtFelizNatalSpider`).
- The spider's name attribute should follow the pattern `state_abbreviation>_<city_name>` (for example, `sp_sao_paulo` or `mt_feliz_natal`).
- Add the `TERRITORY_ID` attribute with the code found in the `territories.csv` file.
- Add the `start_date` attribute with the date of the first available Official Gazette.
- Use `.get()` and `.getall()` instead of `.extract_first()` and `.extract()`.

Example of a spider:

.. code-block:: python

    from datetime import date
    from gazette.spiders.base import BaseGazetteSpider


    class SpJundiaiSpider(BaseGazetteSpider):
        TERRITORY_ID = "3525904"
        name = "sp_jundiai"
        allowed_domains = ["jundiai.sp.gov.br"]
        start_urls = ["https://imprensaoficial.jundiai.sp.gov.br/"]
        start_date = date(2000, 1, 1)

        def parse(self, response):
            # Here we put all our extraction logic
            ...

Field definitions
-------------------

Here are the field definitions you want to extract from each of the Official Gazettes:

- **date (datetime.date)**: Publication date of the Official Gazette.
- **edition_number (string)**: Edition number of the Official Gazette.
- **is_extra_edition (boolean)**: Some gazettes are extra editions. You can usually identify them by terms like *Extra*, *Extraordinário*, *Suplemento*, etc. If it's not possible to identify, return `False` as the default value.
- **territory_id (string)**: The value of the `TERRITORY_ID` attribute of the spider (automatically filled).
- **power (string)**:  Whether the Official Gazette pertains to the Executive, Legislative, or both branches (`executive`, `legislative`, `executive_legislative`). You may need to manually inspect the content of some Official Gazettes to determine this information.
- **scraped_at (datetime.datetime)**: Set to `datetime.datetime.utcnow()` (automatically filled).
- **file_urls (URL list)**: List of URLs to download the Official Gazettes. Typically, there's only one value in this list for each gazette.

Filter by date
---------------

We want all spiders to be able to collect **ALL** available Official Gazettes. However, we also want to be able to restrict the amount of data by a time period. This should be achieved through the `start_date` and `end_date` attributes that we can pass when running the spiders.

The default value for `start_date` should be the date of the first available Official Gazette on the municipality's page (*hard coded*). You should manually research and find this date, then add it as an argument to your spider.

In the following example, we have a spider that considers September 15, 2001, as the initial date, which is the first date where an Official Gazette is available for collection:

.. code-block:: python

    from datetime import date
    from gazette.spiders.base import BaseGazetteSpider

    class MyCitySpider(BaseGazetteSpider):
        # (...)
        name = "my_city_spider"
        start_date = date(2001, 9, 15)

        def parse(self, response):
            ...

If you want to collect data starting from another date, you can use the following command when running your spider (in the example, we will collect data starting from April 28, 2022)


.. code-block:: sh

    scrapy crawl my_city_spider -a start_date=2021-04-28


The default value of `end_date` is today's date (the date when you are running your spider), so there's **no need** to define it in your spider class. This is set in the `BaseGazetteSpider`.

During development, **it's a good practice to minimize the number of requests as much as possible**. Some pages allow you to filter results by specifying a specific period, while others may require you to add extra logic to avoid visiting unnecessary pages and returning Official Gazettes outside the desired date range. This helps optimize your web scraping process and reduces unnecessary network traffic.

Some tips
-------------

- During development, to prevent making repeated calls to city pages, you can use the Scrapy configuration setting `HTTPCACHE_ENABLED`_. This also speeds up executions as all data is stored locally.
- The Scrapy `shell`_ is very helpful for testing CSS and XPath selectors.


Submitting a new PR
---------------------
Before submitting a new PR for your spider, ensure that it is working correctly.

**RUN YOUR SPIDER IN YOUR DEVELOPMENT ENVIRONMENT AND WAIT UNTIL IT COMPLETES ITS EXECUTION**

Here are some suggested checks:

- Check if the downloaded files are valid;
- Verify if you are collecting all available Official Gazettes;
- If specifying the `start_date` and/or `end_date` arguments, ensure that you are not collecting more Official Gazettes than expected;
- Check if, when running the spider more than once, the results remain the same;
- Ensure there are no execution errors (`log/ERROR` in spider statistics);

.. _shell: https://docs.scrapy.org/en/latest/topics/shell.html
.. _HTTPCACHE_ENABLED: https://docs.scrapy.org/en/latest/topics/downloader-middleware.html#httpcache-enabled
