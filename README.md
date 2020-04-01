# External Integration
API di Cam.TV per l'accesso, autenticazione e collegamento dell'account Cam.TV di un utente direttamente al dominio dell'applicazione.

Le API consentono il login e la registrazione tramite credenziali Cam.TV, permettono di ottenere i dati anagrafici dell'utente e dei relativi sponsor. Offrono inoltre la possibilità di attribuire all'utente uno specifico abbonamento o di un upgrade dello stesso. 
Infine offrono la possibilità di scollegare l'utente dalla piattaforma rispetto a Cam.TV.

Il processo di autenticazione dell'utente coinvolge anche la parte client della piattaforma, in quanto proprio da questa si origina la richiesta iniziale. Cam.TV fornisce uno script javascript da inserire nella pagina, che esporta una funzione di inizializzazione del flusso di autenticazione dell'utente presso Cam.TV. Il flusso avviene in una finestra pop-up su domnio Cam.TV, alla conclusione il controllo ritorna alla finestra chiamante (il sito dell'applicazione), unitamente ad un TemporaryAuthToken che permette al server dell'applicazione di completare la procedura ed ottenere i dati dell'utente. La figura seguente illustra gli step previsti.

![alt flusso di autenticazione](https://bootcamp.r.worldssl.net/camtv_xnet_auth.jpg "Flusso di autenticazione")

# Implementazione Frontend
Per aggiungere il popup di login con Cam.TV, va inserito il seguente script:
	
	<script type="text/javascript" src="https://www.cam.tv/assets/js/camtv_login.js"></script>
	<div class="wrapper">
		<script>
			var settings = {
				RedirectURI: "{{RedirectURI}}",
				ApiKey: "{{SecretApiKey}}"
			}
		</script>
		<button onclick="CTVLogin.Login(settings);">Accedi con Cam.TV</button>
	</div>

## Le chiavi PrivateApiKey e PublicApiKey
Per autenticare l'utente e abilitarlo all'utilizzo delle API ExternalIntegration è necessario che la vostra piattaforma disponga delle chiavi PublicApiKey e PrivateApiKey. Queste chiavi sono essenzialmente delle credenziali che vengono fornite da parte di Cam.TV su richiesta. Queste chiavi non devono essere assolutamente condivise con terzi, e per motivi di sicurezza non potranno essere fornite nuovamente se perdute. Le chiavi, insieme al TempAuthToken che è un token temporaneo generato al login dell'utente in funzione delle due chiavi, sono l'unico modo per fornire a piattaforme di terzi i dati dell'utente Cam.TV che si è autenticato, insieme a quelli dei suoi sponsor.
	
## Versione
API ExternalIntegration Cam.TV V2.0

## Requests
Tutte le API Requests prevedono il metodo POST form-url-encoded. Le api sono intese server-to-server, l'autenticazione avviene mediante header Authorization con Bearer costituito da una SecretApiKey fornita da Cam.TV ed unicamente associata al dominio / IP del server dell'applicazione.

## Responses
Tutte le API Responses sono in formato JSON

## Esempi
Esempi sull'utilizzo delle API ExternalIntegration di Cam.TV possono essere consultate dalla [Documentazione di Postman](https://documenter.getpostman.com/view/9304344/SW7Z48Z2)

## Contatto
Per informazioni o richieste di chiarimento, scrivere a:  [admin@cam.tv](mailto:admin@cam.tv)
