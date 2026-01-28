
| Özellik        | html.parser      | lxml                      |
| -------------- | ---------------- | ------------------------- |
| Hız            | Orta             | Çok Hızlı                 |
| Kütüphane      | Python ile gelir | Dışarıdan yükleme gerekir |
| Hata Toleransı | Orta             | Yüksek                    |


---

### 1. Veri Temizleme (strip)

Web sitelerinden veri çekerken metinlerin başında ve sonunda çok fazla boşluk veya `\n` (alt satır) karakteri olur.

- **Kritik Not:** `tag.text` yerine her zaman `tag.text.strip()` kullanmalısın.

Text yerine get_text() kullanmanda bir sakınca yok

**Otomatik Temizleme (Strip):** Metnin başındaki ve sonundaki boşlukları tek seferde silebilirsin.

```python
# Hem metni alır hem de boşlukları siler 
metin = tag.get_text(strip=True)
```