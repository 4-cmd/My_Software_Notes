
Mevcut verilere dayanarak yeni bir değer üretir. (Örneğin; `first_name` ve `last_name` varsa otomatik `full_name` oluşturur

```python
from pydantic import BaseModel, computed_field

class Person(BaseModel):
    first_name: str
    last_name: str
  
    @computed_field
    @property
    def full_name(self) -> str:
        return f"{self.first_name} {self.last_name}"

  

p = Person(first_name="Şevki",last_name="Dumankaya")
print(f"Name : {p.first_name}\nSurname : {p.last_name}\nFull Name : {p.full_name}")

# Çıktı 
"""
Name : Şevki
Surname : Dumankaya
Full Name : Şevki Dumankaya
"""


```