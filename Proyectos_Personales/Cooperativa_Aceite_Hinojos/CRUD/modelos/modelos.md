## Contenidos del directorio

```folder-index-content
```

## Como pedirle a chatgpt un nuevo modelo
Creame el modelo para los albaranes sabiendo que este es el create table de postgres:

***AQUÍ EL CREATE TABLE DE LA BBDD***

Y este es un modelo de ejemplo:

from sqlalchemy import Column, Integer, String, Boolean, Text, DECIMAL, ForeignKey

from .base import Base

class Socio(Base):

__tablename__ = 'socios'

id_socio = Column(Integer, primary_key=True)

id_localidad = Column(Integer, nullable=True)

id_banco = Column(Integer, ForeignKey('bancos.id_banco'))

numero_cliente = Column(String(50), unique=True, nullable=False)

codigo_postal = Column(String(10))

activo = Column(Boolean, default=True)

dni = Column(String(20), unique=True, nullable=False)

nombre = Column(String(255), nullable=False)

direccion = Column(Text)

telefono1 = Column(String(15))

telefono2 = Column(String(15))

fax = Column(String(15))

movil = Column(String(15))

notas = Column(Text)

numero_cuenta = Column(String(34)) # IBAN

saldo = Column(DECIMAL(12, 2), default=0)