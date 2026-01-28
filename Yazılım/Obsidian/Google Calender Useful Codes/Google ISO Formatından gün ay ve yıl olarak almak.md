```python
import datetime
import pytz

def eventları_gün_ay_ve_yıl_olarak_almak():
    """2025-12-13T17:30:00+03:00 Böyle bir zamanı 13 Aralık 2025 olarak almak için yaratıldı"""


    aylar = {
        1: "Ocak", 2: "Şubat", 3: "Mart", 4: "Nisan",
        5: "Mayıs", 6: "Haziran", 7: "Temmuz", 8: "Ağustos",
        9: "Eylül", 10: "Ekim", 11: "Kasım", 12: "Aralık"
    }

    UTC = datetime.UTC
   # Hata veren satır yerine doğru dize oluşturma yöntemi:
    now_aware = datetime.datetime.now(UTC).replace(microsecond=0)
    timeMin_utc = now_aware.isoformat().replace('+00:00', 'Z')
    tz = pytz.timezone("Europe/Istanbul")

	event_results = service.events().list(calendarId="primary",timeMin=timeMin_utc,singleEvents=True,orderBy="startTime",timeZone=tz).execute()
    events_summary_tarih_list : list[Find_Event_Class_Date_Class] = []
    events = event_results.get("items")

    for event in events:

        summary = event.get("summary")
        iso_randevu_formati = event.get("start").get("dateTime")
        dt = datetime.datetime.fromisoformat(iso_randevu_formati)
        
        gün = dt.day
        ay = aylar[dt.month]
        yıl = dt.year
        
        randevu_tarihi = f"{gün} {ay} {yıl}"
        new_instance = Find_Event_Class_Date_Class(summary=summary,randevu_tarihi=randevu_tarihi)
        events_summary_tarih_list.append(new_instance)
        print(f"Summary : {summary}\n\n\nRandevu Tarihi : {randevu_tarihi}")

    return events_summary_tarih_list


```