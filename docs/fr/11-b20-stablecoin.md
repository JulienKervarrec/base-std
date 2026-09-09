# Chapitre 11 -- IB20Stablecoin, la variante simplifiee

Face a `IB20Asset` et ses options d emission generaliste, `IB20Stablecoin` prend le chemin inverse : une interface volontairement minimale, qui n ajoute qu une seule fonction a `IB20`, `currency()`, retournant un code devise auto-declare (`"USD"`, `"EUR"`, `"JPY"`...). Le nombre de decimales n est plus un choix a la creation : il est fixe a 6 pour toute la variante Stablecoin, la ou `IB20Asset` autorise n importe quelle valeur entre 6 et 18 via `B20Constants.MIN_ASSET_DECIMALS` et `MAX_ASSET_DECIMALS`.

Cote creation, `B20StablecoinCreateParams` ne demande que le nom, le symbole, l administrateur initial et la devise -- pas de multiplicateur, pas d annonces, pas de batchMint. Tout le reste (roles, policies, pause, permit EIP-2612) vient directement du socle `IB20` commun aux deux variantes, sans duplication de logique.

Le code devise est valide par la Factory a la creation : une chaine non vide doit contenir uniquement des caracteres ASCII majuscules `A` a `Z`, sous peine de `InvalidCurrency`. La Factory encode ensuite ce code dans `variantEventParams` du `B20Created` emis a la creation, via `B20StablecoinEventParams`, pour que les indexeurs puissent retrouver la devise d un stablecoin sans avoir a interroger le contrat lui-meme.

Fichier central : `src/interfaces/IB20Stablecoin.sol`, `src/interfaces/IB20Factory.sol`.

[Chapitre suivant : l Activation Registry et l evolution du protocole](12-activation-registry-hardforks.md)
