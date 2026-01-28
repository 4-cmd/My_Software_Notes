```python
"""
Listeleme
🚨 Eğer maxResults belirtilmez ise varsayılan değer 2500'dür
"""

def get_events(randevu_istek_sayisi : int | None):
    "Bu fonksiyon, Kullanıcının Tüm eventları getirebilir ya da belli sayıda event getirebilir ve string olarak döndürür"

    info = ""

    now = datetime.datetime.now().isoformat() + "Z"
    max_etkinlik_sayisi = 0
    if randevu_istek_sayisi is not None:
        if randevu_istek_sayisi < 0:
            info = "Negatif Bir Değer Girdiğiniz için işlem yapılamadı"
            return info
        max_etkinlik_sayisi = randevu_istek_sayisi
        events_result = service.events().list(calendarId="primary",timeMin=now,maxResults=max_etkinlik_sayisi,singleEvents=True,orderBy="startTime").execute()

    else:
        events_result = service.events().list(calendarId="primary",timeMin=now,singleEvents=True,orderBy="startTime").execute()    

    events = events_result.get("items",[])
    for event in events:
        summary = event.get("summary")
        randevu_başlangıç_tarihi = event.get("start").get("dateTime")
        
        info += f"Başlık : {summary}\n\n\nRandevu Tarihi : {randevu_başlangıç_tarihi}"
        info += "\n\n\n"
        print(f"Randevu Tarihi Türü : {type(randevu_başlangıç_tarihi)}")

    return info

```