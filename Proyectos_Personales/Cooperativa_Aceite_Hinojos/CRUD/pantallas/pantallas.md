## Contenidos del directorio
```folder-index-content
```

## Cosas a cambiar
- Importar todos los métodos CRUD:
`from services.socios_service import *`
- Utilizar el CRUD: OJO- Tener cuidado. Todo lo que se muestre tendrá que estar en formato string, así que quizá haya que hacer conversiones de tipos.
`socios = get_socios()  # Obtener los socios de la base de datos`
`for socio in socios:`
	`self.socios_table.setItem(row, 0, QTableWidgetItem(socio.numero_cliente))`

Ejemplo de representación:
```
def load_socios(self):

        try:

            socios = get_socios()  # Obtener los socios de la base de datos

            self.socios_table.setRowCount(0)  # Limpiar la tabla antes de agregar los nuevos datos

  

            for socio in socios:

                row = self.socios_table.rowCount()

                self.socios_table.insertRow(row)

                self.socios_table.setItem(row, 0, QTableWidgetItem(socio.numero_cliente))    # Numero del cliente

                self.socios_table.setItem(row, 1, QTableWidgetItem(socio.nombre))    # Nombre

                self.socios_table.setItem(row, 2, QTableWidgetItem(socio.dni))       # DNI

                self.socios_table.setItem(row, 3, QTableWidgetItem("Si" if socio.activo == True else "No"))    # Estado

                self.socios_table.setItem(row, 4, QTableWidgetItem(socio.telefono1))  # Teléfono 1

                self.socios_table.setItem(row, 5, QTableWidgetItem(socio.telefono2))  # Teléfono 2

                self.socios_table.setItem(row, 6, QTableWidgetItem(socio.direccion))  # Dirección

                self.socios_table.setItem(row, 7, QTableWidgetItem(str(socio.saldo)))     # Saldo

                self.socios_table.setItem(row, 8, QTableWidgetItem(socio.notas))     # Notas

        except Exception as e:

            QMessageBox.critical(self, "Error", f"Error al cargar los socios: {str(e)}")
```
