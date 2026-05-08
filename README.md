# 🌌 TP 5 : Maîtrise du Multitâche et des Ressources Partagées (FreeRTOS)

Bienvenue dans le dépôt du **Travail Pratique N°5**. Ce projet marque une évolution majeure dans la conception de nos systèmes embarqués : le passage d'une architecture séquentielle classique (Bare Metal) à l'intégration d'un **Système d'Exploitation Temps Réel (RTOS)** avec **FreeRTOS**.

L'objectif est d'apprendre à gérer des tâches concurrentes, l'ordonnancement basé sur les priorités, et la protection des ressources critiques sur le microcontrôleur **STM32 Nucleo-C031C6**.

## 🚀 Fonctionnalités du Jumeau Numérique (Web)

Afin d'illustrer visuellement les concepts abstraits du multitâche, le fichier `compte_rendu_tp5.html` intègre un simulateur dynamique (HTML/JS) comprenant :
* **Comparateur Séquentiel vs RTOS :** Visualisation en temps réel de l'impact des délais bloquants (`HAL_Delay`) par rapport aux délais gérés de manière asynchrone par l'OS (`vTaskDelay`).
* **Simulateur de Conflit (Race Condition) :** Démonstration visuelle du "scrambling" (mélange de données) sur un bus I2C (Écran LCD) lorsque deux tâches tentent d'y accéder simultanément sans protection.
* **Synchronisation par Mutex :** Résolution du conflit matériel via un verrouillage par Mutex, garantissant un affichage propre et ordonné.
* **Analyse d'Ingénierie (Post-Lab) :** Réponses d'ingénierie détaillées concernant les états de tâches (Blocked state), l'impact de la préemption, l'utilisation avancée des Queues, et la gestion des Sémaphores binaires.

## 📋 Manipulations Réalisées (Code C via FreeRTOS)

1. **Création de Tâches (`xTaskCreate`) :**
   * Implémentation de 3 tâches indépendantes gérant le clignotement de LEDs sur `PA5`, `PA6` et `PA7` à des fréquences distinctes (1000ms, 500ms, 400ms). Cela élimine l'effet de blocage (goulot d'étranglement) d'une boucle `while(1)` classique.
2. **Protection par Mutex (`xSemaphoreCreateMutex`) :**
   * Encapsulation des requêtes d'écriture de l'écran LCD I2C entre les fonctions `xSemaphoreTake()` et `xSemaphoreGive()` pour sécuriser la ressource partagée.
3. **Synchronisation Asynchrone :**
   * Utilisation d'un sémaphore binaire (`xSemaphoreTake` avec `portMAX_DELAY`) pour réveiller instantanément une tâche d'affichage de log de son état de veille (0% CPU) suite à un événement matériel (appui sur un bouton).

## 🛠️ Matériel & Outils Requis

* **Carte :** STM32 Nucleo-C031C6 (Matériel physique ou simulation virtuelle via [Wokwi](https://wokwi.com/stm32)).
* **Périphériques :** 3x LEDs externes, 1x Bouton poussoir, 1x Écran LCD 16x2 I2C.
* **Logiciel :** STM32CubeIDE (avec le Middleware **STM32duino FreeRTOS** activé via le gestionnaire de packages).
* **Environnement Web :** Navigateur web moderne pour lancer le rapport interactif.

## ⚙️ Instructions d'Exécution

### 1. Visualiser le Jumeau Numérique (Web)
Double-cliquez sur le fichier `compte_rendu_tp5.html`. Naviguez entre les onglets pour observer les différences de comportement entre un système Bare Metal bloquant et l'ordonnancement intelligent de FreeRTOS.

### 2. Exécuter le Code C (STM32 / Wokwi)
1. Importez le projet dans STM32CubeIDE. Assurez-vous d'avoir installé la librairie FreeRTOS.
2. Vérifiez que les broches `PA5`, `PA6`, et `PA7` sont configurées en `GPIO_Output`.
3. Compilez et flashez le code sur la carte.
4. Observez le parallélisme parfait des trois LEDs, chose impossible à réaliser proprement avec de simples `HAL_Delay()`.

## 👥 Équipe du Projet

**Année Universitaire :** 2025/2026
* **Binôme :** Dahane Ahmed Lamine & Tabbi Meriem
* **Enseignante Responsable :** Mme. Afaf Saoud
* **Département :** Électronique
