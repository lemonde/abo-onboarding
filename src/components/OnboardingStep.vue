<template>
  <div class="step">
    <h2>{{ step.title }}</h2>
    <p v-html="step.description"></p>
    <div class="buttons">
      <button id="next-step" @click="handleNextStep">Suivant</button>
      <button id="back-step" @click="handlePreviousStep">Retour en arrière</button>
    </div>
    <div v-if="animationText" id="animation" :class="animationClass">{{ animationText }}</div>
    <div v-if="error" class="troubleshooting">
      <h3>Problèmes rencontrés :</h3>
      <div v-html="renderedTroubleshooting"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    step: Object,
    stepIndex: Number
  },
  data() {
    return {
      error: false,
      animationText: "",
      animationClass: "",
    };
  },
  computed: {
    renderedTroubleshooting() {
      return this.step.troubleshooting; // Rend le troubleshooting en HTML
    }
  },
  methods: {
    handleNextStep() {
      if (this.step.check) {
        this.step.check().then(success => {
          if (success) {
            this.$emit('completed');
            this.displaySuccessAnimation();
          } else {
            this.error = true;
          }
        });
      }
    },
    handlePreviousStep() {
      this.error = false;
      this.$emit('back');
    },
    displaySuccessAnimation() {
      const animations = [
      "📅 Focus sur Semana : Parce que réserver un bureau n'a jamais été aussi excitant.",
      "🔒 Twingate en action ! Plus sécurisé que ton mot de passe Netflix.",
      "💬 Slack maîtrisé : Maintenant, tu peux envoyer des gifs sans culpabiliser.",
      "🏗️ Architecture Globale de la Stack : C'est comme construire Lego, mais avec du code.",
      "🏷️ Présentation des Marques : Parce que chaque marque mérite son moment de gloire.",
      "📥 README suivi : Le manuel d'instructions pour les braves.",
      "✅ Installation vérifiée : Tu as survécu à l'installation, bravo !",
      "🔄 CI/CD configuré : Automatiser le déploiement, ou comment être paresseux de manière productive.",
      "💻 Exploration du Code : Plonge dans le code comme un explorateur dans une jungle.",
      "⚙️ Configuration Phalcon : Ajuster les rouages pour que tout fonctionne comme sur des roulettes.",
      "🔌 Injection de Dépendances : Brancher les bons services, sans faire sauter le circuit.",
      "🔑 Flow de Connexion : Authentification fluide, comme un café bien serré.",
      "🔄 Synchronisation Majorel : Synchroniser les données, parce que l'ordre règne ici.",
      "🌐 Fastly en place : Rapide comme l'éclair, ou presque.",
      "🚀 Performances optimisées : Faire tourner le site sans transpirer.",
      "📚 Aller plus loin : Parce que la curiosité n'a jamais tué le développeur.",
      ""
    ];
    
    const animationClasses = [
      "animation-bounce",
      "animation-tada",
      "animation-pulse",
      "animation-flash"
    ];
    
    // Sélectionner un message basé sur l'index de l'étape
    this.animationText = animations[this.stepIndex] || "🎉 Étape complétée !";
    
    // Sélectionner une classe d'animation basée sur l'index de l'étape
    const animationClassIndex = this.stepIndex % animationClasses.length;
    this.animationClass = animationClasses[animationClassIndex];
    }
  }
}
</script>

<style scoped>
.buttons {
    margin-top: 20px;
    text-align: center;
}

#animation {
    color: #007bff;
    font-weight: bold;
    margin-top: 20px;
    animation: fadeIn 1s ease-in-out;
}

.animation-0 {
    animation: bounce 1s ease-in-out;
}

.animation-1 {
    animation: tada 1.2s ease-in-out;
}

.animation-2 {
    animation: pulse 1.4s ease-in-out;
}

@keyframes fadeIn {
    0% { opacity: 0; }
    100% { opacity: 1; }
}

@keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
        transform: translateY(0);
    }
    40% {
        transform: translateY(-30px);
    }
    60% {
        transform: translateY(-15px);
    }
}

@keyframes tada {
    0% { transform: scale(1); }
    10%, 20% { transform: scale(0.9) rotate(-3deg); }
    30%, 50%, 70%, 90% { transform: scale(1.1) rotate(3deg); }
    40%, 60%, 80% { transform: scale(1.1) rotate(-3deg); }
    100% { transform: scale(1) rotate(0); }
}

@keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.2); }
    100% { transform: scale(1); }
}

/* Style des snippets de code en bloc (comme ceux utilisés avec ```bash) */
pre {
    background: #f8f9fa; /* Fond gris très clair pour les blocs de code */
    color: #333; /* Couleur du texte */
    padding: 15px;
    border-radius: 5px;
    font-size: 0.95em;
    font-family: 'Courier New', Courier, monospace; /* Police type machine à écrire */
    line-height: 1.5;
    overflow-x: auto; /* Permet de scroller horizontalement si le code est trop long */
    border: 1px solid #e0e0e0;
}

/* Style des snippets de code inline (comme ceux entourés par `backticks`) */
code {
    background: #f1f1f1; /* Fond légèrement gris */
    color: #d63384; /* Couleur pour mettre en évidence le code */
    padding: 2px 6px;
    border-radius: 3px;
    font-size: 0.95em;
    font-family: 'Courier New', Courier, monospace;
}

.troubleshooting {
    margin-top: 20px;
    padding: 15px;
    border-left: 4px solid #d9534f; /* Rouge pour indiquer un problème */
    background-color: #f8d7da;
    color: #721c24;
}
</style>
