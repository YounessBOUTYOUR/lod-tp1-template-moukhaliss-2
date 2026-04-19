# Plan de normalisation et d'identification

## 1. Entités prioritaires

| Type d'entité | Pourquoi prioritaire ? | Identifiant actuel | Stratégie proposée |
|---------------|------------------------|-------------------|-------------------|
| Université | Point d'ancrage pour 162 établissements | Nom texte uniquement | Alignement vers Wikidata (Qxxxx) ou ROR ; en attendant, URI locale : `https://data.gov.ma/id/universite/{nom_slug}` |
| Ville | Nécessaire pour géolocalisation et filtres | Nom texte (variations possibles) | Alignement vers GeoNames (ID numérique) ; normalisation des noms via table de correspondance |
| Établissement | Unité principale du dataset | record_id (interne) | Construction d'URI locale : `https://data.gov.ma/id/etablissement/{univ_code}-{type}-{nom_slug}` |

## 2. Champs à normaliser

| Champ | Problème observé | Règle de normalisation proposée | Impact attendu |
|-------|-----------------|--------------------------------|----------------|
| institution_name | Noms avec/sans accents, abréviations variables | 1. Supprimer les accents pour le matching 2. Extraire le type (École, Faculté, Université) 3. Standardiser la casse | Meilleur appariement avec Wikidata/ROR |
| city | Orthographes variables ("Rabat" vs "RABAT") | Conversion en minuscules + table de correspondance vers GeoNames | Liaison fiable avec référentiels géographiques |
| institution_type | Valeurs hétérogènes ("public school", "faculty", etc.) | Mapping vers une nomenclature contrôlée : {university, faculty, engineering_school, research_institute} | Filtrage et agrégation cohérents |
| website | URLs avec/sans https, avec/sans www | Normalisation : forcer https, supprimer www si présent | Détection de doublons via site officiel |

## 3. Stratégie d'identifiants

| Entité cible | Base de construction envisagée | Risque | Recommandation |
|--------------|-------------------------------|--------|----------------|
| Université | Slug du nom + pays : `um5-morocco` | Collision si deux universités ont le même nom dans d'autres pays | Ajouter un code pays ISO : `um5-ma` |
| Ville | ID GeoNames si appariement réussi ; sinon slug + pays | Ville homonyme dans plusieurs pays | Toujours ajouter le code pays : `rabat-ma` |
| Établissement | `{univ_slug}-{type_slug}-{nom_slug}` ex: `um5-engineering-ensias` | Longueur excessive, complexité | Documenter la règle de génération ; prévoir un registre central |

## 4. Risques et limites
- **Champs trop faibles pour servir d'identifiant** : `short_name` (trop court, ambigu), `city` seul (homonymies)
- **Collisions possibles** : plusieurs "Faculté des Sciences" dans différentes villes → toujours combiner avec `institution_name` complet + `city`
- **Informations manquantes** : coordonnées GPS, codes postaux, identifiants officiels nationaux → compliquent la désambiguïsation automatique
- **Hypothèse forte** : on suppose que le `website` est unique et officiel pour chaque établissement → à valider manuellement pour les cas critiques