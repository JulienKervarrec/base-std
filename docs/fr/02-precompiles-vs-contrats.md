# Chapitre 2 -- Precompiles contre contrats classiques

Un contrat Solidity ordinaire est du bytecode EVM stocke a une adresse ; l interpreteur EVM le lit et l execute instruction par instruction. Une precompile est different : c est du code compile dans le client lui-meme (ici, le node Base, ecrit en Rust). Sur chaque `CALL` ou `STATICCALL`, le node verifie d abord si l adresse cible figure dans un registre de precompiles. Si oui, le code natif tourne directement, sans jamais charger ni interpreter de bytecode. Sinon, le chemin EVM classique s applique.

Les precompiles historiques d Ethereum (`ecrecover`, `sha256`, `modexp`, etc.) suivent deja ce mecanisme -- elles sont pures et sans etat. B20 est different : c est la premiere precompile a etat persistant de Base. Le Factory, le Policy Registry, l Activation Registry et chaque token B20 lisent et ecrivent dans le meme modele d etat EVM que n importe quel contrat classique, avec le meme layout de stockage ERC-7201 (une racine de namespace, des champs a des offsets fixes, des mappings hashes comme le ferait Solidity). La seule chose qui change, c est que le code qui lit et ecrit cet etat n est pas interprete depuis du bytecode -- il est natif.

Deux categories de precompiles B20 existent : les singletons (Factory, Policy Registry, Activation Registry), a adresse fixe et enregistres dans la table statique du node, et les tokens B20 eux-memes, crees dynamiquement a l execution et reconnus par un mecanisme different (chapitre 5).

Fichier central : `docs/architecture.md` section 1.

[Chapitre suivant : IB20Factory et createB20](03-factory-createb20.md)
