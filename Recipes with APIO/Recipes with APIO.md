autoscale: true

## Creating Hypermedia REST APIs with Apio Architect

---

# [fit] Recipes with [Apio](https://www.google.com/search?q=what+does+apio+mean+in+spanish)

[_@alejandrohdezma_](https://twitter.com/alejandrohdezma)
[_@votilde_](https://twitter.com/votilde)


![inline](https://raw.githubusercontent.com/ahdezma/talks/master/.images/cutting_celery_anime.gif)

---

# [fit] Ask any doubt!

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/doubt.jpg)

---

Slides available here:

**slidr.io/ahdezma/recipes-with-apio**

---

# [fit] [apio-workshop](https://github.com/liferay-labs/apio-workshop)

![inline](https://raw.githubusercontent.com/ahdezma/talks/master/.images/daftpunktocat-thomas.gif)

---

# Liferay APIs in 2018

- _SOAP_
- _JSONWS API_ (`/api/jsonws`)
- _JAX-RS (7.0+)_
...

---

# Preparing the environment

* Clone this repo: _**github.com/liferay-labs/apio-workshop**_
* Download [Postman](https://www.getpostman.com) (a REST client)
* Download a [Liferay CE Portal 7.1 GA2](https://sourceforge.net/projects/lportal/files/Liferay%20Portal/7.1.1%20GA2/liferay-ce-portal-tomcat-7.1.1-ga2-20181112144637000.7z/download)

---

# What are we going to do?

---

# Liferay Portal Headless APP

---

# About what?

---

![inline](https://raw.githubusercontent.com/ahdezma/talks/master/.images/homer.gif)

---

An API that publish Liferay Portal organisations as restaurants

---

An API that publish Liferay Portal organisations as restaurants. And also publish their recipes.

---

An API that publish Liferay Portal organisations as restaurants. And also publish their recipes. And follows standards.

---

An API that publish Liferay Portal organisations as restaurants. And also publish their recipes. And follows standards. **And works!**

---

![inline](https://raw.githubusercontent.com/ahdezma/talks/master/.images/how.gif)

---

# JAX-RS

---

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/wait-what.gif)

---

Creating Hypermedia REST APIs with **Apio Architect**?

---

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/foundations.gif)

---

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/foundations.gif)

# Is important to understand foundations

^ Es importante entender la base, para que cuando lleguemos con caballería (aka Apio Architect) no nos pase como a este señor.

---

# Let's start!

---

# JAX-RS

---

# [fit] JAX-RS: Java API for RESTful Web Services

---

```java
@GET
@Path("/blogs/{id}")
@Produces(APPLICATION_JSON)
public Response getBlogsEntry(@PathParam("id") long id) {
    BlogsEntry blogsEntry = blogsEntryService.getBlogsEntry(id);

    return Response.ok(blogsEntry).build();

}
```

---

## What do we need to create a JAX-RS API?

- Application
- Resources
- Extensions

---

## Application

- Mandatory
- Manages the API resources

```java
@Component
@ApplicationPath("api")
public class MyApplication extends Application {

	@Override
	public Set<Class<?>> getClasses() {
		return Collections.singleton(new Resource());
	}

}
```

---

## Resources

- Optional (if you don't want to modularize)
- Manages endpoints in a specific path

```java
@Path("/blogs")
public class Resource {

    @GET
    @Path("/{id}")
    @Produces(APPLICATION_JSON)
    public Response getBlogsEntry(@PathParam("id") long id) {
        BlogsEntry blogsEntry = blogsEntryService.getBlogsEntry(id);

        return Response.ok(blogsEntry).build();

    }
    
}
```

---

## Extensions

- Optional
- Adds additional logic to the API

```java
@PreMatching
public class AuthFilter implements ContainerRequestFilter {

    public void filter(ContainerRequestContext ctx) {
        String authHeader = request.getHeaderString(HttpHeaders.AUTHORIZATION);

        if (authHeader == null) {
            throw new NotAuthorizedException("Not authenticated");
        }

        if (!verifyUser(authHeader)) {
            throw new NotAuthorizedException("User not authorized");
        }
    }
    
    private boolean verifyUser(String authHeader) {}

}
```

---

# How do we use<br>JAX-RS in Liferay Portal?

---
 
## [JAX-RS Whiteboard](https://github.com/apache/aries-jax-rs-whiteboard)

- Aries JAX-RS Whiteboard is the reference implementation of the [OSGi JAX-RS Services Whiteboard 1.0](https://osgi.org/specification/osgi.cmpn/7.0.0/service.jaxrs.html) (JAX-RS for OSGI)
- Component-property based

```java
@Component(service = Resource.class, property = JAX_RS_RESOURCE + "=true")
@Path("/blogs")
public class Resource {

    @GET
    @Path("/{id}")
    @Produces(APPLICATION_JSON)
    public Response getBlogsEntry(@PathParam("id") long id) {
        BlogsEntry blogsEntry = blogsEntryService.getBlogsEntry(id);

        return Response.ok(blogsEntry).build();
    }
    
}
```

#### Thanks @csierra & @rotty3000!

---

# Completing the environment

* Open the project _**workshop**_ (included in the repository)
* Set your Liferay Home
* Run `./gradlew prepare`
* Start Liferay Portal: `path/to/liferay_home/tomcat-9.0.10/bin/catalina.sh jpda run`
* Wait for starting...
* Run `./gradlew deploy`

---

# Let's test the APIs!

* Click on the ![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/postman-button.png) button
* Follow the instructions of our handsome speaker

---

# What do we have?

---

# Restaurants

![inline, fill](https://raw.githubusercontent.com/ahdezma/talks/master/.images/churrasic.jpg)

---

# Chefs

![inline, fill](https://raw.githubusercontent.com/ahdezma/talks/master/.images/ramsay.jpg)

---

# Kitchen Assistants

![inline, fill](https://raw.githubusercontent.com/ahdezma/talks/master/.images/assistant.png)

---

# Recipes

![inline, fill](https://raw.githubusercontent.com/ahdezma/talks/master/.images/recipe.gif)

---

# Our tools

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/tools.jpg)

---

# GoGo Shell

`telnet localhost 11311`

![inline left](https://raw.githubusercontent.com/ahdezma/talks/master/.images/gogo.png)

---

# GoGo Shell

- `apio:restaurants`

|ID|Name|Chef Email|
|:-:|:-:|:-:|
|35301|Diverxo|the.chef@diverxo.com|

- `apio:assistants`

|Kitchen Assistant Email|Organizations|
|:-:|:-:|
|the.assistant@diverxo.com|Diverxo|

---

# Postman

![original](https://raw.githubusercontent.com/ahdezma/talks/master/.images/postman.png)

---

# Our IDE

- Eclipse
- Liferay Dev Studio
- IntelliJ
- Visual Studio Code
- ...

---

# Demo time

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/popcorn.gif)