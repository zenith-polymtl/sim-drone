# Installation des logiciels requis 

Ce guide explique étape par étape comment installer les programmes requis pour notre projet : **WSL**, **ROS 2 Humble**, **QGroundControl** et **PX4**. Des placeholders sont laissés pour insérer des images et des liens vers les sites officiels.


## 🖥️ 1. Installation de WSL (Windows Subsystem for Linux)

### 👉 Étape 1 : Activer WSL
1. **Ouvrir un terminal** (PowerShell) et exécuter la commande suivante :
   ```bash
   wsl --install
   wsl --update
   ```

   ![Placeholder Image: Installation WSL](/docs/images/wsl-install.png)

### 👉 Étape 2 : Activer les fonctionnalités requises
1. Ouvrir **"Turn Windows features on or off"** (Activer ou désactiver des fonctionnalités Windows).
2. Cocher les options suivantes :
   - ✅ **Virtual Machine Platform**
   - ✅ **Windows Hypervisor Platform**
   - ✅ **Windows Subsystem for Linux**
3. **Redémarrer l'ordinateur** après avoir activé ces fonctionnalités.

   ![Placeholder Image: Activer fonctionnalités Windows](/docs/images/features-on.png)



## 🛒 2. Installation d'Ubuntu 22.04 via Microsoft Store

### 👉 Étape 1 : Télécharger Ubuntu
1. Ouvrir le **Microsoft Store** et rechercher **Ubuntu 22.04**.
2. **Installer** l'application Ubuntu 22.04 LTS.

### 👉 Étape 2 : Configuration initiale
Lors du premier lancement, suivre les étapes :
   - Entrer un **nom d'utilisateur**.
   - Entrer un **mot de passe**.

   ![Placeholder Image: Ubuntu 22.04 Microsoft Store](/docs/images/ubuntu-install.png)

**Si vous êtes connecté directement en tant que root, vous devez créer votre propre utilisateur avec des privilèges administratifs**
1. Créer un nouvel utilisateur :
   ```bash
   adduser <nom_utilisateur>
   ```

2. Donner les privilèges sudo à l'utilisateur
   ```bash
   usermod -aG sudo <nom_utilisateur>
   ```

3. Dans Powershell
   ```bash
   Ubuntu-22.04 config --default-user <nom_utilisateur>
   ```

### 👉  3. Mise à jour d'Ubuntu 22.04

Dans le terminal Ubuntu, exécuter les commandes suivantes pour mettre à jour les paquets :
```bash
sudo apt update
sudo apt upgrade
```
dduser <username> sudo

## 🤖 3. Installation de ROS 2 Humble

### 👉 Étape 1 : Suivre les instructions officielles
1. Suivre les étapes d'installation de ROS 2 Humble sur le [site officiel](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html).

⚠️ **Attention** : Installer la version **Desktop** avec cette commande :
   ```bash
   sudo apt install ros-humble-desktop
   ```

### 👉 Étape 2 : Configurer l'environnement
Pour sourcer ROS automatiquement à chaque session, ajouter cette ligne à votre **~/.bashrc** :
```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 📦 Étape 3 : Installer les dépendances Python
```bash
sudo apt install python3-pip
pip install --user -U empy==3.3.4 pyros-genmsg setuptools==67.7.2
sudo apt install python3-colcon-common-extensions
sudo apt install python3-argcomplete
```


## 🛠️ 4. Installation de PX4 

### 🚁 Étape 1 : Cloner le référentiel PX4
Cloner le dépôt PX4-Autopilot depuis GitHub dans un répertoire de votre choix :
```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive -b release/1.14
```

### 📦 Étape 2 : Installer les dépendances PX4
Se déplacer dans le répertoire cloné et exécuter le script d'installation des dépendances :
```bash
cd PX4-Autopilot
bash ./Tools/setup/ubuntu.sh
```

Reboot dans PowerShell
```bash
wsl --shutdown
wsl -d Ubuntu-22.04
```

### 📦 Étape 3 : Installer MicroDDS pour Gazebo
```bash
git clone https://github.com/eProsima/Micro-XRCE-DDS-Agent.git
cd Micro-XRCE-DDS-Agent
mkdir build
cd build
cmake ..
make
sudo make install
sudo ldconfig /usr/local/lib/
```


## 🛠️ 5. Installation de QGroundControl
Télécharger **QGroundControl** depuis le [site officiel](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html#ubuntu) et suivre les instructions d'installation.

Copier/coller l'image QGroundControl téléchargée dans votre environnement Linux
   
   ![Placeholder Image: QGroundControl dans l'environnement Linux](/docs/images/qground-folder-linux.png)


## ✅ 6. Vérification de l'installation

### 🔍 Créer un Workspace, clone les exemples et build
```bash
mkdir -p ~/ws_ros2/src/
cd ~/ws_ros2/src/
git clone https://github.com/PX4/px4_msgs.git
git clone https://github.com/PX4/px4_ros_com.git
cd ..
colcon build
```
```bash
echo "export DISPLAY=:0" >> ~/.bashrc
source ~/.bashrc
```
```bash
cd ~/PX4-Autopilot
make px4_sitl gz_x500
```

Dans un autre terminal
```bash
MicroXRCEAgent udp4 -p 8888
```

Dans un autre terminal, rouler l'exemple Offboard
```bash
cd ~/ws_ros2
source install/local_setup.bash
ros2 run px4_ros_com offboard_control
```


## 🔗 Liens Utiles

- 🌐 **ROS 2 Humble Installation** : [Site officiel ROS 2](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)
- 🌐 **Docs PX4** : [PX4 User Guide](https://docs.px4.io/main/en/ros2/user_guide.html)
- 🌐 **Docs Gazebo** : [Gazebo Binary Installation](https://gazebosim.org/docs/harmonic/install_ubuntu/)
- 🌐 **QGroundControl** : [Téléchargement QGroundControl](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html#ubuntu)


---

✨ **Bon courage pour l'installation !** 🚀