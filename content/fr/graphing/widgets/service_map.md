---
title: Widget Service Map
kind: documentation
description: 'Affiche une carte de service avec tous les services qui l''appellent, et tous les services appelés.'
further_reading:
  - link: graphing/graphing_json/
    tag: Documentation
    text: Créer des dashboards avec JSON
---
Ce widget affiche une carte de service avec tous les services qui l'appellent, et tous les services appelés. Le nœud au centre du widget représente le service mappé. Les services qui appellent le service mappé s'affichent à gauche (avec des flèches entre le service à l'origine de l'appel et le service appelé). Les services appelés s'affichent à droite (avec des flèches illustrant la direction des appels).

Il n'est pas possible de configurer un intervalle pour le widget Service Map. Ce dernier affiche toujours les services à l'origine d'un appel lors des deux dernières semaines. Les métriques sont calculées en temps réel pour chaque service en temps réel à partir des données de l'heure précédente.

{{< img src="graphing/widgets/service_map/test_service_map.png" alt="configuration de Service Map" responsive="true" style="width:80%;">}}

## Implémentation

{{< img src="graphing/widgets/service_map/service_map.png" alt="Configuration Service Map" responsive="true" style="width:80%;">}}

### Configuration

1. Choisissez votre contexte d'[environnement][1], votre [tag principal][2] (ou `*`) s'il est défini pour votre compte, ainsi que votre nom du [service][3].
2. Saisissez un titre pour votre graphique.

### Résultats

Les nœuds sont dimensionnés l'un par rapport à l'autre en fonction du taux de requête. La circonférence d'un nœud est colorée en fonction du statut du monitor : vert pour OK, jaune pour un avertissement, rouge pour une alerte et gris en absence de données.

Les services connectés au service mappé sont triés vers l'extérieur à partir du milieu selon le taux de requête. Les cinq services avec le plus grand nombre de requêtes sont étiquetés par défaut. De plus, un service avec un monitor en état d'alerte est toujours étiqueté.

## API

Le [schéma JSON][4] utilisé pour le widget Service Map est le suivant :

```
SERVICEMAP_SCHEMA = {
        "type": "object",
        "properties": {
            "type": {"enum": [WIDGET_TYPE]},
            "filters": {"type": "array", "items": {"type": "string"}, "minItems": 1},
            "service": {"type": "string"},
            "title": WidgetSchema.TITLE
        },
        "required": ["type", "filters", "service"],
        "additionalProperties": False,
    }
```

| Paramètre | Type   | Obligatoire | Description                                                       |
|-----------|--------|----------|-------------------------------------------------------------------|
| type      | string | oui      | Type du widget (utilisez `servicemap` pour le widget Service Map). |
| service   | string | oui      | L'ID du service que vous souhaitez mapper.                            |
| filters   | objet | oui      | Votre environnement et votre tag principal (ou `*` si cette option est activée sur votre compte).    |
| title     | string | non       | Le titre de votre widget.                                         |


## Pour aller plus loin

{{< partial name="whats-next/whats-next.html" >}}

[1]: /fr/tracing/send_traces
[2]: /fr/tracing/advanced/setting_primary_tags_to_scope
[3]: /fr/tracing/visualization/service
[4]: /fr/graphing/graphing_json/widget_json