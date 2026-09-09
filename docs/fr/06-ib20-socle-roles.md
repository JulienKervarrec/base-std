# Chapitre 6 -- IB20, le socle ERC-20 et les roles

`IB20` est la surface que tout token B20 implemente, quelle que soit sa variante. Elle reprend l ERC-20 standard (`transfer`, `transferFrom`, `approve`, `balanceOf`, `allowance`) et l enrichit de variantes memo (`transferWithMemo`, etc.) qui emettent un evenement `Memo` juste apres le `Transfer` standard, utile pour attacher une reference off-chain a une operation on-chain sans changer sa semantique.

Le controle d acces suit le modele `AccessControl` d OpenZeppelin, mais integre nativement a la precompile plutot que comme bibliotheque importee : un unique `DEFAULT_ADMIN_ROLE` accorde et revoque les roles operationnels (`MINT_ROLE`, `BURN_ROLE`, `PAUSE_ROLE`, `UNPAUSE_ROLE`, `METADATA_ROLE`, `SEIZE_ROLE`, `BURN_BLOCKED_ROLE`). Chaque fonction privilegiee verifie d abord le role, puis le vecteur de pause correspondant. Un detail volontaire : `renounceLastAdmin` permet de faire basculer un token dans un etat sans administrateur, de facon permanente et irreversible -- utile pour un token dont on veut prouver qu il ne sera plus jamais administre.

Cote metadonnees, `updateName` et `updateSymbol` sont reserves a `METADATA_ROLE` et emettent respectivement `NameUpdated` et `SymbolUpdated` ; changer le nom declenche en plus `EIP712DomainChanged`, car le nom entre dans le calcul du domaine EIP-712 utilise par `permit` (EIP-2612), egalement supporte nativement.

Fichier central : `src/interfaces/IB20.sol`.

[Chapitre suivant : les quatre vecteurs de pause et la saisie](07-pause-vectors.md)
