# Chapitre 4 -- La fenetre de bootstrap et les initCalls

Entre la creation d un token et le retour de `createB20`, une fenetre courte et unique s ouvre : celle des `initCalls`. Chaque entree de ce tableau est executee sur le token fraichement cree, dans l ordre, avant que quiconque n en ait le controle normal.

Pendant cette fenetre, la Factory beneficie d un contournement partiel des controles habituels : les verifications de role et les policies cote transfert (`TRANSFER_SENDER_POLICY`, `TRANSFER_RECEIVER_POLICY`, `TRANSFER_EXECUTOR_POLICY`) sont desactivees pour les appels provenant de la Factory. C est ce qui permet a un seul `createB20` d enchainer `grantRole`, `updatePolicy`, un mint initial et un transfert de bootstrap, sans que la Factory elle-meme ait besoin de detenir le moindre role.

Ce contournement n est pas total, et c est la partie a retenir. Trois garde-fous restent actifs meme pendant la fenetre : `MINT_RECEIVER_POLICY` est toujours evaluee, pour qu un mint initial ne puisse jamais atterrir sur un compte refuse par la politique de conformite ; la pause n est jamais court-circuitee, elle demarre a l etat "rien de pause" donc un token qui doit naitre deja en pause doit placer son `pause(...)` en dernier dans la liste des `initCalls` ; et les invariants du token (comptabilite des soldes, plafond de supply) s appliquent sans exception.

Des que `createB20` retourne, la fenetre se referme definitivement.

Fichier central : `src/interfaces/IB20Factory.sol`, section `createB20`.

[Chapitre suivant : comment un token B20 est reconnu](05-reconnaissance-token-0xb2.md)
