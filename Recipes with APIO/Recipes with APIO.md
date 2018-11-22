autoscale: true
theme: Work, 0

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

# Preparing the environment

* Clone the workshop repo:
[_**github.com/liferay-labs/apio-workshop**_](https://github.com/liferay-labs/apio-workshop)
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

## Extensions

- Optional
- Adds additional logic to the API

```java
@Component
public class UserProvider implements ContextProvider<User> {

    @Override
    public User createContext(Message message) {
        try {
            return _userService.getCurrentUser();
        } catch (PortalException e) {
            throw new NotAuthorizedException(e, "Basic");
        }
    }

    @Reference
    private UserService _userService;

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

**Demo time**

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/popcorn.gif)

---

**Interactive demo time**

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/clicking.gif)

---

# Apio Architect

---

# What is Apio Architect?

- Server-side library that facilitates the creation of Hypermedia REST APIs
- OSGi
- JAX-RS as foundation

---

# APIO Architect Goals

- Evolvable
- Reduce boilerplate
- Auto-documentation
- Fun!

---

# Hypermedia

---

# HATEOAS?

---

# How many of you have used Hypermedia today?

---

All of you

![inline](https://raw.githubusercontent.com/ahdezma/talks/master/.images/hypermedia_today.png)

---

In essence hypermedia is just another name for everything that we see, hear, and interact with on the Web. 

---

## This is not a new concept

---

> REST style is an abstraction of the architectural elements within a distributed **hypermedia** system
-- Roy Fielding, 2000

---

> REST "well" done (Richardson Maturity Model)
-- Martin Fowler's blog

![fit right](https://raw.githubusercontent.com/ahdezma/talks/master/.images/glory_of_rest.png)

---

# How are we solving this?

---

# 🏠

## Home URL

### Consumers must only know ONE URL
### And how to navigate from it

---

```json
{
    "@context": "https://apiosample.wedeploy.io/doc",
    "@id": "https://apiosample.wedeploy.io",
    "@type": "EntryPoint",
    "collection": [
        {
            "@id": "https://apiosample.wedeploy.io/blog-postings",
            "@type": "Collection",
            "manages": {
                "object": "schema:BlogPosting",
                "property": "rdf:type"
            }
        },
        {
            "@id": "https://apiosample.wedeploy.io/people",
            "@type": "Collection",
            "manages": {
                "object": "schema:Person",
                "property": "rdf:type"
            }
        }
    ]
}
```

Entrypoint with all the "root" APIs (JSON-LD)

---

# 📄

## Affordance types

### Contract with consumer defines affordance types (relations, actions, …)
### Start with [IANA’s 80 relation types](https://www.iana.org/assignments/link-relations/link-relations.xhtml)

---

```json, [.highlight: 4-19]
{
    "total": 43,
    "count": 30,
    "_links": {
        "collection": {
            "href": "http://apiosample.wedeploy.io/p/people"
        },
        "first": {
            "href": "http://apiosample.wedeploy.io/p/people?page=1&per_page=30"
        },
        "last": {
            "href": "http://apiosample.wedeploy.io/p/people?page=2&per_page=30"
        },
        "next": {
            "href": "http://apiosample.wedeploy.io/p/people?page=2&per_page=30"
        },
        "self": {
            "href": "http://apiosample.wedeploy.io/p/people?page=1&per_page=30"
        }
    },
}
```

Pagination ([HAL](https://github.com/mikekelly/hal_specification))


---

```json, [.highlight: 5,8,11]
{
    "total": 43,
    "count": 30,
    "_links": {
        "collection": {
            "href": "http://apiosample.wedeploy.io/p/people"
        },
        "first": {
            "href": "http://apiosample.wedeploy.io/p/people?page=1&per_page=30"
        },
        "last": {
            "href": "http://apiosample.wedeploy.io/p/people?page=1&per_page=30"
        },
        "self": {
            "href": "http://apiosample.wedeploy.io/p/people?page=1&per_page=30"
        }
    },
}
```

Standard link types ([IANA Link Relations](https://www.iana.org/assignments/link-relations/link-relations.xhtml))

---

```json
{
    "@id": "http://apiosample.wedeploy.io/p/people",
    "@type": [
        "Collection"
    ],
    "members": [
        ...
    ],
    "numberOfItems": 10,
    "operation": [
        {
            "@id": "people/create",
            "@type": "Operation",
            "expects": "http://apiosample.wedeploy.io/f/c/people",
            "method": "POST"
        }
    ]
}
```

Actions ([Hydra + JSON-LD](http://www.hydra-cg.com/spec/latest/core/))

---

```json, [.highlight:14]
{
    "@id": "http://apiosample.wedeploy.io/p/people",
    "@type": [
        "Collection"
    ],
    "members": [
        ...
    ],
    "numberOfItems": 10,
    "operation": [
        {
            "@id": "people/create",
            "@type": "Operation",
            "expects": "http://apiosample.wedeploy.io/f/c/people",
            "method": "POST"
        }
    ]
}
```

Actions ([Hydra + JSON-LD](http://www.hydra-cg.com/spec/latest/core/))

---

[.code-highlight: none]
[.code-highlight: 2,3,4,33]
[.code-highlight: 5-31]

```json
{
  "title": "Apio Sample API",
  "description": "This API allows developers try a Hypermedia API without creating one",
  "entrypoint": "https://apiosample.wedeploy.io",
  "supportedClass": [
    {
      "@id": "Comment",
      "@type": "Class",
      "title": "Comment",
      "supportedOperation": [
        {
          "@id": "_:comments/retrieve",
          "@type": [
            "Operation"
          ],
          "method": "GET",
          "returns": "Comment",
          "comment": "Return the list of comments."
        },
        ...
      ],
      "supportedProperty": [
        {
          "@type": "SupportedProperty",
          "property": "dateCreated",
          "comment": "The date on which the Comment was created or theitem was added to a DataFeed."
        },
        ...
      ]
    }
  ],
  "@id": "/doc",
  "@type": "ApiDocumentation"
}
```

Profile ([Hydra + JSON-LD](http://www.hydra-cg.com/spec/latest/core/))

---

```json, [.highlight: 5-7]
{
    "headline": "A blog",
    "alternativeHeadline": "A subtitle",
    "_links": {
        "creator": {
            "href": "http://apiosample.wedeploy.io/p/people/1"
        },
        "self": {
            "href": "http://apiosample.wedeploy.io/p/blog-postings/12"
        }
    },
}
```

Links to other resources ([HAL](https://github.com/mikekelly/hal_specification))

---

# How is APIO Architect enabling this?

---

## Annotations

---

# Vocabulary annotations

- `@Vocabulary.Type`
- `@Vocabulary.Field`
- `@Vocabulary.LinkedModel`
- `@Vocabulary.RelativeURL`
- `@Vocabulary.RelatedCollection`
- `@Id`

---

```java
@Type("BlogPosting")
public interface BlogPosting extends Identifier<Long> {

	@Id
	public Long getId();

	@Field("alternativeHeadline")
	public String getAlternativeHeadline();

	@Field("articleBody")
	public String getArticleBody();

	@Field("creator")
	@LinkedModel(Person.class)
	public Long getCreatorId();

	@Field("headline")
	public String getHeadline();

	@Field("review")
	public List<Review> getReviews();

}
```

---

# Exercise 1: Returning Apio Types

- Split our POJOs into interface and implementation
- Annotate interface with Apio Vocabulary annotations

---

# `final` branch

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/final_branch.png)

---

# Action annotations

- `@Actions.Action`
- `@Actions.Retrieve`
- `@Actions.Create`
- `@Actions.Remove`
- `@Actions.Replace`
- `@Actions.EntryPoint`

---

# Abstract away REST

```java
@Component
public class BlogPostingActionRouter implements ActionRouter<BlogPosting> {

	@Retrieve
	public BlogPosting retrieve(@Id long id);

	@EntryPoint
	@Retrieve
	public PageItems<BlogPosting> retrievePage(Pagination pagination);

}
```

---

# Exercise 2: Transforming JAX-RS GET endpoints

- Use `Actions` annotations to transform GET endpoints into Apio Architect actions.

---

# Let's try the API

---

# Exercise 3: Pagination

- Return `PageItems` instead of `List`
- Add `Pagination` param

---

# Which format is better?

---

```json
{
    "gender": "female",
    "familyName": "Hart",
    "givenName": "Sophia",
    "jobTitle": "Junior Executive",
    "name": "Sophia Hart",
    "birthDate": "1965-04-12T00:00Z",
    "email": "sophia.hart@example.com",
    "_links": {
        "self": {
            "href": "http://localhost:8080/o/api/p/people/30723"
        }
    }
}
```

[HAL](http://stateless.co/hal_specification.html)

---

```json
{
    "class": "BlogPosting",
    "properties": {
        "headline": "Hello DEVCON!",
        "content": "The content of this blog posting"
    },
    "actions": [
        {
            "name": "delete-blog-posting",
            "title": "Delete Blog Posting",
            "method": "DELETE",
            "href": "http://localhost:8080/o/p/blogs/32400"
        }
    ],
}
```

[SIREN](https://github.com/kevinswiber/siren)

---

```json
{
    "gender": "female",
    "familyName": "Hart",
    "givenName": "Sophia",
    "jobTitle": "Junior Executive",
    "name": "Sophia Hart",
    "birthDate": "1965-04-12T00:00Z",
    "email": "sophia.hart@example.com",
    "@id": "http://localhost:8080/o/api/p/people/30723",
    "@type": "Person",
    "@context": "http://schema.org"
}
```

[JSON-LD](https://json-ld.org/)

---

# Which format is better?

---

# [fit] All of them

---

# [fit] All of them

## or none

---

# It depends on your case

---

# Whats the solution then?

---

# Apio Architect

---

# Try changing the `Accept` header to:

- `application/json`
- `application/hal+json`
- `application/ld+json`

---

# Exception handling in Apio Architect

---

# Exercise 4: Transforming JAX-RS `ExceptionMapper` into Apio Architect's

---

With the **Vocabulary annotations** internal models are **transformed** to types and stored as a **generic representation** that can be then **mapped** to the representation format chosen by the user

---

# What are the advantages of this approach?

---

- Enforce the mapping

---

- Enforce the mapping
- Abstract from URLs

---

- Enforce the mapping
- Abstract from URLs
- Generic representation

---

- Enforce the mapping
- Abstract from URLs
- Generic representation
- Link relations

---

- Enforce the mapping
- Abstract from URLs
- Generic representation
- Link relations
- Navigability

---

# Navigability?

---

# Exercise 5: Add `recipes` field to `Organization`

---

# Exercise 6: Transform non-GET endpoints

- `@ParentId`
- `@Body`

--- 

# [fit] How do we design APIs that are capable of evolving?

---

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/foundations.jpg)

---

# 🔌

## Standard types

### [schema.org](https://schema.org): 597 types y 867 properties

---

# [schema.org](https://schema.org)

---

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/schema_org.png)

---

![inline](https://raw.githubusercontent.com/ahdezma/talks/master/.images/riched_content.png)

---

```html
<div class="reviewSnippetCard" itemtype="http://schema.org/Review">
    <span itemprop="publisher" itemtype="http://schema.org/Organization">
        <meta itemprop="name" content="Gartner, Inc.">
        <meta itemprop="url" content="https://www.gartner.com">
    </span>
    <span itemprop="provider" itemtype="http://schema.org/Organization">
        <meta itemprop="name" content="liferay">
        <meta itemprop="url" content="/reviews/market/horizontal-portals/vendor/liferay">
    </span>
    <span class="product-names" itemprop="itemReviewed" itemtype="http://schema.org/Product">
        <h3 itemprop="name">Liferay DXP</h3>
    <span>
    <div itemprop="reviewRating" itemtype="http://schema.org/Rating">
        <meta itemprop="worstRating" content="1">
        <meta itemprop="bestRating" content="5">
        <meta itemprop="ratingValue" content="4">
    </div>
</div>
```

---

```html, [.highlight: 2-5]
<div class="reviewSnippetCard" itemtype="http://schema.org/Review">
    <span itemprop="publisher" itemtype="http://schema.org/Organization">
        <meta itemprop="name" content="Gartner, Inc.">
        <meta itemprop="url" content="https://www.gartner.com">
    </span>
    <span itemprop="provider" itemtype="http://schema.org/Organization">
        <meta itemprop="name" content="liferay">
        <meta itemprop="url" content="/reviews/market/horizontal-portals/vendor/liferay">
    </span>
    <span class="product-names" itemprop="itemReviewed" itemtype="http://schema.org/Product">
        <h3 itemprop="name">Liferay DXP</h3>
    <span>
    <div itemprop="reviewRating" itemtype="http://schema.org/Rating">
        <meta itemprop="worstRating" content="1">
        <meta itemprop="bestRating" content="5">
        <meta itemprop="ratingValue" content="4">
    </div>
</div>
```

---

```html, [.highlight: 6-9]
<div class="reviewSnippetCard" itemtype="http://schema.org/Review">
    <span itemprop="publisher" itemtype="http://schema.org/Organization">
        <meta itemprop="name" content="Gartner, Inc.">
        <meta itemprop="url" content="https://www.gartner.com">
    </span>
    <span itemprop="provider" itemtype="http://schema.org/Organization">
        <meta itemprop="name" content="liferay">
        <meta itemprop="url" content="/reviews/market/horizontal-portals/vendor/liferay">
    </span>
    <span class="product-names" itemprop="itemReviewed" itemtype="http://schema.org/Product">
        <h3 itemprop="name">Liferay DXP</h3>
    <span>
    <div itemprop="reviewRating" itemtype="http://schema.org/Rating">
        <meta itemprop="worstRating" content="1">
        <meta itemprop="bestRating" content="5">
        <meta itemprop="ratingValue" content="4">
    </div>
</div>
```

---

```html, [.highlight: 10-12]
<div class="reviewSnippetCard" itemtype="http://schema.org/Review">
    <span itemprop="publisher" itemtype="http://schema.org/Organization">
        <meta itemprop="name" content="Gartner, Inc.">
        <meta itemprop="url" content="https://www.gartner.com">
    </span>
    <span itemprop="provider" itemtype="http://schema.org/Organization">
        <meta itemprop="name" content="liferay">
        <meta itemprop="url" content="/reviews/market/horizontal-portals/vendor/liferay">
    </span>
    <span class="product-names" itemprop="itemReviewed" itemtype="http://schema.org/Product">
        <h3 itemprop="name">Liferay DXP</h3>
    <span>
    <div itemprop="reviewRating" itemtype="http://schema.org/Rating">
        <meta itemprop="worstRating" content="1">
        <meta itemprop="bestRating" content="5">
        <meta itemprop="ratingValue" content="4">
    </div>
</div>
```

---

```html, [.highlight: 13-17]
<div class="reviewSnippetCard" itemtype="http://schema.org/Review">
    <span itemprop="publisher" itemtype="http://schema.org/Organization">
        <meta itemprop="name" content="Gartner, Inc.">
        <meta itemprop="url" content="https://www.gartner.com">
    </span>
    <span itemprop="provider" itemtype="http://schema.org/Organization">
        <meta itemprop="name" content="liferay">
        <meta itemprop="url" content="/reviews/market/horizontal-portals/vendor/liferay">
    </span>
    <span class="product-names" itemprop="itemReviewed" itemtype="http://schema.org/Product">
        <h3 itemprop="name">Liferay DXP</h3>
    <span>
    <div itemprop="reviewRating" itemtype="http://schema.org/Rating">
        <meta itemprop="worstRating" content="1">
        <meta itemprop="bestRating" content="5">
        <meta itemprop="ratingValue" content="4">
    </div>
</div>
```

---

# How do we port this to APIs?

---

```json
{
    "blogsEntryId": "24557",
    "subtitle": "Vero ex excepturi exercitationem.",
    "content": "Exercitationem exercitationem temporibus eum quasi aliquid.",
    "userId": "23422",
    "create_date": "2017-12-30T17:31Z",
    "modified_date": "2017-30-12",
    "title": "Recalled to Life"
}
```

# Our good old JSON-WS

---

```json, [.highlight: 2, 5]
{
    "blogsEntryId": "24557",
    "subtitle": "Vero ex excepturi exercitationem.",
    "content": "Exercitationem exercitationem temporibus eum quasi aliquid.",
    "userId": "23422",
    "create_date": "2017-12-30T17:31Z",
    "modified_date": "2017-30-12",
    "title": "Recalled to Life"
}
```

# Our coupled old JSON-WS

---

```json, [.highlight: 3, 4, 6, 7, 8]
{
    "blogsEntryId": "24557",
    "subtitle": "Vero ex excepturi exercitationem.",
    "content": "Exercitationem exercitationem temporibus eum quasi aliquid.",
    "userId": "23422",
    "create_date": "2017-12-30T17:31Z",
    "modified_date": "2017-30-12",
    "title": "Recalled to Life"
}
```

# Our too coupled old JSON-WS

---

[.code-highlight: all]
[.code-highlight: 4]
[.code-highlight: 5,6,11,12,13]
[.code-highlight: 7-10]

```json
{
    "@context": "http://apiosample.wedeploy.io/doc",
    "@id": "https://apiosample.wedeploy.io/p/blog-postings/24557",
    "@type": "BlogPosting",
    "alternativeHeadline": "Vero ex excepturi exercitationem.",
    "articleBody": "Exercitationem exercitationem temporibus eum quasi aliquid.",
    "creator": {
       "@id": "https://apiosample.wedeploy.io/p/people/23422",
       "@type": "Person"
    },
    "dateCreated": "2017-12-30T17:31Z",
    "dateModified": "2017-12-30T17:31Z",
    "headline": "Recalled to Life"
}
```

### Shared vocabularies

#### Mapping internal `BlogsEntry` with [schema.org](https://schema.org) `BlogPosting`

---

Defining **types** and their **mapping** to **internal models** and actions is the most important API design activity

---

# Let's use standard types!

---

# Exercise 7: Use standard types

- Look up schema.org [**Recipe**](https://schema.org/Recipe), [**Restaurant**](https://schema.org/Restaurant), [**PostalAddress**](https://schema.org/PostalAddress) and [**Person**](https://schema.org/Person)
- Update `@Vocabulary.Field` & `@Vocabulary.Type` annotations accordingly

---

# Altering the data

- Go to: `path/to/liferay/home/osgi/configs/com.liferay.recipes.demo.internal.RecipesDemo-default.cfg`
- Tweak the numbers to generate more data

---

# Apio Architect API features

- `embedded`
- `fields`

---

# The future

---

## Java Bean Validations

[.code-highlight: all]
[.code-highlight: 8,12,16]

```java
@Type("BlogPosting")
public interface BlogPosting extends Identifier<Long> {

	@Field("alternativeHeadline")
	public String getAlternativeHeadline();

	@Field("articleBody")
	@Email(message = "Email should be valid")
	public String getArticleBody();

	@Field("headline")
	@NotNull
	public String getHeadline();

	@Field("review")
	@NotEmpty
	public List<Review> getReviews();

}
```

---

## OpenAPI

Automatically generated by Apio Architect

---

## Documentation

---

# [fit] [apiosample.wedeploy.io](https://apiosample.wedeploy.io)

![inline](https://raw.githubusercontent.com/ahdezma/talks/master/.images/apio_sample.png)

---

# [fit] Thank you!

---

# [fit] Recipes with [Apio](https://www.google.com/search?q=what+does+apio+mean+in+spanish)

[_@alejandrohdezma_](https://twitter.com/alejandrohdezma)
[_@votilde_](https://twitter.com/votilde)


![inline](https://raw.githubusercontent.com/ahdezma/talks/master/.images/cutting_celery_anime.gif)