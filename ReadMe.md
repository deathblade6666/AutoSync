# Script d'autosync FSM pour ArmaLife 4.4R4
---

**Fonctionnement :**

- La configuration se fait dans le config_Master, les options de configuration suivantes sont disponibles :  
>1. Activation/Désactivation du système  
>2. Configuration du délai avant première sync  
>3. Configuration de la valeur d’attente de base  
>4. Configuration de la valeur d’attente random  
- Le délai avant le syncdata est aléatoire pour chaque joueur pour éviter
plusieures requêtes simultanées. Ce délai est la somme d’une variable fixe et d’un random sur une valeur définie.  
- Dans le cas où le joueur aurai déjà sync manuellement dans les 5 dernières minutes, l’autosync n’est pas executé.  
- L'état et les actions de l'autosync sont loggées dans le RPT.  

**Instructions d'installation :**

1.  Dans le **\config\Config_Master.hpp**  
Cherchez :
```
   /* Database Related Settings */  
     /* Player Data Saving */
```        
	
Rajoutez dans cette section :
```
  /* AutoSync Settings */  
    	enable_autoSync = true; // Autosync activé ?    
    	first_autoSync_delay = 600; // Délai avant premier autoSync   
    	base_time = 600; // Temps minimum avant sync    
    	random_time = 300; // Valeur random ajoutée au délai de base avant sync    
```

2. Dans **\core\init.sqf**
	
Coller tout en bas juste avant les lignes “diag_log” :
```
// Check si autoSync activé ?  
if ((LIFE_SETTINGS(getNumber,"enable_autoSync") isEqualTo 1) && (LIFE_SETTINGS(getNumber,"base_time") >= 600 )) then {
	[] execFSM "core\fsm\autoSync.fsm";  
	diag_log "Starting FSM-based autoSync...";  
} else {    
	diag_log "AUTOSYNC : AutoSync disabled dans la config ou valeur de base_time incorrecte (min 600), abandon...";  
};  
```
