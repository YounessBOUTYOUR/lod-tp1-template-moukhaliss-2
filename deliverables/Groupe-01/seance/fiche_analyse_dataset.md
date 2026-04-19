Fiche d'analyse du jeu de données

**Identification**
- Groupe : [01]
- Date : [19/04]
- Nom du jeu de données : Établissements de l'enseignement supérieur universitaire public au Maroc (2024-2025)
- Source / producteur : MESRSI (Ministère de l'Enseignement Supérieur)
- URL de la source : https://data.gov.ma/data/dataset/etablissements-de-l-enseignement-superieur-universitaire-public-ouverts
- Domaine métier : Éducation / Enseignement supérieur

**Description générale**
- Objectif : Recenser les établissements publics d'enseignement supérieur ouverts pour l'année universitaire 2024-2025
- Format(s) disponible(s) : XLSX, CSV (via export)
- Langue(s) : Français, Arabe (noms d'établissements)
- Licence : ODbL (Open Database License)
- Fréquence de mise à jour : Annuelle
- Unité d'observation : Un établissement d'enseignement ou de recherche
- Granularité des enregistrements : 1 ligne = 1 établissement

**Structure du jeu de données**
| Champ / colonne | Signification | Exemple de valeur | Observation |
|-----------------|---------------|-------------------|-------------|
| record_id | Identifiant technique interne | 1, 2, 3... | Stable mais non persistant hors du portail |
| institution_name | Nom complet de l'établissement | "Ecole Nationale Supérieure d'Informatique et d'Analyse des Systèmes" | Noms longs, variations d'accents possibles |
| short_name | Sigle ou nom court | "ENSIAS", "EMI" | Utile mais ambigu (plusieurs écoles peuvent avoir le même sigle) |
| city | Ville d'implantation | "Rabat", "Casablanca" | Orthographe à normaliser (majuscules/minuscules) |
| country | Pays | "Morocco" | Valeur constante, peu utile pour le filtrage local |
| website | Site web officiel | "https://www.ensias.um5.ac.ma" | Excellent critère d'appariement si fiable |
| institution_type | Type d'établissement | "public school", "faculty", "university" | Nomenclature hétérogène à harmoniser |
| main_field | Domaine principal | "computer science", "engineering" | Valeurs en anglais, à mapper vers un thésaurus |
| year_created | Année de création | 1992, 1958... | Donnée historique utile, format numérique propre |
| portal_hint | Source du référencement | "data.gov.ma" | Redondant, peut être supprimé en LOD |

**Évaluation "Open Data"**
| Critère | Observation | Score / 5 |
|---------|-------------|-----------|
| Accessibilité | Téléchargement direct sans authentification | 5 |
| Réutilisabilité | Format structuré, champs explicites | 4 |
| Documentation | Métadonnées minimales sur le portail | 3 |
| Format exploitable | XLSX lisible, export CSV possible | 4 |
| Présence d'une licence | Licence ODbL clairement indiquée | 5 |

**Évaluation "Linked Data readiness"**
| Point observé | Oui / Non / Partiel | Commentaire |
|---------------|---------------------|-------------|
| Identifiants stables pour les enregistrements | Partiel | `record_id` stable mais non persistant hors du portail |
| Entités clairement distinguables | Oui | Chaque ligne = 1 établissement, pas de doublons apparents |
| Valeurs normalisées | Non | `institution_type`, `city` : variations de casse/orthographe |
| Informations géographiques exploitables | Partiel | Ville présente, mais pas de coordonnées GPS ni code postal |
| Possibilité de lien vers des référentiels externes | Oui | `website`, `institution_name`, `city` permettent l'appariement |
| Présence de clés candidates plausibles | Partiel | `website` est le meilleur candidat, mais pas garanti unique |
| Présence de codes ou nomenclatures réutilisables | Non | Pas de référence à des codes officiels (ex: code établissement national) |

**Maturité "5 étoiles"**
| Niveau | Atteint ? | Justification |
|--------|-----------|---------------|
| 1 étoile : données disponibles sur le Web | Oui | Publiées sur data.gov.ma |
| 2 étoiles : données structurées | Oui | Format tabulaire XLSX/CSV |
| 3 étoiles : format non propriétaire | Partiel | XLSX est propriétaire, mais export CSV disponible |
| 4 étoiles : usage d'URI pour identifier les choses | Non | Pas d'URI pour les établissements, seulement un `record_id` interne |
| 5 étoiles : liens vers d'autres données | Non | Aucun lien explicite vers Wikidata, GeoNames, etc. |

**Clés candidates et identifiants**
| Objet ou entité cible | Champ(s) candidat(s) | Fiabilité | Limites | Recommandation |
|-----------------------|----------------------|-----------|---------|----------------|
| Établissement | `website` | Élevée (si officiel) | Risque de site non officiel ou changé | Valider manuellement les cas critiques |
| Établissement | `institution_name` + `city` | Moyenne | Homonymies possibles ("Faculté des Sciences" dans plusieurs villes) | Combiner avec `university` de rattachement |
| Université | `institution_name` (si type=university) | Élevée | 12 universités seulement, faciles à apparier | Aligner vers Wikidata (Qxxxx) ou ROR |
| Ville | `city` | Moyenne | Orthographes variables, homonymies internationales | Mapper vers GeoNames via table de correspondance |

**Normalisation préalable nécessaire**
| Champ | Problème observé | Impact pour le LOD | Action recommandée |
|-------|-----------------|-------------------|-------------------|
| institution_name | Accents, casse, abréviations | Appariement difficile avec Wikidata | Normaliser : minuscules, suppression accents pour le matching |
| city | "Rabat" vs "RABAT" vs "rabat" | Liaison géographique incertaine | Convertir en minuscules + table de correspondance GeoNames |
| institution_type | Valeurs hétérogènes ("public school", "faculty") | Filtrage et agrégation incohérents | Mapper vers une nomenclature contrôlée (5-6 types max) |
| website | Avec/sans https, avec/sans www | Détection de doublons compliquée | Forcer https, supprimer www, valider l'unicité |

**Qualité des données**
- **Forces observées** : Structure claire, champs pertinents, licence ouverte, présence de sites web officiels
- **Faiblesses observées** : Absence d'identifiants persistants, nomenclatures non contrôlées, pas de géocoordonnées
- **Ambiguïtés ou doublons potentiels** : Plusieurs "Faculté des Sciences" dans différentes villes ; sigles courts potentiellement ambigus
- **Champs manquants pour une meilleure interconnexion** : Coordonnées GPS, code établissement officiel, identifiant ROR/Wikidata
- **Risque principal pour une future mise en relation** : Faux appariement d'établissements homonymes sans validation manuelle

**Conclusion courte**
Ce jeu de données constitue une **bonne base de départ** pour une future publication en Linked Open Data, car il est structuré, documenté et sous licence ouverte. Cependant, une phase de préparation est indispensable : (1) normaliser les champs textuels critiques (`institution_name`, `city`, `institution_type`), (2) valider l'unicité des `website` comme identifiants de secours, et (3) construire une stratégie d'URI locales en attendant l'alignement vers des référentiels externes comme Wikidata ou GeoNames. Ces actions permettront de transformer ce fichier tabulaire en ressources interconnectables, prêtes pour une modélisation RDF lors du TP2.