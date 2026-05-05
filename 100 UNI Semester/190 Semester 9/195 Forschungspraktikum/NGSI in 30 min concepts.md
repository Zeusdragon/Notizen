# Datenmodell
![[Pasted image 20250927132110.png]]

1. Entität gibt die **id** **type** und **scope** an 
2. Attribute teil der entität bspw sensor werte oder so 

Darstellung entität heater hat typ Heater und hat id irgendeine urn heater hat aber auch attribut wärmemenge mit type und wert wärmemenge könnte noch ein unter attribut haben wie zeitpunkt mit observedAt oder einheit mit type und weert


## Payload

Payload ist json kann drei typen sein 
![[Pasted image 20250927133247.png]]
## @context 
Einfach gesagt eine Liste von Key Value paare wo die values auf internet seiten verweisen wo weiterer context steht


## Orion nutzen 
schauen ob context erreichbar dann entity erstellen
application/ld+json als Content-Type wichtig für header

durch Patch PUT oder POST befehl kann ich meine entitäten updaten wichtig für service 
entitäten lösche ich durch delete
![[Pasted image 20250927162326.png]]


![[Pasted image 20251004172513.png]]

![[Pasted image 20251004173504.png]]


