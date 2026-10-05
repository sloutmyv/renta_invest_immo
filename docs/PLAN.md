# Renta Invest Immo — Plan d'implémentation

Date : 2026-10-04 — Statut : PLAN (en attente de validation)
Préambule : validation utilisateur du 2026-10-04 :
- Export PDF par bien, un bien à la fois (pas de comparatif multi-biens).
- Frais de notaire : saisie manuelle.
- Fiscalité : taux simple paramétrable (mensualisé sur les loyers).
- 2 assurances distinctes : assurance de prêt (emprunteur) et
  assurance habitation propriétaire non occupant (PNO).

## Architecture

1 fichier `index.html` :
- `<style>` inline (thème sombre léger + `@media print` pour le PDF).
- `<script>` inline organisé en modules logiques :
  1. `state` : objet de saisie (champ par champ) + localStorage.
  2. `loan` : échéancier de crédit (mensualité, amortissement par
     année, capital restant, intérêts, assurance de prêt).
  3. `revenue` : recettes locataire, charges, vacance locative,
     gestion, travaux, PNO, taxe foncière, fiscalité.
  4. `resale` : revente, plus-value, impôt plus-value, flux final.
  5. `metrics` : rentabilités brut / net / TRI, prix au m²...
  6. `charts` : SVG maison — barres empilées + courbes.
  7. `pdf` : bloc `@media print` + bouton "Exporter en PDF".
- Tabs ou sections dépliables : Saisie | Tableau de bord | Visuels
  | Fiche PDF.

## Découpage en blocs (1 validation utilisateur par bloc)

### Bloc 1 — Squelette + Saisie
- Layout de la page (sections, onglets), thème.
- Formulaire complet de saisie (tous les champs ci-dessous), avec
  sauvegarde auto en localStorage et bouton "Effacer".
- Formatage XPF (séparateur milliers, pas de centimes dans la saisie
  des montants en XPF), pourcentages décimaux.
- Contact/validation : je livre la maquette statique (sans calculs).

### Bloc 2 — Moteur de calcul + Tableau de bord
- Module `loan` (échéancier annuel complet, pas seulement la
  mensualité) et `revenue`.
- Tableau de bord avec tous les indicateurs (voir CALCULS.md).
- Validation : on vérifie ensemble les chiffres sur un cas test
  (ex : T2 Nouméa 33 M XPF, 10 % apport, 3,5 % sur 20 ans).

### Bloc 3 — Scénario de revente + TRI
- Plus-value, impôt plus-value, flux final de revente, TRI,
  multiple d'argent investi, profil TRI vs durée de détention.
- Validation : cas test de revente (10 ans, prix +1 %/an).

### Bloc 4 — Visuels SVG
- Graphique 1 : amortissement annuel (barres capital/intérêts/
  assurance empilées + ligne capital restant).
- Graphique 2 : prise en charge locataire vs reste à charge
  (annuel + cumulé).
- Graphique 3 (optionnel, si validé) : profile du TRI / net selon
  la durée de détention.
- Validation : visuel sur cas test.

### Bloc 5 — Fiche PDF
- `@media print` : masque le formulaire, mode "aperçu fiche PDF"
  (en-tête bien, indicateurs, tableau annuel condensé, graphiques 1
  et 2, scénario de revente), bouton "Exporter en PDF" (Ctrl+P).
- Champ "référence de l'option / commentaire de la fiche".
- Validation : sortie PDF téléchargée et lue.

### Bloc 6 (bonus, si validé) — Divers
- Aide contextuelle sur les champs (tooltips expliquant la formule).
- Cas d'usage "maison" (pré-remplir quelques valeurs types).
- Export/import de la saisie en JSON (sauvegarder plusieurs biens).

## Champs de saisie (définitifs pour bloc 1)

### Bien
- Adresse / description (texte libre) — pour la fiche PDF.
- Type (T1/T2/T3/T4/Maison) ; année de construction (int).
- Surface habitable (m²) + surface annexes (m², informatif).
- Prix d'achat FAI (XPF).
- Prix au m² : calculé, affiché.

### Financement
- Apport (XPF), montant emprunté : calculé = prix FAI + frais notaire
  - apport, avec possibilité de le surcharger (car parfois le banque
  finance aussi les frais de notaire — je te propose : case à cocher
  "la banque finance une partie des frais de notaire" et le montant
  est recalculé, sinon saisi manuellement).
- Durée du crédit (années) ; taux nominal annuel (%).
- Assurance de prêt : choisir le mode → % du capital initial / an,
  OU montant mensuel fixe XPF.
- Frais de dossier banque (XPF), frais de garantie (XPF),
  frais de courtier (XPF) — intégrés au calcul du coût total du
  crédit affiché, mais pas ancrés dans le plan d'amortissement.

### Recettes
- Loyer mensuel hors charges (XPF).
- Charges locatives mensuelles (provision payée par le locataire, XPF).
- Vacance locative : % du temps sans locataire (par défaut 5 %).
- Frais de gestion locative : % du loyer (0 si gestion directe).

### Charges propriétaire
- Charges copropriété / propriétaire mensuelles (XPF).
- Taxe foncière annuelle (XPF).
- Assurance PNO : annuelle (XPF).
- Provision entretien / travaux : % du loyer mensuel (par défaut 5 %).

### Fiscalité
- Taux d'imposition sur le résultat locatif (% mensualisé, simple,
  appliqué au résultat annuel : recettes - charges - intérêts -
  assurance - frais récurrents).

### Scénario de revente
- Durée de détention avant revente (années ≤ durée du crédit
  autorisée ; sinon capital restant à solder par le prix de vente).
- Prix de revente : mode au choix → montant saisie (XPF), OU % de
  variation annuelle du marché appliqué au prix FAI.
- Frais de vente agence : % ou 0 (si vente en direct).
- Taux d'imposition sur la plus-value (%) (0 si exonéré).

## Détail des calculs
Voir docs/CALCULS.md (à valider avec le bloc 2).
