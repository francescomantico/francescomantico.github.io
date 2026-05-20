# Ansible Cloud Lab: Managing Ubuntu and RHEL on Oracle Cloud

A practical project to set up a centralized Ansible control plane in WSL to manage a mixed-OS environment on Oracle Cloud (OCI) Free Tier.

## Il Progetto / Context

Un progetto pratico per configurare un pannello di controllo centralizzato con Ansible all'interno di WSL, per gestire un ambiente con sistemi operativi misti su Oracle Cloud (OCI) Free Tier.

**L'obiettivo principale** era automatizzare la gestione degli aggiornamenti dei pacchetti OS e distribuire Docker su una nuova istanza, il tutto garantendo l'assoluta stabilità di un container Nextcloud già attivo e in produzione su un nodo più vecchio.

## a Struttura dell'Infrastruttura

*   **Control Node:** Server locale con WSL (Ubuntu).
*   **Nodo 1 (Ubuntu):** Istanza di produzione che ospita Nextcloud.
*   **Nodo 2 (Oracle Linux / RHEL):** Nuova istanza pulita dedicata al provisioning di Docker.

## Configurazione Infrastruttura e SSH 

Dato che Oracle Cloud genera chiavi SSH diverse per ogni istanza se non vengono create nello stesso identico momento, non ho potuto usare una singola chiave globale. 

Per risolvere il problema senza compromettere la sicurezza, ho gestito la separazione in due step:

### 1. Protezione delle chiavi in WSL
Ho trasferito le chiavi scaricate da Windows all'interno del file system nativo di Linux (nella directory `~/.ssh/`) e ho ristretto i permessi di lettura. Questo passaggio è obbligatorio, altrimenti OpenSSH rifiuta la connessione per policy di sicurezza.

```bash
# Spostamento delle chiavi da Windows a WSL
cp /mnt/c/Users/franc/Desktop/ubuntu_folder/ssh-key-2026-05-17.key ~/.ssh/oracle_key 
cp /mnt/c/Users/franc/Desktop/rhel_folder/ssh-key-2026-05-18.key ~/.ssh/rhel_key

# Configurazione dei permessi restrittivi
chmod 700 ~/.ssh
chmod 600 ~/.ssh/oracle_key 
chmod 600 ~/.ssh/rhel_key
```
