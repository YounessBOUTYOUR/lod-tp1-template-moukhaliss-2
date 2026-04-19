Carte des entités et des liens potentiels

**1. Inventaire des entités**
| Entité ou type d'entité | Exemple local | Pourquoi s'agit-il d'une entité ? | Attributs associés | Identifiant local potentiel |
|-------------------------|---------------|-----------------------------------|-------------------|----------------------------|
| Établissement | ENSIAS | Unité principale du dataset, objet réel avec existence autonome | nom, ville, site, type, domaine, année | `https://data.gov.ma/id/etablissement/ensias-rabat` |
| Université | Université Mohammed V | Entité de niveau supérieur, regroupe plusieurs établissements | nom, ville, site, année de création | `https://data.gov.ma/id/universite/um5` |
| Ville | Rabat | Entité géographique, permet la localisation et le filtrage spatial | nom, pays, région (implicite) | `https://data.gov.ma/id/ville/rabat` |
| Domaine disciplinaire | computer science | Concept thématique, permet de catégoriser les établissements | libellé, code (à définir) | `https://data.gov.ma/id/domaine/computer-science` |
| Type d'établissement | engineering school | Nomenclature de classification, valeur contrôlée à part entière | libellé, définition | `https://data.gov.ma/id/type/engineering-school` |
| Pays | Morocco | Entité géographique de niveau national | nom, code ISO | `https://data.gov.ma/id/pays/ma` |

**2. Relations conceptuelles observées**
| Source | Relation conceptuelle | Cible | Cardinalité | Commentaire |
|--------|----------------------|-------|-------------|-------------|
| Établissement | est affilié à | Université | N → 1 | Un établissement appartient à une seule université |
| Établissement | est situé dans | Ville | N → 1 | Localisation géographique principale |
| Établissement | appartient au type | Type d'établissement | N → 1 | Classification fonctionnelle |
| Établissement | traite du domaine | Domaine disciplinaire | N → 1 | Domaine principal d'enseignement/recherche |
| Université | est située dans | Ville | N → 1 | Siège administratif de l'université |
| Ville | est située dans | Pays | N → 1 | Contexte géographique national |

**3. Liens externes proposés**
| Entité locale | Ressource externe candidate | Type de lien envisagé | Critères d'appariement | Justification | Bénéfice attendu | Niveau de confiance | Risque |
|---------------|----------------------------|----------------------|------------------------|---------------|------------------|---------------------|--------|
| Université Mohammed V | Wikidata:Q1360237 | sameAs | nom exact + ville + site um5.ac.ma | Entrée Wikidata bien documentée | Accès aux données multilingues, liens DBpedia | ✅ Élevée | Faible : nom unique au Maroc |
| Rabat (ville) | GeoNames:2538475 | locatedIn / sameAs | nom + pays "Morocco" | Identifiant GeoNames stable pour Rabat | Coordonnées GPS, hiérarchie administrative | ✅ Élevée | Faible : homonymie internationale gérée par le pays |
| ENSIAS | Wikidata:Q28451632 | sameAs | nom complet + site ensias.um5.ac.ma | Page Wikidata existante avec propriétés structurées | Interconnexion avec le web de données académiques | ✅ Élevée | Moyen : vérifier que l'entrée correspond bien à l'établissement marocain |
| Université Cadi Ayyad | ROR:01kxg5k70 | sameAs | nom + site uca.ma | ROR spécialisé dans les organisations de recherche | Identifiant persistant dédié à la recherche internationale | ✅ Élevée | Faible : ROR est une source fiable |
| Casablanca (ville) | GeoNames:2553604 | locatedIn / sameAs | nom + pays | Identifiant GeoNames officiel | Géolocalisation précise, liens vers d'autres datasets géo | ✅ Élevée | Faible |
| Domaine "computer science" | Wikidata:Q11660 | broaderMatch / closeMatch | libellé anglais | Concept bien défini dans Wikidata | Alignement sémantique avec d'autres datasets éducatifs | ⚠️ Moyenne | Moyen : le domaine dans le dataset peut être plus large ou plus étroit que le concept Wikidata |

**4. Schéma conceptuel**
```mermaid
graph LR
  Dataset["Jeu de données : Établissements Maroc"]
  
  subgraph Entités_locales
    E[Établissement]
    U[Université]
    V[Ville]
    D[Domaine]
    T[Type]
    C[Pays]
  end
  
  subgraph Référentiels_externes
    WD[Wikidata]
    GN[GeoNames]
    ROR[ROR]
  end
  
  Dataset --> E
  E -->|affilié à| U
  E -->|situé dans| V
  E -->|type| T
  E -->|domaine| D
  U -->|siège| V
  V -->|dans| C
  
  U -.->|sameAs| WD
  U -.->|sameAs| ROR
  V -.->|sameAs| GN
  E -.->|sameAs| WD
  D -.->|closeMatch| WD