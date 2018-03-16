![http://mpv.io/](https://raw.githubusercontent.com/mpv-player/mpv.io/master/source/images/mpv-logo-128.png)

## mpv

--------------


* [Liens externes] (# liens externes)
* [Aperçu] (# aperçu)
* [Configuration requise] (# exigences système)
* [Téléchargements] (# téléchargements)
* [Changelog] (# changelog)
* [Compilation] (# compilation)
* [FFmpeg vs Libav] (# ffmpeg-vs-libav)
* [Compatibilité FFmpeg ABI] (compatibilité # ffmpeg-abi)
* [Cycle de libération] (# cycle de libération)
* [Bug reports] (# rapports de bug)
* [Contribuer] (# contribuer)
* [Relation avec MPlayer et mplayer2] (# relation-to-mplayer-and-mplayer2)
* [Licence] (# licence)
* [Contact] (# contact)


## Liens externes


* [Wiki] (https://github.com/mpv-player/mpv/wiki)
* [FAQ] (https://github.com/mpv-player/mpv/wiki/FAQ)
* [Manuel] (http://mpv.io/manual/master/)


## Aperçu


**mpv** est un lecteur multimédia basé sur MPlayer et mplayer2. Il prend en charge beaucoup de formats de fichiers vidéo, codecs audio et vidéo et types de sous-titres.

Les Sorties peuvent être trouvés sur la [liste de Sorties][Sorties].

## Configuration requise

- Un Linux pas trop ancien, Windows 7 ou plus récent, ou OSX 10.8 ou plus récent.
- Un bon processeur. Le décodage matériel peut aider si le processeur est trop lent à Décoder la vidéo en temps réel, mais doit être explicitement activé avec `--hwdec` option.
- Un bon GPU. mpv n'est pas destiné à être utilisé avec de mauvais GPU. Il y a de nombreuses mises en garde avec des pilotes ou des compositeurs de système causant des déchirures, bug etc. Sur Windows, vous pouvez vous assurer que les pilotes graphiques sont actuel. Dans certains cas, les anciennes méthodes de sortie vidéo de secours peuvent aider (comme `--vo = xv` sous Linux), mais cette utilisation n'est pas recommandée ni prise en charge.


## Téléchargements


Pour les builds semi-officiels et les packs tiers, veuillez consulter ici [mpv.io/installation](http://mpv.io/installation/).

## Changelog


Il n'y a pas de changelog complet ; Cependant, les modifications apportées à l'interface du lecteur sont répertoriés dans l'[interface changelog][interface-changes].

Les modifications de l'API C sont documentées dans le [client API changelog][api-changes].

Le [liste de Sorties][Sorties] hcomme un résumé de la plupart des changements importants à chaque sortie.

Les modifications apportées aux raccourcis clavier par défaut sont indiquées dans [restore-old-bindings.conf][restore-old-bindings].

## Compilation


La compilation avec des fonctionnalités complètes nécessite des fichiers de développement pour plusieurs bibliothèques externes. Voici une liste de certaines exigences importantes.

Le système de construction mpv utilise [waf](https://waf.io/),mais nous ne le stockons pas dans le repository. Le `./bootstrap.py` script va télécharger la dernière version de waf qui a été testé avec le système de construction.

Pour obtenir une liste des options de génération disponibles, utilisez `./waf configure --help`. Si vous pensez que vous avez un support pour certaines fonctionnalités installées mais que la configuration ne parvient pas à le détecter, le fichier `build/config.log` peut contenir des informations sur les raisons de l'échec.

NOTE : pour éviter d'encombrer la sortie avec du spam illisible, `--help` seulement des spectacles l'un des deux switchs pour chaque option. Si l'option est détectée automatiquement par défaut, le `--disable-***` le commutateur est imprimé ; si l'option est désactivée par défaut, le `--enable-***` le switch est imprimé. De toute façon, vous pouvez utiliser `--enable-***` or `--disable-**` indépendamment de ce qui est imprimé par `--help`.

Pour construire le logiciel, vous pouvez utiliser `./waf build`: le résultat de la compilation sera situé dans `build / mpv`. Vous pouvez utiliser `./waf install` pour installer mpv au * prefix * après sa compilation.
Exemple :

    ./bootstrap.py
    ./waf configure
    ./waf
    ./waf install

Dépendances essentielles (liste incomplète) :

- gcc ou clang
- En-têtes de développement X (xlib, xrandr, xext, xscrnsaver, xinerama, libvdpau, libGL, GLX, EGL, xv, ...)
- En-têtes de développement de sortie audio (libasound/ALSA, pulseaudio)
- Bibliothèques FFmpeg (libavutil libavcodec libavformat libswscale libavfilter et soit libswresample ou libavresample)
- zlib
- iconv (normalement fourni par le système libc)
- libass (OSD, OSC, sous-titres de texte)
- Lua (optional, nécessaire pour l' OSC pseudo-GUI et youtube-dl intégration)
- libjpeg (optionnel, utilisé pour les captures d'écran uniquement)
- uchardet (optionnel, pour la détection de charset de sous-titres)
- vdpau and vaapi bibliothèques pour le décodage matériel sous Linux (optionnel)

Libass dépendances :

- gcc ou clang, yasm sur x86 et x86_64
- fribidi, freetype, en-têtes de développement fontconfig (pour libass)
- harfbuzz (facultatif, requis pour le rendu correct des caractères de combinaison, en particulier pour le rendu correct du texte non anglais sur OSX, et Arabe / Indic scripts sur n'importe quelle plate-forme)


FFmpeg dépendances : 
- gcc ou clang, yasm sur x86 et x86_64
- OpenSSL ou GnuTLS (doivent être activés explicitement lors de la compilation de FFmpeg)
- libx264 / libmp3lame / libfdk-aac si vous voulez utiliser l'encodage (doit être explicitement activé lors de la compilation de FFmpeg)
- Pour la lecture DASH native, FFmpeg doit être construit avec --enable-libxml2 (bien qu'il y ait des implications de sécurité).
- Pour un bon support nvidia sous Linux, assurez-vous que nv-codec-headers est installé et peut être trouvé par configurer.
- Libav fonctionne également, mais certaines fonctionnalités ne fonctionneront pas. (Voir la section ci-dessous.)

La plupart des bibliothèques ci-dessus sont disponibles dans des versions appropriées sur les distributions Linux normales. Pour faciliter la compilation du tout dernier git master de tout, vous pouvez utiliser le wrapper de construction disponible séparément ([mpv-build] [mpv-build]) qui compile d'abord les librairies FFmpeg et libass, puis compile le lecteur lié statiquement.  Si vous voulez construire un binaire Windows, vous devez soit utiliser MSYS2 et MinGW, soit effectuer une compilation croisée à partir de Linux avec MinGW. Pour en savoir plus [Windows compilation][windows_compilation].

## FFmpeg vs. Libav

En général, MPV devrait fonctionner avec la dernière version ainsi que la version git à la fois FFmpeg et Libav. Mais FFmpeg est préféré, et certaines fonctionnalités MPV fonctionnent avec FFmpeg seulement (formats de sous-titres en particulier).


## Compatibilité FFmpeg ABI 

mpv ne supporte pas la liaison avec les versions FFmpeg avec lesquelles il n'a pas été construit, même si la version liée est supposément compatible ABI avec la version, il était compilé contre. Attendez-vous à des dysfonctionnements, des plantages et des problèmes de sécurité si vous fais-le quand même.

La raison de ne pas soutenir cela est parce que cela crée beaucoup trop de complexité avec peu ou pas d'avantages, couplé avec API FFmpeg absurde et inutilisable artefacts.

Les nouvelles versions de MPV refuseront de démarrer si le temps d'exécution et de compilation FFmpeg incompatibilité des versions de bibliothèque.

## Cycle de sortie

Tous les deux mois, un instantané arbitraire git est fait, et est assigné un numéro de version 0.X.0. Aucun autre entretien n'est effectué.

L'objectif des versions est de rendre les distributions Linux heureuses. Distributions Linux sont également attendus à appliquer leurs propres correctifs en cas de bugs et de sécurité problèmes.

Les versions autres que la dernière version ne sont pas prises en charge et ne sont pas maintenues.

Voir le document [document pour la politique de confidentialité] [release-policy] pour plus d'informations.


## Rapports de bogues


S'il vous plaît utiliser le [questionnaire de tracking] [issue-tracker] fourni par GitHub pour nous envoyer un bug rapports ou demandes de fonctionnalités. Suivez les instructions du modèle ou le problème sera probablement ignoré ou fermé comme non valide.

Utilisation du bug tracker comme lieu pour des questions simples est bien, mais IRC est recommandé (voir [Contact] (# Contact) ci-dessous).



## Contribution


Veuillez lire [contrib.md] [contribute.md].

Pour de petits changements, vous pouvez simplement nous envoyer des demandes de pull via GitHub. Pour plus grand les changements viennent et nous parlent sur IRC avant que vous commenciez à travailler dessus. Ce sera rendre l'examen du code plus facile pour les deux parties plus tard.

Tu peux vérifier [le wiki](https://github.com/mpv-player/mpv/wiki/Stuff-to-do) ou le [traceur de problèmes GitHub ](https://github.com/mpv-player/mpv/issues?q=is%3Aopen+is%3Aissue+label%3A%22feature+request%22) pour des idées.

## Relation avec MPlayer et mplayer2

mpv est un dérivé de MPlayer. Beaucoup de choses ont changé, et en général, mpv devrait être considéré comme un programme complètement nouveau, plutôt que comme une substitution de MPlayer.

Pour plus de détails voir [FAQ entry](https://github.com/mpv-player/mpv/wiki/FAQ#How_is_mpv_related_to_MPlayer).

Si vous vous demandez ce qui est différent de mplayer2 et MPlayer, une liste de changements incomplète et largement non maintenue se trouve [ici][mplayer-changes].

## Licence

GPLv2 "ou plus " par défaut, LGPLv2.1 "or plus" avec `--enable-lgpl`.
Pour en savoir plus [détails.] (Https://github.com/mpv-player/mpv/blob/master/Copyright)


## Contact


La plupart des activités se déroulent sur le canal IRC et le tracker de problèmes de github.

 - **traceur de problèmes GitHub **: [traceur de problèmes ][issue-tracker] (signaler les bugs ici)
 - **Chaîne IRC de l'utilisateur**: `#mpv` sur `irc.freenode.net`
 - **Canal IRC du développeur**: `#mpv-devel` sur `irc.freenode.net`

Pour contacter l'équipe `mpv` en privé, écrivez` mpv-team @ googlegroups.com`. Utiliser seulement si la discrétion est requise.

[Sortie]: https://github.com/mpv-player/mpv/releases
[mpv-build]: https://github.com/mpv-player/mpv-build
[homebrew-mpv]: https://github.com/mpv-player/homebrew-mpv
[issue-tracker]:  https://github.com/mpv-player/mpv/issues
[ffmpeg_vs_libav]: https://github.com/mpv-player/mpv/wiki/FFmpeg-versus-Libav
[release-policy]: https://github.com/mpv-player/mpv/blob/master/DOCS/release-policy.md
[windows_compilation]: https://github.com/mpv-player/mpv/blob/master/DOCS/compile-windows.md
[mplayer-changes]: https://github.com/mpv-player/mpv/blob/master/DOCS/mplayer-changes.rst
[interface-changes]: https://github.com/mpv-player/mpv/blob/master/DOCS/interface-changes.rst
[api-changes]: https://github.com/mpv-player/mpv/blob/master/DOCS/client-api-changes.rst
[restore-old-bindings]: https://github.com/mpv-player/mpv/blob/master/etc/restore-old-bindings.conf
[contribute.md]: https://github.com/mpv-player/mpv/blob/master/DOCS/contribute.md
