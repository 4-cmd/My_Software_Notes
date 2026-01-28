```python

def calender_ID_remove(silenecek_IDler : list[str]):
    """
    * Bu fonksiyon Takvimdeki Etkinlikleri siler Parametre olarak ise Takvim IDlerini alır
    * Silme işleminin başarılı mı değil mi olduğunu döndürerek gösterir
    """

    silme_işlemi = False
    for ID in silenecek_IDler:
        try:
            clean_id = str(ID).strip()
            service.events().delete(calendarId="primary",eventId=clean_id).execute()
            print(f"Silme işlemi başarıyla tamamlandı")
            silme_işlemi = True
        
        except Exception as hata:
            print(f"Hata : {hata}")
            silme_işlemi = False

    return silme_işlemi



```