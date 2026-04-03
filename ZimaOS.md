# Tutoriel : Virtualisation de Home Assistant sur ZimaOS (Zimaboard 2)
Voici l'ensemble des commandes nécessaires pour transférer votre Dongle USB Zigbee vers votre Machine Virtuelle Home Assistant et activer le démarrage automatique.

## Étape 1 : Identifier votre Dongle USB
Une fois connecté en SSH sur votre ZimaOS et passé en mode administrateur (`sudo -i`), tapez la commande suivante pour lister les périphériques USB :

```
lsusb
```

Repérez la ligne correspondant à votre dongle Zigbee (ex: Sonoff, SkyConnect) et notez l'ID (les deux suites de 4 caractères séparées par deux points, par exemple `1a86:55d4`).

## Étape 2 : Créer le fichier de configuration XML
Nous allons créer un fichier pour indiquer à ZimaOS comment gérer ce périphérique.

```
nano /DATA/VM/zigbee.xml
```

Copiez et collez le code ci-dessous dans l'éditeur. **Attention :** Pensez à remplacer `0x1a86` et `0x55d4` par les valeurs de votre propre dongle trouvées à l'étape 1.

```
<hostdevmode='subsystem'type='usb'>
<source>
  <vendorid='0x1a86'/>
    <productid='0x55d4'/>
  </source>
</hostdev>
```

**Pour sauvegarder et quitter :**
1. Appuyez sur `Ctrl + O` puis *Entrée* pour sauvegarder.
2. Appuyez sur `Ctrl + X` pour quitter l'éditeur.

## Étape 3 : Associer le Dongle à la Machine Virtuelle
Affichez la liste de vos machines virtuelles pour récupérer le nom exact de votre VM Home Assistant :

```
virsh list --all
```

Attachez ensuite le périphérique USB à votre VM (remplacez `NOM_DE_TA_VM` par le nom trouvé à la commande précédente) :

```
virsh attach-device NOM_DE_TA_VM --file /DATA/VM/zigbee.xml --persistent
```

Une confirmation devrait s'afficher à l'écran.

## Étape 4 : Activer le démarrage automatique
Pour vous assurer que Home Assistant redémarre tout seul en cas de coupure de courant, lancez cette dernière commande (en remplaçant toujours `NOM_DE_TA_VM`) :

```
virsh autostart NOM_DE_TA_VM
```
