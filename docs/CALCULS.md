# Renta Invest Immo — Formules de calcul

Version : 1 (pour validation, avant bloc 2) — Devise : XPF
Note : XPF est indexé sur l'euro (1 EUR = 119.33174 XPF) mais tous
les calculs sont faits uniquement en XPF, aucune conversion n'est
nécessaire si tous paramètres sont en XPF.

## Convention de découpage

- Échéancier de crédit calculé **mensuellement**, agrégé par année
  pour l'affichage et le TRI (les flux annuels sont plus lisibles
  pour un TRI sur 20 ans).
- Année 1 = première année de détention (dès achat). La revente a
  lieu en fin d'année N (N = durée de détention choisie).

## 1. Crédit

- `M` = mensualité (annuité constante) :
  M = C * i / (1 - (1+i)^-n) avec C = capital emprunté,
      i = taux annuel / 12, n = durée en mois.
- Assurance de prêt :
  - mode % du capital initial : A_mens = C * (taux_assurance/12)
  - mode forfait : A_mens = forfait donné.
- Pour chaque mois m (1..n) :
  intérêts_m = CR_début_mois * i
  capital_m = M - intérêts_m
  CR_fin_mois = CR_début_mois - capital_m
- Coût total du crédit = SOMME(intérêts) + SOMME(assurance) +
  frais dossier + garantie + courtier (les 3 derniers comptés une
  seule fois, en début de projet).

## 2. Recettes locatives

- Vacance locative : les recettes annuelles sont multipliées par
  (1 - vacance) — la vacance est en jours/habitant, appliquée
  uniformément chaque année.
- Loyer annuel perçu = loyer_mensuel * 12 * (1 - vacance)
- Charges locatives rétrocédées = charges_locatives_mensuelles * 12
  * (1 - vacance)
- Frais de gestion locative = gestion_pct * loyer annuel hors charges.

## 3. Charges propriétaire (annuelles)

- Charges copro / proprio = charges_mensuelles * 12
- Taxe foncière = montant annuel saisi
- PNO = montant annuel saisi
- Entretien / travaux = entretien_pct * loyer annuel hors charges
- Charges locatives versées au syndic puis rétrocédées ne sont PAS
  une charge nette (elles sont remboursées par le locataire).

## 4. Fiscalité locative (taux simple paramétré)

- Résultat locatif imposable annuel = loyer annuel hors charges
  - frais gestion - charges copro - taxe foncière - PNO - entretien
  - intérêts bancaires de l'année - assurance de prêt de l'année.
- Impôt annuel = taux_imposition * max(0, résultat locatif) :
  pas de report de déficit dans cette version simple ; on ne
  crée pas de "crédit d'impôt" si le résultat est négatif.
  → À noter : les années récentes (fort intérêts en début de
  crédit) sont souvent déficitaires → impôt = 0 ces années-là.

## 5. Flux annuels (cash-flows) avant revente

Pour chaque année k (1..N-1 si revente, 1..n sinon) :

recettes = loyer annuel perçu + charges rétrocédées
dépenses d'exploitation = frais gestion + charges copro
                          + taxe foncière + PNO + entretien
mensualité crédit = M * 12 (capital + intérêts) + assurance * 12

Remboursement réel = (recettes - dépenses - mensualité annuelle -
                      impôt)
= ce que tu verses (ou reçois) de ta poche chaque année.

## 6. Revente (année N)

- Prix de vente à revente = saisi OU = prix FAI * (1 + évolution_annuelle)^N
- Frais de vente agence = vendeur_pct * prix de vente
- Capital restant dû CR_N : solde du crédit à la fin de l'année N.
- Plus-value = prix de vente - frais de vente - prix d'achat FAI
  - (option : - frais notaire, - travaux justifiés) — simplifié :
    prix d'achat retenue = prix FAI (|| surcharge future si besoin).
- Impôt plus-value = taux_PV * max(0, plus-value)
- Flux de revente (année N) = prix de vente - frais de vente
  - CR_N - impôt plus-value

## 7. Indicateurs de rentabilité

- Prix / m² = prix FAI / surface habitable (+ affichage revente).
- Rendement brut = (loyer annuel * 12) / prix FAI (vacance incluse).
- Rendement net d'exploitation (avant dette) =
  (recettes totales - dépenses exploitation - impôt) / prix FAI
- Cash-flow mensuel moyen = somme des flux annuels (hors revente)
  / N / 12.
- TRI (IRR) du projet = taux r tel que :
  -année 0 : apport + frais notaire + frais dossier/garantie/courtier
    (+ coût total hors crédit non financé, ex : frais de notaire à
    payer au comptant = l'apport doit les couvrir)
  -années 1..N-1 : flux annuels
  -année N : flux de revente
  via Newton / dichotomie (JS maison, pas de librairie).
- Multiple d'argent investi = 
  (somme flux positifs + flux de revente) / |somme flux négatifs|.

## 8. Qui paie quoi (visuel 2)

Annuellement :
- Payé par le locataire = recettes (loyer + charges rétrocédées)
  affectées à : intérêts+assurance de l'année, puis capital,
  puis les dépenses d'exploitation au prorata.
- Payé par toi = reste à charge annuel = max(0, - flux annuel).
- Cumul année par année → courbe "la part locataire vs la part
  perso" dans le temps.

## Points ouverts (à trancher avec le bloc 2)

1. Traitement des revenus (affectation au crédit vs charges) dans
   le visuel 2 : ordre ci-dessus (intérêts d'abord) OK ?
   → VALIDÉ 2026-10-04.
2. Plus-value : base retenue = prix FAI revente - frais de vente
   (travaux non tracés en compte).
   → VALIDÉ 2026-10-04.
3. Inflation des loyers : indexation par défaut à 0 %, champ
   "indexation loyer (%/an)" modifiable dans le formulaire. Le loyer
   mensuel de l'année k = loyer_k-1 * (1 + indexation). Vérification:
   charges aussi indexables ? → Non dans V1, seuls les loyers sont
   indexés (champs loyer + charges restent en base fixe, seuls les
   loyers incorporent l'inflation).
   → VALIDÉ 2026-10-04.
