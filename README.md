# springboot-oauth2-keycloak
Projeto destinado a subir uma aplicaçao hello world com autenticação usando oauth2 integrado ao keycloak

## Objetivo
O objetivo do projeto é demonstrar o funcionamento de uma api com oauth2 habilitado usando o spring security e consumindo o keycloak que serve como authentication server

## Considerações
No projeto também, consta uma estrutura simples de validação de escopo, concedendo a cada role específica determinado nível de acesso.

## Configuração do KeyCloak

Para usar o projeto é necessário a configuração de alguns itens direto no keycloak, conforme comentados abaixo:
 - Clients: 
   - external-secret:
     - Roles:
       - admin
       - user
   - Real Roles:
     - app_admin:
       - Associated Roles:
         - app_user
     - app_user:
       - Associete Roles:
         - app_user
   - Users:
     - user1:
       - Role Mapping:
         - app_admin
     - user2:
       - Role Mapping: 
         - app_user
     - user3:
       - Role Mapping:
         - app_user
         - app_admin

## Configuração do Springboot com o Keycloak

> **Você pode subir o keycloak local usando o comando abaixo:**
```
docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:25.0.1 start-dev
```


O processo de integração entre o springboot e o keycloack, basicamente funciona através do código

application.properties

```
spring.application.name=oauth-demoapp
# Security Configuration
spring.security.oauth2.resourceserver.jwt.issuer-uri=http://localhost:8081/realms/External
spring.security.oauth2.resourceserver.jwt.jwk-set-uri=${spring.security.oauth2.resourceserver.jwt.issuer-uri}/protocol/openid-connect/certs

# JWT Configuration
jwt.auth.converter.resource-id=external-client
jwt.auth.converter.principal-attribute=principal_username

# Logging Configuration
logging.level.org.springframework.security=DEBUG
```

## Referencias:
- [Keycloak Integration With SpringSecurity](https://medium.com/javarevisited/keycloak-integration-with-spring-security-6-37999f43ec85)
- [Consuming an Endpoint protected by an oauth2 Resourcer Server from a Springboot Service](https://laurspilca.com/consuming-an-endpoint-protected-by-an-oauth-2-resource-server-from-a-spring-boot-service/)