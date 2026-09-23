## Descripción
Es una aplicación sencilla que iniciar un servidor y hace un CRUD en una base de datos postgresql.
Notas y prerequisitos: [[Notas generales Python y FastAPI]]
## main.py
from fastapi import FastAPI, HTTPException
from models.todo import Todo
from db import session

app = FastAPI()

@app.post("/")
async def create_todo(text:str, is_done:bool=False):
    todo = Todo(text=text, is_done=is_done)
    session.add(todo)
    session.commit()
    return {"todo added": todo.text}

@app.get("/{id}")
async def get_todo(id: int):
    todo_query=session.query(Todo).filter(Todo.id == id)
    todo = todo_query.first()
    if todo is None:
        raise HTTPException(status_code=404, detail="todo not found")
    return todo

@app.put("/{id}")
async def update_todo(id:int, new_text: str, is_done: bool=False):
     todo_query=session.query(Todo).filter(Todo.id == id)
     todo = todo_query.first()
     if new_text:
        todo.text = new_text
     todo.is_done = is_done
     session.add(todo)
     session.commit()   

@app.delete("/{id}")
async def delete_todo(id:int):
    todo_query=session.query(Todo).filter(Todo.id==id)
    todo = todo_query.first()
    session.delete(todo)
    session.commit()
    return {"todo deleted": id}

## todo.py
from sqlalchemy import Boolean, Column, Integer, String
from sqlalchemy.orm import declarative_base
from db import engine

Base = declarative_base()

class Todo(Base):
    __tablename__ = "todos"
    id = Column(Integer, primary_key = True)
    text = Column(String)
    is_done = Column(Boolean, default = False)

Base.metadata.create_all(engine)

## db.py
from sqlalchemy import create_engine
from sqlalchemy.engine import URL
from sqlalchemy.orm import sessionmaker

url = URL(
    drivername = "postgresql",
    username = "postgres",
    password = "postgres",
    host = "localhost",
    database = "postgres",
    port=5432,
    query={}
)

engine = create_engine(url)
Session = sessionmaker(bind=engine)
session = Session()