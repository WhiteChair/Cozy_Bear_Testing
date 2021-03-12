pip install linkedin-jobs-scraper

import logging
from linkedin_jobs_scraper import LinkedinScraper
from linkedin_jobs_scraper.events import Events, EventData
from linkedin_jobs_scraper.query import Query, QueryOptions, QueryFilters
from linkedin_jobs_scraper.filters import RelevanceFilters, TimeFilters, TypeFilters, ExperienceLevelFilters



# Change root logger level (default is WARN)
logging.basicConfig(level = logging.INFO)


def on_data(data: EventData):
    print('[ON_DATA]', data.title, data.company, data.date, data.link, len(data.description))


def on_error(error):
    print('[ON_ERROR]', error)


def on_end():
    print('[ON_END]')


scraper = LinkedinScraper(
    chrome_executable_path= None,  
    chrome_options=None,  # Custom Chrome options here
    headless=True,  # Overrides headless mode only if chrome_options is None
    max_workers=1,  
    slow_mo=1,  # Slow down the scraper to avoid 'Too many requests (429)' errors
)

# Add event listeners
scraper.on(Events.DATA, on_data)
scraper.on(Events.ERROR, on_error)
scraper.on(Events.END, on_end)

queries = [
    Query(
        query='Data',
        options=QueryOptions(
            locations=['Belgium'],
            optimize=True,
            limit=10,
            filters=QueryFilters(
                company_jobs_url='https://www.linkedin.com/jobs/search/?f_C=1508%2C6754%2C3880216%2C631981%2C166278%2C963211%2C3625182%2C256009%2C157326%2C282760%2C3627928%2C1519%2C281207%2C18735883%2C10070%2C98774%2C15245937%2C3683364%2C251838%2C2642837&geoId=92000000',  # Filter by companies
                relevance=RelevanceFilters.RECENT,
                time=TimeFilters.MONTH,
                type=[TypeFilters.FULL_TIME, TypeFilters.TEMPORARY],
                experience=[ExperienceLevelFilters.ENTRY_LEVEL, ExperienceLevelFilters.MID_SENIOR],
            )
        )
    ),
]

scraper.run(queries)

## Currently not working (ie no data export)

results = scraper.run(queries)
import csv
with open("jobs.csv", "a") as csvfile:
    fieldnames = ['Title', 'Date', 'Link','ID']
    csv_writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
    csv_writer.writeheader()
    csv_writer.writerows(results)
