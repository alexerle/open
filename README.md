# ZHZ-Dashboard-API

Zentrale API für alle 10hoch2 JTL-Plugins. Gehostet auf GitHub Pages.

## Struktur

Die API ist **multi-plugin-fähig**. Jedes Plugin kann seine eigenen Portlets und Dokumentation haben.

```
data/
├── dashboard.json      # Alle Daten kombiniert (empfohlen)
├── portlets.json       # Portlet-Definitionen pro Plugin
├── documentation.json  # Dokumentation pro Plugin/Portlet
├── announcements.json  # Globale Ankündigungen
├── company.json        # Firmeninformationen
└── config.json         # API-Konfiguration
```

## JSON-Struktur

### portlets.json
```json
{
  "plugins": {
    "zhz_portlet_suite": {
      "id": "zhz_portlet_suite",
      "name": "ZHZ-Portlet-Suite",
      "latestVersion": "1.3.0",
      "portlets": [
        {
          "id": "zhz_blogfilter",
          "name": "ZHZ-BlogFilter",
          "status": "active",
          ...
        }
      ]
    },
    "zhz_another_plugin": {
      ...
    }
  }
}
```

### documentation.json
```json
{
  "plugins": {
    "zhz_portlet_suite": {
      "portlets": {
        "zhz_blogfilter": {
          "title": "ZHZ-BlogFilter Dokumentation",
          "sections": [...],
          "changelog": [...]
        }
      }
    }
  }
}
```

## Neues Plugin hinzufügen

1. In `portlets.json` unter `plugins` neuen Eintrag hinzufügen
2. In `documentation.json` unter `plugins` Dokumentation hinzufügen
3. In `dashboard.json` unter `plugins` hinzufügen
4. Committen und pushen

## Neues Portlet zu bestehendem Plugin hinzufügen

1. In `portlets.json` → `plugins` → `[plugin_id]` → `portlets` Array erweitern
2. In `documentation.json` → `plugins` → `[plugin_id]` → `portlets` erweitern
3. Auch in `dashboard.json` aktualisieren
4. Committen und pushen

## Beispiel: Neues Portlet hinzufügen

```json
// In portlets.json -> plugins -> zhz_portlet_suite -> portlets Array:
{
  "id": "zhz_productslider",
  "name": "ZHZ-ProductSlider",
  "description": "Eleganter Produkt-Slider...",
  "features": ["Touch Support", "Autoplay"],
  "icon": "images",
  "color": "#f093fb",
  "status": "active",
  "version": "1.0.0"
}
```

## Verwendung im Plugin

```php
use Plugin\zhz_portlet_suite\src\Api\DashboardApiClient;

// Alle Portlets für dieses Plugin laden
$portlets = DashboardApiClient::getPortlets('zhz_portlet_suite');

// Plugin-Info laden
$pluginInfo = DashboardApiClient::getPluginInfo('zhz_portlet_suite');

// Dokumentation für ein Portlet laden
$docs = DashboardApiClient::getDocumentation('zhz_portlet_suite', 'zhz_blogfilter');
```

## Icons

Verfügbare Icons (SVG):
- `newspaper` - Blog/News
- `images` - Bilder/Slider
- `comments` - Kommentare/Testimonials
- `circle-question` - FAQ/Hilfe
- `rocket` - Performance
- `shield` - Sicherheit
- `code` - Code/Entwicklung
- `headset` - Support

## Deployment

1. Repository: `10hoch2/zhz-dashboard-api`
2. GitHub Pages aktivieren (Settings → Pages → main branch)
3. URL: `https://10hoch2.github.io/zhz-dashboard-api/`

## Lokale Entwicklung

```bash
cd zhz-dashboard-api
python -m http.server 8000
# http://localhost:8000/data/dashboard.json
```
