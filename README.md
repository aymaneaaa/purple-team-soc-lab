\# Purple Team SOC Lab



Lab virtualisé (VirtualBox) pour simuler des attaques et écrire des détections avec Wazuh.



\## Architecture



\- pfSense CE : segmentation WAN / LAN / DMZ

\- Suricata (IDS) sur l'interface WAN

\- Wazuh (Manager + Indexer + Dashboard) : collecte et analyse des alertes

\- Honeypot Python en DMZ (port 2222)

\- Kali Linux : machine d'attaque



\## Pipeline de détection



Kali -> pfSense -> Suricata -> Syslog -> Wazuh (décodeur + règles custom) -> Discover



\## Contenu du repo



\- wazuh/local\_decoder.xml : décodeur des alertes Suricata envoyées par pfSense

\- wazuh/local\_rules.xml : règles custom

\- docs/ : architecture et rapports (à venir)



\## Règles de détection



\- 100100 : alerte Suricata générique reçue via pfSense

\- 100101 : anomalie SSH sur le honeypot, tag MITRE ATT\&CK T1021.004



\## Detection engineering log



\- Règle 100100 trop large : le prematch du décodeur captait les logs cron de pfSense (227 fausses alertes). Corrigé avec un prematch qui n'accepte que le format d'alerte Suricata.

\- Règle 100101 ajoutée et testée avec wazuh-logtest.

\- Limite connue : l'alerte SID 2228000 est une anomalie de bannière SSH, pas la preuve d'un mouvement latéral. Le tag T1021.004 est une association raisonnable, à affiner.



\## Prochaines étapes



\- Phishing simulé

\- Keylogger + exfiltration

\- Règle scan de ports (T1046)

\- Tableau de couverture MITRE ATT\&CK

\- Active Response et automatisation SOAR

