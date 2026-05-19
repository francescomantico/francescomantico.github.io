# Ansible Cloud Lab: Managing Ubuntu and RHEL on Oracle Cloud

A practical project to set up a centralized Ansible control plane in WSL to manage a mixed-OS environment on Oracle Cloud (OCI) Free Tier.

### Il Progetto

Un progetto pratico per configurare un pannello di controllo centralizzato con Ansible all'interno di WSL, per gestire un ambiente con sistemi operativi misti su Oracle Cloud (OCI) Free Tier.

L'obiettivo principale era gestire gli aggiornamenti dei pacchetti e distribuire Docker su una nuova istanza, il tutto senza toccare o rischiare di rompere un container Nextcloud gia' attivo e in produzione su un nodo piu' vecchio.

### La Struttura
* **Control Node:** server locale con WSL (Ubuntu).
* **Nodo 1 (Ubuntu):** La mia istanza di produzione che ospita Nextcloud.
* **Nodo 2 (Oracle Linux/RHEL):** Una nuova istanza pulita dove dovevo installare Docker.

---

### Configurazione infrastruttura e SSH

Dato che Oracle Cloud genera chiavi SSH diverse per ogni istanza se non vengono create nello stesso identico momento, non ho potuto usare una chiave globale. Ho mappato gli utenti e le chiavi private specifiche direttamente nel file di inventario hosts.ini.

#### 1. Spostare e proteggere le chiavi in WSL
Ho copiato i file .key scaricati dal Desktop di Windows alla cartella home di WSL e ho ristretto i permessi di lettura, passaggio obbligatorio altrimenti OpenSSH rifiuta la connessione.

```bash
cp /mnt/c/Users/franc/Desktop/ubuntu_folder/ssh-key-2026-05-17.key ~/.ssh/oracle_key
cp /mnt/c/Users/franc/Desktop/rhel_folder/ssh-key-2026-05-18.key ~/.ssh/rhel_key

chmod 600 ~/.ssh/oracle_key
chmod 600 ~/.ssh/rhel_key```