# Kit « Arguments 2.0 » pour Claude Code

Tout ce qu'il faut pour produire les épisodes (Tariq et ses arguments 2.0, style C) chez toi avec Claude Code.

## Contenu
| Dossier | Quoi |
|---|---|
| `CLAUDE.md` | Règles du projet (voix, limites, style C, stats). Chargé automatiquement par Claude Code. |
| `.claude/skills/` | 7 skills : `/tariq-episode`, `/verif-sources`, `/check-moderation`, `/cover`, `/anges-batons`, `/stats-tiktok`, `/traduction-en` |
| `pipeline/code/` | Moteur commun (sprites `chars_v2.py`, `monteur.py`, `render_auto.py`, couvertures, intros…) |
| `pipeline/ep25/` | Le devoir conjugal (couple, jauge, jingle livre + voix du jingle) |
| `pipeline/ep26/` | L'Éthiopien (foule, estrade, frise) → **modèle pour les nouveaux épisodes** |
| `pipeline/ep24/`, `ep23/` | Épisodes précédents (illustrations, `flat_women.py`) |
| `pipeline/anges/`, `merci/` | Anges en bâtons, visuels merci/prière |
| `pipeline/dl/` | Polices pixel/arabe, palettes |
| `fonts/`, `assets/images/` | Inter, FreeSerif, Bangers, Comic Neue, Playfair ; images utilisées |
| `atelier/scripts/` | Export JSON des 14 scripts de l'atelier |
| `scripts/` | `fix_paths.py` (à lancer une fois), `render.sh` (rendu + −14 LUFS) |

Non inclus (trop lourd, se régénère) : `clips/`, `build/`, mp4, frames du sting ep24 (115 Mo), modèle Whisper (téléchargé tout seul).

## Installation (une fois)
1. **Claude Code** : `npm install -g @anthropic-ai/claude-code` (Node 18+), puis `claude` pour te connecter.
2. **ffmpeg** : macOS `brew install ffmpeg` · Windows : utiliser WSL (Ubuntu) puis `sudo apt install ffmpeg`.
3. **Python 3.10+** et les paquets :
   ```bash
   pip install numpy pillow scipy faster-whisper python-bidi arabic-reshaper
   ```
   (Si Pillow ne gère pas l'arabe RTL : `pip install --upgrade pillow` et installer `libraqm` — `brew install libraqm`.)
4. Dézipper le kit où tu veux, puis :
   ```bash
   cd arguments20
   python3 scripts/fix_paths.py     # réécrit tous les chemins vers ce dossier
   ```
5. (Optionnel) Sauvegarde privée sur GitHub : `git init && git add . && git commit -m kit && gh repo create arguments20 --private --source=. --push`

## Utilisation au quotidien
```bash
cd arguments20
claude
```
Puis, dans Claude Code :
- colle ton script → `/tariq-episode` (il vérifie les sources, découpe, te donne les blocs ElevenLabs) ;
- dépose tes prises dans `pipeline/epXX/audio/` (sham.mp3, jms.mp3, tariq.mp3…) et dis « produis la vidéo » ;
- `/cover`, `/check-moderation`, `/stats-tiktok`, `/traduction-en`, `/anges-batons` selon le besoin.

Commandes à la main (si tu veux) :
```bash
cd pipeline/ep26
python3 align.py                                   # découpe des prises → clips/ + timeline.json
python3 ep.py still 1,5,20,60 ../../sortie/test.png # planche d'images
cd ../.. && bash scripts/render.sh pipeline/ep26 sortie/ethiopien.mp4
```
Le 1er `align.py` télécharge Whisper « small » (~480 Mo). Le rendu complet prend quelques minutes (6 processus en parallèle ; 3e argument de render.sh pour changer).

## Re-rendre un épisode existant
`pipeline/ep25` et `ep26` contiennent leurs prises et `timeline.json`. Lance `python3 align.py` (recrée `clips/`, les coupes manuelles sont déjà dans le fichier) puis `render.sh`.

## Pistes en attente
- Tester un **cold open de 2 s** (question choc + mouvement) sur le prochain épisode, jingle après la 1re révélation.
- Animer les anges (≈ 10 s) avec ou sans voix.
- Pilote du compte EN : « La femme noire ».
- Ep26 v3 avec cold open, si tu veux comparer les rétentions.
