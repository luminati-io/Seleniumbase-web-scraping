# Web Scraping With SeleniumBase

[![Promo](https://github.com/luminati-io/LinkedIn-Scraper/blob/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.com)

Simplify web scraping with SeleniumBase using its advanced features and step-by-step guide. Interested in Selenium web scraping? Check out [this guide](https://brightdata.com/blog/how-tos/using-selenium-for-web-scraping).

## What Is SeleniumBase?

SeleniumBase is a Python framework for browser automation, built on top of Selenium/WebDriver APIs. It supports tasks from testing to scraping and includes features like CAPTCHA bypassing and bot-detection avoidance.

## SeleniumBase vs Selenium: Feature and API Comparison

| Feature                  | SeleniumBase                                      | Selenium                                    |
|--------------------------|---------------------------------------------------|---------------------------------------------|
| Built-in test runners    | Integrates with pytest, pynose, and behave        | Requires manual setup for test integration  |
| Driver management        | Auto-downloads matching browser driver            | Manual download and configuration           |
| Web automation logic     | Combines steps into single method call            | Requires multiple lines of code             |
| Selector handling        | Auto-detects CSS or XPath selectors               | Requires explicit selector types            |
| Timeout handling         | Default timeouts to prevent failures              | Immediate failures without explicit timeouts|
| Error outputs            | Clean, readable error messages                    | Verbose, less interpretable error logs      |
| Dashboards and reports   | Built-in dashboards, reports, and screenshots     | No built-in dashboards or reporting         |
| Desktop GUI applications | Visual tools for test running                     | Lacks desktop GUI tools                     |
| Test recorder            | Built-in test recorder                            | Requires manual script writing              |
| Test case management     | Provides CasePlans                                | No built-in test case management            |
| Data app support         | Includes ChartMaker for data apps                 | No additional tools for data apps           |

## Using SeleniumBase for Web Scraping: Step-By-Step Guide

### Step #1: Project Initialization

```bash
mkdir seleniumbase-scraper
cd seleniumbase-scraper
python -m venv env
```

Activate the virtual environment:

- On Linux/macOS: `./env/bin/activate`
- On Windows: `env/Scripts/activate`

Install SeleniumBase:

```bash
pip install seleniumbase
```

### Step #2: SeleniumBase Test Setup

```python
from seleniumbase import SB

with SB() as sb:
    pass
```

Run the script:

```bash
python3 scraper.py --headless
```

### Step #3: Connect to the Target Page

```python
sb.open("https://quotes.toscrape.com/")
```

### Step #4: Select the Quote Elements

```python
quote_elements = sb.find_elements(".quote")
```

### Step #5: Scrape Quote Data

```python
from selenium.webdriver.common.by import By

for quote_element in quote_elements:
    text_element = quote_element.find_element(By.CSS_SELECTOR, ".text")
    text = text_element.text.replace("“", "").replace("”", "")
    author_element = quote_element.find_element(By.CSS_SELECTOR, ".author")
    author = author_element.text
    tags = [tag.text for tag in quote_element.find_elements(By.CSS_SELECTOR, ".tag")]
```

### Step #6: Populate the Quotes Array

```python
quotes.append({"text": text, "author": author, "tags": tags})
```

### Step #7: Implement Crawling Logic

```python
while sb.is_element_present(".next"):
    sb.click(".next a")
```

### Step #8: Export the Scraped Data

```python
import csv

with open("quotes.csv", mode="w", newline="", encoding="utf-8") as file:
    writer = csv.DictWriter(file, fieldnames=["text", "author", "tags"])
    writer.writeheader()
    for quote in quotes:
        writer.writerow({"text": quote["text"], "author": quote["author"], "tags": ";".join(quote["tags"])})
```

### Step #9: Put It All Together

```python
from seleniumbase import SB
from selenium.webdriver.common.by import By
import csv

with SB() as sb:
    sb.open("https://quotes.toscrape.com/")
    quotes = []
    while sb.is_element_present(".next"):
        quote_elements = sb.find_elements(".quote")
        for quote_element in quote_elements:
            text_element = quote_element.find_element(By.CSS_SELECTOR, ".text")
            text = text_element.text.replace("“", "").replace("”", "")
            author_element = quote_element.find_element(By.CSS_SELECTOR, ".author")
            author = author_element.text
            tags = [tag.text for tag in quote_element.find_elements(By.CSS_SELECTOR, ".tag")]
            quotes.append({"text": text, "author": author, "tags": tags})
        sb.click(".next a")
    with open("quotes.csv", mode="w", newline="", encoding="utf-8") as file:
        writer = csv.DictWriter(file, fieldnames=["text", "author", "tags"])
        writer.writeheader()
        for quote in quotes:
            writer.writerow({"text": quote["text"], "author": quote["author"], "tags": ";".join(quote["tags"])})
```

Run the scraper:

```bash
python3 script.py --headless
```

## Advanced SeleniumBase Scraping Use Cases

### Automate Form Filling and Submission

```python
from seleniumbase import BaseCase
BaseCase.main(__name__, __file__)

class LoginTest(BaseCase):
    def test_submit_login_form(self):
        self.open("https://quotes.toscrape.com/login")
        self.type("#username", "test")
        self.type("#password", "test")
        self.click("input[type=\"submit\"]")
        self.assert_text("Top Ten tags")
```

Run the test:

```bash
pytest login.py
```

### Bypass Simple Anti-Bot Technologies

```python
from seleniumbase import SB

with SB(uc=True) as sb:
    url = "https://www.scrapingcourse.com/antibot-challenge"
    sb.uc_open_with_reconnect(url, reconnect_time=4)
    sb.uc_gui_click_captcha()
    sb.save_screenshot("screenshot.png")
```

### Bypass Complex Anti-Bot Technologies

```python
from seleniumbase import SB

with SB(uc=True, test=True) as sb:
    url = "https://gitlab.com/users/sign_in"
    sb.activate_cdp_mode(url)
    sb.uc_gui_click_captcha()
    sb.sleep(2)
    sb.save_screenshot("screenshot.png")
```

## Conclusion

SeleniumBase offers advanced features for web scraping, including UC Mode and CDP Mode for bypassing anti-bot measures. For more robust solutions, consider using cloud-based browsers like [Scraping Browser from Bright Data](https://brightdata.com/products/scraping-browser).
