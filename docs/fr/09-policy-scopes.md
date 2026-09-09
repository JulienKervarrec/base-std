# Chapitre 9 -- Rattacher une policy a un token

Une policy ne fait rien tant qu elle n est pas rattachee a un emplacement precis sur un token. C est le role de `updatePolicy(policyScope, newPolicyId)`, reserve a `DEFAULT_ADMIN_ROLE` : il branche un `policyId` du registre sur un `policyScope` du token, un peu comme on brancherait un hook sur une fonction specifique.

`IB20` expose six scopes : `TRANSFER_SENDER_POLICY` et `TRANSFER_RECEIVER_POLICY`, verifies sur chaque transfert respectivement contre l expediteur et le destinataire ; `TRANSFER_EXECUTOR_POLICY`, verifie uniquement sur `transferFrom` quand l appelant differe du proprietaire des fonds ; `MINT_RECEIVER_POLICY`, verifie sur chaque mint ; et les deux scopes de saisie du chapitre precedent. Un slot jamais configure repond `0`, le sentinel integre "toujours autorise" -- donc un token fraichement cree sans aucune policy attachee se comporte comme un ERC-20 ordinaire.

Quand une operation touche un scope configure, le token interroge le registre via `isAuthorized(policyId, account)` ; en cas de refus, l appel revert avec `PolicyForbids(policyScope, policyId)`, une erreur qui identifie precisement quel slot a bloque l operation. C est le meme mecanisme qui explique pourquoi la fenetre de bootstrap du chapitre 4 doit imperativement continuer a verifier `MINT_RECEIVER_POLICY` : sans cette exception, la protection de conformite deviendrait contournable a la creation.

Fichier central : `src/interfaces/IB20.sol`, section POLICY ; `docs/overview.md`.

[Chapitre suivant : IB20Asset et le multiplicateur d affichage](10-b20-asset-multiplier.md)
