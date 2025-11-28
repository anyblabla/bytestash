# Emmabuntus dock sous Emmabuntus DE3 1.00 | patch correctif

En rapport avec une vidéo publié sur PeerTube Blabla Linux : https://peertube.blablalinux.be/w/oigfQeX636BXp8LPZVfxog

• emmabuntus
• dock

```plaintext
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# En rapport avec une vidéo publié sur PeerTube Blabla Linux : https://peertube.blablalinux.be/w/oigfQeX636BXp8LPZVfxog

sudo geany /usr/bin/init_cairo_dock_exec.sh

if [[ ${1} == root ]] ; then
dir_user=/root
else
dir_user=/home/${1}
fi

sudo cp -R /etc/skel/.config/cairo-dock-language/ ${dir_user}/.config/
sudo chown -R ${1}:${1} ${dir_user}/.config/cairo-dock-language/
```
