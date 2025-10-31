# ffSend Blabla Linux

Utilisation en ligne de commande du service Send Blabla Linux (https://send.blablalinux.be).

• send
• ffsend
• share
• file

```plaintext
# Modifications apportées par Blabla Linux : https://link.blablalinux.be

# GitHub : https://github.com/timvisee/ffsend
# Serveur : https://send.blablalinux.be

# Installation de ffsend snap
sudo apt update
sudo apt install snapd
# OU
# su root
# apt update
# apt install snapd
sudo systemctl reboot
sudo snap install snapd
sudo snap install ffsend

# Téléversement (nombre par défaut de téléchargement : 1 / temps par défaut d'expiration du lien : 1 jour)
# u = upload
# -h = host
ffsend u -h https://send.blablalinux.be/ my-file
# En modifiant le nombre et le temps
ffsend u --downloads 5 --expiry-time 30m -h https://send.blablalinux.be/ my-file
# En demandant une copie du lien de partage dans le presse papier
ffsend u --downloads 5 --expiry-time 30m --copy -h https://send.blablalinux.be/ my-file
# En archivant l'envoi
ffsend u --downloads 5 --expiry-time 30m --archive --copy -h https://send.blablalinux.be/ my-file
# Avec une protection par mot de passe
ffsend u --downloads 5 --expiry-time 30m --password --archive --copy -h https://send.blablalinux.be/ my-file
# Ouvrir le lien partagé dans le navigateur
ffsend u --downloads 5 --expiry-time 30m --password --archive --copy --open -h https://send.blablalinux.be/ my-file

# Téléchargement
# d = download
# -h = host
ffsend d https://send.blablalinux.be/share-url

# Inspecter fichier distant
# Vérifier si un fichier existe
# e = exist
ffsend e https://send.blablalinux.be/share-url
# Récupérer les informations du fichier distant
# i = info
ffsend i https://send.blablalinux.be/share-url

# Autres commandes
# Consultez l'historique de vos fichiers
# h = history
ffsend h
# Modifier le mot de passe après le téléchargement
# p = password
ffsend password https://send.blablalinux.be/share-url
# Supprimer un fichier
# rm ou del = delete
ffsend rm https://send.blablalinux.be/share-url

# Aide
ffsend --help
USAGE:
    ffsend [FLAGS] [OPTIONS] [SUBCOMMAND]
FLAGS:
    -f, --force          Force the action, ignore warnings
    -h, --help           Prints help information
    -i, --incognito      Don't update local history for actions
    -I, --no-interact    Not interactive, do not prompt
    -q, --quiet          Produce output suitable for logging and automation
    -V, --version        Prints version information
    -v, --verbose        Enable verbose information and logging
    -y, --yes            Assume yes for prompts
OPTIONS:
    -A, --api <VERSION>                 Server API version to use, '-' to lookup [env: FFSEND_API]
        --basic-auth <USER:PASSWORD>    Protected proxy HTTP basic authentication credentials (not FxA) [env: FFSEND_BASIC_AUTH]
    -H, --history <FILE>                Use the specified history file [env: FFSEND_HISTORY]
    -t, --timeout <SECONDS>             Request timeout (0 to disable) [env: FFSEND_TIMEOUT]
    -T, --transfer-timeout <SECONDS>    Transfer timeout (0 to disable) [env: FFSEND_TRANSFER_TIMEOUT]
SUBCOMMANDS:
    upload        Upload files [aliases: u, up]
    download      Download files [aliases: d, down]
    debug         View debug information [aliases: dbg]
    delete        Delete a shared file [aliases: del, rm]
    exists        Check whether a remote file exists [aliases: e]
    generate      Generate assets [aliases: gen]
    help          Prints this message or the help of the given subcommand(s)
    history       View file history [aliases: h]
    info          Fetch info about a shared file [aliases: i]
    parameters    Change parameters of a shared file [aliases: params]
    password      Change the password of a shared file [aliases: pass, p]
    version       Determine the Send server version [aliases: v]
```
