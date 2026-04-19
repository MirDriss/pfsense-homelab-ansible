# 🚀 pfSense Homelab Automation (Ansible)

Ce dépôt contient le code d'infrastructure (IaC) permettant d'automatiser et de configurer de manière idempotente un routeur pfSense. Il est spécialement conçu pour sécuriser et router l'accès à un environnement Homelab (Proxmox) via un tunnel OpenVPN.

## 📝 Contexte du Projet

L'objectif de ce projet était de passer d'une configuration réseau manuelle à une approche **100% Infrastructure as Code (IaC)**. 
Grâce à Ansible et à une structure basée sur des rôles, la configuration du routeur est modulaire, versionnée et facilement reproductible en cas de crash ou de migration.

### ✨ Fonctionnalités principales
- **Configuration WAN Statique :** Attribution d'IP et création de la passerelle par défaut.
- **NAT Hybride :** Maquillage des requêtes VPN pour permettre la communication avec les VM Proxmox (résolution du problème de routage asymétrique).
- **Routage OpenVPN :** Injection automatique de la route réseau locale (`IPv4 Local Network`) vers les clients connectés (Split-Tunneling).
- **Idempotence :** Utilisation de scripts PHP embarqués (Scouts) pour garantir que le playbook ne modifie l'état de pfSense que si c'est strictement nécessaire.

## 🏗️ Architecture du projet

Le projet est structuré autour du rôle `pfsense_vpn` pour séparer les variables de la logique :

```text
📁 pfsense-homelab-ansible/
├── 📄 inventory.ini               # Inventaire des cibles réseau
├── 📄 deploy_pfsense.yml          # Playbook principal (Master)
└── 📁 roles/
    └── 📁 pfsense_vpn/
        ├── 📁 tasks/
        │   ├── 📄 main.yml        # Orchestrateur du rôle
        │   ├── 📄 interfaces.yml  # Configuration WAN & Passerelle
        │   ├── 📄 routing.yml     # Règles Firewall & NAT Hybride
        │   └── 📄 users_vpn.yml   # Déclaration des accès VPN
        └── 📁 vars/
            └── 📄 main.yml        # Variables (IPs, réseaux, noms d'utilisateurs)
