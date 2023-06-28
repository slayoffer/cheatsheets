To enable CORS in Keycloak, you can add the following configuration to your `standalone.xml` file:

```
xmlCopy code<subsystem xmlns="urn:jboss:domain:undertow:12.0" default-server="default-server" default-virtual-host="default-host" default-servlet-container="default">
  <server name="default-server">
    <http-listener name="default" socket-binding="http" redirect-socket="https" enable-http2="true" cors-enabled="true"/>
  </server>
</subsystem>
```

To enable CORS in Gatekeeper, you can add the following configuration to your `config.yml` file:

```
yamlCopy codeupstream: http://your-upstream-url
server:
  listen:
  - 0.0.0.0:3000
  cors_origins:
  - "*"
  cors_credentials: true
```

In the above configuration, `cors_origins` specifies the allowed origins for CORS requests, and `cors_credentials` specifies whether credentials (such as cookies) are allowed to be included in CORS requests.

With these configurations in place, your Keycloak and Gatekeeper instances should be able to handle CORS requests properly.

------

```bash
cors-origins:
  - '*'
```

This is invalid for https or for call with credentials. Try to specify origins explicitly.

- check preflight (OPTION) request, maybe it is requesting CORS with some special headers (`Access-Control-Request-Headers`)/method(`Access-Control-Request-Method`) - they need to be allowed in gatekeeper configuration, e.g.:

```markdown
cors-methods:
    - GET
    - POST
    - OPTIONS
    - HEAD
    - DELETE
cors-headers:
    - authorization
    - content-type
```

------

You probably need to set the Web Origins on your Keycloak server for your Keycloak client:

1. Login to the Keycloak admin screen, select the realm pwe-realm and then your client pwe-web.
2. Scroll to the Web Origin settings and type the plus sign. Do not click on the (+) button, but literally type + . That adds all the Valid Redirect URIs that you defined above to the Web Origins headers. You will also need to add the URI of your angular application to the Valid Redirect URIs list.
3. Press Save at the bottom of the screen.

---

add `"enable-cors": true` in `keycloak.json`

---

Web Origins must be set correctly for a configured client, yes, but it too has some pitfalls because a "+" setting depends on other values.

E.g., I had this **wrong** configuration for local tests:

```js
Root URL: http://localhost:3000/
Valid redirect URIs: *
Web Origins: +
```

... and it failed with a CORS issue like you describe.

The issue lay with the other two settings, so this is **correct** and works now:

```js
Root URL: http://localhost:3000
Valid redirect URIs: /*
Web Origins: +
```

---

```bash
cors-origins: - '*' 

cors-headers: 
	- Accept 
	- Content-Type 
	- Cache-Control 
	- Pragma 
	- X-Custom-Header 
	- Source 
	- Origin 
	- X-Requested-With 
	- Access-Control-Request-Method 
	- Access-Control-Request-Headers 
	- Access-Control-Allow-Origin 
	- Authorization 
	
cors-methods: 
	- GET 
	- POST 
	- PUT 
	- OPTIONS 
	- HEAD 
	- DELETE 

debug: true
```



