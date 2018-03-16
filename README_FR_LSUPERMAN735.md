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

Voir le document [release policy document] [release-policy] pour plus d'informations.


## Rapports de bogues


S'il vous plaît utiliser le [question tracker] [issue-tracker] fourni par GitHub pour nous envoyer un bug rapports ou demandes de fonctionnalités. Suivez les instructions du modèle ou le problème sera probablement ignoré ou fermé comme non valide.

Utilisation du bug tracker comme lieu pour des questions simples est bien, mais IRC est recommandé (voir [Contact] (# Contact) ci-dessous).



## Contribution


Veuillez lire [contrib.md] [contribute.md].

Pour de petits changements, vous pouvez simplement nous envoyer des demandes de pull via GitHub. Pour plus grand les changements viennent et nous parlent sur IRC avant que vous commenciez à travailler dessus. Ce sera rendre l'examen du code plus facile pour les deux parties plus tard.

Tu peux vérifier [the wiki](https://github.com/mpv-player/mpv/wiki/Stuff-to-do) ou l'[issue tracker](https://github.com/mpv-player/mpv/issues?q=is%3Aopen+is%3Aissue+label%3A%22feature+request%22) pour des idées sur ce que vous pourriez contribuer.

## Relation avec MPlayer et mplayer2

mpv est un dérivé de MPlayer. Beaucoup de choses ont changé, et en général, mpv devrait être considéré comme un programme complètement nouveau, plutôt que comme un substitut de remplacement de MPlayer.

Pour plus de détails voir [FAQ entry](https://github.com/mpv-player/mpv/wiki/FAQ#How_is_mpv_related_to_MPlayer).

Si vous vous demandez ce qui est différent de mplayer2 et MPlayer, une liste de changements incomplète et largement non maintenue se trouve [here][mplayer-changes].

## Licence

GPLv2 "or later" par défaut, LGPLv2.1 "or later" avec `--enable-lgpl`.
Pour en savoir plus [détails.] (Https://github.com/mpv-player/mpv/blob/master/Copyright)


## Contact


La plupart des activités se déroulent sur le canal IRC et le tracker de problèmes de github.

 - **GitHub issue tracker**: [issue tracker][issue-tracker] (signaler les bugs ici)
 - **User IRC Channel**: `#mpv` on `irc.freenode.net`
 - **Developer IRC Channel**: `#mpv-devel` on `irc.freenode.net`

Pour contacter l'équipe `mpv` en privé, écrivez` mpv-team @ googlegroups.com`. Utiliser seulement si la discrétion est requise.

[releases]: https://github.com/mpv-player/mpv/releases
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
