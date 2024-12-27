# Bot de Commits Diarios usando GitHub Actions

Este proyecto utiliza **GitHub Actions** para automatizar la tarea de realizar un commit diario en este repositorio. El bot genera un archivo llamado `archivo_diario.txt` donde agrega la fecha y hora de cada actualización automáticamente.

## Características
- Realiza commits diarios sin intervención manual.
- Funciona directamente desde GitHub Actions, por lo que no depende de un servidor o computadora personal.
- El commit incluye la fecha y hora exacta del cambio.
- Configurado para ejecutarse diariamente a las **12:00 PM (hora local de Colombia)**, equivalente a **12:00 UTC**.

---

## Configuración del workflow
El workflow está definido en el archivo `.github/workflows/commit-daily.yml` con el siguiente contenido:

```yaml
name: Commit Diario

on:
  schedule:
    - cron: '00 12 * * *' # Ejecutar a las 12:00 UTC cada día

jobs:
  commit-job:
    runs-on: ubuntu-latest

    steps:
      - name: Clonar el repositorio
        uses: actions/checkout@v3

      - name: Modificar o añadir archivo
        run: |
          echo "Actualización del día: $(date)" >> archivo_diario.txt

      - name: Configurar Git
        run: |
          git config --global user.name "Daily Commit Bot"
          git config --global user.email "tu-id+username@users.noreply.github.com"

      - name: Hacer commit y push
        run: |
          git add .
          git commit -m "Commit automático: $(date)"
          git push
```

---

## Cómo funciona
1. **Clonar el repositorio**: GitHub Actions clona el repositorio automáticamente.
2. **Modificar o crear archivo**: El archivo `archivo_diario.txt` se actualiza con la fecha actual.
3. **Configurar Git**: Se establece el nombre y correo para los commits realizados por el bot.
4. **Realizar commit y push**: Los cambios se comiten y suben automáticamente al repositorio remoto.

---

## Configuración del cron
El cron está configurado para ejecutarse diariamente a las **21:15 UTC**. Si deseas cambiar la hora:
- Usa el formato CRON: `minuto hora * * *`.
- Asegúrate de convertir tu hora local a UTC. Por ejemplo, las 12:00 PM en Colombia (UTC-5) corresponden a las 12:00 UTC.

Ejemplo:
```yaml
cron: '00 12 * * *' # Para ejecutar a las 12:00 PM UTC
```

---

## Personalización
- **Nombre del bot**: Cambiado a `Daily Commit Bot` para identificarlo en los logs.
- **Correo del bot**: Configurado como `tu-id+username@users.noreply.github.com` para mantener privacidad y autenticidad.

---

## Cómo probar el bot manualmente
1. Ve a la pestaña **Actions** en el repositorio.
2. Selecciona el workflow llamado **Commit Diario**.
3. Haz clic en **Run workflow** para ejecutarlo manualmente y verificar su funcionamiento.

---

## Beneficios
- Automatización total: No se necesita intervención manual.
- Independencia: Funciona desde GitHub sin depender de infraestructura externa.
- Simplicidad: Configuración mínima y mantenimiento prácticamente nulo.

---

