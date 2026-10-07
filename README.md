# Nodorax 0.3 — edición para pruebas

Aplicación en español para Android, iOS y web, con API Python, SQLite local o PostgreSQL, integración opcional con n8n y el logo suministrado por el propietario.

Incluye contactos, productos, cotizaciones y facturas de prueba en PDF, tickets, notas, reportes CSV/Excel, usuarios con varias organizaciones, roles, 2FA, biometría local, sesiones, notificaciones, planes, upgrades simulados, programación de tareas y 17 recetas propias.

**Empieza por [la guía de pruebas](docs/GUIA_PRUEBAS_0_3.md):** explica instalación, los 30 puntos del sistema, las 15 automatizaciones, límites, permisos y cómo preparar la migración a PostgreSQL.

## Arrancar en Windows

Requisitos: Python 3.12, Node.js 24 y npm. Desde esta carpeta:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r backend/requirements.txt
Copy-Item backend/.env.example backend/.env
python tools/start_api.py
```

En otra terminal:

```powershell
cd mobile
npm ci
Copy-Item .env.example .env
npm start
```

En Linux/macOS activa el entorno con `source .venv/bin/activate` y usa `cp` para copiar archivos. API local: `http://127.0.0.1:8000`; documentación técnica en `/docs` fuera de producción.

Configura `EXPO_PUBLIC_API_URL` en `mobile/.env`: `http://localhost:8000` para web, `http://10.0.2.2:8000` para el emulador Android o la IP LAN de tu equipo para un teléfono en desarrollo. Las versiones nativas de producción requieren HTTPS.

## Probar y construir

```powershell
python -m pip install -r backend/requirements-dev.txt
python -m pytest backend/tests -q
cd mobile
npm ci
npm run typecheck
npx expo export --platform all --output-dir testing-bundles
```

Para Android local: `npx expo prebuild --platform android` y `npx expo run:android`, con SDK y JDK compatibles. Para distribución interna: `npx eas-cli build --platform android --profile preview`, con tu cuenta Expo/EAS y firma. Para iOS necesitas cuenta Apple y certificados; la compilación local requiere macOS/Xcode. No se incluyen claves de firma.

Bundles exportados vigentes: `mobile/testing-bundles/`. El ZIP no contiene un APK, AAB o IPA firmado ni un servidor desplegado. La exportación y la prueba web no sustituyen las pruebas en teléfonos reales.

## Límites de esta entrega

- Los cobros son simulados y están deshabilitados en producción; no se cobra dinero.
- Los PDF no tienen validez fiscal ni NCF. Validar formatos RNC/RFC no verifica un registro oficial.
- El inglés está preparado en la navegación y los recursos, con traducción parcial.
- n8n conserva un motor activo dedicado por organización. Las conexiones adicionales permiten inventario y health checks, sin seleccionar múltiples motores por flujo.
- Las recetas n8n calculan resultados con datos suministrados; las tareas de base se ejecutan desde la API propia.
- Las auditorías móviles muestran avisos altos pendientes. CI mantiene el bloqueo de lanzamiento hasta resolverlos o completar la revisión técnica correspondiente.
- No están implementados pagos reales, login Google/Apple, Instagram DM, WhatsApp Cloud ni un chatbot de IA.

El objetivo de diciembre de 2026 / enero de 2027 requiere terminar las integraciones previstas y validar seguridad, dispositivos, PostgreSQL, backups y despliegue.

## Archivos

- `backend/app/`: API, modelos, módulos de negocio, recetas y generadores de documentos.
- `backend/assets/`: logo y fuentes incluidas con su licencia para PDF.
- `backend/tests/`: pruebas de permisos, datos, cálculos, cuotas, exportación y recetas Python/JavaScript.
- `mobile/`: interfaz React Native/Expo, módulo Negocio y configuración nativa.
- `tools/`: arranque, backups, transferencia a base vacía, bootstrap del administrador y QA web.
- `infra/`: laboratorio Docker y ejemplo TLS; Docker no se ejecutó en esta sesión.
- `docs/`: guía vigente, resultados y capturas; archivos anteriores son evidencia histórica.
- `catalog/`: metadata de 2.063 workflows en cuarentena; no son plantillas funcionales incluidas.

No se entregan `node_modules`, bases de usuarios, `.env`, claves privadas ni el monorepo de n8n. Crea tu cuenta y conserva tus datos y clave maestra por separado al migrar.
