# Chapitre 1 -- Presentation de base-std et du standard B20

base-std est la bibliotheque Solidity officielle qui documente et teste les precompiles B20 de Base : des interfaces, des bibliotheques d aide et des mocks, pas une implementation de token a deployer soi-meme. B20 est le standard natif de Base pour emettre et gerer des actifs programmables on-chain -- une extension d ERC-20 pensee des le depart pour l emission d actifs du monde reel (RWA) et de stablecoins, avec la conformite comme brique de base plutot que comme ajout.

La difference cle avec un ERC-20 classique : un token B20 ne tourne pas comme bytecode EVM ordinaire. Il s execute comme precompile natif dans le client Base lui-meme. Cela veut dire que ce depot ne contient pas le token, seulement la surface Solidity (interfaces) qui permet d appeler ce token, plus les mocks utilises pour tester sans dependre d une vraie precompile.

Trois precompiles composent le systeme : la Factory (creation des tokens), le Policy Registry (listes de conformite partagees) et l Activation Registry (interrupteur de fonctionnalites controle par Base). Ce parcours suit ces trois pieces puis le token B20 lui-meme et ses deux variantes, Asset et Stablecoin.

Fichiers centraux : `src/StdPrecompiles.sol`, `src/interfaces/IB20.sol`, `src/interfaces/IB20Factory.sol`, `src/interfaces/IPolicyRegistry.sol`, `src/interfaces/IActivationRegistry.sol`, `src/interfaces/IB20Asset.sol`, `src/interfaces/IB20Stablecoin.sol`, `src/lib/B20Constants.sol`, `src/lib/B20FactoryLib.sol`, ainsi que `docs/overview.md` et `docs/architecture.md` pour le contexte.

Rien n a ete installe, compile ni execute pour ecrire ces chapitres.

[Chapitre suivant : precompiles contre contrats classiques](02-precompiles-vs-contrats.md)
