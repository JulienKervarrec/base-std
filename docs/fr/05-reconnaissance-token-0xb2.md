# Chapitre 5 -- Comment un token B20 est reconnu

Les trois precompiles singletons (Factory, Policy Registry, Activation Registry) vivent a des adresses fixes, enregistrees une fois pour toutes dans la table statique du node : `0xB20f...00` pour la Factory, `0x8453...02` pour le Policy Registry, `0x8453...01` pour l Activation Registry -- voir `StdPrecompiles.sol`. Mais un token B20 est cree dynamiquement, a l execution : il ne peut pas figurer dans cette table fixee a l avance.

Le node le reconnait donc autrement, par lecture directe de l adresse et du bytecode du compte. L adresse suit un format precis : l octet `[0]` vaut `0xB2`, les octets `[1:9]` sont a zero, l octet `[10]` encode la variante (`0x00` pour Asset, `0x01` pour Stablecoin), et le reste derive de `keccak256(sender, salt)`. `isB20(address)` lit ce prefixe directement, sans consulter le moindre registre -- et peut donc repondre vrai pour une adresse que la Factory n a pas encore effectivement creee.

La verification forte est `isB20Initialized` : elle exige en plus que le compte porte le bytecode `0xef`, un octet unique plante par la Factory a la creation. Cet octet est le prefixe reserve par EIP-3541 -- aucun `CREATE` ou `CREATE2` ordinaire ne peut le produire. Avant `createB20`, l adresse predite a deja le bon prefixe mais pas ce stub : l appeler ne fait rien. C est l apparition du stub `0xef` qui bascule l adresse de "ressemble a un B20" vers "est un B20 vivant", et qui declenche le routage vers la logique native de la bonne variante.

Fichier central : `docs/architecture.md` section 2.

[Chapitre suivant : IB20, le socle ERC-20 et les roles](06-ib20-socle-roles.md)
