Parfait — on ajuste pour **RabbitMQ (pas Kafka)** et je te refais un kit **questions + réponses détaillées en français**, orientées explication + action + raisonnement.
Objectif : que tu puisses évaluer la compréhension réelle, pas les réponses par mots-clés.

Je garde le contexte :
👉 Java / Spring Boot
👉 traitements Cash / Settlement
👉 systèmes critiques
👉 batch + messaging
👉 Docker / K8s
👉 qualité & résilience

---

# 🐰 RabbitMQ — Questions + réponses détaillées

---

### ❓ Peux-tu m’expliquer comment fonctionne RabbitMQ dans une architecture applicative ?

✅ Bonne réponse attendue (explicative) :

« RabbitMQ est un broker de messages basé sur AMQP. Les producteurs envoient des messages vers un exchange, pas directement vers une queue. L’exchange route ensuite les messages vers une ou plusieurs queues selon des règles de routing (binding key, routing key, type d’exchange).
Les consommateurs lisent les messages depuis les queues.
Ça permet de découpler les services, d’absorber les pics de charge, et d’assurer une communication asynchrone fiable. »

Doit mentionner :

* exchange
* queue
* binding
* routing
* découplage

---

### ❓ Différence entre direct / topic / fanout exchange ?

✅ Attendu :

« Direct : routage exact sur la routing key.
Topic : routage par pattern (wildcards), utile pour catégoriser les événements.
Fanout : broadcast vers toutes les queues liées, sans regarder la routing key. »

Bon candidat → donne cas d’usage concrets.

---

### ❓ Comment tu gères les erreurs de consommation RabbitMQ ?

✅ Réponse attendue (actionnelle) :

« Je ne fais pas de retry infini côté consumer.
Je mets une stratégie :

* retry limité
* exponential backoff
* si échec → dead letter queue
  Je garde la trace métier de l’échec pour pouvoir rejouer plus tard.
  Et surtout je rends le traitement idempotent pour éviter les doublons lors des retries. »

Excellent candidat → mention DLQ + idempotence.

---

### ❓ Comment éviter de perdre des messages ?

✅ Attendu :

« Il faut :

* queues durables
* messages persistants
* acknowledgements manuels
* publisher confirm
* pas d’auto-ack
* supervision broker »

---

### ❓ Ack automatique ou manuel ?

✅ Attendu :

« Manuel dans les systèmes critiques.
Je fais l’ack seulement quand le traitement est terminé avec succès.
Sinon je nack / requeue selon la stratégie. »

---

### ❓ Comment tu gères la montée en charge côté consumer ?

✅ Attendu :

« Je scale horizontalement les consumers.
Je configure le prefetch pour contrôler le nombre de messages non ack par consumer.
J’évite qu’un consumer monopolise trop de messages. »

---

# 🌱 Spring Boot — Questions détaillées

---

### ❓ Comment tu structures un projet Spring Boot propre quand il grossit ?

✅ Attendu :

« Je sépare par domaine métier plutôt que par type technique.
Chaque module contient controller, service, repository, model.
La logique métier reste dans le service/domaine.
Je limite les dépendances croisées.
Je définis des contrats clairs entre modules. »

---

### ❓ Comment tu gères la configuration multi-environnement ?

✅ Attendu :

« Profiles Spring (dev, test, prod).
Externalisation config.
Variables d’environnement.
Secrets hors code (Vault / K8s secret).
Jamais de credentials dans le repo. »

---

### ❓ Comment tu sécurises une API Spring ?

✅ Attendu :

« Spring Security.
Auth forte (OAuth2/JWT).
Contrôle des rôles.
Validation des entrées.
Rate limiting si nécessaire.
Logs d’audit. »

---

# 🧵 Concurrence & batch

---

### ❓ Tu dois traiter des opérations cash en batch — comment tu garantis la cohérence ?

✅ Attendu :

« Je rends les traitements idempotents.
Je garde des identifiants métier uniques.
Je journalise chaque étape.
Je peux rejouer sans doublonner.
Je découpe en transactions courtes.
Je prévois reprise sur incident. »

---

### ❓ Un batch plante à 80% — que fais-tu ?

✅ Attendu :

« Je ne relance pas tout aveuglément.
Je regarde :

* périmètre traité
* données validées
* erreurs
  Je rejoue uniquement les éléments en échec.
  Je vérifie l’impact comptable.
  Je communique métier. »

---

# 🧪 Tests — réponses explicatives

---

### ❓ Comment tu testes un consumer RabbitMQ ?

✅ Attendu :

« Test unitaire de la logique métier.
Test d’intégration avec broker embarqué ou conteneur.
Vérification :

* ack
* retry
* DLQ
  Je simule erreurs de traitement. »

---

# 🐳 Kubernetes — réponses actionnelles

---

### ❓ Service Spring instable en K8s — démarche ?

✅ Attendu :

« Je regarde :

* logs pod
* metrics
* probes
* mémoire
* thread pool
* connexions DB
* config
  Je compare config locale vs cluster. »

---

# 💾 Transactions — Spring

---

### ❓ Comment fonctionne @Transactional concrètement ?

✅ Attendu :

« Spring crée un proxy autour du bean.
Le proxy ouvre/commit/rollback la transaction.
Ça ne marche pas si on appelle la méthode depuis la même classe.
Propagation configurable. »

Très bon candidat → parle proxy AOP.

---

# 🧠 Question conception critique — Cash

---

### ❓ Message Rabbit reçu 2 fois — impact cash — que fais-tu ?

✅ Attendu :

« Je protège par idempotence métier.
Clé unique transaction.
Vérification existant avant traitement.
Journalisation.
Rejeu contrôlé. »

---

# 🚩 Red flags candidat

Si tu entends :

❌ “Rabbit c’est comme Kafka pareil”
❌ “Auto ack c’est plus simple”
❌ “On retry en boucle”
❌ “On relance tout le batch”
❌ “@Transactional marche toujours”
❌ “On verra en prod”

→ niveau insuffisant pour système cash critique.

---

Si tu veux, je peux maintenant te produire :

✅ entretien complet 60 min chronométré
✅ grille de scoring
✅ version senior/lead
✅ test oral RabbitMQ + Spring
✅ scénarios incidents settlement réels
✅ questions pièges + réponses attendues
✅ évaluation production-readiness


Parfait — on va transformer ça en **kit complet d’entretien** :
👉 questions conversationnelles
👉 réponses attendues (guidelines, pas récitation)
👉 focus Java + Spring + architecture + cash/settlement
👉 format utilisable directement en entretien

Je te mets les **réponses “bon candidat”** — ce ne sont pas des réponses uniques, mais les éléments que tu dois entendre.

---

# 🎬 Introduction + réponses attendues

### ❓ Présente-moi un projet dont tu es fier

✅ Réponse attendue :

* contexte métier clair
* rôle précis (pas “on”, mais “je”)
* décisions techniques
* contraintes (perf, sécurité, volumétrie)
* impact mesurable

⚠️ Alerte si : réponse floue / uniquement fonctionnelle / aucune responsabilité technique.

---

# ☕ Java — Questions + réponses attendues

---

### ❓ Différences utiles entre Java 11 / 17 / 21 ?

✅ Attendu :

* records
* switch expressions
* text blocks
* pattern matching
* sealed classes
* amélioration GC / perf
* virtual threads (Java 21)

⚠️ Mauvais signe : “pas trop de différence”.

---

### ❓ Différence HashMap vs ConcurrentHashMap ?

✅ Attendu :

* HashMap non thread-safe
* ConcurrentHashMap segmenté / lock partiel
* pas de null keys/values
* meilleur en multi-thread

---

### ❓ equals vs hashCode — pourquoi important ?

✅ Attendu :

* contrat obligatoire
* utilisé par HashMap / HashSet
* cohérence clé → bucket
* sinon comportement incohérent

---

### ❓ Optional — bonne ou mauvaise idée ?

✅ Attendu :

* bon pour retour API
* pas pour champs d’entité JPA
* pas pour paramètres
* évite null explicite

---

### ❓ Stream — quand éviter ?

✅ Attendu :

* logique complexe → moins lisible
* besoin debug fin
* gros volumes → attention perf
* side effects → dangereux

---

# 🌱 Spring Boot — Questions + réponses

---

### ❓ Pourquoi Spring Boot plutôt que Spring classique ?

✅ Attendu :

* auto-configuration
* starter dependencies
* moins de config
* prod-ready features
* actuator

---

### ❓ @Component vs @Service vs @Repository ?

✅ Attendu :

* techniquement équivalent
* sémantique
* @Repository → exception translation
* lisibilité architecture

---

### ❓ Injection constructeur ou field ?

✅ Attendu :

* constructeur recommandé
* testable
* immutable
* dépendances explicites

⚠️ Mauvais signe : préfère field injection.

---

### ❓ @Transactional — pièges ?

✅ Attendu :

* proxy-based
* pas actif appel interne même classe
* propagation
* rollback runtime exceptions
* attention méthodes privées

Très bon candidat → mention AOP proxy.

---

### ❓ Gestion exceptions globale ?

✅ Attendu :

* @ControllerAdvice
* mapping HTTP codes
* error model standardisé
* logging central

---

### ❓ Différence @RestController vs @Controller ?

✅ Attendu :

* @RestController = @Controller + @ResponseBody
* JSON direct
* API REST

---

### ❓ Cycle de vie Bean Spring ?

✅ Attendu :

* instanciation
* injection
* post construct
* ready
* destroy

Bonus : mention @PostConstruct / @PreDestroy.

---

# 🧱 Architecture — Questions + réponses

---

### ❓ Architecture hexagonale — but ?

✅ Attendu :

* découpler métier / infra
* ports & adapters
* testabilité
* remplaçable DB / API

---

### ❓ Où va la logique métier ?

✅ Attendu :

* domaine / service métier
* jamais controller
* jamais repository

---

### ❓ DDD — value object vs entity ?

✅ Attendu :
Entity → identité
Value object → immuable, par valeur

---

# 🧪 Tests — Questions + réponses

---

### ❓ Test unitaire vs intégration ?

✅ Attendu :
Unitaire → isolé, mock
Intégration → contexte réel

---

### ❓ Quand mocker ?

✅ Attendu :

* dépendances externes
* I/O
* API
* DB

---

### ❓ TDD — avantage réel ?

✅ Attendu :

* design émergent
* régression
* contrat clair
* refactor safe

---

# ⚡ Kafka — Questions + réponses

---

### ❓ Gérer doublons ?

✅ Attendu :

* idempotence
* clé métier
* déduplication
* stockage offset

---

### ❓ Rejouer messages ?

✅ Attendu :

* reset offset
* topic replay
* DLQ pattern

---

# 🐳 Kubernetes — Questions + réponses

---

### ❓ Pod ne démarre pas ?

✅ Attendu :

* logs pod
* describe
* events
* config map / secret
* probe failures

---

### ❓ Readiness vs liveness ?

✅ Attendu :

* readiness = prêt trafic
* liveness = vivant ou restart

---

# 💾 Data — Questions + réponses

---

### ❓ Transaction longue — risque ?

✅ Attendu :

* locks
* contention
* timeout
* rollback coûteux

---

### ❓ Optimiser requête lente ?

✅ Attendu :

* index
* plan explain
* cardinalité
* pagination

---

# 🏦 Cash / Settlement — mise en situation

---

### ❓ Batch cash échoue partiellement — que fais-tu ?

✅ Attendu :

* stop propagation erreur
* identifier périmètre
* idempotence
* replay ciblé
* audit trail
* communication métier

Excellent candidat → parle de **traçabilité + rejouabilité**.

---

# 🔍 Observabilité

---

### ❓ Service lent prod ?

✅ Attendu :

* métriques
* logs corrélés
* thread dump
* pool saturation
* DB latency

---

# 🤝 Collaboration

---

### ❓ Code review — tu regardes quoi ?

✅ Attendu :

* logique
* lisibilité
* complexité
* test couverture
* sécurité

---

# 🎯 Question bonus senior Java

---

### ❓ Pourquoi éviter @Autowired sur champ ?

✅ Attendu :

* testabilité
* immutabilité
* dépendances cachées
* anti-pattern DI

---

Si tu veux, je peux maintenant te générer :

✅ grille d’évaluation chiffrée
✅ version entretien senior
✅ version entretien tech lead
✅ pièges fréquents candidats Spring
✅ réponses “red flag”
✅ test oral 20 minutes Java/Spring
✅ scénario incident prod réaliste settlement

Tu veux un niveau **développeur**, **senior**, ou **lead** pour ce poste ?

Dis-moi le niveau du poste — dev / senior / lead — et je te calibre ça finement.
