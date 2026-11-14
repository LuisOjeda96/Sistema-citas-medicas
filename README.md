# Sistema Web de Citas Médicas Portable

Prototipo de agenda de citas médicas construido con PHP y SQLite, desarrollado para la
asignatura **Interacción Humano-Computador**, 4.º nivel, paralelo C2 — Universidad Estatal
de Milagro.

Unidad 4: Evaluación de interfaces, usabilidad y accesibilidad

El sistema es portable: no necesita un gestor de base de datos instalado ni importar
archivos SQL. La base de datos es un archivo local que el propio sistema crea la primera
vez que se ejecuta.

---

## Contenido del repositorio

| Archivo | Qué es |
|---|---|
| `index.php` | Punto de entrada: sesión, registro de citas y reporte |
| `db_config.php` | Conexión a SQLite y creación automática del esquema |
| `css/style.css` | Hoja de estilos de la interfaz |
| `EJECUTAR_SISTEMA.bat` | Lanzador del servidor local en Windows |

No se versionan dos elementos:

- `citas_medicas.db`, porque el sistema lo genera solo en la primera ejecución.
- La carpeta `php/`, porque contiene los binarios del motor PHP portable.

---

## Requisitos

- PHP 7.4 o superior.
- Extensión `pdo_sqlite` activa. Para comprobarlo:

```bash
php -m
```

La salida debe incluir `pdo_sqlite`. Si no aparece, abrir `php/php.ini`, buscar la línea
del controlador y quitarle el punto y coma inicial.

---

## Cómo ejecutarlo

En Windows, doble clic en `EJECUTAR_SISTEMA.bat`.

Desde la línea de comandos, en cualquier sistema:

```bash
php -S localhost:8000
```

Luego abrir `http://localhost:8000/index.php` en el navegador.

### Credenciales por omisión

| Usuario | Contraseña |
|---|---|
| `admin` | `1234` |

Se crean automáticamente la primera vez que arranca el sistema.

---

## Base de datos

El archivo `db_config.php` abre la conexión y crea el esquema si no existe:

```
usuarios (id, usuario, password)
citas    (id, paciente_nombre, especialidad, fecha, hora, estado)
```

El archivo `citas_medicas.db` se crea en la misma carpeta del proyecto, de modo que mover
o copiar la carpeta mueve también los datos. Para empezar con una base vacía basta con
borrar ese archivo: se regenera en el siguiente arranque.

---

## Pruebas realizadas

| Prueba | Resultado esperado | Resultado obtenido |
|---|---|---|
| Arranque sin `citas_medicas.db` | El archivo se crea con las dos tablas y el usuario `admin` | Correcto |
| Acceso con credenciales válidas | Se abre el panel de control | Correcto |
| Acceso con credenciales inválidas | Mensaje de acceso denegado, sin abrir el panel | Correcto |
| Registro de una cita | La cita aparece de inmediato en el reporte | Correcto |
| Cierre de sesión y nuevo acceso | Las citas registradas siguen presentes | Correcto |

---

## Limitaciones conocidas

Son propias de un prototipo académico y están documentadas en el manual técnico:

- Las contraseñas se guardan y comparan en texto plano.
- Los formularios no llevan protección contra falsificación de peticiones entre sitios.
- El formulario acepta fechas pasadas y permite dos citas en el mismo horario y especialidad.
- El campo `estado` de la tabla `citas` nunca se actualiza desde la interfaz.
- Los campos de fecha, hora y especialidad no tienen etiqueta asociada, lo que afecta a los
  lectores de pantalla.
- Por debajo de 390 px de ancho, la tabla del reporte provoca desplazamiento horizontal.
