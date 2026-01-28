 **Bu kod yapısında şu gösteriliyor** 
* Thread'ın target nesnesinde ya da işlemin gerçekeştiği fonksiyonda (burada dökümanı_vektörleştirme_function(uploaded_files)) streamlit'in st.session_state'deki değerleri güncellenemez
* Bu yüzden önceden belirlenen bir değişken belirlenir ve args parametresine atanır
* Sonra hedef target'da gereken değer atanır 
* Ardından da atanan değerler ile işlem yapılabilir 
* ⚠️ Bu yöntemde kullanıcının eklediği dosya vektörleştirilene kadar beklemek zorundadır


```python
import threading
sonuc_kutusu = []

Thread_variable = threading.Thread
(
target=background_job,
args=(uploaded_files,sonuc_kutusu),
daemon=True
)

Thread_variable.start()

Thread_variable.join() # thread işlemi sona erene kadar beklemek zorundasın

if sonuc_kutusu:
	print(f"sonuç kutusu değeri : {sonuc_kutusu}")
	st.session_state.vektor_islem_sonucu = sonuc_kutusu                

def background_job(uploaded_files,sonuc_kutusu : list):
	# Dökümanı vektörleştirmeden bir türün geldiğini düşün bu bir liste int ya da str olabilir ama burada list 
    result_list = dökümanı_vektörleştirme_function(uploaded_files)
    sonuc_kutusu.extend(result_list)
    print(f"background_job fonksiyon çalıştı ve sonuc_kutusu güncellendi mevcut değeri : {sonuc_kutusu}")
```

**Yöntem -2:**
⚠️ Bu işlemde ise vektörleştirme sonrası başarılı yazısı hemen çıkmaz belirli bir süre ya da sayfanın kendiliğinden yenilenmesi gerekiyor (süre yaklaşık 25 30 saniye)

```python
st.session_state.show_vector_processing = False  # Vektörleştirme işlemi devam ediyor mu?

# Arka plandaki thread tamamlandı mı kontrol et

if st.session_state.show_vector_processing:
	if "sonuc_kutusu_ref" in st.session_state and len(st.session_state.sonuc_kutusu_ref) > 0:
		# Thread sonuç verdi
		st.session_state.vektor_islem_sonucu = st.session_state.sonuc_kutusu_ref.copy()
		vektörleştirme_işlemi_başarılı_mı_function()
		st.session_state.show_vector_processing = False
		print(f"Toast gösterildi, sonuç: {st.session_state.vektor_islem_sonucu}")


sonuc_kutusu = []
# Daemon thread olarak başlat - UI bloklanmaz

Thread_variable = threading.Thread(
	target=background_job,
	args=(uploaded_files, sonuc_kutusu),
	daemon=True  # Uygulamayı kapatırken thread otomatik sonlanır
)
print(f"Thread başlatılıyor (arka planda çalışacak)")
st.toast(body="Dökümanınız vektörleştirme işlemi için işleme alındı. Lütfen bekleyiniz...", icon="ℹ️")

# Thread'i başlat ve BİTMESİNİ BEKLEME (asenkron)
Thread_variable.start()

# İşleme alındığını işaretle
st.session_state.show_vector_processing = True

# Thread için sonuç kutusunu state'e kaydet (thread güncelleyecek)
st.session_state.sonuc_kutusu_ref = sonuc_kutusu

```
