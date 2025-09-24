# 📱 PDF Route Tracker App

Aplicación móvil multiplataforma que combina:
- **Lector de PDF** para procesar documentos con direcciones.
- **Geolocalización en tiempo real** para ubicar al usuario.
- **Mapas interactivos** para mostrar destinos.
- **Gestión de checkpoints**: marcar destinos como completados y avanzar al siguiente más cercano.

---

## 🚀 Tecnologías recomendadas

### Frontend móvil
- [React Native](https://reactnative.dev/) (JavaScript/TypeScript)  
  Alternativa: [Flutter](https://flutter.dev/) (Dart).

### Backend (opcional)
- [Node.js](https://nodejs.org/) con [Express](https://expressjs.com/) o [NestJS](https://nestjs.com/).  
- Alternativa sin backend propio: [Firebase](https://firebase.google.com/) (plan gratuito).

---

## 📄 Lector de PDF
- **React Native**: [`react-native-pdf`](https://github.com/wonday/react-native-pdf)  
- **Flutter**: [`flutter_pdfview`](https://pub.dev/packages/flutter_pdfview)  
- Para grandes volúmenes de PDFs: almacenar metadatos (direcciones extraídas) en **SQLite** local.

---

## 📍 Geolocalización y Mapas
- **Geolocalización del usuario**:
  - React Native: [`react-native-geolocation-service`](https://github.com/Agontuk/react-native-geolocation-service)  
  - Flutter: [`geolocator`](https://pub.dev/packages/geolocator)

- **Mapas gratuitos**:
  - [Leaflet.js](https://leafletjs.com/) + [OpenStreetMap](https://www.openstreetmap.org/)  
  - Alternativa: [Mapbox](https://www.mapbox.com/) (plan gratuito con límites).

- **Cálculo de rutas y distancias**:
  - [OSRM](http://project-osrm.org/) (Open Source Routing Machine).  
  - [GraphHopper](https://www.graphhopper.com/).  

- **Geocodificación (dirección ↔ coordenadas)**:
  - [Nominatim](https://nominatim.org/) (basado en OSM, gratuito).

---

## 🗂️ Flujo de la App
1. **Carga de PDFs** → extracción de direcciones (regex o NLP ligero).  
2. **Geocodificación** → convertir direcciones a coordenadas con Nominatim.  
3. **Mapa** → mostrar ubicación actual + destinos.  
4. **Ordenar destinos** → calcular distancia y priorizar el más cercano.  
5. **Checkpoints** → marcar destino como completado y avanzar al siguiente.  
6. **Persistencia** → guardar progreso en SQLite local o Firestore (si se requiere nube).

---

## 🔑 APIs y librerías gratuitas
- **Mapas**: OpenStreetMap + Leaflet.  
- **Geocodificación**: Nominatim.  
- **Rutas**: OSRM / GraphHopper.  
- **Persistencia local**: SQLite.  
- **Opcional**: Firebase (auth + sync).

---

## 🏗️ MVP inicial
1. App en **React Native**.  
2. Lector de PDF con `react-native-pdf`.  
3. Guardar direcciones en SQLite.  
4. Geocodificación con Nominatim.  
5. Mapa con Leaflet/Mapbox.  
6. Botón **“Completado”** para avanzar al siguiente destino.

---

## 📌 Roadmap
- [ ] Integrar extracción automática de direcciones desde PDFs.  
- [ ] Implementar orden dinámico de destinos según ubicación actual.  
- [ ] Añadir sincronización en la nube (Firebase/Backend propio).  
- [ ] Optimizar rutas con algoritmos de grafos (Dijkstra/A*).  
- [ ] Mejorar UI/UX con animaciones y transiciones suaves.  

---

## ⚖️ Licencia
Este proyecto se apoya en tecnologías **open source** (OSM, Leaflet, OSRM, etc.).  
Revisar términos de uso de cada API antes de producción.

---

## 🏗️ Arquitectura de la aplicación

```text
+-------------------+         +-------------------+
|                   |         |                   |
|   PDF Reader      |         |   Geolocation     |
| (react-native-pdf)|         | (GPS del móvil)   |
|                   |         |                   |
+---------+---------+         +---------+---------+
          |                             |
          v                             v
+------------------------------------------------+
|                Data Layer (SQLite)             |
| - Guarda direcciones extraídas de PDFs         |
| - Estado de destinos (pendiente/completado)    |
+------------------------------------------------+
                          |
                          v
+------------------------------------------------+
|              Geocoding & Routing               |
| - Nominatim (dirección -> coordenadas)         |
| - OSRM/GraphHopper (distancias y rutas)        |
+------------------------------------------------+
                          |
                          v
+------------------------------------------------+
|                  Map View (UI)                 |
| - Leaflet / Mapbox                             |
| - Muestra ubicación actual                     |
| - Renderiza destinos ordenados por cercanía    |
| - Botón "Completado" -> actualiza SQLite       |
+------------------------------------------------+
                          |
                          v
+------------------------------------------------+
|              Backend opcional (Node/Firebase)  |
| - Sincronización en la nube (multi-dispositivo)|
| - Autenticación de usuarios                    |
+------------------------------------------------+
