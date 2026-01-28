
**<font color="#ff0000">Eager Loading Nasıl Yapılır?</font>**


```python
from sqlalchemy.orm import joinedload
from sqlalchemy.future import select

# Kullanıcıyı (User) çekerken, onunla ilişkili olan 'posts' (yazılar) tablosunu da getirir.
stmt = select(User).options(joinedload(User.posts))

# Sonuçları aldığında, her kullanıcının .posts özelliği önceden yüklenmiş olur.
result = session.execute(stmt).scalars().all()


```

**⚠️ Eğer bire-çok (one-to-many) bir ilişki varsa `joinedload`, çok-v-çok (many-to-many) veya çok geniş tablolar varsa `selectinload` kullanmak performans açısından daha iyidir.**