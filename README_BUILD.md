# ISIPANEL — App Android (proyecto listo para compilar)

Esta carpeta es un proyecto Android **completo** que empaqueta el panel `ISIPANEL.html`
(está en `app/src/main/assets/ISIPANEL.html`) dentro de una app nativa con WebView.
El motor WebView del teléfono es Chrome moderno, así que el panel funciona igual que en el navegador.

- Nombre de la app: **ISIPANEL** · Paquete: `com.isigenere.isipanel`
- Icono corporativo incluido (todas las densidades)
- Permiso de **cámara** activado (para el módulo Scan-OCR)
- Los enlaces `https` (SharePoint, Outlook) y `mailto:` se abren en la app del sistema

Para actualizar el panel en el futuro: reemplaza `app/src/main/assets/ISIPANEL.html`
por la última versión y vuelve a compilar.

---

## Opción A — Obtener el APK SIN instalar nada (GitHub Actions) ✅ recomendada

1. Crea un repositorio en GitHub y sube **todo el contenido de esta carpeta**.
2. GitHub compila el APK solo (flujo `.github/workflows/build-apk.yml`).
   - Se lanza al hacer *push*, o manualmente en la pestaña **Actions → Build ISIPANEL APK → Run workflow**.
3. Cuando termine (≈3–5 min), entra en la ejecución y descarga el artefacto
   **ISIPANEL-debug-apk** → dentro está `app-debug.apk`.
4. Pásalo al teléfono e instálalo (activa «Instalar apps de orígenes desconocidos»).

## Opción B — Compilar en tu PC con Android Studio

1. Instala **Android Studio** (incluye el Android SDK).
2. **File → Open** y elige esta carpeta. Deja que sincronice Gradle.
3. **Build → Build App Bundle(s) / APK(s) → Build APK(s)**.
4. El APK queda en `app/build/outputs/apk/debug/app-debug.apk`.

## Opción C — Compilar por línea de comandos

Requisitos: **JDK 17** y **Android SDK** (variable `ANDROID_SDK_ROOT` configurada).

```bash
gradle assembleDebug          # o:  ./gradlew assembleDebug  (si generas el wrapper)
# APK -> app/build/outputs/apk/debug/app-debug.apk
```

> Si `./gradlew` no existe todavía, ejecútalo una vez con Gradle instalado:
> `gradle wrapper --gradle-version 8.7`

---

## APK firmado para distribución (release)

El `app-debug.apk` sirve para pruebas. Para una versión firmada:

```bash
keytool -genkey -v -keystore isipanel.keystore -alias isipanel \
  -keyalg RSA -keysize 2048 -validity 10000
gradle assembleRelease
# firma con apksigner usando isipanel.keystore
```

O usa **PWABuilder** (https://www.pwabuilder.com): como el panel ya es una PWA,
si lo alojáis en una URL https de SharePoint, PWABuilder genera un APK/AAB firmado
listo para Play Store sin tocar código.

---

Configuración: `compileSdk 34`, `minSdk 24` (Android 7.0+), `targetSdk 34`, AGP 8.5.2, Gradle 8.7.
