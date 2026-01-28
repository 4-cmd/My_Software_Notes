```python

from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.service import Service
from selenium.webdriver import Chrome
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.options import Options

  

name_of_url = "google.com/"

service = Service(ChromeDriverManager().install())

  

chrome_options = Options()
chrome_options.add_argument("--headless") # Tarayıcıyı gizli çalıştırır
chrome_options.add_argument("--window-size=1920,1080") # Sanal ekran boyutu
  

driver = Chrome(service=service,options=chrome_options)
driver.get(name_of_url) # url sayfasına gider

# driver.fullscreen_window() # Her şeyi gizler, sadece web sitesini bırakır (F11 modu)
# driver.maximize_window() # Pencereyi büyütür ama sekmeler ve Windows çubuğu durur.

all_html = driver.page_source # tüm html kodunu alır 

element_by_ID = driver.find_element(By.ID,"element-id-değeri") # id değerine göre seçer tekli
element_by_Class_Name = driver.find_element(By.CLASS_NAME,"elementin-sinif-ismi")
element_by_TAG_NAME = driver.find_element(By.TAG_NAME, "a")

element_by_CSS_SELECTOR = driver.find_element(By.CSS_SELECTOR, "#main-form .submit-btn")
# CSS Seçicisi ile elementi bulur (ID için # - sınıf için . kullanılır).

id_name_attribute = element_by_ID.get_attribute("name")
waited_ID_element = (By.ID,"element_id_değeri") # id yerine tag class veya herhangi şey kullanılabilir

  

waited_element = WebDriverWait(driver,15).until(EC.presence_of_element_located(waited_ID_element))

driver.back() # web sitede geriye gitmek için kullanılır

driver.close() # sadece aktif olan sayfayı ortadan kaldırır,tarayıcıda tek sayfa var kapatılsa bilene tarayıcı kapanır ama arka planda çalışmaya devam eder

driver.quit() # hem sekmeleri hem de tarayıcıyı kapatır


```