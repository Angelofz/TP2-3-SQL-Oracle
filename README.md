# TP2 SQL Oracle
## Exercice 6

1. Ecrivez pour le département des ressources humaines une interrogation produisant l'adresse de tous les départements. Utilisez les tables `LOCATIONS` et `COUNTRIES`. Affichez dans les résultats l'ID de lieu, la rue, la ville, le département et le pays. Utilisez une jointure naturelle (NATURAL JOIN) pour produire les résultats.

```sql
SELECT 
    location_id,
    street_address,
    city,
    state_province,
    country_name
FROM locations
NATURAL JOIN countries;
```
![Cover]()
