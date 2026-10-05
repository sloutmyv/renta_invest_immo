# Renta Invest Immo — Proposition générale

Date : 2026-10-04 — Statut : PROPOSITION (en attente de validation)

## Objectif

Petit outil (site HTML local) pour estimer la rentabilité d'un achat
immobilier à Nouméa / Nouvelle-Calédonie, en XPF. Sortie : un PDF
par option d'achat (biens comparés) pour constituer un dossier.

## Concept et déroulé

1. **Proposition générale** (ce fichier) → ta validation.
2. **Planification** : cahier des charges détaillé, plan de découpage
   (fichiers, calculs, UI), maquette de structure → ta validation.
3. **Implémentation** : code par blocs, avec ta validation à chaque étape.

Orchestration : GLM-5.3 — Code : GLM-5.3 Flash ou DeepSeek v4.1 Flash.
(Proconisations enregistrées dans `config/models-preferences.md`,
adaptables ensuite.)

## Périmètre assumé (petit outil, zéro infra)

- Un seul fichier `index.html` (HTML + CSS + JS inline), utilisable hors
  ligne en ouvrant le fichier, ou servi via `python -m http.server`.
- Formules de finance classique ; pas d'API externe.
- Export PDF via l'impression navigateur (bouton + feuille de style
  print) — pas besoin de librairie lourde → fiable et sans dépendance.

## Partie 1 — Paramètres d'entrée (form unique,Groupée)

### Le bien
- Adresse / description libre (pour le dossier PDF).
- Surface habitable (m²), surface(s) annexes (terrasse, parking...).
- Prix d'achat FAI (XPF), prix de vente souhaité à la revente (XPF).
- Type : T1 / T2 / T3 / maison ; année de construction.
- Étages, parking, cave... (libre, juste descriptif).

### Financement
- Montant emprunté (XPF) et apport (XPF).
- Durée (années / mois), taux annuel nominal (%).
- Assurance emprunteur : % du capital initial OU montant mensuel fixe.
- Frais de dossier de banque, frais de garantie (caution/hypothèque — %
  ou forfait), frais de courtier (facultatif).
- Frais de notaire (option : calcul auto à ~4,7 % du prix en NC ---
  à valider --- ou saisie manuelle).

### Recettes (locataire)
- Loyer mensuel charges comprises versé par le locataire (XPF).
- Charges locatives (provision mensuelle versée par le locataire).

### Dépenses récurrentes du propriétaire
- Charges du propriétaire (copro, eau non récupérable...) : mensuel XPF.
- Taxe foncière annuelle (XPF).
- Assurance propriétaire non occupant (PNO) mensuelle ou annuelle.
- Entretien / travaux provision : % du loyer ou forfait mensuel.
- Frais de gestion locative : % du loyer (ou 0 si gestion directe).
- Vacance locative : % du temps sans locataire (ex : 5 %).

### Fiscalité (NC)
- Impôt sur le revenu locatif (régime NC imposable des revenus au barème
  ou abattement selon ton régime réel) → paramètre simple :
  taux marginal applicable au résultat (ex : 30 %), paramétrable.
- Plus-value à la revente : régime NC du cas par cas (exonération
  résidence principale probablement pas applicable en locatif) →
  paramètre : % d'imposition sur plus-value, paramétrable.

### Scénario de revente
- Nombre d'années de détention avant revente (ex : 20 ans).
- Prix de revente : saisi directement OU estimé par % d'évolution
  annuelle du marché (ex : +1 %/an sur le prix FAI).

## Partie 2 — Résultats et visuels

### Résultats synthétiques (tableau de bord)
- Mensualité crédit totale (capital + intérêts + assurance).
- Part prise en charge par le locataire = (loyer CC) +
  (charges locatives) ; et part du loyer couvrant le crédit.
- Reste à charge mensuel pour toi (cash-flow mensuel).
- Effort d'épargne total sur N années.
- Rentabilités : brut, net avant impôt, net après impôt et charges,
  rendement TRI (IRR) du projet complet (flux annuels + revente).
- Prix au m² du bien (achat et revente estimé).
- Montant total des intérêts et de l'assurance sur la durée.
- Gain (ou perte) cumulé à la revente, plus-value brute et nette d'impôt.

### Visuel 1 — Évolution du prêt dans le temps
- Courbe capital restant dû + répartition capital/intérêts/assurance
  par année (comme un tableau d'amortissement annuel).

### Visuel 2 — Prise en charge par le locataire
- Chaque année : part du crédit payée par le locataire (recettes) vs
  part payée par toi (reste à charge). Barres empilées + ligne
  cumulée "qui a payé quoi" au fil du temps.

### Visuel 3 — Rentabilité totale à l'horizon de revente
- Cash-flux annuels signés, la revente en dernière année
  (prix revente - crédit restant - impôt plus-value), et le TRI +
  le multiple d'argent investi (Total in - Total out).
- Option : courbe TRI / rentabilité nette en fonction de la durée de
  détention (0 → 30 ans) pour voir quand ça devient rentable.

## Partie 3 — Export PDF "dossier"

- Mise en page print propre (mode @media print) : la page d'un bien
  = 1 page (ou 2) A4 : en-tête (adresse, prix, m², prix/m²), tableau
  de bord, visuel 2 (prise en charge locataire), visuel 3 (TRI).
- Champ "nom / référence de l'option" pour distinguer les dossiers.
- Le PDF est généré par Ctrl+P sur la fiche du bien ; possibilité
  d'imprimer plusieurs biens à la suite ("Imprimer tout le dossier").
- Comparatif multi-biens : écran de synthèse des biens saisis,
  triable (par TRI, par cash-flow...), impriment ensemble.

## Structure du projet

```
renta_invest_immo/
├── index.html          # App complète (HTML+CSS+JS), zéro dépendance
├── README.md           # Usage rapide
└── docs/
    ├── PROPOSITION.md  # Ce fichier
    ├── PLAN.md         # Cahier des charges + étapes implémentation
    └── CALCULS.md      # Détail des formules (validées avant code)
```

## Ce que je te propose de valider / trancher

1. Le périmètre en 3 parties ci-dessus (entrées / visuels / PDF).
2. Approche 1 fichier HTML autonome, zéro dépendance, PDF via Ctrl+P.
3. PDF = fiche par bien + écran comparatif imprimable — tu voulais
   "un petit dossier" : une fiche PDF par bien (séparées) te convient-elle,
   ou un dossier PDF unique multi-options (tout sur un seul PDF) ?
4. Frais de notaire : calcul auto (~4,7 %) avec possibilité de
   surcharger manuellement ?
5. Fiscalité : je propose des paramètres simples (taux marginal sur
   revenus locatifs, % sur plus-value) — pas de barème NC complet.
   OK à ce niveau de simplification ?
6. Visuel par tableaux annuels + graphiques dessinés en SVG maison
   (sans librairie) : OK ?
