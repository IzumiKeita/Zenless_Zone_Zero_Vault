# Zenless Zone Zero Vault (Obsidian)

Este repositorio es un Vault de Obsidian para organizar lore, personajes, facciones y localizaciones de **Zenless Zone Zero (ZZZ)** mediante enlaces internos.

## Requisitos

- [Obsidian](https://obsidian.md/)

## Cómo usarlo en Obsidian

1. Obtén el Vault de una de estas formas:
   - **Descarga ZIP:** en GitHub, usa **Code → Download ZIP**.
   - **Clonado (recomendado si vas a actualizar):**
     - `git clone <URL-del-repo>`
2. Coloca la carpeta del Vault en una ruta local estable (evita carpetas temporales). La carpeta debe contener subcarpetas como `00_Agentes/`, `01_Facciones/` y `.obsidian/`.
3. Abre el Vault en Obsidian:
   - Escritorio: **Open folder as vault** → selecciona la carpeta raíz `Zenless_Zone_Zero_Vault`.
   - Móvil: **Open folder as vault** / **Abrir carpeta como bóveda** → elige la carpeta desde el almacenamiento del dispositivo.
4. Si Obsidian te pregunta por **Trust this vault?**:
   - Si confías en el origen, marca como confiable para que puedan funcionar configuraciones y plugins.
   - Si prefieres máxima seguridad, ábrelo como no confiable (algunas funciones de plugins pueden quedar desactivadas).
5. Si ves enlaces “rotos” al abrir por primera vez:
   - Espera a que Obsidian termine de indexar el Vault.
   - Revisa que no estés abriendo una subcarpeta interna por error (debe ser la carpeta raíz).
6. (Opcional) Plugins:
   - El Vault puede incluir configuración en `.obsidian/`. En **Settings → Community plugins**, habilita solo lo que quieras usar.

## Estructura del Vault (referencia rápida)

- `00_Agentes/`: fichas de personajes por facción
- `01_Facciones/`: organizaciones, corporaciones y grupos
- `02_Lore_Mundo/`: conceptos del mundo y elementos de lore
- `03_Localizaciones/`: lugares y zonas
- `04_Enemigos/`: enemigos y entidades
- `99_Attachments/`: imágenes y adjuntos
- `.obsidian/`: configuración del Vault (tema, plugins, workspace, etc.)

## Notas

- Los enlaces internos usan el formato `[[Nombre de nota]]`.
- Si notas cambios frecuentes en archivos de configuración de Obsidian, es normal: Obsidian actualiza varios JSON al cambiar vistas, paneles o el grafo.
