<template>
  <div id="wrapper">
    <!-- Barre de progression -->
    <div class="progress-container">
      <div class="progress-bar" :style="{ width: progressPercentage + '%' }"></div>
    </div>

    <!-- Contenu principal -->
    <div id="layout">
      <!-- Barre latérale gauche avec les étapes -->
      <nav id="breadcrumb">
        <div id="logo-container">
          <img src="@/assets/logo-lemonde.png" alt="Le Monde" id="logo" />
        </div>
        <ul>
          <li v-for="(step, index) in steps" :key="index" 
              :class="{ active: index <= currentStepIndex }"
              @click="goToStep(index)">
            <span>{{ index + 1 }}. {{ step.title }}</span>
          </li>
        </ul>
      </nav>

      <!-- Contenu principal de chaque étape -->
      <main id="main-content">
        <h1>Onboarding Abonnement Le Monde</h1>

        <!-- Contenu de l'étape -->
        <OnboardingStep 
          v-if="currentStepIndex < steps.length" 
          :step="steps[currentStepIndex]" 
          :stepIndex="currentStepIndex"
          @completed="nextStep"
          @back="previousStep" />

        <!-- Message de fin -->
        <div v-else class="completion-container">
          <h2>🎉 Onboarding terminé ! 🎉</h2>
          <p>Félicitations, tu a terminé le processus d'onboarding pour l'abonnement Le Monde ! Tu es maintenant prêt à plonger dans l'aventure.</p>

          <p>Il est maintenant temps de faire votre première <strong>Pull Request</strong> 🚀. <br><br>N'oubliez pas : même les plus grands projets ont commencé par une simple PR !</p>

          <p>📅 <b>À noter :</b> Dans un mois, tu devras soumettre un <strong>rapport d'étonnement</strong> aux CTO. <br><br><em>Un rapport d'étonnement est un document où tu partages tes premières impressions, découvertes et suggestions suite à ton intégration.<br>C'est l'occasion idéale pour exprimer ce qui t'a surpris, ce que tu as apprécié et les idées que tu pourrais avoir pour améliorer notre processus ou nos outils.</em></p>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import OnboardingStep from './components/OnboardingStep.vue';

export default {
  name: 'App',
  components: {
    OnboardingStep
  },
  data() {
    return {
      steps: [
      {
        title: "🏠 Bienvenue à la Direction Technique",
        description: `
          Félicitations pour avoir rejoint la direction technique au sein du pôle abonnement pour Le Monde. 🎉<br><br>
          Nous travaillons principalement sur le projet <strong>phalcon-user</strong>, qui est le site utilisateur regroupant :
          <ul>
            <li>La création de compte</li>
            <li>La prise d'abonnement</li>
            <li>La gestion de l'abonnement</li>
            <li>L'inscription aux newsletters</li>
          </ul>
          Ce site va te guider pas à pas sur la prise en main du projet, te présenter nos outils et bonnes pratiques.<br><br>
          Prêt à commencer ? 🚀
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "📅 Focus sur Semana",
        description: `
          Dans cette étape, nous allons faire un <b>focus sur Semana</b>.<br><br>
          <b>Qu'est-ce que Semana ?</b><br><br>
          <b>Semana</b> est notre système de réservation d'espace de travail, particulièrement important car environ <b>90% de la Direction technique</b> sont des membres externes. Ces derniers doivent se rendre au bureau <b>2 jours par semaine</b>.<br><br>
          <b>Jours Obligatoires :</b><br>
          - <b>Jeudi</b> : Ce jour est obligatoire pour tous les membres externes.<br>
          - <b>Autre Jour</b> : Le deuxième jour peut être choisi en accord avec votre équipe.<br><br>
          <b>Flex Office :</b><br><br>
          Nous fonctionnons en <b>flex office</b>, ce qui signifie qu'il n'y a pas de bureau attribué de manière fixe à chaque employé. Tu dois donc réserver ta place avant de venir au bureau.<br><br>
          <b>Comment Réserver avec Semana :</b><br><br>
          - Accédez à la plateforme Semana via <a href="https://app.semana.io/le-monde/floorplan">Semana</a>.<br>
          - Sélectionnez la date à laquelle tu souhaite venir au bureau.<br>
          - Choisissez une place disponible parmi les options proposées.<br>
          - Confirmez votre réservation.<br><br>
          <b>Important :</b><br>
          - <b>Réservation Obligatoire</b> : Tu dois réserver votre place sur Semana avant de venir au bureau pour assurer la disponibilité de l'espace.<br>
          - <b>Coordination avec l'Équipe</b> : Assures-toi de coordonner le choix de votre deuxième jour avec les membres de votre équipe pour optimiser la collaboration et la présence au bureau.<br><br>
          <b>Avantages de Semana :</b><br><br>
          - <b>Optimisation de l'Espace</b> : Permet une utilisation efficace des espaces de travail disponibles.<br>
          - <b>Flexibilité</b> : Offre la possibilité de choisir les jours qui conviennent le mieux à votre emploi du temps et à celui de votre équipe.<br>
          - <b>Coordination Facilitée</b> : Simplifie la planification des présences au bureau, évitant ainsi les conflits de réservation.<br><br>
          Assures-toi de bien comprendre le fonctionnement de <b>Semana</b> et de l'utiliser régulièrement pour garantir une expérience de travail fluide et organisée au bureau.
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "🔒 Utilisation de Twingate pour l'Accès aux Ressources Internes",
        description: `
          Dans cette étape, nous allons aborder l'accès aux sites internes et aux partenaires via notre système de réseau.<br><br>
          Pour accéder aux ressources internes, il est nécessaire d'utiliser <b>Twingate</b>.<br><br>
          <b>Qu'est-ce que Twingate ?</b><br><br>
          Twingate n'est pas un VPN traditionnel. C'est une solution de réseau d'accès sécurisé moderne qui fournit un accès aux ressources internes sans les inconvénients des VPN classiques.<br><br>
          Contrairement aux VPN traditionnels qui créent un tunnel entre votre appareil et l'ensemble du réseau de l'entreprise, Twingate établit des connexions sécurisées uniquement vers les ressources spécifiques dont tu a besoin. Cela améliore la sécurité et les performances du réseau.<br><br>
          <b>Gestion des Droits d'Accès :</b><br><br>
          Twingate s'appuie sur les groupes Google Workspace pour attribuer les droits d'accès appropriés à chaque utilisateur. En fonction de votre rôle et de votre équipe, tu sera ajouté aux groupes correspondants, ce qui te donnera accès aux ressources nécessaires pour votre travail.<br><br>
          <b>Comment Utiliser Twingate :</b><br><br>
          - Installez le client Twingate sur votre appareil (disponible pour Windows, macOS, Linux, iOS et Android).<br>
          - Connectes-toi avec vos identifiants professionnels.<br>
          - Une fois connecté, tu pourra accéder aux sites internes et aux services des partenaires comme si tu est sur le réseau interne de l'entreprise.<br><br>
          Assures-toi de toujours être connecté à Twingate lorsque tu travaille sur des ressources internes ou que tu a besoin d'accéder aux services des partenaires.
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "💬 Prise en Main de Slack",
        description: `
          Dans cette étape, nous allons aborder l'utilisation de <b>Slack</b> au sein de l'entreprise.<br><br>

          <b>Canaux Importants :</b><br><br>

          - <b>#general</b> : Regroupe l'ensemble des salariés du groupe. <b>Évitez d'écrire des messages</b> dans ce canal pour ne pas surcharger tous les employés.<br>
          - <b>#tech-general</b> : Réunit toutes les équipes de la direction technique. Utilisez ce canal pour discuter avec les autres développeurs de sujets techniques généraux.<br>
          - <b>#tech-abo-{titre}</b> : Un canal par marque (par exemple, <b>#tech-abo-lm</b>) où tu trouvera les membres de votre équipe.<br>
          - <b>#abo-release</b> : Inclus dans le Continuous Deployment (CD), ce canal envoie les changelogs à chaque fusion sur la branche <b>master</b>.<br>
          - <b>#tech-abo-latency</b> : Tu y trouvera les logs des routes lentes, utile pour le suivi des performances.<br>
          - <b>#tech-err-phalcon-user-{env}</b> : Canal d'erreurs où tu retrouveras les sorties standard (<b>stdout</b>) de l'application pour <b>Le Monde</b> uniquement.<br>
          - <b>#tech-err-phalcon-user-ediis-prd</b> : Logs d'erreurs pour <b>Télérama</b> et <b>Le Nouvel Obs</b> en production.<br>
          - <b>#tech-err-phalcon-user-majorel-prd</b> : Logs d'erreurs pour <b>Courrier International</b> et <b>Le Monde Diplomatique</b> en production.<br>
          - <b>#tech-deploy-phalcon-user</b> : Utilisé pour déployer les serveurs encore sur GCP grâce à un petit bot (ce canal est voué à disparaître avec la migration).<br>
          - <b>tech-sre-developers</b> : Pour communiquer avec l'équipe SRE en cas de problème sur notre infrastructure.<br><br>

          <b>Bonnes Pratiques :</b><br><br>

          - <b>Familiarise-toi</b> avec ces canaux pour être au courant des discussions et des informations importantes.<br>
          - <b>Utilisez le canal de votre équipe</b> pour poser des questions ou partager des informations spécifiques à votre projet.<br>
          - <b>Respectez les usages de chaque canal</b> pour maintenir une communication claire et efficace.<br>
          - <b>Surveillez les canaux d'erreurs et de latence</b> pour être proactif dans la résolution des problèmes.<br><br>

          Slack est un outil essentiel pour la communication au sein de l'équipe et de l'entreprise. Une utilisation appropriée facilite le travail collaboratif et le partage d'informations.
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "🏗️ Architecture Globale de la Stack",
        description: `
          <div style="text-align: center;">
            <img src="/assets/phalcon-user.png" alt="Architecture Globale" style="width: 85%;" />
          </div>

          Le projet <strong>phalcon-user</strong> se compose principalement des éléments suivants :<br><br>

          - <b>Serveur</b> : Gère les requêtes entrantes et la logique applicative.<br>
          - <b>Base de Données</b> : Stocke les informations essentielles liées aux utilisateurs, abonnements, etc.<br>
          - <b>Nginx</b> : Sert de serveur web pour diriger le trafic vers les bons services.<br>
          - <b>Routes Web Service</b> : Expose des API pour permettre la communication avec d'autres services.<br><br>

          Nous utilisons également <b>Directus</b> qui est une plateforme CMS Headless open-source permettant de gérer le contenu et les données métiers de manière efficace.<br><br>

          <b>Flux d'Abonnement des Utilisateurs :</b><br>

          - <b>Application Mobile</b> : Les utilisateurs peuvent s'abonner via les applications mobiles en utilisant iTunes ou le Play Store. Cela crée un reçu chez nous en stockant un abonnement "gratuit" chez le partenaire, car la facturation est gérée par ces fournisseurs.<br>
          - <b>Pages Web</b> : Les utilisateurs peuvent également s'abonner directement sur nos pages web en utilisant les paiements Stripe.<br><br>

          <b>Gestion des Newsletters :</b><br>

          - Nous envoyons quotidiennement des exports au service CRM pour la gestion des newsletters et la communication avec les utilisateurs.<br><br>

          <b>Synchronisation des Données :</b><br>

          - Une des principales problématiques du projet est que les données peuvent être soit chez nous, soit chez le partenaire. Étant donné que nous sommes responsables de l'affichage de ces données, nous devons nous synchroniser fréquemment avec le partenaire pour assurer la cohérence et l'exactitude des informations présentées aux utilisateurs.<br><br>

          Prenez le temps d'étudier l'architecture pour bien comprendre comment les différents composants interagissent entre eux. N'hésitez pas à poser des questions si certains points ne sont pas clairs.
        `,
        check: () => {
          // Vous pouvez ajouter une vérification pour cette étape si nécessaire
          return Promise.resolve(true);
        },
      },
      {
        title: "🏷️ Présentation des Marques et Partenaires",
        description: `
          Le projet <strong>phalcon-user</strong> est utilisé pour 5 marques distinctes :
          <ul>
            <li><strong>Le Monde</strong></li>
            <li><strong>Télérama</strong></li>
            <li><strong>Le Nouvel Obs</strong></li>
            <li><strong>Courrier International</strong></li>
            <li><strong>Le Monde Diplomatique</strong></li>
          </ul>
          Pour différencier ces marques dans le code source, nous utilisons une variable d'environnement nommée <code>BASE_SITE</code>. Cette variable permet de définir quel site est en cours d'exécution et d'adapter les comportements spécifiques.

          Nous sommes dépendants d'un partenaire d'abonnement qui a trois grandes responsabilités :
          <ul>
            <li>Gérer les <strong>paiements récurrents</strong></li>
            <li>Assurer les <strong>livraisons des journaux</strong></li>
            <li>Fournir les <strong>états comptables</strong></li>
          </ul>

          Les marques <strong>Télérama</strong> et <strong>Le Nouvel Obs</strong>, identifiées par <code>BaseSite::useEdiis()</code>, travaillent avec le partenaire <strong>Ediis</strong>.<br>
          Les autres marques (Le Monde, Courrier International, Le Monde Diplomatique), identifiées par <code>BaseSite::useMage()</code>, sont avec le partenaire <strong>Majorel</strong>.

          Concernant <strong>Majorel</strong>, il y a deux grands services que nous utilisons :
          <ul>
            <li><strong>Mage</strong> : une API utilisée pour gérer les flux de commandes, les flux de changements d'adresses, etc.</li>
            <li><strong>Magento</strong> (et son API <code>Arvato</code>) : qui était l'ancien espace connecté des utilisateurs.</li>
          </ul>
          Cela implique que notre code doit gérer ces deux partenaires de manière différente selon la marque sur laquelle nous travaillons. <br>
          Assure-toi de bien comprendre ces distinctions car elles sont essentielles pour l'intégration des services d'abonnement. 
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "📥 Suivre le README pour l'installation",
        description: `
          Pour installer le projet, il est important de suivre les instructions fournies dans le <strong>README</strong> du dépôt.<br><br>
          Voici quelques points à retenir :
          <ul>
            <li>Le projet utilise actuellement <strong>Google Cloud Platform (GCP)</strong> pour nos serveurs. Une migration vers AWS est en cours, mais Le Monde n'est pas encore sur AWS, donc ne suivez pas la partie AWS du README.</li>
            <li>Pendant la configuration, assure-toi de <strong>modifier ton fichier <code>/etc/hosts</code></strong> afin d'ajouter les entrées nécessaires pour accéder aux différents services en local. Cela te permettra d'accéder aux sous-domaines requis par le projet.</li>
          </ul>
          Lisez attentivement chaque section du README pour éviter des erreurs de configuration. 
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "✅ Vérifier l'installation",
        description: `
          Après avoir terminé l'installation, nous allons vérifier que le projet fonctionne correctement en faisant un appel à l'URL suivante :
          <pre>
          https://loc-secure.lemonde.io/sfuser/connexion
          </pre>
          Cette URL permet de vérifier que l'API est bien disponible et que tout est correctement configuré.
        `,
        check: () => {
          return new Promise((resolve) => {
            fetch('https://loc-secure.lemonde.io/sfuser/connexion', { mode: 'no-cors' })
              .then(() => resolve(true))
              .catch(() => resolve(false));
          });
        },
        troubleshooting: `
          Si l'appel à l'URL échoue : <br>
          - Vérifie que le projet est bien lancé (utilise <code>make ps</code> pour vérifier les conteneurs en cours d'exécution). <br>
          - Assure-toi que tu as bien modifié ton fichier <code>/etc/hosts</code> pour ajouter les entrées nécessaires. <br>
          - Consulte les logs du projet pour voir si des erreurs sont signalées (commande <code>make logs</code>). <br>
        `
      },
      {
        title: "🔄 CI/CD",
        description: `
          Dans cette étape, nous allons aborder le processus de <b>CI/CD</b> utilisé dans notre projet.<br><br>
          <b>CI/CD sur GCP :</b><br><br>
          - À chaque push sur une branche <b>feature</b>, les tests Travis sont exécutés.<br>
          - En général, il faut <b>2 approbations</b> minimum pour merger une PR.<br>
          - Déploiement du projet via <b>hubot</b> sur le canal Slack <b>#tech-deploy-phalcon-user</b>. Voici quelques exemples de commandes :<br>
          <pre>
      deploy phalcon-lemonde on stg branch master
      deploy phalcon-lemonde on stg branch feat/home
      deploy phalcon-lemonde on stg commit a9154efdacb1d5d39c6c6ba3c5
      deploy phalcon-lemonde on stg commit a9154efd
      rollback phalcon-lemonde on stg
      history phalcon-lemonde on stg
      status phalcon-lemonde on stg
          </pre>
          - À la fin du traitement, le <b>code source est déployé</b>.<br><br>
          <b>CI/CD sur AWS :</b><br><br>
          - À chaque push sur une branche <b>feature</b>, les tests Travis sont exécutés.<br>
          - En général, il faut <b>2 approbations</b> minimum pour merger une PR.<br>
          - Le push sur la branche <b>master</b> lance un déploiement automatique via <b>ArgoCD</b>, génère le changelog et envoie le changelog sur <b>#tech-abo-release</b>.<br><br>
          <b>Points Clés :</b><br><br>
          - Les processus de CI/CD assurent que le code est testé et validé avant d'être déployé.<br>
          - Les approbations requises pour les PR garantissent une revue de code adéquate.<br>
          - Les outils comme Travis, hubot et ArgoCD automatisent les déploiements et améliorent l'efficacité.<br>
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "💻 Exploration du Code Source",
        description: `
          Dans le dossier <strong>app</strong>, tu trouvera plusieurs sous-dossiers qui corresponderait a des modules Symfony :

          <ul>
            <li><strong>back-office</strong> : Probablement du code obsolète ou non utilisé actuellement.</li>
            <li><strong>cli</strong> : Contient toutes les interfaces en ligne de commande (Command Line Interface) utilisées pour diverses tâches.</li>
            <li><strong>dpo</strong> : Gère tout ce qui est lié aux aspects légaux, tels que la FAQ, les contacts, etc.</li>
            <li><strong>mailing</strong> : Semble être du code obsolète également.</li>
            <li><strong>offers</strong> : Correspond au mur d'offres, c'est-à-dire la présentation des différentes offres d'abonnement.</li>
            <li><strong>sf</strong> : Regroupe tout ce qui concerne la connexion, l'inscription, et les fonctionnalités liées aux utilisateurs.</li>
            <li><strong>tunnel</strong> : Contient tout ce qui est lié au paiement et au tunnel d'abonnement.</li>
          </ul>

          Une variable d'environnement importante à noter est <code>HOSTS_MAPPING</code>. Elle est utilisée pour faire le mapping entre ces modules et les sous-domaines correspondants. Cela permet de diriger les requêtes vers le bon module en fonction du sous-domaine utilisé.

          Prenez le temps d'explorer ces modules pour te familiariser avec la structure du projet. N'hésitez pas à consulter la documentation interne ou à poser des questions si quelque chose n'est pas clair.
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "⚙️ Configuration Phalcon",
        description: `
        Dans cette étape, nous allons explorer les configurations utilisées dans notre projet Phalcon. Pour en savoir plus, consulte la documentation <a href="https://docs.phalcon.io/3.4/introduction/">Phalcon 3.4</a>. <br><br>
        Dans le dossier <code>/config</code>, on retrouve tout ce qui est configuration, avec des données non sensibles.<br> Tous les fichiers de configuration sont inclus dans <code>/config/config.php</code> et "injectés" dans l'application. Cela permet d'accéder facilement aux configurations nécessaires dans différentes parties de l'application.<br><br>
  
        Voici deux exemples pour récupérer une configuration en utilisant l'Injection de Dépendances (DI) :
        <pre>
          $di = Di::getDefault();
          $paymentConfig = $di->get('paymentConfig');
        </pre>
        
        Ou encore :
        <pre>
          $di = $this->getDI();
          $this->mailer = $di->get('emailSenderService');
        </pre><br>
        
        Prends un peu de temps pour te familiariser avec ces fichiers de configuration, leur organisation et leur contenu. Une bonne compréhension de la configuration facilitera l'intégration de nouvelles fonctionnalités et la gestion des environnements.
        `,
        check: () => {
          return Promise.resolve(true);
        }
      },
      {
        title: "🔌 Injection de Dépendances dans Phalcon",
        description: `
          Dans cette étape, nous allons approfondir le sujet de l'Injection de Dépendances (DI) dans Phalcon. Deux fichiers importants à connaître sont :<br><br>

          - <code>config/services.php</code><br>
          - <code>config/services_cli.php</code><br><br>

          Ces fichiers sont utilisés pour enregistrer les services et les composants que tu peux injecter dans différentes parties de l'application.<br><br>

          <b>Exemple d'enregistrement d'un service partagé :</b>

          <pre>
          $di->setShared('config', function () {
              return include BASE_PATH . '/config/config.php';
          });
          </pre>

          La méthode <code>setShared</code> enregistre un service en tant que singleton, ce qui signifie qu'une seule instance du service sera utilisée partout dans l'application. Cela permet d'accéder à la configuration de l'application à partir de n'importe quel endroit en utilisant :

          <pre>
          $config = $di->get('config');
          </pre>

          <b>Attention Importante :</b><br><br>

          Dans ce projet, l'Injection de Dépendances a été utilisée de manière excessive, ce qui impacte désormais les performances de l'application.<br><br> Il est donc <b>formellement interdit</b> d'ajouter une nouvelle entrée dans l'un de ces deux fichiers (<code>services.php</code> et <code>services_cli.php</code>) sans l'accord préalable du Tech Lead.<br><br>

          Avant d'ajouter un nouveau service, veuillez consulter le Tech Lead pour discuter des alternatives possibles ou des optimisations nécessaires.
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "🔑 Flow de Connexion Utilisateur",
        description: `
          <div style="text-align: center;">
            <img src="/assets/flow-connexion.png" alt="Flow connexion" style="width: 85%;" />
          </div>
          Dans cette étape, nous allons détailler le <b>flux de connexion</b> au sein de l'application.<br><br>
          <b>Processus de Connexion :</b><br><br>
          - <b>Point d'entrée utilisateur :</b> Les utilisateurs se connectent via l'URL <code>https://secure.lemonde.fr/sfuser/connexion</code>.<br>
          - <b>Vérification des identifiants :</b> Après soumission du formulaire de connexion, le système vérifie le couple <b>email</b> et <b>mot de passe</b> dans notre base de données.<br>
          - <b>Création du cookie <code>lmd_a_s</code> :</b> Si les informations sont correctes, une réponse est renvoyée avec un cookie nommé <code>lmd_a_s</code>.<br>
            * Ce cookie contient un <b>hash de l'identifiant utilisateur</b>.<br>
            * Il est partagé sur le domaine <code>.lemonde.fr</code> et tous ses sous-domaines.<br>
          - <b>Authentification des requêtes ultérieures :</b> Ce cookie sert à authentifier les appels ultérieurs à <code>phalcon-user</code>.<br><br>
          <b>Interaction avec le Site Éditorial :</b><br><br>
          - <b>Lecture du cookie :</b> Le site éditorial (par exemple, <code>www.lemonde.fr</code>) reconnaît le cookie <code>lmd_a_s</code> et sait qu'un utilisateur est connecté.<br>
          - <b>Stockage des informations utilisateur :</b> Le site éditorial stocke les informations de l'utilisateur dans son propre cache <b>Redis</b>.<br>
          - <b>Appel à <code>auth/user</code> si nécessaire :</b><br>
            * Si le cache Redis est vide ou expiré, le site éditorial fait un appel à l'API <code>auth/user</code> de <code>phalcon-user</code> pour récupérer les informations mises à jour de l'utilisateur.<br>
          - <b>Gestion du cache Redis :</b><br>
            * Le TTL (Time To Live) du cache Redis côté éditorial est de <b>30 jours</b>.<br>
            * Lorsqu'une modification survient sur l'utilisateur (par exemple, prise d'un abonnement, expiration de carte bancaire, etc.), nous avons la possibilité de <b>purger le cache</b> pour forcer la mise à jour des informations lors du prochain appel.<br><br>
          <b>Points Clés à Retenir :</b><br><br>
          - <b>Sécurité :</b> Le hash dans le cookie <code>lmd_a_s</code> assure que l'identifiant utilisateur n'est pas exposé en clair.<br>
          - <b>Performance :</b> L'utilisation du cache Redis côté éditorial améliore les performances en évitant des appels répétés à <code>phalcon-user</code>.<br>
          - <b>Synchronisation des Données :</b> La capacité à purger le cache garantit que les informations critiques (comme l'état de l'abonnement) sont à jour pour l'utilisateur.<br><br>
          Comprendre ce flux est essentiel, car il illustre comment les différents systèmes interagissent pour offrir une expérience utilisateur fluide tout en maintenant la sécurité et la performance de l'application.
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "🔄 Synchronisation Majorel",
        description: `
          <b>Définition :</b><br><br>
          La synchro (synchronization) est un ensemble de fichiers qui permettent de mettre à jour les données de l’utilisateur en base de données avec les données chez le prestataire Mage.<br><br>
          Elle est découpée en différents synchronizers, qui représentent les entités du code :<br>
          - CustomerSynchronizer<br>
          - AddressSynchronizer<br>
          - MediaSynchronizer<br>
          - BankCoordinateSynchronizer<br>
          - OrderSynchronizer<br>
          - SubscriptionSynchronizer<br>
          - OrderErrorSynchronizer<br>
          - BeneficiarySynchronizer<br>
          - ConversionSynchronizer<br>
          - (soon ⇒ SuspensionSynchronizer)<br><br>
          La synchro tourne en automatique toutes les 15 minutes avec une tâche CRON qui déclenche la commande <code>mage:sync</code>.<br><br>
          Elle est déclenchée également lorsque le prestataire Mage nous indique qu’une modification a été effectuée (push), dans ce cas, c’est la commande <code>mage:push:sync</code> qui est exécutée.<br><br>
          Elle peut être déclenchée sur un utilisateur précis sur la route <code>secure.lemonde.fr/sfuser/syncUser</code>, ou avec un connectAs via un bouton sur le dashboard. Dans ce cas, elle sera lancée sur la queue <code>mage.sync_user</code>.<br><br>
          La synchro est terminée par un sommaire qui résume toutes les actions effectuées sur chaque synchronizer. Dans la console, on peut rechercher cette information en filtrant par l’id du user, et par le terme <code>SUMMARY</code>.<br><br>
          
          <b>Les statuts de synchro :</b><br><br>
          Une entité peut avoir plusieurs statuts de synchro, voici les principaux et leur signification :<br>
          - 1 ⇒ en attente de synchronisation (sera récupéré lors de la prochaine synchro dans le <code>mage:sync</code>)<br>
          - 3 ⇒ erreur de synchro (une erreur est survenue, ou alors le max de synchro (6) a été tenté)<br>
          - 4 ⇒ bien synchronisé<br>
          - 23 ⇒ lock, c’est le statut qu’on met lorsque le user entre en synchro, cela évite qu’il soit récupéré sur une autre synchro. Ce statut est enlevé à la fin de la synchro<br>
          - 22 ⇒ le user a subi un push par Mage, il sera donc récupéré dans le <code>mage:push:sync</code><br>
          - 100 ⇒ c’est un statut de batch sync, une synchro de masse <code>batch:sync</code><br>
          - 91 ⇒ c’est un statut d’attente lors d’un import en preprod, on attend que le mail yopmail soit bien arrivé chez Mage pour synchro le user.<br><br>
          
          <b>Les queues :</b><br><br>
          Il y a différentes queues PubSub pour les synchros :<br>
          - <code>mage.sync</code> ⇒ la classique, qui est déclenchée toutes les 15 minutes, sync_status 1, 2<br>
          - <code>mage.push_sync</code> ⇒ push envoyé par Mage, déclenchée toutes les 7 minutes, sync_status 22<br>
          - <code>mage.sync_user</code> ⇒ déclenchée via le bouton du dashboard après connectAs ou via l’url <code>sfuser/syncUser</code>, pas de statut, elle est déclenchée à l’action.<br>
          - <code>mage.batch_sync</code> ⇒ déclenchée toutes les 10 minutes, utilisée pour des synchros d’utilisateurs en masse, sync_status 100<br><br>
          
          <b>Le SyncDataContainer :</b><br><br>
          C’est l’objet qui va contenir les données que l’on va traiter lors de la synchro :<br>
          - local ⇒ les datas de la base de données (ex : <code>$user->getValidOrders</code>)<br>
          - remote ⇒ les datas chez Mage (ex : <code>$externalApiConnector->getCustomerOrders()</code>)<br>
          - option ⇒ les données optionnelles, qui ne sont ni des données de l’entité locales ou de chez Mage (exemple : dans l’OrderSynchronizer, si on a besoin d’une donnée spécifique hors order de la DB ou order Mage, on la met dans une option )<br>
          - payload ⇒ là-dedans on va mettre des données transformées que l’on voudra enregistrer plus tard<br>
          - action ⇒ il va contenir la “rule”, et peut aussi contenir des datas utiles pour exécuter la “rule”. (ex : <code>$data->addAction(RULE, ["key" => "value"])</code> donnera <code>["action" => "RULE", "data" => ["key" => "value"]]</code>)<br><br>
          
          <b>Les erreurs de la synchro :</b><br><br>
          Une “Rule” de la synchro peut être considérée comme une erreur. En effet, certaines rules doivent empêcher le user de se synchroniser pour que la synchro se relance plus tard.<br>
          Dans ce cas, on ajoute la Rule dans le handleError du synchronizer.<br><br>
          Dans ce handleError, on peut décider si cette erreur doit être affichée dans slack ou non. Dans ce cas, c’est une erreur “maîtrisée”/”voulue”, donc on veut juste qu’elle laisse le user en sync_status 1, mais pas forcément voir l’erreur s’afficher tout le temps.<br><br>
          
          <b>Comment investiguer sur la synchro d’un utilisateur ?</b><br><br>
          Tout d’abord, il faut savoir sur quelle queue sa synchro s’est déroulée.<br>
          Une fois qu’on a la queue, on peut aller voir dans la console Google les messages de la queue.<br>
          Dans la barre de recherche, on peut mettre l’id du user, ce qui permettra de voir tous les messages de la queue concernant le user, ou alors affiner en recherchant l’id user + <code>SUMMARY</code>, ce qui va afficher tous les résumés de synchro de l’utilisateur sur la queue.<br><br>
          Ensuite, il faut comparer le résumé avec les données de la DB, car le résumé nous donne les actions qui ont été effectuées sur la synchro.<br><br>
          
          <b>Comment ajouter une règle dans la synchro ?</b><br><br>
          1) Récupérer les données dans l’extracteur<br>
          1bis) (OPTIONNEL) : formater les données dans le Transformer<br>
          2) Créer une RULE dans les constantes du Rule concerné, et l’appliquer dans le code au bon endroit suivant l’effet voulu<br>
          3) Traiter la nouvelle Rule dans le Loader<br>
          4) OPTIONNEL ⇒ Ajouter la rule dans les cas d’erreur si elle doit être considérée comme une RULE erreur ou si elle doit laisser le User en <code>sync_status = 1</code><br>
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "🌐 Utilisation de Fastly",
        description: `
          <b>Qu'est-ce que Fastly ?</b><br><br>
          Fastly est un <b>Content Delivery Network (CDN)</b> que nous utilisons pour toutes nos marques.<br><br>
          <b>Pourquoi Fastly ?</b><br><br>
          - Absorber un maximum de charge<br>
          - Améliorer les performances de chargement<br>
          - Assurer la disponibilité des sites même en période de fort trafic<br><br>
          Fastly permet de distribuer notre contenu de manière efficace en servant les utilisateurs depuis des emplacements proches d'eux, réduisant ainsi la latence et améliorant l'expérience utilisateur.<br><br>
          En utilisant Fastly comme CDN, nous assurons que nos sites restent rapides et réactifs, même avec une base de données en croissance continue.<br><br>
          <b>Points Clés :</b><br><br>
          - Fastly aide à maintenir la performance et la disponibilité de nos sites.<br>
          - Il est essentiel de surveiller les métriques de Fastly pour identifier et résoudre rapidement les problèmes de distribution.<br>
          - L'intégration de Fastly avec notre infrastructure CI/CD permet des déploiements rapides et sans interruption.<br><br>
          Assures-toi de comprendre comment Fastly est utilisé dans notre architecture et de suivre les meilleures pratiques pour optimiser les performances et la fiabilité de nos services.
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "🚀 Performances",
        description: `
          Dans cette étape, nous allons aborder les <b>performances</b> de notre application.<br><br>
          Nous travaillons sur des sites à <b>fort trafic</b> avec une <b>base de données en constante croissance</b>.<br><br>
          <b>Quelques chiffres :</b><br>
          - <b>Le Monde</b> : Environ <b>5 millions d'utilisateurs</b><br>
          - <b>Abonnés</b> : <b>600 000</b><br>
          - <b>Articles en favoris</b> : <b>45 millions</b><br><br>
          <b>Points Importants :</b><br><br>
          - Il est primordial de <b>penser les performances</b> lors de l'écriture du code.<br>
          - Optimiser les <b>temps de traitement en base de données</b> en évitant les <b>requêtes redondantes</b>.<br>
          - Garder en tête que la <b>codebase est utilisée par 4 autres marques</b>, ce qui nécessite une attention particulière à la performance globale.<br><br>
          <b>Bonnes Pratiques :</b><br><br>
          - <b>Optimisation des requêtes SQL</b> : Utilisez des index appropriés et évitez les jointures complexes lorsque ce n'est pas nécessaire.<br>
          - <b>Mise en cache</b> : Implémentez des mécanismes de cache (comme Redis) pour réduire la charge sur la base de données.<br>
          - <b>Profilage du code</b> : Utilisez des outils de profilage pour identifier et optimiser les goulots d'étranglement.<br>
          - <b>Revue de Code</b> : Assures-toi que chaque morceau de code est examiné pour son impact sur les performances.<br>
          - <b>Scalabilité</b> : Concevez votre architecture pour qu'elle puisse facilement s'adapter à une augmentation du trafic.<br><br>
          En suivant ces bonnes pratiques, tu contribuera à maintenir une application performante et réactive, capable de gérer un trafic élevé et une base de données en pleine expansion.
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      {
        title: "📚 Aller plus loin",
        description: `
          <b>Documentation du Projet :</b><br><br>
          - <b>Docusaurus Documentation</b> : Pour accéder à la documentation du projet, naviguez dans le répertoire <code>developers</code> (dans phalcon-user), exécutez <code>yarn start</code>, puis ouvrez <a href="http://localhost:3000">localhost:3000</a> dans votre navigateur.<br><br>
          - <b>README.md</b> : Consultez le <a href="https://github.com/lemonde/phalcon-user/blob/master/README.md">README du projet phalcon-user</a><br>
          - <b>Guidelines de Contribution</b> : Découvrez les guidelines de <a href="https://github.com/lemonde/phalcon-user/blob/master/.github/CONTRIBUTING.md">contribution</a><br>
          - <b>La Synchronisation</b> : Accédez à la <a href="https://www.notion.so/lemondedesign/La-Synchro-64c18adc2387429586694593f405f178?pvs=4">page Notion</a> sur la synchro majorel<br>
          - <b>Conventional Commits</b> : Apprenez-en plus sur <a href="https://www.conventionalcommits.org/en/v1.0.0/">Conventional Commits</a><br><br>
          Ces ressources te permettront d'approfondir vos connaissances sur le projet, de comprendre les processus de contribution, et d'adopter les bonnes pratiques en matière de gestion des commits.
        `,
        check: () => {
          return Promise.resolve(true);
        },
      },
      ],
      currentStepIndex: 0,
    };
  },
  computed: {
    progressPercentage() {
      return ((this.currentStepIndex + 1) / this.steps.length) * 100;
    }
  },
  methods: {
    nextStep() {
      this.currentStepIndex++;
      this.saveCurrentStep();
    },
    previousStep() {
      if (this.currentStepIndex > 0) {
        this.currentStepIndex--;
        this.saveCurrentStep();
      }

    },
    goToStep(index) {
      this.currentStepIndex = index;
      this.saveCurrentStep();
    },
    saveCurrentStep() {
      localStorage.setItem('onboardingStep', this.currentStepIndex);
    },
    loadCurrentStep() {
      const savedStep = localStorage.getItem('onboardingStep');
      if (savedStep !== null) {
        this.currentStepIndex = parseInt(savedStep, 10);
      }
    }
  },
  mounted() {
    this.loadCurrentStep();
  }
}
</script>
