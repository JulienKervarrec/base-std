# Chapitre 3 -- IB20Factory et createB20

Un token B20 ne peut naitre que d un seul point d entree : `createB20(variant, salt, params, initCalls)` sur la Factory, precompile singleton unique. L appelant choisit une variante (`ASSET` ou `STABLECOIN`), un salt arbitraire, des `params` encodes en ABI contenant le nom, le symbole, l administrateur initial et les champs propres a la variante, et une liste optionnelle `initCalls` de rappels executes juste apres la creation.

L adresse du nouveau token n est pas aleatoire : elle est deterministe, derivee de `(variant, sender, salt)` via `getB20Address`, ce qui permet de la predire avant meme d appeler `createB20`. Si un token existe deja a cette adresse, l appel echoue avec `TokenAlreadyExists` -- il faut changer de salt.

`B20FactoryLib` fournit des encodeurs purs pour construire ces `params` et ces `initCalls` sans jamais toucher a la precompile elle-meme : `encodeAssetCreateParams`, `encodeStablecoinCreateParams`, et des constructeurs de lots comme `buildRoleGrants` qui transforment un struct `B20RoleHolders` en une liste d appels `grantRole` prets a etre passes en `initCalls`, en sautant automatiquement les adresses nulles.

Une fois `createB20` termine, la Factory n a plus aucun acces persistant au nouveau token : la creation est un evenement en une seule transaction, sans lien de dependance apres coup.

Fichiers centraux : `src/interfaces/IB20Factory.sol`, `src/lib/B20FactoryLib.sol`.

[Chapitre suivant : la fenetre de bootstrap](04-fenetre-bootstrap-initcalls.md)
