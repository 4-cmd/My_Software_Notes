```python
import os
from google.oauth2.credentials import Credentials
from google.auth.transport.requests import Request
from googleapiclient.discovery import build
from google_auth_oauthlib.flow import InstalledAppFlow

# Gereklilikler 
# pip install google-api-core google-api-python-client google-api-python-client-stubs google-auth google-auth-httplib2 google-auth-oauthlib google-auth-stubs googleapis-common-protos

SCOPES = ["[https://www.googleapis.com/auth/calendar](https://www.googleapis.com/auth/calendar)"]

def get_calendar_service():
    creds = None
    if os.path.exists("token.json"):
        creds = Credentials.from_authorized_user_file("token.json", SCOPES)

    if not creds or not creds.valid:
        if creds and creds.expired and creds.refresh_token:
            creds.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file(
                "client_secret_250698893592-qpmtdnrb9genkv69r3ov2c9g5jlio0oe.apps.googleusercontent.com.json",
                SCOPES
            )
            creds = flow.run_local_server(port=0)

        with open("token.json",'w') as token:
            token.write(creds.to_json())

    try:
        service = build("calendar", "v3", credentials=creds)
        print(f"Servis Döndürülüyor")
        return service

    except HttpError as error:
          print(f"An error occurred: {error}")

service = get_calendar_service()

```

**<font color="#ff0000">Bağlantılı Başarılı mı Kodu</font>**

```python
try:
    # Kullanıcının Takvim Listesini Çekmeye Çalışıyoruz
    calendar_list = service.calendarList().list().execute()

    if calendar_list and 'items' in calendar_list:
        print("\n✅ BAĞLANTI BAŞARILI!")
        print("Google Takvim hizmetine başarıyla bağlandınız.")
        print(f"Yetkilendirilen e-posta adresi: {calendar_list['items'][0]['id']}")
        print(f"Toplam {len(calendar_list['items'])} adet takvim listelendi.")

        # Takvimleri listelemeye devam edin (mevcut kodunuz)

        events = service.events().list(
            calendarId="primary",
            maxResults=10
        ).execute()

        print("\nSon 10 etkinlik başarıyla çekildi:")
        for e in events.get("items", []):
            print(f"- {e.get('summary', 'Başlıksız Etkinlik')} ({e['start'].get('dateTime', e['start'].get('date'))})")

    else:

        print("\n⚠️ BAĞLANTI BAŞARISIZ!")
        print("Servis oluşturuldu ancak takvim listesi boş döndü. Yetkilendirmeyi kontrol edin.")

except Exception as e:
    # Eğer API erişiminde herhangi bir hata oluşursa (örneğin hala 403 hatası alıyorsanız)
    print("\n❌ BAĞLANTI VEYA ERİŞİM HATASI!")
    print("Hata Detayı:", e)
    print("Lütfen Google Cloud Console'da OAuth ayarlarını ve Test Kullanıcılarını tekrar kontrol edin.")

```