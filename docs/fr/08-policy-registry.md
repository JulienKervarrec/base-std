# Chapitre 8 -- Le Policy Registry et les listes de conformite

Le Policy Registry est un singleton partage par tous les tokens B20 : au lieu que chaque emetteur code sa propre logique de conformite, il cree une policy dans ce registre central et la reference par un `uint64 policyId`. Deux types simples existent, `BLOCKLIST` (autorise par defaut, sauf figuration sur la liste) et `ALLOWLIST` (refuse par defaut, sauf figuration sur la liste), plus deux types composites qui combinent jusqu a quatre policies simples existantes : `UNION` (autorise si au moins une policy enfant autorise) et `INTERSECT` (autorise seulement si toutes les policies enfant autorisent). Un composite ne peut jamais referencer un autre composite -- seulement des policies simples.

Chaque policy a un administrateur, transferable en deux etapes : `stageUpdateAdmin` propose un nouvel administrateur, qui doit ensuite appeler lui-meme `finalizeUpdateAdmin` pour prendre effet. Un administrateur peut aussi renoncer definitivement via `renounceAdmin` : le set de membres se fige alors pour toujours, mais les requetes `isAuthorized` continuent de fonctionner normalement.

Point important pour l integrateur : `isAuthorized(policyId, account)` ne revert jamais, meme pour un `policyId` inexistant ou malforme -- il retombe silencieusement sur la semantique d un ensemble vide (`false` pour une allowlist, `true` pour une blocklist). C est aux appelants qui stockent des `policyId` de valider `policyExists` au moment de l ecriture.

Fichier central : `src/interfaces/IPolicyRegistry.sol`.

[Chapitre suivant : rattacher une policy a un token](09-policy-scopes.md)
