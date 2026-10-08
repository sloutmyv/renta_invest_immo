# Renta Invest Immo

Simulateur d'investissement locatif **100 % autonome** (un seul fichier `index.html`, aucune dépendance) — devise **XPF**, contexte Nouvelle-Calédonie.

**Version 1.4** (08/10/2026) : ergonomie & cohérence —
- part locataire **mensualisée** dans le tableau de bord,
- **reste à charge mensuel calculé sur l'horizon de revente** (et non sur 30 ans),
- **rendement net avant dette ramené au prix total d'acquisition** (prix FAI + notaire + travaux + frais de prêt) — la version sans frais reste affichée dans l'info,
- section **Frais achat placée avant Financement** (ordre d'achat logique),
- **vacance locative et gestion locative déplacées** dans le bloc *Charges propriétaire*,
- **tooltips ⓘ** au survol de chaque colonne du comparatif fiscal des régimes (mode de calcul explicite).

**Version 1.3** (05/10/2026) : **imposition à la revente (PVI NC réelle)** — le taux fixe de plus-value devient un **sélecteur du régime PVI** : réel NC (20 % IR-PVI + 4 % CCS = 24 %, abattement 10 %/an au-delà de 10 ans de détention, **exonération totale à 20 ans**), taux fixe manuel, ou exonéré (résidence principale). La base prend en compte les frais d'acquisition réels (notaire + travaux saisis) ou, à défaut de justificatifs, un forfait de 15 % du prix de vente dès 2 ans de détention. Le TRI et le multiple intègrent l'impôt à la revente.

**Version 1.2** (05/10/2026) : **volet fiscalité NC** — le taux fixe 25 % est remplacé par un **sélecteur de régime** avec 4 modes (nom propre nue / meublé forfait 50 % / meublé réel amortissable / SCI transparente quote-part), champs CCS (4 %), tranche marginale IRNC, quote-part associé, durée d'amortissement. **Tableau comparatif des 4 régimes** (résultat imposable, impôt, CCS, impôt moyen/an, total sur 15 ans, cash-flow moyen/mois) ajouté sous le tableau annuel.

**Version 1.1** (05/10/2026) : ajout du champ **Travaux** (rénovation/aménagement) dans « Frais achat » — défaut **0 XPF**, s'ajoute à l'investissement initial et se répercute sur l'argent investi, TRI et multiple.

## Ouverture

Ouvrir `index.html` dans un navigateur — c'est tout. Les saisies sont sauvegardées automatiquement en localStorage.

## Fonctionnalités

| Bloc | Fonction |
|---|---|
| 1 | Saisie complète (bien, financement, frais achat, recettes, charges, fiscalité, revente) + défauts NC préremplis |
| 2 | Moteur de calcul (crédit annuité constante, exploitation annuelle, fiscalité simple) + tableau de bord KPI |
| 3 | Scénario de revente (3 modes : prix d'achat / montant saisi / % évolution), TRI (dichotomie), multiple d'argent investi |
| 4 | 3 visuels SVG maison : amortissement, locataire vs reste à charge, profil TRI/multiple selon la durée (1-30 ans) |
| 5 | Fiche PDF imprimable (A4) : indicateurs, scénario de revente, graphiques 1-2, tableau annuel condensé 30 ans |

Spécificités : horizon 30 ans (dépasse la fin du crédit), colonnes « argent investi cumulé », « cash si revente chaque année », « gain/perte total si revente », « multiple par année ».

## Tests

Console du navigateur avec `?debug.test` en suffixe d'URL : cas de test bloc 2 (T2 33 M) et bloc 3 (revente 15 ans, +1 %/an) s'affichent automatiquement.

Modules exposés en console : `RentaInvestBloc2` à `RentaInvestBloc5`.

## Docs

- `docs/PROPOSITION.md` / `CAHIER-DES-CHARGES.md` : spécification V1 validée
- `docs/CALCULS.md` : formules de référence (maille crédit / exploitation / revente / TRI)
- `docs/PLAN.md` : plan d'implémentation par blocs
- `docs/models-preferences.md` : choix IA (GLM - Flash via opencode)

## Licence

Usage privé — pas de licence explicite.
