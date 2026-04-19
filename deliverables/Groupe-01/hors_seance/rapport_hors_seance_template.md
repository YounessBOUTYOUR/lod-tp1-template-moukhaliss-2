# Rapport hors séance - TP1

**Module** : Linked Open Data  
**Groupe** : [01]  
**Étudiants** : [MOUKHALISS Hiba]  
**Date** : [19/04/2026]

## 1. Introduction
Ce rapport présente une étude préparatoire à la publication en Linked Open Data du jeu de données "Établissements de l'enseignement supérieur universitaire public au Maroc (2024-2025)", publié par le MESRSI sur data.gov.ma. L'objectif est d'évaluer les conditions techniques nécessaires pour transformer ce fichier tabulaire en ressources interconnectables. Nous nous concentrons sur trois types d'entités : université, ville et établissement.

## 2. Types d'entités étudiés
- **Université** : point d'ancrage hiérarchique (12 entités), facile à apparier avec Wikidata/ROR
- **Ville** : nécessaire pour la géolocalisation et les filtres régionaux (≈15 villes distinctes)
- **Établissement** : unité principale du dataset (165 entités), nécessite une stratégie d'identifiants robuste

Champs utiles pour l'appariement : `institution_name`, `city`, `website`, `institution_type`.

## 3. Benchmark des référentiels externes
[Reprendre le tableau du benchmark_referentiels.md en le synthétisant]

→ **Conclusion** : Wikidata est le référentiel le plus équilibré pour les universités et établissements connus ; GeoNames est incontournable pour les villes.

## 4. Cas d'appariement manuel significatifs
[Sélectionner 3-4 cas les plus parlants du benchmark, avec justification]

Exemple : "Université Mohammed V de Rabat" → Wikidata Q1360237  
Indices : nom exact, ville Rabat, site officiel um5.ac.ma, présence dans DBpedia  
Confiance : élevée → appariement validé

## 5. Plan de normalisation et d'identification
[Reprendre les éléments clés du plan_normalisation.md]

Règles prioritaires :
1. Normaliser les noms d'établissements (accents, casse, abréviations)
2. Mapper les villes vers GeoNames via une table de correspondance
3. Générer des URI locales selon un schéma documenté

## 6. Analyse des risques
- **Faux appariements** : risque modéré pour les établissements peu connus → toujours valider via le site officiel
- **Désambiguïsation** : les "Facultés des Sciences" nécessitent la ville + université de rattachement
- **Limites des référentiels** : ROR et Wikidata sous-représentent les petites écoles marocaines → prévoir des URI locales transitoires
- **Données manquantes** : absence de coordonnées GPS dans le dataset source → complique la validation géographique

## 7. Difficultés rencontrées
- Absence d'identifiant stable dans le fichier source → nécessité de créer une stratégie locale
- Variations orthographiques dans les noms de villes → table de correspondance manuelle nécessaire
- Couverture inégale des référentiels externes → certains établissements ne pourront être liés immédiatement

## 8. Conclusion
Cette étude préparatoire montre que le jeu de données marocain constitue une **bonne base** pour une future publication en LOD, à condition de :
1. Normaliser les champs textuels critiques (noms, villes)
2. Adopter une stratégie d'identifiants hybride (externes quand possible, locaux sinon)
3. Documenter clairement les règles de génération d'URI

Ce travail facilite directement le TP2 : les entités identifiées et normalisées pourront être modélisées en RDF, et les liens externes testés manuellement pourront être automatisés via des requêtes SPARQL.

