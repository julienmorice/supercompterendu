# Compte rendu de reunion depuis un audio — version statique (GitHub Pages)

Application **100% statique** (HTML/CSS/JavaScript), sans serveur. Elle tourne
directement dans le navigateur et appelle l'API Mistral cote client.

- **Transcription et diarisation** : `voxtral-mini-2602` (Voxtral Mini Transcribe V2).
- **Identification des noms et redaction du compte rendu** : modele de chat Mistral.
- **Generation .docx** : entierement dans le navigateur via la librairie `docx`
  (chargee depuis un CDN public).

## Pourquoi ca marche sans serveur

L'API Mistral autorise les appels directs depuis un navigateur : elle renvoie
`Access-Control-Allow-Origin: *` (CORS ouvert) sur les endpoints de transcription
et de chat. Chaque utilisateur saisit **sa propre cle**, qui reste dans son
navigateur et n'est envoyee qu'a Mistral.

## Ecoute des prises de parole

A l'etape d'identification, chaque participant dispose d'un bouton "Ecouter" qui
joue sa premiere prise de parole (grace aux horodatages de la diarisation). Cela
permet de saisir le bon nom sans se tromper. Le fichier audio reste en memoire du
navigateur, rien n'est renvoye au serveur.

## Limites de la version statique

- **Audio uniquement** : mp3, wav, m4a, flac, ogg. Pas de conversion video
  (ffmpeg n'existe pas dans un site statique). Pour les fichiers video, extraire
  la piste audio au prealable, ou utiliser la version Streamlit.
- Une connexion internet est requise (appels API + librairie docx via CDN).

## Lancer en local

Aucune installation. Deux options :

1. Ouvrir simplement `index.html` dans un navigateur (double-clic).
2. Ou via un petit serveur local :
   ```
   cd web_cr_audio
   python3 -m http.server 8000
   ```
   puis ouvrir http://localhost:8000

## Deployer sur GitHub Pages

1. Creer un depot GitHub et y placer le contenu de ce dossier (au minimum
   `index.html`) a la racine.
2. Dans le depot : **Settings > Pages**.
3. Source : `Deploy from a branch`, branche `main`, dossier `/ (root)`.
4. Enregistrer. L'URL publique apparait apres une minute, du type
   `https://<utilisateur>.github.io/<depot>/`.

Aucune cle API n'est commitee : chaque utilisateur saisit la sienne dans
l'interface.

## Confidentialite

Aucune donnee n'est stockee. Les fichiers audio, transcripts et comptes rendus
existent uniquement le temps de la session, en memoire du navigateur. La cle API
reste dans le navigateur et n'est transmise qu'a l'API Mistral.

## Personnalisation du gabarit .docx

Modifier en tete du `<script>` dans `index.html` : `ACCENT`, `HEADER_INSTITUTION`,
`FOOTER_TEXT`. La couleur d'accent par defaut est le violet IMT-BS `AD1D89`.
