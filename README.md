# Huber-Congo (prototype) — Application de mobilité pour le Congo-Brazzaville

But
- Créer un service de mise en relation passagers / conducteurs (type “Uber”) adapté au contexte du Congo-Brazzaville (Brazzaville & Pointe-Noire).
- Priorité : accessibilité pour tous, fiabilité GPS, intégration Mobile Money, simplicité d’interface et faible consommation de données.

Objectifs du dépôt
- Contenir le code client (Android/iOS), le backend et l’admin dashboard.
- Héberger la documentation, les spécifications MVP et le backlog d’issues.

MVP (résumé)
- Application Passager : inscription, géolocalisation, demander course, estimation tarifaire, suivi en temps réel, paiement via Mobile Money (pas d'espèces).
- Application Conducteur : inscription/KYC basique, réception de courses, navigation vers le passager, statut (disponible/indisponible).
- Backend : gestion courses, matching basique, historique, notifications push, dashboard admin pour surveiller opérations.

Tech stack recommandé (exemple)
- Mobile : React Native ou Flutter (gain de temps multi‑plateforme) — ciblage Android + iOS (tous types de téléphones).
- Backend : Node.js (NestJS / Express) ou Django REST Framework.
- Base : PostgreSQL + Redis (caching / matching en temps réel).
- Temps réel : WebSocket (Socket.IO) ou MQTT pour le tracking.
- Cartographie : Mapbox (coûts et contrôle) ou Google Maps si licences OK.
- Paiements : intégration Mobile Money local (MTN MoMo / Airtel Money / Wave). Le système applique une commission fixe (500 XAF) par transaction MoMo vers le numéro de la plateforme.
- Hébergement : AWS/GCP (région la plus proche), ou OVH / Scaleway si préférence européenne.

Livrables initiaux
- README (ce fichier)
- MVP_SPEC.md (détails des fonctions, API, priorités)
- Backlog initial (issues) — prêt à être créé

Contribution
- Issues et PRs bienvenues. Voir /docs pour spécs détaillées.