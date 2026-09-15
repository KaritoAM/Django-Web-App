## Estructura del proyecto

```
taskmanager/
├── manage.py
├── requirements.txt
├── taskmanager/          # Configuración del proyecto (settings, urls, wsgi)
│   ├── settings.py
│   ├── urls.py
│   └── ...
├── accounts/             # Registro, login y logout de usuarios
│   ├── forms.py          # RegistroForm (extiende UserCreationForm)
│   ├── views.py          # RegistroView (CreateView)
│   ├── urls.py
│   └── tests.py
├── projects/             # Gestión de proyectos y tareas
│   ├── models.py         # Proyecto y Tarea
│   ├── forms.py          # ProyectoForm y TareaForm (ModelForm + validaciones)
│   ├── views.py           # Vistas CRUD con LoginRequiredMixin
│   ├── admin.py           # Personalización del admin
│   ├── urls.py
│   └── tests.py
├── templates/            # Herencia de plantillas (base.html + hijos)
│   ├── base.html
│   ├── registration/     # login.html, register.html
│   └── projects/         # listado, detalle, formularios, confirmaciones
└── static/css/styles.css
```

## Requisitos

- Python 3.11 o superior
- pip

## Instalación

1. **Clonar o descomprimir el proyecto** y ubicarse en la carpeta raíz
   (donde está `manage.py`).

2. **Crear un entorno virtual**:

   ```bash
   python3 -m venv venv
   source venv/bin/activate      
   ```

3. **Instalar las dependencias**:

   ```bash
   pip install -r requirements.txt
   ```

4. **Aplicar las migraciones** para crear la base de datos (SQLite por
   defecto):

   ```bash
   python manage.py migrate
   ```

5. **Crear un superusuario** para acceder al panel de administración:

   ```bash
   python manage.py createsuperuser
   ```

6. **Levantar el servidor de desarrollo**:

   ```bash
   python manage.py runserver
   ```

7. Abrir el navegador en `http://127.0.0.1:8000/`.
   - Serás redirigido a `/proyectos/`, y si no has iniciado sesión, Django
     te llevará al login (`/accounts/login/`).
   - El panel de administración está en `http://127.0.0.1:8000/admin/`.

## Uso

1. Entra a `/accounts/registro/` para crear una cuenta nueva (te autentica automáticamente al registrarte).
2. Desde "Mis proyectos" crea un nuevo proyecto.
3. Dentro del detalle de un proyecto, agrega tareas indicando título, descripción, estado, prioridad, fecha límite y (opcionalmente) a quién
   se asigna.
4. Puedes editar o eliminar tanto proyectos como tareas desde la misma interfaz. Cada usuario solo puede ver y modificar sus propios proyectos.
5. Como superusuario, en `/admin/` puedes gestionar todos los proyectos, tareas y usuarios del sistema, con filtros y edición rápida de estado
   y prioridad.

## Ejecutar las pruebas

El proyecto incluye pruebas unitarias para los modelos, formularios y vistas principales (registro, login/logout, CRUD de proyectos y tareas,
y restricciones de acceso):

```bash
python manage.py test
```


## Funcionalidades implementadas

**Gestión de usuarios**
- Registro y autenticación con `django.contrib.auth`.
- Redirecciones post-login/logout configuradas en `settings.py`(`LOGIN_REDIRECT_URL`, `LOGOUT_REDIRECT_URL`).
- Restricción de acceso con `LoginRequiredMixin` en todas las vistas de proyectos y tareas.

**Gestión de tareas y proyectos**
- Modelos `Proyecto` y `Tarea` relacionados entre sí y con el usuario.
- Un usuario puede gestionar múltiples proyectos y cada proyecto múltiples tareas; las tareas pueden asignarse a un usuario.

**Interfaz de usuario con templates**
- Herencia de plantillas (`base.html`) para una estructura modular.
- Formularios con `forms.ModelForm` y validaciones personalizadas (nombre mínimo de proyecto, título mínimo de tarea, fecha límite no puede ser
  pasada).
- Información dinámica mostrada vía contexto (progreso de tareas completadas, badges de estado/prioridad, mensajes flash).

**Sitio administrativo**
- `Proyecto` y `Tarea` registrados en `admin.py` con búsqueda, filtros, edición en línea de tareas dentro de un proyecto y edición rápida de
  estado/prioridad.

**Seguridad y validaciones**
- Cada vista filtra el queryset por el usuario autenticado y usa un mixin propio (`EsPropietarioMixin`) para impedir que un usuario acceda a
  proyectos o tareas de otro.
- Validación de datos en los formularios antes de guardarlos en la base de datos.

**Pruebas y documentación**
- Pruebas unitarias para modelos y vistas (`accounts/tests.py`, `projects/tests.py`).
- Este `README.md` con instrucciones de instalación y uso.


