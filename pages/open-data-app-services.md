# Open Data App Services

Jede App kann auf Dienste im ODAS zurückgreifen. 

Folgende Dienste sind verfügbar:

* Email-Service
* AI-Service

## Email-Service

Mit diesem Service wird eine Email an die vom ODP-Betreiber konfigurierte Email-Adresse
geschickt. Das ist hilfreich, wenn Feedback von einer App an den ODP-Betreiber erwünscht ist, 
z.B. nach dem Ausfüllen von Formularen.

URL: `https://open-data-app-store.ckan.de/view/<odp-name>/<app-name>/<instanz-id>/mail`

Parameter: 
* content: string
* emailCC: string (optional)
* userIP: string (optional)

## AI-Service

Mit diesem Service kann eine App auf einen Künstlicher-Intelligenz-Dienst zugreifen.

Details kommen hierzu noch...




