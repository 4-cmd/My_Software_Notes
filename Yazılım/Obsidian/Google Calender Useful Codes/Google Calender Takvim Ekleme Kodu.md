```python
from googleapiclient.errors import HttpError
import datetime

def add_note_to_calendar(summary, description, start_datetime : datetime.datetime, end_datetime : datetime.datetime, time_zone='Europe/Istanbul'):
    global notification
    """ Google Takvim'e belirtilen detaylarla yeni bir etkinlik (not) ekler. """

    # Etkinlik (Not) verisinin API'ye uygun JSON yapısı

    event = {
        'summary': summary, # Notun başlığı
        'description': description, # Notun detaylı açıklaması
        'start': {
            'dateTime': start_datetime.isoformat(), # ISO 8601 formatına dönüştürülmüş başlangıç zamanı
            'timeZone': time_zone,
        },
        'end': {
            'dateTime': end_datetime.isoformat(),
            'timeZone': time_zone,
        },
    }

    try:
        # Etkinliği birincil (primary) takvime ekleme
        # calendarId='primary' parametresi, varsayılan takviminize ekler.

        event = service.events().insert(calendarId='primary', body=event).execute()
        notification = "🎉 NOT BAŞARIYLA EKLENDİ!"
        print("\n🎉 NOT BAŞARIYLA EKLENDİ!")
        print(f"Başlık: {summary}")
        print(f"Takvim Linki: {event.get('htmlLink')}")
        return "🎉 NOT BAŞARIYLA EKLENDİ!"

    except HttpError as error:
        notification = "❌ ETKİNLİK EKLEME HATASI OLUŞTU"
        print(f"\n❌ ETKİNLİK EKLEME HATASI OLUŞTU: {error}")
        return "❌ ETKİNLİK EKLEME HATASI OLUŞTU"

```