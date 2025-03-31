# SSH-SANS-MDP
Tuto SSH sans MDP

# DEMO SSH AVEC IDENTIFICATION AUTO

## UBUNTU -> UBUNTU

### ON EXECUTE TERMINAL SUR LES DEUX MACHINES

1. Vérification d'openssh serveur sur le client :**
   ```bash
  sshd -V                              # Pour vérifier si installé
 sudo apt install openssh-server      # Si pas installé
 sshd -V                              # Pour vérifier
 systemctl status ssh                 # Vérifier si activé
 systemctl enable ssh                 # Rend actif au démarrage
  systemctl status ssh
  systemctl status sshd                # Tout doit être vert et enable

   2 Mettre sur le même réseau IP (VM en pont) et redémarrer le service :

  sudo systemctl restart Networking (ou NetworkManager)
ip a                                 # Pour récupérer IP
ping [adresse IP]                    # Vérifier la communication

 3 Se connecter depuis le serveur pour vérifier :

ssh client@[IP]                      # Confirmer avec "oui" et entrer le mdp
exit

 4 Créer une clé SSH :

ssh-keygen -t ecdsa
# Passphrase non obligatoire
ls -a                                # Vérifier le fichier .ssh (lieu de stockage)

 5 Déployer la clé SSH publique vers le client :

ssh-copy-id -i /home/user/.ssh/id_ecdsa.pub client@[IP]
# Entrer le mot de passe, se connecter

 6 Vérifier la clé sur les machines :

cat /home/wcs/.ssh/id_ecdsa.pub      # Sur le serveur
cat .ssh/authorized-keys             # Sur le client

 7 Se connecter sans mot de passe :

ssh client@[IP]

WINDOWS -> WINDOWS
ON EXECUTE POWERSHELL EN ADMIN SUR LES DEUX MACHINES
1 Vérification des services clients :

get-service ssh-agent

2 Activer la fonctionnalité client SSH :

Système > Fonctionnalités facultatives > Client SSH.

3 Vérification et activation du serveur SSH sur le client :

get-service sshd                        # Vérifier si activé
get-service sshd | Set-Service -StartupType automatic
Restart-Service sshd                    # Démarrer
get-service sshd                        # Vérifier

** 4 Mettre sur le même réseau IP (VM en pont) :

ipconfig                                # Récupérer l'IP du client

 5 Tester la connexion SSH depuis le serveur :

ssh client@[IP] powershell              # Connecter, entrer le mot de passe
exit

 6 Créer une clé SSH sur le serveur :

ssh-keygen -t ecdsa
Set-Location C:\Users\client\.ssh
Get-ChildItem                           # Vérifier la clé

 7 Déployer la clé SSH publique vers le client :

Get-Content -Path .\.ssh\id_ecdsa.pub   # Copier la clé
ssh client@[IP]                         # Connecter en SSH au client
Add-Content -Path .ssh\authorized_keys -Value "clé copiée"

 8 Configurer le client (fichier masqué) :

C:\ProgramData\ssh\sshd_config
# Commenter MATCH group administrator
Restart-Service sshd

 9 Connexion SSH sans mot de passe :

ssh client@[IP]

UBUNTU -> WINDOWS
TERMINAL & POWERSHELL EN ADMIN SUR LES MACHINES RESPECTIVES

 1 Connexion SSH :

ssh client@[IP] powershell             # Tester la connexion
exit

2 Déployer la clé depuis Ubuntu vers Windows :

cat .ssh/id_ecdsa.pub                   # Copier la clé
ssh client@[IP]                         # Connecter en SSH au client
Add-Content -Path .ssh\authorized_keys -Value "clé copiée"

 3 Configurer le client :

C:\ProgramData\ssh\sshd_config
# Commenter MATCH group administrator
Restart-Service sshd

4 Connexion SSH sans mot de passe :

ssh client@[IP]

WINDOWS -> UBUNTU
TERMINAL & POWERSHELL EN ADMIN SUR LES MACHINES RESPECTIVES

 1 Connexion SSH :

ssh client@[IP]
exit
2 Déployer la clé depuis Windows vers Ubuntu :

Get-Content -Path .\.ssh\id_ecdsa.pub   # Copier la clé
ssh client@[IP]                         # Connecter en SSH au client
nano .ssh/authorized_keys               # Coller la clé

** 3 Connexion SSH sans mot de passe :

ssh client@[IP]
