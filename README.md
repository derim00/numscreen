# NumScreen : ton ordinateur dans ta NumWorks

**NumScreen affiche l'écran de ton ordinateur sur une calculatrice NumWorks, et
la calculatrice devient sa souris et son clavier.** Branche-la en USB à un Mac
ou à un PC Windows : tu vois l'écran, tu bouges la souris avec les flèches, tu
cliques avec `OK` et tu tapes avec les touches de la calculatrice. Et tu peux
même jouer, à Roblox par exemple.

- Écran de l'ordinateur en 320×200, environ 15 images/s, avec le curseur
- Souris aux flèches : un appui court pour la précision, maintenue elle accélère
- Clic, double clic, glisser, clic droit, molette
- Tout le clavier : lettres, chiffres, majuscules, symboles, Entrée, Échap…
- Les touches maintenues sur la calculatrice restent maintenues sur l'ordinateur
- **Mode jeu** : les flèches deviennent celles du clavier et `OK` saute ;
  `var` met `Z` `Q` `S` `D` sur `8` `4` `5` `6` pour jouer en AZERTY
- Mac et Windows, sans pilote à installer

NumScreen fait partie de **[NumSocial](https://github.com/derim00/numsocial)** :
le même firmware contient aussi **[NumTube](https://github.com/derim00/numtube)**
(YouTube) et **[NumChat](https://github.com/derim00/numchat)** (Snapchat), et
une seule appli, **NumSocial**, installe celles que tu choisis.

## Sommaire

1. [Pourquoi un patch et pas un firmware](#pourquoi-un-patch-et-pas-un-firmware)
2. [Ce qu'il faut](#ce-quil-faut)
3. [Tutoriel : obtenir l'appli NumSocial](#tutoriel--obtenir-lappli-numsocial)
4. [Tutoriel : installer sur la calculatrice](#tutoriel--installer-sur-la-calculatrice)
5. [Tutoriel : autorisations sur Mac](#tutoriel--autorisations-sur-mac)
6. [Utilisation : les touches](#utilisation--les-touches)
7. [Jouer (Roblox…)](#jouer-roblox)
8. [Problèmes fréquents](#problèmes-fréquents)
9. [Comment ça marche](#comment-ça-marche)
10. [Tests](#tests)
11. [Avertissements](#avertissements)

## Pourquoi un patch et pas un firmware

Depuis juillet 2021, le code d'Epsilon (le logiciel de la NumWorks) n'est plus
sous licence libre : NumWorks n'autorise plus sa redistribution. Ce dépôt ne
contient donc **aucun code ni aucun firmware de NumWorks**, seulement :

| Dossier | Contenu |
| --- | --- |
| `patches/numsocial-epsilon.patch` | les modifications à appliquer à Epsilon (lien USB de Termia, NumScreen, NumTube, NumChat, choix des applis) |
| `EPSILON_COMMIT` | la version exacte d'Epsilon sur laquelle le patch s'applique |
| `scripts/build-firmware.sh` | télécharge Epsilon chez NumWorks, applique le patch, compile |
| `app/` | l'appli NumSocial : installeur et passerelle (page dans le navigateur) |
| `gateway/numscreen_*.py` | la partie NumScreen de la passerelle : capture d'écran, souris, clavier |
| `tests/fake_numscreen.py` | une fausse calculatrice pour tester sans rien cliquer |

Le firmware se compile chez toi (ou dans ta copie privée sur GitHub), à partir
des sources officielles de NumWorks. Le firmware compilé et l'appli qui
l'embarque sont pour ton usage : ne les partage pas, partage ce dépôt.

## Ce qu'il faut

- Une NumWorks **N0120** ou **N0115** en version officielle **26.4.0** (testé)
- Un câble USB qui transfère les données (pas seulement la charge)
- Un **Mac** (testé avec une calculatrice) ou un **PC Windows 10/11** (compilé
  et vérifié par GitHub, pas encore essayé avec une calculatrice)

## Tutoriel : obtenir l'appli NumSocial

### Sans rien compiler (Windows ou Mac)

1. En haut de cette page : **Use this template** > **Create a new repository**.
2. Donne-lui un nom et coche **Private** (obligatoire : la compilation refuse de
   tourner dans un dépôt public, pour que le firmware ne soit pas
   téléchargeable par tous), puis **Create repository**.
3. Dans ta copie : onglet **Actions**, à gauche **Build NumSocial**, à droite
   **Run workflow** > **Run workflow**.
4. Environ 15 minutes plus tard, ouvre la compilation terminée (coche verte) et
   télécharge en bas de la page **NumSocial-windows** ou **NumSocial-mac**.
   Décompresse.
5. Lance l'appli :
   - **Windows** : `NumSocial\NumSocial.exe`. Si SmartScreen s'affiche :
     « Informations complémentaires » > « Exécuter quand même ».
   - **Mac** : clic droit sur `NumSocial.app` > **Ouvrir** > **Ouvrir**.

### En compilant sur ton Mac

Il faut `git`, `make`, `python3` et le kit de compilation ARM de NumWorks (le
script te dit quoi lancer s'il manque).

```bash
git clone https://github.com/derim00/numscreen.git && cd numscreen
./scripts/build-firmware.sh          # télécharge Epsilon, applique le patch, compile
.venv/bin/python3 app/build.py       # fabrique l'appli
open app/dist/NumSocial.app
```

## Tutoriel : installer sur la calculatrice

1. L'appli NumSocial est ouverte : sa page s'affiche dans ton navigateur
   (`http://127.0.0.1:8737`).
2. Branche la calculatrice. Elle doit afficher **« Calculatrice connectée »**.
   Sinon : appuie sur **reset** au dos (avec un trombone), puis rebranche.
3. La page lit toute seule la version officielle de la calculatrice.
4. Dans **Applis à installer**, coche **NumScreen** (et NumTube, NumChat si tu
   les veux aussi).
5. Clique **Installer sur la calculatrice** (environ 1 minute, ne débranche pas).
6. La calculatrice redémarre sur « UNOFFICIAL SOFTWARE » : c'est normal.
   NumScreen est en bas de l'écran d'accueil.

Le firmware officiel n'est jamais effacé : NumSocial va dans l'autre
emplacement de la mémoire, et un reset au dos ramène le firmware officiel.

## Tutoriel : autorisations sur Mac

macOS demande deux autorisations avant qu'une appli puisse voir l'écran et
piloter la souris. Ça se fait une seule fois, dans **Réglages Système >
Confidentialité et sécurité** :

1. **Enregistrement de l'écran** (ou « Enregistrement de l'écran et audio ») :
   active **NumSocial**. S'il n'est pas dans la liste, clique **+** et
   choisis `NumSocial.app`. Puis quitte et relance NumSocial.
2. **Accessibilité** : active **NumSocial** de la même façon.

Tant qu'il en manque une, la calculatrice l'écrit en bas de l'écran. Sans
« Enregistrement de l'écran », l'image ne montre que le fond d'écran ; sans
« Accessibilité », les touches ne font rien.

> Si tu recompiles l'appli, macOS ne la reconnaît plus : retire NumSocial des
> deux listes avec **−** puis remets-le avec **+**.

Sous Windows, il n'y a rien à autoriser.

## Utilisation : les touches

Laisse NumSocial ouvert sur l'ordinateur et ouvre **NumScreen** sur la
calculatrice. L'écran de l'ordinateur s'affiche en haut ; en bas, le mode et
l'état du clavier (lettres ou chiffres, majuscules, ZQSD).

### Souris et modes

| Touche | Mode souris | Mode jeu |
| --- | --- | --- |
| flèches | déplacent la souris (maintenir : de plus en plus vite) | flèches du clavier |
| `OK` | clic (deux fois vite : double clic ; maintenir : glisser) | `Espace` (sauter) |
| boîte à outils | clic droit (maintenu + flèches : tourner la caméra) | clic droit |
| `alpha` + `▲` `▼` | molette | |
| `x,n,t` | passer en mode jeu | revenir en mode souris |

### Clavier (dans les deux modes)

| Touche | Sur l'ordinateur |
| --- | --- |
| lettres | par défaut, les touches tapent les lettres écrites au-dessus d'elles |
| `shift` | passe aux chiffres, puis revient aux lettres |
| `alpha` | une majuscule (deux fois : majuscules verrouillées) ; en chiffres : le symbole jaune `[ ] { } _ = < >` |
| `var` | `Z` `Q` `S` `D` sur `8` `4` `5` `6` (`var` à nouveau : retour) |
| `EXE` / `retour` / `⌫` | `Entrée` / `Échap` / `Retour arrière` |
| `Ans` / `×10ˣ` | `Espace` / apostrophe |
| en chiffres : `exp` / `ln` / `log` | `Maj` / `Ctrl` / `Alt` de l'ordinateur, tant qu'on appuie |
| maison | arrêter le partage |

Le texte tapé suit la disposition de ton clavier (AZERTY, QWERTY…) : un `a` sur
la calculatrice donne un `a` sur l'ordinateur.

## Jouer (Roblox…)

1. Lance le jeu sur l'ordinateur et ouvre NumScreen sur la calculatrice.
2. `x,n,t` : **mode jeu**. Les flèches deviennent celles du clavier, `OK` fait
   sauter.
3. `var` : **ZQSD** sur `8` `4` `5` `6`, comme sur un clavier AZERTY. Maintiens
   la touche pour avancer : elle reste enfoncée sur l'ordinateur.
4. Pour viser ou tourner la caméra : repasse en mode souris (`x,n,t`) et
   maintiens la boîte à outils (clic droit) en appuyant sur les flèches.

L'image fait 320×200 à environ 15 images/s, avec un peu de retard : c'est
parfait pour naviguer et jouer tranquillement, pas pour un jeu de réflexes.

## Problèmes fréquents

| Ce que tu vois | Quoi faire |
| --- | --- |
| Seulement le fond d'écran | Autorisation « Enregistrement de l'écran » manquante (Mac), puis relance NumSocial |
| Les touches ne font rien | Autorisation « Accessibilité » manquante (Mac) |
| « Le PC ne répond plus » | NumSocial n'est pas ouvert sur l'ordinateur |
| « OFFICIAL UPGRADE REQUIRED » | La version saisie ne correspond pas : reset au dos, réinstalle |
| Le jeu ne réagit pas | Clique d'abord dans la fenêtre du jeu (mode souris, `OK`) pour lui donner le clavier |

## Comment ça marche

```
NumWorks (appli NumScreen)  ⇄ USB ⇄  appli NumSocial (passerelle)  ⇄  écran, souris et clavier du système
```

- **Image** : la passerelle capture l'écran (bibliothèque
  [mss](https://github.com/BoboTiG/python-mss)), le réduit en 320×200 et
  dessine le curseur. Elle le code comme la vidéo de NumTube : des blocs de 4×4
  pixels, et seuls ceux qui ont changé partent. La calculatrice accuse
  réception de chaque image, avec au plus 2 images en vol, pour qu'il n'y ait
  jamais de retard qui s'accumule.
- **Touches** : la calculatrice lit l'état de son clavier en continu et envoie
  chaque appui et chaque relâchement. Tout le reste se fait sur l'ordinateur
  (lettres ou chiffres, majuscules, mode jeu, vitesse de la souris), avec les
  interfaces du système : CoreGraphics sur Mac, `SendInput` sous Windows.
- **USB** : interface USB propriétaire pilotée avec libusb, trames `TMIA` avec
  CRC-16, reprises de [Termia](https://github.com/Matinthereal/termia). Sous
  Windows, la calculatrice déclare le pilote WinUSB : rien à installer.

Ce dépôt contient le même code que [NumSocial](https://github.com/derim00/numsocial),
[NumTube](https://github.com/derim00/numtube) et [NumChat](https://github.com/derim00/numchat).

## Tests

```bash
cd tests
../.venv/bin/python3 fake_numscreen.py    # image de test, rien n'est cliqué ni tapé
```

La fausse calculatrice vérifie le flux d'images et le contrôle de débit,
décode l'image avec le même algorithme que la calculatrice, et contrôle que
chaque touche donne la bonne action (souris, lettres, chiffres, majuscules,
ZQSD, mode jeu, relâchement de tout à l'arrêt).

## Avertissements

- Projet non officiel, sans lien avec NumWorks, Apple, Microsoft ou Roblox. Les
  noms cités sont des marques de leurs propriétaires.
- NumScreen pilote vraiment ta souris et ton clavier : ne laisse pas la
  calculatrice entre de mauvaises mains pendant que NumSocial est ouvert.
- Un firmware non officiel ne peut pas passer en mode examen.

## Licence

Le code de ce dépôt est sous licence
[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.fr),
comme Termia dont il reprend le lien USB. Voir [LICENSE](LICENSE).
