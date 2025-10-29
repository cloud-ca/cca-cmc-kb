---
title: "Configuration Client :  Windows"
slug: cca-config-client-windows
---

Ces instructions permettent de configurer le VPN d'accès distant avec le client natif de Windows 10 et 11.

#### Ajouter le certificat à la liste des certificats autorisés

1. Executer `mmc.exe`
1. Cliquer sur *Files > Add/Remove Snap-in…*
1. Sélectionner *Certificates* dans la liste à gauche puis cliquer sur *Add >*.
1. Dans la fenêtre qui apparaît, sélectionner **Computer account**.

![Certificates snap-in](/assets/Win-1-Computer-Account.png)

5. Dans la prochaine fenêtre, sélectionner **Local Computer: (the computer this console is running on)**  puis cliquer sur *Finish*.
5. Dans la fenêtre de la console, sélectionner *Console Root > Certificates (Local Computer) > Trusted Root Certification Authorities > Certificates*.
5. Faire un clic droit sur *Certificates* puis cliquer sur *All Tasks > Import…*.

![All tasks menu, import option](/assets/Win-2-Import.png)

8. Dans la prochaine fenêtre, cliquer sur *Next*.
8. Cliquer sur *Browse…* et sélectionner le certificat que vous avez enregistré avec l'extension **.crt** (le certificat est disponible dans l'interface de Hypertec Cloud, sur la page dédiée à la configuration du VPN d'accès distant) puis cliquer sur *Next*.

![Certificate import wizard](/assets/Win-3-Browse.png)

10. Garder **Place all certificates in the following store: Trusted Root Certification Authorities** puis cliquez sur *Next*
10. Cliquer sur *Finish*. Vous devriez voir le message *The import was successful*. Vous pouvez fermer les fenêtres ouvertes.
10. Si votre server est derrière un NAT, suivre cette procédure: [Microsoft link](https://support.microsoft.com/en-us/help/926179/how-to-configure-an-l2tp-ipsec-server-behind-a-nat-t-device-in-windows-vista-and-in-windows-server-2008)

#### Modifier le Registre Windows pour forcer un chiffrement fort

**Important :** La modification ci-dessous sera nécessaire pour la prochaine mise à jour de l’infrastructure infonuagique, actuellement prévue pour le 5 novembre 2025. Veuillez ne pas appliquer cette modification avant cette date.

Les utilisateurs de macOS ou Linux ne rencontreront pas ce problème. Ceux qui utilisent Windows 10 ou Windows 11 devront appliquer cette modification à partir du 5 novembre 2025. Par défaut, Windows 10 et Windows 11 utilisent un chiffrement faible lors de la configuration d'un VPN IKEv2, ce qui provoque une erreur de non-concordance de stratégie. Pour forcer Windows 10 et Windows 11 à utiliser un chiffrement plus robuste, il faut ajouter au Registre Windows la valeur `DWORD (32bit)NegotiateDH2048_AES256` avec la valeur `1` à la sous-clé `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\Parameters`.

Veuillez noter qu'une prochaine mise à jour nécessitera la mise en place de cette modification.

**Chemin :**
```
Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\Parameters
```

**Entrée :**
```
NegotiateDH2048_AES256 (DWORD 32 bits)
Valeur : 1
```

Les éléments suivants peuvent être ajoutés à un document texte, enregistrés avec l'extension `.reg`, puis importés en double-cliquant dessus :

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\Parameters]"
NegotiateDH2048_AES256"=dword:00000001
```

Un exemple de fichier de registre peut être téléchargé [ici](https://objects-qc.cloud.ca/v1/5f78f47d34e54036947d4b983e88f2e6/public/force_strong_cipher.reg).

#### Créer la connexion VPN:
[Paramètres](/assets/Win-4-Settings.png)

![VPN](/assets/Win-5-VPN.png)

![Ajouter connexion](/assets/Win-6-Add-Connection.png)

![Détails sur la connexion](/assets/Win-7-Connection-Details.png)

![Sélectionner la connexion](/assets/Win-8-Select-Connection.png)


#### Initialiser la connexion VPN:
![Se connecter](/assets/Win-9-Connect.png)

![Connectée](/assets/Win-10-Connected.png)
