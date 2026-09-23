## Contenidos del directorio
```folder-index-content
```

## Como pedirle a chatgpt un nuevo servicio
Teniendo en cuenta el modelo de ***AQUÍ EL MODELO QUE UTILIZARÁ EL SERVICIO**** creame el servicio con todo el CRUD, tomando como referencia este:

from sqlalchemy.orm import Session
from models.socio import Socio
from services.databaseConnection import get_session

def get_socios():
    """
    Devuelve todos los socios de la base de datos.
    """
    db = get_session()
    return db.query(Socio).all()

def insert_socio(socio_data: dict):
    """
    Inserta un nuevo socio en la base de datos.
    :param socio_data: Diccionario con los datos del socio.
    """
    db = get_session()
    
    nuevo_socio = Socio(**socio_data)
    db.add(nuevo_socio)
    db.commit()
    db.refresh(nuevo_socio)
    return nuevo_socio
