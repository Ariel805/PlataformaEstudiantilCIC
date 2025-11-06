Instrucciones para subir `synapselogo.png` y usar la URL RAW de GitHub

1) Coloca el archivo `synapselogo.png` en la carpeta `imagenes/` del repo local (ya lo tienes en tu equipo en `C:\Users\Consuelo\Desktop\proyecto de gradoo beta 1\imagenes\synapselogo.png`).

2) Desde PowerShell, añade, comitea y sube la imagen al repo (reemplaza `main` por tu rama si es otra):

```powershell
# desde la carpeta del proyecto
git add imagenes/synapselogo.png
git commit -m "Add synapselogo.png"
# si tu rama es 'main'
git push origin main
```

Si no tienes Git configurado en tu máquina, instala Git para Windows (https://git-scm.com/download/win) y configura tu usuario:

```powershell
git config --global user.name "Tu Nombre"
git config --global user.email "tu@correo"
```

3) Comprueba la URL RAW de la imagen (sustituye los placeholders):

```
https://raw.githubusercontent.com/<TU-USUARIO>/<TU-REPO>/<RAMA>/imagenes/synapselogo.png
```

4) Usa esa URL en tu `index.html` (o cualquier HTML) así:

```html
<img src="https://raw.githubusercontent.com/TU-USUARIO/TU-REPO/main/imagenes/synapselogo.png" alt="Logo">
```

Notas y consejos
- GitHub distingue mayúsculas/minúsculas. Asegúrate que el nombre del archivo en el repo coincide exactamente (por ejemplo `synapselogo.png` vs `SynapseLogo.png`).
- Si usas GitHub Pages con un repositorio de usuario/organización, las rutas relativas funcionan si colocas el archivo en el mismo repo y mantienes la estructura (por ejemplo `<img src="imagenes/synapselogo.png">`).
- Para producción es mejor servir imágenes desde el mismo dominio (relative paths) o desde un CDN; raw.githubusercontent puede tener limitaciones de cache y ancho de banda para sites con mucho tráfico.
