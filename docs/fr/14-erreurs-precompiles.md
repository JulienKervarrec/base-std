## Erreurs des precompiles B20

Les erreurs des precompiles B20 doivent etre lues comme des refus de precondition, pas comme de simples echecs de transaction. Une integration doit distinguer un token non reconnu, une policy absente, un appel hors fenetre de bootstrap et une operation suspendue. Les evenements ne suffisent pas toujours a reconstruire la cause : le code appelant doit conserver le contexte et verifier les roles. Cette fiche relie les categories d erreurs aux controles a effectuer avant envoi. Lecture statique uniquement, sans execution de test.
