# Hacking d'une Application Web : OWASP Juice Shop

Ce projet documente l'analyse et l'exploitation de vulnérabilités web courantes sur l'application volontairement vulnérable **OWASP Juice Shop**. Le but de cet exercice est de mettre en pratique les techniques de test d'intrusion d'applications web.

## Phase 1 : Mise en Place du Laboratoire

-   **Objectif :** Déployer un environnement de test sécurisé et fonctionnel.
-   **Outils :** **Docker** a été utilisé pour télécharger et lancer l'application OWASP Juice Shop dans un conteneur isolé.
-   **Commande :** `docker run --rm -p 3000:3000 bkimminich/juice-shop`

## Phase 2 : Interception de Requêtes (Man-in-the-Middle)

-   **Objectif :** Capturer et analyser le trafic entre le navigateur et le serveur web.
-   **Outils :** **Burp Suite** (Community Edition) a été configuré comme proxy d'interception. L'extension **FoxyProxy** sur Firefox a permis de rediriger le trafic du navigateur vers Burp.
-   **Résultat :** Une requête de connexion (`POST /rest/user/login`) a été interceptée avec succès, révélant l'email et le mot de passe envoyés en clair dans le corps de la requête.



## Phase 3 : Découverte de Contenu Caché

-   **Objectif :** Identifier des pages et des répertoires non accessibles via la navigation standard du site.
-   **Outils :** **Gobuster** a été utilisé en mode `dir` avec la wordlist `directory-list-2.3-medium.txt` pour brute-forcer les chemins d'URL.
-   **Résultat :** Plusieurs répertoires cachés ont été découverts, notamment `/ftp`, `/promotion` et `/metrics`. Le scan a nécessité l'utilisation de l'argument `--exclude-length` pour contourner la protection de l'application (wildcard).

## Phase 4 : Exploitation d'une Faille XSS (Cross-Site Scripting)

-   **Objectif :** Exploiter une faille XSS dans la barre de recherche pour exécuter du code JavaScript arbitraire.
-   **Technique :** Le premier payload basique (`<script>alert(...)`) a été bloqué par les filtres du navigateur. Un payload de contournement a donc été utilisé.
-   **Payload de Contournement :** `<iframe src="javascript:alert('XSS Reussi')"></iframe>`
-   **Résultat :** Le payload a été exécuté avec succès, déclenchant une alerte JavaScript dans le navigateur et confirmant la présence d'une vulnérabilité de type XSS Reflected.



## 🧠 Compétences acquises

-   Déploiement d'applications conteneurisées avec **Docker**.
-   Configuration et utilisation de **Burp Suite** pour l'interception et l'analyse de trafic HTTP.
-   Découverte de contenu par brute-force avec **Gobuster**.
-   Compréhension et exploitation pratique d'une vulnérabilité du **Top 10 OWASP** (Cross-Site Scripting).
-   Mise en œuvre de **techniques de contournement** de filtres de sécurité basiques.
