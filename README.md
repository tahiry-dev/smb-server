## SERVEUR SAMBA
![rsz_samba](https://user-images.githubusercontent.com/47100064/173196449-66806857-39d8-434c-bf51-8d8b47bffc9e.png)

### SPECIFICITE DE SAMBA

  C'est un serveur qui permet l'adaptation avec d'autre système d'exploitation. 
  Par Exemple, le partage de donnée entre Windows et Linux ou entre Linux et MacOS 
  
### INSTALLATION

  1. Se connecter en tant que super utilisateur, la commande dépendera de votre système Linux (su ou sudo) 
  
  2. Mettre a jour la liste des paquets d'installation
 
        `apt-get update`
        
  3. Installer samba avec APT

        `apt-get install -y samba`
        
  4. Vérification de l'installation avec la commande

        `smbd --version`
        
  5. Vérification de l'état du serveur si celui-ci est active ou pas

        `systemctl status smbd`
       
  6. Activation automatique du serveur
      
        `systemctl enable smbd`
        
        
### CONFIGURATION POUR LE PARTAGE DE DOSSIER 

La configuration se fait en deux étapes:
1. Configuration  de smb.conf
2. La préparation du groupe / utilisateur / dossier du partage

I-  Configuration  de smb.conf

smb se trouve dans "/etc/samba/smb.conf" , pour l'éditer, il faut utiliser la commande suivante

`nano /etc/samba/smb.conf`

Copier dans les lignes suivantes suivante:

      [partage]
      
          comment = Partage de données
                
          path = /srv/partage
                
          guest ok = no
                
          read only = no
                
          browsable = yes
                
          valid users = @partage
          
 L'instruction précédente spécifie le nom et la description du partage, le chemin du dossier à partager, ainsi que les permissions.
 
 Après cette modification, il faut re-démarrer le serveur avec la commande suivante
 
    `systemctl restart smbd`
    
 II- Préparation du groupe / utilisateur / dossier du partage.
 
 Créer un groupe et un utilisateur du nom de "HEI-partage" qui sera membre de ce groupe
    
    `adduser hei-partage`
    
ce commande va nous demander d'ajouter un mot de passe, ainsi que différent informations qui peuvent être laissé par défaut ou complété

Ensuite il faut procédé à la création du groupe partage, mentioné dans le fichier conf smb.conf
  
   `groupadd partage`
   
Il faut ajouter maintenant notre utilisateur HEI-partage dans le groupe partage

  `gpasswd -a hei-partage partage`
  
Et enfin, il faut préparer le dossier partage par la création du dit dossier, ajout de propriétaire et attribution de permission 

   `mkdir /srv/partage`

   `chgrp -R partage /srv/partage/`

   `chmod -R g+rw /srv/partage/`
   
 On peut désormais vérifier les permissions avec la commande
    
    `ls -l /srv/`
    
 Le dossier est maintenant visible sous-windows
 

