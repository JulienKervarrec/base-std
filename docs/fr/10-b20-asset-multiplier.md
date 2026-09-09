# Chapitre 10 -- IB20Asset et le multiplicateur d affichage

`IB20Asset` etend `IB20` pour l emission generaliste d actifs, RWA compris, avec trois ajouts majeurs. Le premier est le multiplicateur UI, standardise par ERC-8056 (`IScaledUIAmount` et ses extensions) : chaque token garde ses soldes bruts inchanges, mais expose un `uiMultiplier` (precision WAD, `1e18` = 1.0) qui redimensionne uniquement la vue affichee, via `toUIAmount` / `fromUIAmount`. Le principe rappelle wstETH qui enveloppe stETH -- sauf qu ici, aucun wrapping n est necessaire : c est le token lui-meme qui expose sa vue mise a l echelle. `updateUIMultiplier` planifie un changement a une date future (usage vise : splits d actions, reinvestissement de dividendes) ; un seul changement peut etre en attente a la fois, annulable via `cancelUIMultiplierUpdate`.

Le second ajout est `announce` : une fonction qui poste un evenement `Announcement`/`EndAnnouncement` autour d une serie d appels internes executes par self-delegatecall, ideale pour grouper une annonce corporate (par exemple un split) avec l operation qui l accompagne, de facon atomique et tracable.

Le troisieme est `batchMint`, qui emet vers plusieurs destinataires en un seul appel tout-ou-rien, et `extraMetadata`, un magasin cle/valeur libre pour des champs comme `"category"` ou `"region"` que l emetteur definit lui-meme.

Fichier central : `src/interfaces/IB20Asset.sol`, `src/interfaces/IERC8056.sol`.

[Chapitre suivant : IB20Stablecoin, la variante simplifiee](11-b20-stablecoin.md)
