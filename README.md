# RomSync

App Android para sincronizar juegos, partidas y archivos entre una tablet o teléfono y el PC, por SFTP y
**sin instalar nada en el PC** (usa el servidor OpenSSH que trae Windows). La hice con Claude, como mis otras apps.

## Qué hace

- **Biblioteca**: los juegos del PC y de la tablet juntos, con carátulas de SteamGridDB. Cada juego dice cuánto
  ocupa y cuánto espacio queda; descargar y borrar piden confirmación, y borrar nunca toca las partidas.
- Reconoce juegos de varios archivos (varios discos, actualizaciones y contenido adicional) y deja elegir qué
  partes bajar.
- **Descargas** en segundo plano, con pausa y reanudación si se corta.
- **Partidas**: respaldo en el PC (manual o automático cada N horas con WiFi), historial y restauración.
  Las que están en Android/data necesitan **Shizuku**. Ver [docs/INVESTIGACION.md](docs/INVESTIGACION.md).
- **Web**: la tablet sirve una página con QR y PIN para administrar juegos, partidas y archivos desde el
  navegador del PC.

## Preparar el PC (una vez)

1. Configuración → Sistema → Características opcionales → Agregar → **Servidor OpenSSH**.
2. PowerShell como administrador:
   ```powershell
   Start-Service sshd
   Set-Service -Name sshd -StartupType Automatic
   ```
3. En la app: Ajustes → PC: la IP del PC (`ipconfig`), tu usuario y contraseña de Windows y la carpeta de juegos
   (por ejemplo `D:\Juegos`, con una carpeta por plataforma).

## Compilar

GitHub Actions compila la APK en cada push (pestaña Actions → artefacto `RomSync-apk`). En el PC, igual que TBF
Companion: Java 17 y `.\gradlew.bat assembleRelease`.

Pruebas (en la JVM): `.\gradlew.bat testReleaseUnitTest` (agrupación de juegos, tar, globs y SFTP contra un
servidor SSH de prueba).
