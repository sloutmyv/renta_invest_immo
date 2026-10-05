# Renta Invest Immo — Cahier des charges V1 validé

Version 1.0 — 2026-10-04 — VALIDÉ par Sylvain
(Devise : XPF — Fichier unique index.html sans dépendance)

## Décisions validées

1. Export PDF : fiche par bien, un seul bien à la fois (pas de
   comparatif multi-biens).
2. Frais de notaire : saisie manuelle (pas de calcul auto).
3. Fiscalité locative : taux simple paramétrable (pas de barème NC
   complet), pas de report du déficit sur autres années (si résultat
   négatif → impôt = 0 la même année).
4. 2 assurances distinctes : assurance de prêt (emprunteur) et
   assurance habitation PNO (propriétaire non occupant).
5. Visuel "qui paie quoi" : affectation des recettes locataire
   → intérêts+assurance, puis capital, puis charges (ordre tous
   validés).
6. Plus-value : base = prix de revente - frais de vente - prix FAI.
7. Indexation des loyers : champ "indexation loyer (%/an)" dans le
   formulaire, par défaut 0 %, modifiable (seuls les loyers sont
   indexés, pas les charges).

## Champs de saisie V1 (définitifs)

Bien :
- Adresse / description (texte) ; Type (T1..T4/Maison dropdown) ;
  Année construction ; Surface habitable m² ; Surface annexes m² ;
  Prix FAI XPF. Prix au m² calculé et affiché.

Financement :
- Apport XPF ; Montant emprunté XPF (champ explicite, pas calculé —
  c'est toi qui décides si ta banque finance les frais) ; Durée
  années ; Taux annuel % ; Assurance prêt : toggle % capital/an
  OU forfait mensuel XPF ; Frais dossier XPF ; Frais garantie XPF ;
  Frais courtier XPF.

Frais à l'achat (unes fois) :
- Frais de notaire XPF (saisie manuelle).

Recettes :
- Loyer mensuel hors charges XPF.
- Indexation loyer %/an (défaut 0).
- Charges locatives mensuelles XPF.
- Vacance locative % (défaut 5).
- Gestion locative % du loyer (défaut 0).

Charges propriétaire :
- Charges copro mensuelles XPF ; Taxe foncière annuelle XPF ;
  Assurance PNO annuelle XPF ; Entretien/travaux % du loyer
  (défaut 5).

Fiscalité :
- Taux imposition résultat locatif % (défaut 0).

Scénario de revente :
- Durée détention années (≤ durée crédit autorisée ; sinon on
  solder le CR avec le prix de vente) ; Mode prix : montant XPF
  OU % évolution annuelle sur prix FAI ; Frais de vente agence %
  (défaut 0) ; Taux imposition plus-value % (défaut 0).

## Livrables V1

- Bloc 1 : squelette + formulaire + sauvegarde localStorage.
- Bloc 2 : moteur calcul crédit & exploitation + tableau de bord
  indicateurs.
- Bloc 3 : scénario de revente + TRI + multiple d'argent investi.
- Bloc 4 : 3 visuels SVG (amortissement annuel ; locataire vs moi ;
  profil TRI / net selon durée détention).
- Bloc 5 : fiche PDF (media print) par bien, avec champ référence
  option/commentaire.
- Bloc 6 (bonus, à la demande) : tooltips, cas type, export/import
  JSON multi-biens.

## Non-périmètre de la V1

- Comparaison multi-biens à l'écran (un bien à la fois).
- Barème fiscal NC complet (taux simple uniquement).
- Indexation des charges (seuls les loyers s'indexent).
- Conversion EUR.
- Travaux déduits de la base de la plus-value.
