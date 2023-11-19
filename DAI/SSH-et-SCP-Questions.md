# SSH et SCP Questions

**Qu'est-ce que SSH ?**
- SSH (Secure Shell) est un protocole de réseau sécurisé qui permet d'établir des connexions cryptées entre des ordinateurs distants.   
  Il est utilisé pour l'accès à distance sécurisé, l'exécution de commandes distantes et le transfert de fichiers.   

**Comment fonctionne SSH ?**
- SSH fonctionne en établissant une connexion chiffrée entre un client et un serveur.   
  Il utilise des clés cryptographiques pour l'authentification et le chiffrement des données échangées, garantissant ainsi la sécurité des communications.   

**Comment copier des fichiers depuis/vers une machine distante avec SCP ?**
- SCP (Secure Copy Protocol) permet de transférer des fichiers de manière sécurisée entre des ordinateurs distants via une connexion SSH.   
  Pour copier un fichier depuis la machine locale vers une machine distante, utilisez la commande scp fichier_local utilisateur@adresse_serveur:chemin_de_destination.   
  Pour copier depuis une machine distante vers la machine locale, utilisez scp utilisateur@adresse_serveur:chemin_distant fichier_local.   

**Pourquoi n'est-il pas recommandé d'utiliser un mot de passe pour se connecter à une machine distante ?**
- Utiliser un mot de passe pour se connecter à distance présente des risques de sécurité, car les mots de passe peuvent être sujets à des attaques par force brute ou à des tentatives de phishing.   
  Les clés SSH offrent une méthode plus sécurisée d'authentification sans nécessiter de mot de passe.   

**Quelle est la différence entre la clé privée et la clé publique ?**
- La clé privée et la clé publique sont des composants d'une paire de clés SSH.   
  La clé publique est partagée avec d'autres et est utilisée pour chiffrer les données.   
  La clé privée est conservée en sécurité sur l'ordinateur local et permet de déchiffrer les données cryptées par la clé publique correspondante.   

**Quelle est la différence entre la clé privée et la phrase de passe ?**
- La clé privée est une composante d'une paire de clés SSH utilisée pour l'authentification.   
  Elle est stockée sur l'ordinateur local.   
  La passphrase est un mot de passe utilisé pour protéger la clé privée.   
  Elle ajoute une couche de sécurité en empêchant l'accès non autorisé à la clé privée, même si celle-ci est compromise ou volée.   

