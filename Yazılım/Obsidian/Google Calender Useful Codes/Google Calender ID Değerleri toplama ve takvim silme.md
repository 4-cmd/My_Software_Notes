```python
def removing_note_to_calender(date_str : str):
    """Google Calender'dan randevu silmek için yaratılmıştır
    
    * ID Değerlerini Toplar"
    * Date Tarihi string ve şu şekilde olmalı : Yıl Ay Gün
    * Eğer Date Tarihi None ise Tüm Eventlar değilse O güne ait Eventlar olacak"""

# timeMin = Başlangıç zamanını belirtir
# timeMax = Bitiş Zamanını Belirtir

    now = datetime.datetime.now().isoformat() + "Z"
    if date_str is not None and date_str.strip() != "":
        tz = pytz.timezone("Europe/Istanbul")
        dt = datetime.datetime.strptime(date_str, "%Y %m %d")
        
        start_local = tz.localize(datetime.datetime(dt.year,dt.month,dt.day,0,0,0))
        end_local = tz.localize(datetime.datetime(dt.year,dt.month,dt.day,23, 59, 59))
        event_results = service.events().list(calendarId="primary",timeMin=start_local.isoformat(),timeMax=end_local.isoformat(),singleEvents=True,orderBy="startTime").execute()

    else:
        event_results = service.events().list(calendarId="primary",timeMin=now,singleEvents=True,orderBy="startTime").execute()  

    Event_info_list : List[Event_Class_Removing_Keep] = []

    events = event_results.get("items",[])

    for event in events:

        summary = event.get("summary")
        randevu_tarihi = event.get("start").get("dateTime")

        ID = event.get("id")
        yeni_event = Event_Class_Removing_Keep(ID=ID,randevu_tarihi=randevu_tarihi,summary=summary)
        Event_info_list.append(yeni_event)

    return Event_info_list




