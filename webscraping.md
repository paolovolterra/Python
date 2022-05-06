# Webscraping

## [ESTRARRE FILE JS E CSS DA UNA PAGINA WEB CON PYTHON ](https://pixelhub.it/blog/come-estrarre-file-js-e-css-da-una-pagina-web-con-python)
```python
from bs4 import BeautifulSoup

import requests
from bs4 import BeautifulSoup as bs
from urllib.parse import urljoin

# URL della pagina web da estrarre
url = "https://www.bancaifis.it/business/pmi/finanziamenti-medio-lungo-termine/#mutui-garantiti-dal-fondo-di-garanzia"

# inizializza una sessione
session = requests.Session()
# imposta l'User-agent come un browser
session.headers["User-Agent"] = "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/44.0.2403.157 Safari/537.36"

# prende il contenuto HTML della pagina
html = session.get(url).content

# fa il parsing HTML della pagina con beautiful soup
soup = bs(html, "html.parser")

# prende i file Javascript
script_files = []

for script in soup.find_all("script"):
    if script.attrs.get("src"):
        # se il tag ha l'attributo 'src'
        script_url = urljoin(url, script.attrs.get("src"))
        script_files.append(script_url)


# prende i file CSS
css_files = []

for css in soup.find_all("link"):
    if css.attrs.get("href"):
        # se il tag ha un attributo 'href'
        css_url = urljoin(url, css.attrs.get("href"))
        css_files.append(css_url)

        
print("Script JS totali in pagina:", len(script_files))
print("Totale file CSS in pagina:", len(css_files))

# write file links into files
with open("javascript_files.txt", "w") as f:
    for js_file in script_files:
        print(js_file, file=f)

with open("css_files.txt", "w") as f:
    for css_file in css_files:
        print(css_file, file=f)
```
