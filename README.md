## PostgREST API with Keycloak
------------------------------

**1. Prerequisites**
  ..* Setup PostgreSQL & PostgREST : Follow the tutorial of PostgREST configuration : https://postgrest.org/en/v6.0/tutorials/tut0.html
  ..* For these examples :
    - Have a keycloak realm
    - Public keycloak client
    - An user to connect
**2. Get the public key or certificate of your client to authenticate**
  Keycloak example :
    ..* In the settings of your realm, go to the "Keys" tab
    ..* Get the certificate and wrap it with : -----BEGIN CERTIFICATE----- / -----END CERTIFICATE-----
    ..* Use a tool such as pem-jwk to generate the associated jwk
**3. Pass the claims in your jwt token**
  Keycloak example :
    ..* In your realm, create a client mapper for the client you need
    ..* Select hardcoded claim as mapper type
    ..* Set the value you want for the claim
**4. Set up your postgrest.conf file** _( don't forget to escape your jwk )_ 
  Keycloak example :
    ..* Create a new role for your postgresql database named like the role-claim-key
    ..* Grant it the rights to do actions to your database
**5. Test your security**
  Postman/Keycloak example :
    ..* Connect to the client of your realm : 
      - Request : POST http://<host>/auth/realms/<realm>/protocol/openid-connect/token
      - Body Type: x-www-form-urlencoded
      - Body Content : username, password, client_id, grant_type
    ..* Copy the token in "access_token"
    ..* Paste it in your PostgREST request
      - Authorization : Bearer Token, paste it in the token field 
      - GET http://<host>:<port>/<db_name> 
