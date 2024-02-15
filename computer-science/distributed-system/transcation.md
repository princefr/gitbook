# Transaction

Il existe de nombreux algorithmes pour implémenter les transactions distribuées, tels que le 2PC et le 3PC basés sur le protocole XA, l'implémentation TCC des transactions distribuées basée sur les affaires, ainsi que les transactions distribuées basées sur les messages.

**Une transaction distribuée** est la combinaison de plusieurs transactions individuelles.

## 2PC

Le protocole de validation à deux phases (The two-phase commit protocol, 2PC) est l'une des façons d'implémenter le protocole XA, il garantit une forte cohérence. Il y a deux rôles : le **gestionnaire de transactions** et le **gestionnaire de ressources locales**.

Le gestionnaire de transactions agit en tant que coordinateur et est responsable de la soumission et du rejet auprès de chaque gestionnaire de ressources local.

Le 2PC comporte deux phases : le vote (voting) et la soumission (commit).

1. Le gestionnaire de transactions (coordinateur) envoie une demande CanCommit au gestionnaire de ressources local (participant).
2. Le participant exécute l'opération de transaction, enregistre le journal mais ne le soumet pas. Selon le succès de l'exécution, il renvoie un message Yes ou No.
3. Le coordinateur envoie une demande DoCommit ou DoAbort en fonction de Yes ou No.
4. Le participant soumet ou annule la transaction en fonction de DoCommit ou DoAbort, et renvoie un message HaveCommitted.
5. Le coordinateur reçoit le message HaveCommitted, ce qui signifie que la transaction est terminée.

Inconvénients :

- Problème de blocage synchrone : lorsque le gestionnaire de ressources local occupe une ressource critique, les autres gestionnaires de ressources qui tentent d'accéder à la même ressource se retrouvent bloqués.
- Problème de panne unique : si un gestionnaire de transactions tombe en panne, tout le système devient indisponible. En particulier lors de la phase de soumission, le gestionnaire de ressources verrouille constamment les ressources de transaction.
- Problème d'incohérence des données : si, en raison d'une anomalie réseau, seuls certains des gestionnaires de ressources reçoivent la demande DoCommit, les données de tout le système seront incohérentes.

## 3PC

Le protocole de validation à trois phases (Three-phase commit protocol, 3PC) est également une façon d'implémenter le protocole XA, il garantit une forte cohérence. Il améliore le 2PC en introduisant un mécanisme de temporisation et une phase de préparation.

- Les participants et le coordinateur ont tous des mécanismes de temporisation et, après expiration, choisissent de soumettre ou d'annuler la transaction en fonction de l'état actuel.
- Trois phases : CanCommit, PreCommit, DoCommit.

1. Le coordinateur envoie une demande CanCommit.
2. Les participants envoient Yes ou No en fonction de l'état.
3. Si le coordinateur reçoit un No, il envoie un Abort. S'il reçoit que des Yes, il envoie une demande PreCommit.
4. Si un participant reçoit un Abort ou si aucun message n'est reçu après expiration, il annule la transaction. S'il reçoit une demande PreCommit, il exécute la transaction, enregistre les journaux Undo et Redo, et envoie un ACK en réponse.
5. Si le coordinateur reçoit un ACK, il envoie une demande DoCommit.

Rarement utilisé car il nécessite plus de négociations de messages que le 2PC, ce qui augmente la charge du système et le délai de réponse.

## TCC

Try-Confirm-Cancel, cohérence finale. Pour chaque opération, une opération de confirmation et d'annulation correspondante doit être enregistrée. Les opérations de confirmation et d'annulation doivent être idempotentes. C'est un protocole au niveau métier et ne dépend pas des transactions de base de données, les 3 opérations doivent être mises en œuvre dans le code métier.

Avantages :

- Ne dépend pas des transactions de base de données.

Inconvénients :

- Implémentation complexe.

## Basé sur les messages

Cohérence finale. Le 2PC et le 3PC nécessitent tous deux de verrouiller les ressources, ce qui peut être résolu par des messages.