# Spécification MVP — Huber-Congo

Contexte
- Cible initiale : Brazzaville et Pointe‑Noire.
- Langues : Français, Lingala, Kituba et Anglais.
- Monnaie : XAF (CFA franc).

Priorités MVP (v1)
1. Core flow (essentiel)
   - Inscription passager (téléphone + OTP)
   - Inscription conducteur (téléphone + photo, voiture, plaque)
   - Passager : demander course (pickup + destination)
   - Matching basique : trouver conducteurs à proximité
   - Navigation et suivi temps réel (position du conducteur côté passager)
   - Tarif estimé (distance + temps)
   - Paiement : Mobile Money uniquement (MTN MoMo / Airtel Money / Wave). Le paiement client déclenche automatiquement le prélèvement d'une commission fixe de 500 XAF qui est transférée au numéro de téléphone de la plateforme (configurable dans l'admin). Aucune option "paiement en espèces" dans le MVP.
   - Notifications push (Firebase Cloud Messaging)
2. Admin & Opérations
   - Dashboard simple : liste courses, statut, utilisateur/driver, recherche
   - Configuration du numéro de plateforme pour la réception automatique des commissions MoMo
   - Logs et métriques basiques (nombre courses/jour)
3. Sécurité & conformité
   - KYC basique pour conducteurs (photo, N° téléphone)
   - Blocage/ban utilisateur
   - Protection des données personnelles (conformité basique)
4. Support & opérations
   - Chat ou numéro hotline (WhatsApp/SMS)
   - Onboarding conducteur (formation courte, docs PDF)

Fonctionnalités détaillées

- Passager (app)
  - Écran d’accueil avec position actuelle (GPS)
  - Entrer destination (suggestions, historique)
  - Estimation prix avant confirmation
  - Commande en 1 clic, annulation (règles limitées)
  - Suivi temps réel du conducteur
  - Historique courses et reçu numérique (incluant détail des frais : course, commission plateforme 500 XAF)
  - Paiement Mobile Money (intégration opérateurs locaux)
  - Évaluer course / laisser commentaire

- Conducteur (app)
  - Statut disponible / indisponible
  - Recevoir demande course (accept/decline)
  - Navigation intégrée (s’ouvrir dans Google Maps / Mapbox turn-by-turn)
  - Historique et gains (montant total perçu, déduction des commissions si applicable)
  - KYC et documents (photo, permis, plaque)
  - Chat/numéro de contact client (masqué via proxy si besoin)

- Backend API (exemples)
  - Auth : /auth/otp, /auth/verify
  - Users : GET/POST /users, /drivers
  - Trips : POST /trips (create), GET /trips/:id, PATCH /trips/:id/status
  - Matching : POST /match (basic nearest)
  - Payments : POST /payments/initiate, webhook /payments/callback (inclure logique de split: transférer 500 XAF vers platform_phone_number)
  - Notifications : /push/send

Architecture proposée (haute-niveau)
- Mobile <-> Backend (HTTPS + WebSocket pour position)
- Backend services : API Gateway -> Auth service, Trip service, Matching service, Payments connector
- DB : PostgreSQL, cache & queue : Redis (pub/sub), stockage fichiers (S3)
- Monitoring : Prometheus/Grafana + logs (ELK ou Cloud provider)

Aspects locaux et risques
- Cartes : vérifier couverture Mapbox/Google pour Brazzaville & Pointe-Noire ; prévoir gestion d’accuracy GPS.
- Paiements : accords avec opérateurs locaux peuvent prendre du temps (contrats + KYC entreprise). Le routage automatique d'une commission de 500 XAF par paiement MoMo nécessite un mécanisme de split de paiement ou des transferts programmés via l'API opérateur.
- Régulation : vérifier licences locales pour service de transport payant.
- Réseau : optimiser pour réseaux faibles (compressions, rafraîchissement position toutes les 5s par défaut).

Equipe et estimations (MVP)
- Équipe type (3-6 mois) : 1 PM/Product, 1-2 devs mobile, 1 dev backend, 1 devops, 1 QA, 1 UX/UI designer (part-time).
- Estimation coût (approx.) : USD 30k–120k selon localisation des devs, choix plateforme et intégrations.

Prochaines étapes concrètes
1. Valider périmètre fonctionnel (villes, types véhicules: taxis + voitures).
2. Choisir stack (React Native vs Flutter).
3. Rédiger API contract et ERD (schéma BDD).
4. Commencer sprint 0 : setup repo, CI/CD, provision infra, prototypes UI.
5. Implémenter v1 core flow (6–10 semaines).

Notes opérationnelles
- Config admin: numéro de téléphone plateforme (pour recevoir 500 XAF par paiement MoMo): +242067494837, clé API opérateurs Mobile Money, wallet d’entreprise.
- Respecter la législation locale concernant prélèvements automatiques et transparence tarifaire (afficher clairement la commission 500 XAF sur le reçu).