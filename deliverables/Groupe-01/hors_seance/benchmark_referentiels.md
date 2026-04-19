# Benchmark des référentiels externes

**Groupe** : [01]  
**Jeu de données étudié** : Établissements de l'enseignement supérieur universitaire public au Maroc (2024-2025)  
**Types d'entités retenus** : 
1. Université Mohammed V
2. Rabat
3. ENSIAS

## 1. Référentiels comparés

| Référentiel | URL | Types couverts | Intérêt potentiel | Limites observées |
|-------------|-----|----------------|-------------------|-------------------|
| **Wikidata** | https://www.wikidata.org | Universités, villes, établissements | Identifiants stables (Qxxxx), multilingue, liens vers DBpedia/GeoNames | Couverture inégale pour les petites écoles marocaines |
| **ROR** | https://ror.org | Organisations de recherche/universités | Identifiants persistants dédiés à la recherche, API simple | Peu d'établissements marocains référencés actuellement |
| **GeoNames** | https://www.geonames.org | Villes, régions, pays | Hiérarchie administrative claire, coordonnées GPS | Ne couvre pas les entités "établissement" |
| **data.gov.ma** | https://data.gov.ma | Villes, régions marocaines | Référentiel national cohérent, contexte local | Pas d'identifiants stables documentés pour toutes les entités |

## 2. Critères de comparaison
- **Couverture** : % d'entités marocaines présentes dans le référentiel
- **Qualité des identifiants** : stabilité, persistance, format URI
- **Facilité d'appariement** : possibilité de matcher via nom + ville + site web
- **Richesse sémantique** : propriétés disponibles (coordonnées, site officiel, type)

## 3. Cas d'appariement manuel (6 exemples)

| Entité locale | Type | Référentiel cible | Correspondance candidate | Indices utilisés | Confiance | Décision |
|---------------|------|-------------------|--------------------------|-----------------|-----------|----------|
| Université Mohammed V de Rabat | Université | Wikidata | Q1360237 | Nom exact + ville + site um5.ac.ma | Élevée | Appairer |
| Université Cadi Ayyad | Université | ROR | 01kxg5k70 | Nom + site uca.ma | Élevée | Appairer |
| Rabat | Ville | GeoNames | 2538475 | Nom exact + pays Maroc | Élevée | Appairer |
| Casablanca | Ville | data.gov.ma | | Nom + région Casablanca-Settat | Moyenne | À valider |
| ENSIAS | Établissement | Wikidata | Q28451632 | Nom complet + site ensias.um5.ac.ma | Élevée | Appairer |
| Ecole Mohammadia d'Ingénieurs | Établissement | Wikidata | Q3579154 | Nom + site emi.ac.ma + ville Rabat | Élevée | Appairer |

## 4. Synthèse
- **Universités** : Wikidata est le plus utile (couverture correcte, identifiants stables)
- **Villes** : GeoNames est préférable pour la géolocalisation ; data.gov.ma en complément pour le contexte national
- **Établissements** : Wikidata pour les plus connus ; pour les autres, créer des URI locales en attendant un référencement ROR
- **Cas ambigus** : établissements avec noms similaires (ex: "Faculté des Sciences" dans plusieurs villes) → nécessitent la ville + université de rattachement pour désambiguïser