# automatisation_traitement_factures
Workflow N8N qui reçoit des factures par Telegram, en extrait les données automatiquement grâce à l'IA, les enregistre dans Google Sheets et confirme le traitement par message.  
## Ce que fait le système

Un chauffeur ou un gestionnaire envoie une facture (PDF ou photo) sur Telegram. En quelques secondes, le système extrait toutes les informations clés, les structure et les enregistre dans un tableur — sans aucune saisie manuelle.

---

## Comment ça fonctionne

### Étape 1 — Réception de la facture via Telegram
Le workflow se déclenche dès qu'un fichier est envoyé dans le bot Telegram. Il peut s'agir d'un PDF ou d'une photo de facture prise avec un smartphone.

### Étape 2 — Récupération et détection du type de fichier
Le système télécharge le fichier et applique une règle de routage : si c'est un PDF, il suit la branche d'extraction de texte. Si c'est une image, il suit la branche d'analyse visuelle. Les deux branches produisent le même résultat structuré.

### Étape 3a — Extraction depuis un PDF
Le texte est extrait directement du PDF, puis envoyé à Claude (Anthropic) qui identifie et structure les données de la facture : fournisseur, date, montant, numéro de facture, lignes de détail…

### Étape 3b — Analyse depuis une image
Claude analyse l'image de la facture (OCR intelligent) et en extrait les mêmes informations, même si la photo est prise à la main ou légèrement floue.

### Étape 4 — Structuration en JSON
La réponse de l'IA est convertie en JSON propre, puis séparée en autant de lignes que nécessaire (une par ligne de facture si besoin).

### Étape 5 — Enregistrement dans Google Sheets
Chaque entrée est ajoutée automatiquement dans le tableur : une ligne par facture avec toutes les informations extraites, prête à être exploitée pour la comptabilité ou le suivi.

### Étape 6 — Confirmation par Telegram
Une fois l'enregistrement effectué, le système envoie un message de confirmation sur Telegram pour indiquer que la facture a bien été traitée.

---

## Résultat

Zéro saisie manuelle. La facture est reçue, lue, structurée et enregistrée en quelques secondes. Le gestionnaire reçoit une confirmation immédiate et retrouve toutes ses factures centralisées dans Google Sheets.

---

## Stack technique

- **n8n** — orchestration du workflow
- **Telegram Bot** — point d'entrée (envoi des factures + confirmation)
- **Claude Sonnet** (Anthropic) — extraction des données PDF et analyse d'images
- **Google Sheets** — stockage et centralisation des factures traitées

