# Chapitre 12 -- L Activation Registry et l evolution du protocole

L Activation Registry est le plus simple des trois singletons, et le plus politique : un unique administrateur d activation peut activer ou desactiver des fonctionnalites identifiees par un `bytes32` opaque, via `activate` et `deactivate`. `isActivated` ne revert jamais ; `checkActivated` est un simple raccourci qui revert avec `FeatureNotActivated` si besoin, pour eviter que chaque appelant ne redefinisse cette meme erreur. C est ce registre que `createB20` interroge avant d autoriser la creation d une nouvelle variante -- desactiver une variante bloque les nouvelles creations, sans jamais toucher aux tokens deja emis.

Ce controle d activation s inscrit dans un mecanisme plus large : B20 evolue par hardforks, les memes moments de changement de consensus que le reste de Base. Un hardfork peut introduire une toute nouvelle precompile (l Activation Registry lui-meme n existe qu a partir du hardfork Beryl) ou publier une nouvelle version de la logique d une precompile existante. La contrainte absolue est que chaque version deja livree reste figee pour toujours : un bloc produit sous la logique Beryl doit continuer, indefiniment, a s executer avec cette meme logique Beryl, meme apres qu un hardfork Cobalt a introduit une nouvelle version. C est ce qui permet a un node qui resynchronise depuis le bloc zero d aboutir exactement au meme etat qu un node reste en ligne en continu -- aucune reecriture retroactive n est possible.

Fichier central : `src/interfaces/IActivationRegistry.sol`, `docs/architecture.md` section 3.

[Chapitre suivant : limites et perimetre de ce parcours](13-limites-perimetre.md)
