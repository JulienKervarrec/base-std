# Chapitre 7 -- Les quatre vecteurs de pause et la saisie

La pause d un token B20 n est pas globale : elle se decoupe en quatre vecteurs independants, `TRANSFER`, `MINT`, `BURN` et `SEIZE`, chacun controlable separement. Un emetteur peut ainsi geler uniquement les nouvelles emissions (`MINT`) pendant qu un audit se termine, sans bloquer les transferts existants. `pause` exige `PAUSE_ROLE`, `unpause` exige `UNPAUSE_ROLE` -- deux roles distincts, pour qu un compte puisse avoir le pouvoir de geler sans avoir celui de degeler.

Le mecanisme le plus specifique a la conformite reste `seizeWithMemo` : une operation d administrateur qui reassigne de force un solde d un compte `from` vers un compte `to`, gatee par `SEIZE_ROLE`. Elle contourne volontairement l allowance et les policies de transfert habituelles, mais reste soumise a deux verifications de membership inversees par rapport au transfert normal : `from` doit etre non autorise sous `SEIZE_EXEMPT_POLICY` (c est l inverse d une policy normale -- etre "autorise" sous ce slot rend un compte protege contre la saisie, pas eligible a une operation), et `to` doit etre autorise sous `SEIZE_RECEIVER_POLICY`. Trois evenements sont emis dans l ordre : `Transfer`, `Memo`, puis `Seized`, qui trace explicitement l appelant, la source et la destination.

Une methode `burnBlocked`, marquee deprecated, subsiste pour compatibilite : elle brule le solde d un compte deja bloque sous `TRANSFER_SENDER_POLICY`, sans consommer d allowance. La documentation recommande desormais `seizeWithMemo` suivi d un `burn` classique.

Fichier central : `src/interfaces/IB20.sol`, sections PAUSE et MINT / BURN.

[Chapitre suivant : le Policy Registry et les listes de conformite](08-policy-registry.md)
