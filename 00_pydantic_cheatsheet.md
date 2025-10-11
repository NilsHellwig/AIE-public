# Pydantic Cheatsheet 

## 1. Grundlegendes Modell erstellen

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
    email: str
```

## 2. Modell instanziieren

```python
user = User(name="Max", age=25, email="max@example.com")
print(user.name)  # Max
```

## 3. Optionale Felder

```python
from typing import Optional

class User(BaseModel):
    name: str
    age: Optional[int] = None
```

## 4. Standardwerte setzen

Standardwerte sorgen dafür, dass Felder nicht zwingend angegeben werden müssen.

```python
class User(BaseModel):
    name: str
    age: int = 18
    active: bool = True
```

## 5. Validierung mit Field

```python
from pydantic import BaseModel, Field

class User(BaseModel):
    name: str = Field(..., min_length=1, max_length=50)
    age: int = Field(..., ge=0, le=120)
```

## 6. Daten zu Dictionary konvertieren

```python
user = User(name="Max", age=25, email="max@example.com")
user_dict = user.model_dump()
# oder für Pydantic v1: user.dict()
```

## 7. Daten zu JSON konvertieren

```python
user_json = user.model_dump_json()
# oder für Pydantic v1: user.json()
```

## 8. Modell aus Dictionary erstellen

```python
data = {"name": "Max", "age": 25, "email": "max@example.com"}
user = User(**data)
```

## 9. Modell aus JSON erstellen

```python
json_str = '{"name": "Max", "age": 25, "email": "max@example.com"}'
user = User.model_validate_json(json_str)
# oder für Pydantic v1: User.parse_raw(json_str)
```

## 10. Benutzerdefinierte Validierung

```python
from pydantic import field_validator

class User(BaseModel):
    name: str
    age: int
    
    @field_validator('age')
    @classmethod
    def check_age(cls, v):
        if v < 0:
            raise ValueError('Age must be positive')
        return v
```

## 11. Email-Validierung

```python
from pydantic import EmailStr

class User(BaseModel):
    email: EmailStr
```

## 12. URL-Validierung

```python
from pydantic import HttpUrl

class Website(BaseModel):
    url: HttpUrl
```

## 13. Nested Models

```python
class Address(BaseModel):
    street: str
    city: str
    
class User(BaseModel):
    name: str
    address: Address
```

## 14. Listen von Modellen

```python
from typing import List

class Team(BaseModel):
    name: str
    members: List[User]
```

## 15. Enum-Validierung

```python
from enum import Enum

class Role(str, Enum):
    ADMIN = "admin"
    USER = "user"
    GUEST = "guest"

class User(BaseModel):
    name: str
    role: Role
```

## 16. Datum und Zeit

```python
from datetime import datetime

class Event(BaseModel):
    name: str
    date: datetime
```

## 17. UUID-Felder

```python
from uuid import UUID

class User(BaseModel):
    id: UUID
    name: str
```

## 18. Konstanten mit Literal

```python
from typing import Literal

class Config(BaseModel):
    env: Literal["dev", "staging", "prod"]
```

## 19. Model Config - Extra Felder verbieten

```python
class User(BaseModel):
    model_config = {"extra": "forbid"}
    
    name: str
    age: int
```

## 20. Model Config - Unveränderliche Modelle

```python
class User(BaseModel):
    model_config = {"frozen": True}
    
    name: str
    age: int
```

## 21. Root Validator

**Achtung:** `@model_validator` kann nicht für Structured Outputs verwendet werden. Die validation wird nur auf dem finalen LLM-Output angewendet.

```python
from pydantic import model_validator

class User(BaseModel):
    password: str
    password_confirm: str
    
    @model_validator(mode='after')
    def check_passwords_match(self):
        if self.password != self.password_confirm:
            raise ValueError('Passwords do not match')
        return self
```
