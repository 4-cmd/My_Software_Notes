

```python
import requests
from bs4 import BeautifulSoup

url = "https://www.webtekno.com/"

r = requests.get(url)
islem_basarili_mi = r.status_code

  

if islem_basarili_mi == 200:
    print(f"istek başarılıdır")
    html_code = r.text
    print(f"html code : {html_code}")

  

    soup = BeautifulSoup(html_code,"lxml")
    tag = soup.find("h5") # sayfadaki ilk h5 bulur ve yazar birden fazlası için findall

  

    if tag:
        print(f"h5 içeriği bulundu ve değeri : {tag.get_text(strip=True).strip()}")
    
    else:
        print(f"h5 bulunamadı")

  

    linkler = soup.find_all("a")
    for link in linkler:
        attribute_href = link.get("href")
       
	# Class ismi 'email-subject' olan tüm <span> etiketlerini bul
    mailler = soup.find_all("span",class_ = "email-subject")

    # ID'si 'main-container' olan bölümü bul
    ana_bolum = soup.find("div", id="main-container")

    # 4. İç İçe Etiketlerde Gezinme
    tablo = soup.find("table")
    satirlar = tablo.find_all("tr") # Sadece o tablonun içindeki satırları alır

  

    # CSS nitelik seçici
    nitelik_seçici = 'a[target="_blank"]'
    a_elemtleri = soup.select(nitelik_seçici)

    for element in a_elemtleri:
        pass

    # 3. İçinde Belirli Bir Metin Geçenler (En Çok Kullanılan)
    # Bazen linklerin sonu veya başı değişebilir.
    # Bu durumda şu operatörler hayat kurtarır:
    
    """
    ^= : İle başlayan    $= : İle biten     *= : İçeren
    """

    # href değeri "mailto:" ile başlayanlar (E-posta linkleri)
    a_elemtleri = soup.select('a[href^="mailto:"]')
    
    # href içinde "python" kelimesi geçenler
    a_elemtleri = soup.select('a[href*="python"]')

  

    # Dosyaya Yazma İşlemi
    with open("haberler.txt","a",encoding="utf-8") as dosya:
        for link in linkler:
            link_hrefi = link.get("href") + "\n"
            dosya.write(link_hrefi)

else:
    print(f"işlem başarısız")

```