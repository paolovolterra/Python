# Webscraping



# [Esercitazione di scraping Web Python ](https://www.geeksforgeeks.org/python-web-scraping-tutorial/)


```python
import requests
from bs4 import BeautifulSoup

url = 'https://www.mps.it/small-business/finanziamenti-a-medio-lungo-termine/sostimutuo-imprese.html'

headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.76 Safari/537.36'} # This is chrome, you can set whatever browser you like
r = requests.get(url, headers=headers)

# Parsing the HTML
soup = BeautifulSoup(r.content, 'html.parser')

print(r)
 
# print content of request
print(r.content)

# print request object
print(r.url)
   
# print status code
print(r.status_code)

soup

print(soup.prettify())

# Getting the title tag
print(soup.title)
# Getting the name of the tag
print(soup.title.name)
# Getting the name of parent tag
print(soup.title.parent.name)
 
# use the child attribute to get
# the name of the child tag

s = soup.find('div', class_='cnt')
lines = s.find_all('p')
 
for line in lines:
    print(line.text)
    
for link in soup.find_all('a'):
    print(link.get('href'))
    
images_list = []
 
images = soup.select('img')
for image in images:
    src = image.get('src')
    alt = image.get('alt')
    images_list.append({"src": src, "alt": alt})
     
for image in images_list:
    print(image)


```















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
