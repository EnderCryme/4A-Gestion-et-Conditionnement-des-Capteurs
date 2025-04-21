# Projet Capteur - LTspice

Ce dépôt contient les fichiers LTspice utilisés pour la prévalidation de nos montages électroniques en vue de la conception de la carte sous KiCad. Cette étape a été déterminante pour garantir une carte 100 % fonctionnelle et ainsi limiter les erreurs liées à la phase de conception.

## Fichiers disponibles

> **Resistive.asc** : Ce fichier correspond au montage de type résistif, décomposé en plusieurs sous-étages :
  1. Convertisseur Tension-Courant
  2. Amplificateur d'instrumentation
  3. Deux circuits de soustraction différents (afin de comparer plusieurs solutions)

> **Capacitif.asc** : Ce fichier comprend les montages basés sur le NE555, en configuration monostable et astable.
