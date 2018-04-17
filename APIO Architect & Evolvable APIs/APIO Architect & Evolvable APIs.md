# APIO Architect & Evolvable APIs

### [@alejandrohdezma](https://twitter.com/alejandrohdezma)

---

Slides available here:

**slidr.io/ahdezma/apio-architect-evolvable-apis**

---

# Vulcan 

## 🖖


---

## ~~Vulcan~~

# APIO

---

# Liferay APIs in 2018

- SOAP
- JSONWS API (/api/jsonws)
- JAX-RS (7.0)
...


---

# APIO Architect Goals

- Evolvable
- Code reuse
- Auto-documentation
- Fun!

---

# [fit] How do we design APIs that are capable of evolving?

---

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/foundations.jpg)

---

## These aren't new concepts

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/foundations.jpg)

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
    "resources": {
        "blog-postings": {
            "href": "http://apiosample.wedeploy.io/p/blog-postings"
        },
        "people": {
            "href": "http://apiosample.wedeploy.io/p/people"
        }
    }
}
```

Entrypoint with all the "root" APIs ([JSON HOME](https://mnot.github.io/I-D/json-home/))

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

```json
{
    "@context": "http://www.w3.org/ns/hydra/context.jsonld",
    "@id": "http://apiosample.wedeploy.io/f/c/people",
    "@type": "Class",
    "description": "This form can be used to create or update a person",
    "supportedProperty": [
        {
            "@type": "SupportedProperty",
            "property": "#familyName",
            "readable": false,
            "required": true,
            "writeable": true
        },
        ...
    ],
    "title": "The person form"
}
```

Forms ([Hydra + JSON-LD](http://www.hydra-cg.com/spec/latest/core/))

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

```json
{
    "@context": {
        "creator": { "@type": "@id" },
        "@vocab": "http://schema.org/"
    },
    "@id": "https://apiosample.wedeploy.io/p/blog-postings/24557",
    "@type": "BlogPosting",
    "alternativeHeadline": "Vero ex excepturi exercitationem.",
    "articleBody": "Exercitationem exercitationem temporibus eum quasi aliquid.",
    "creator": "https://apiosample.wedeploy.io/p/people/23422",
    "dateCreated": "2017-12-30T17:31Z",
    "dateModified": "2017-12-30T17:31Z",
    "headline": "Recalled to Life"
}
```

### Shared vocabularies

#### Mapping internal `BlogsEntry` with [schema.org](https://schema.org) `BlogPosting`

---

```json, [.highlight: 7]
{
    "@context": {
        "creator": { "@type": "@id" },
        "@vocab": "http://schema.org/"
    },
    "@id": "https://apiosample.wedeploy.io/p/blog-postings/24557",
    "@type": "BlogPosting",
    "alternativeHeadline": "Vero ex excepturi exercitationem.",
    "articleBody": "Exercitationem exercitationem temporibus eum quasi aliquid.",
    "creator": "https://apiosample.wedeploy.io/p/people/23422",
    "dateCreated": "2017-12-30T17:31Z",
    "dateModified": "2017-12-30T17:31Z",
    "headline": "Recalled to Life"
}
```

---

```json, [.highlight: 8-14]
{
    "@context": {
        "creator": { "@type": "@id" },
        "@vocab": "http://schema.org/"
    },
    "@id": "https://apiosample.wedeploy.io/p/blog-postings/24557",
    "@type": "BlogPosting",
    "alternativeHeadline": "Vero ex excepturi exercitationem.",
    "articleBody": "Exercitationem exercitationem temporibus eum quasi aliquid.",
    "creator": "https://apiosample.wedeploy.io/p/people/23422",
    "dateCreated": "2017-12-30T17:31Z",
    "dateModified": "2017-12-30T17:31Z",
    "headline": "Recalled to Life"
}
```

---

```json, [.highlight: 3, 10]
{
    "@context": {
        "creator": { "@type": "@id" },
        "@vocab": "http://schema.org/"
    },
    "@id": "https://apiosample.wedeploy.io/p/blog-postings/24557",
    "@type": "BlogPosting",
    "alternativeHeadline": "Vero ex excepturi exercitationem.",
    "articleBody": "Exercitationem exercitationem temporibus eum quasi aliquid.",
    "creator": "https://apiosample.wedeploy.io/p/people/23422",
    "dateCreated": "2017-12-30T17:31Z",
    "dateModified": "2017-12-30T17:31Z",
    "headline": "Recalled to Life"
}
```

---

```json, [.highlight: 4, 7]
{
    "@context": {
        "creator": { "@type": "@id" },
        "@vocab": "http://schema.org/"
    },
    "@id": "https://apiosample.wedeploy.io/p/blog-postings/24557",
    "@type": "BlogPosting",
    "alternativeHeadline": "Vero ex excepturi exercitationem.",
    "articleBody": "Exercitationem exercitationem temporibus eum quasi aliquid.",
    "creator": "https://apiosample.wedeploy.io/p/people/23422",
    "dateCreated": "2017-12-30T17:31Z",
    "dateModified": "2017-12-30T17:31Z",
    "headline": "Recalled to Life"
}
```

---

Defining **types** and their **mapping** to **internal models** and actions is the most important API design activity

---

# How is APIO Architect enabling this?

---

# How is APIO Architect enabling this?

- Representor pattern
- Routes
- Permissions
- Affordances

---

# Representor Pattern

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

# Representor Pattern

---

```java
public Representor<BlogsEntry, Long> representor(Builder<BlogsEntry, Long> builder) {
	return builder.types(
		"BlogPosting"
	).identifier(
		BlogsEntry::getEntryId
	).addDate(
		"createDate", BlogsEntry::getCreateDate
	).addDate(
		"publishedDate", BlogsEntry::getLastPublishDate
	).addString(
		"alternativeHeadline", BlogsEntry::getSubtitle
	).addString(
		"articleBody", BlogsEntry::getContent
	).addString(
		"description", BlogsEntry::getDescription
	).addString(
		"headline", BlogsEntry::getTitle
	).build();
}
```

It's just a builder

---

```java
public Representor<BlogsEntry, Long> representor(Builder<BlogsEntry, Long> builder) {
	return builder.types(
		"BlogPosting"
	).identifier(
		blogsEntry -> blogsEntry.getEntryId()
	).addDate(
		"createDate", blogsEntry -> blogsEntry.getCreateDate()
	).addDate(
		"publishedDate", blogsEntry -> blogsEntry.getLastPublishDate()
	).addString(
		"alternativeHeadline", blogsEntry -> blogsEntry.getSubtitle()
	).addString(
		"articleBody", blogsEntry -> blogsEntry.getContent()
	).addString(
		"description", blogsEntry -> blogsEntry.getDescription()
	).addString(
		"headline", blogsEntry -> blogsEntry.getTitle()
	).build();
}
```

Lambdas FTW

---

```java, [.highlight: 2, 3]
public Representor<BlogsEntry, Long> representor(Builder<BlogsEntry, Long> builder) {
	return builder.types(
		"BlogPosting"
	).identifier(
		blogsEntry -> blogsEntry.getEntryId()
	).addDate(
		"createDate", blogsEntry -> blogsEntry.getCreateDate()
	).addDate(
		"publishedDate", blogsEntry -> blogsEntry.getLastPublishDate()
	).addString(
		"alternativeHeadline", blogsEntry -> blogsEntry.getSubtitle()
	).addString(
		"articleBody", blogsEntry -> blogsEntry.getContent()
	).addString(
		"description", blogsEntry -> blogsEntry.getDescription()
	).addString(
		"headline", blogsEntry -> blogsEntry.getTitle()
	).build();
}
```

Defining the type...

---

```java, [.highlight: 4, 5]
public Representor<BlogsEntry, Long> representor(Builder<BlogsEntry, Long> builder) {
	return builder.types(
		"BlogPosting"
	).identifier(
		blogsEntry -> blogsEntry.getEntryId()
	).addDate(
		"createDate", blogsEntry -> blogsEntry.getCreateDate()
	).addDate(
		"publishedDate", blogsEntry -> blogsEntry.getLastPublishDate()
	).addString(
		"alternativeHeadline", blogsEntry -> blogsEntry.getSubtitle()
	).addString(
		"articleBody", blogsEntry -> blogsEntry.getContent()
	).addString(
		"description", blogsEntry -> blogsEntry.getDescription()
	).addString(
		"headline", blogsEntry -> blogsEntry.getTitle()
	).build();
}
```

Abstracting from URLs...

---

```java, [.highlight: 6, 8, 10, 12, 14, 16, 18, 20]
public Representor<BlogsEntry, Long> representor(Builder<BlogsEntry, Long> builder) {
	return builder.types(
		"BlogPosting"
	).identifier(
		blogsEntry -> blogsEntry.getEntryId()
	).addDate(
		"createDate", blogsEntry -> blogsEntry.getCreateDate()
	).addDate(
		"publishedDate", blogsEntry -> blogsEntry.getLastPublishDate()
	).addString(
		"alternativeHeadline", blogsEntry -> blogsEntry.getSubtitle()
	).addString(
		"articleBody", blogsEntry -> blogsEntry.getContent()
	).addString(
		"description", blogsEntry -> blogsEntry.getDescription()
	).addString(
		"headline", blogsEntry -> blogsEntry.getTitle()
	).build();
}
```

Semantic DSL...

---

```java, [.highlight: 7, 9, 11, 13, 15, 17, 19, 21]
public Representor<BlogsEntry, Long> representor(Builder<BlogsEntry, Long> builder) {
	return builder.types(
		"BlogPosting"
	).identifier(
		blogsEntry -> blogsEntry.getEntryId()
	).addDate(
		"createDate", blogsEntry -> blogsEntry.getCreateDate()
	).addDate(
		"publishedDate", blogsEntry -> blogsEntry.getLastPublishDate()
	).addString(
		"alternativeHeadline", blogsEntry -> blogsEntry.getSubtitle()
	).addString(
		"articleBody", blogsEntry -> blogsEntry.getContent()
	).addString(
		"description", blogsEntry -> blogsEntry.getDescription()
	).addString(
		"headline", blogsEntry -> blogsEntry.getTitle()
	).build();
}
```

Static information...

---

With the **Representor Pattern** internal models are **transformed** to types and stored as a **generic representation** that can be then **mapped** to the representation format chosen by the user

---

- Enforce the mapping

---

- Enforce the mapping
- Abstract from URLs

---

- Enforce the mapping
- Abstract from URLs
- Type safe!

---

- Enforce the mapping
- Abstract from URLs
- Type safe!
- Generic representation

---

- Enforce the mapping
- Abstract from URLs
- Type safe!
- Generic representation
- Link relations

---

```java, [.highlight: 10-11]
public Representor<BlogsEntry, Long> representor(Builder<BlogsEntry, Long> builder) {
	return builder.types(
		"BlogPosting"
	).identifier(
		blogsEntry -> blogsEntry.getEntryId()
	).addDate(
		"createDate", blogsEntry -> blogsEntry.getCreateDate()
	).addDate(
		"publishedDate", blogsEntry -> blogsEntry.getLastPublishDate()
	).addLinkedModel(
		"author", PersonIdentifier.class, blogsEntry -> blogsEntry.getUserId()
	).addString(
		"alternativeHeadline", blogsEntry -> blogsEntry.getSubtitle()
	).addString(
		"articleBody", blogsEntry -> blogsEntry.getContent()
	).addString(
		"description", blogsEntry -> blogsEntry.getDescription()
	).addString(
		"headline", blogsEntry -> blogsEntry.getTitle()
	).build();
}
```

---

# How is APIO Architect enabling this?

- ~~Representor pattern~~
- Routes
- Permissions
- Affordances

---

# Abstract away REST

---

```java
public ItemRoutes<BlogsEntry, Long> itemRoutes(Builder<BlogsEntry, Long> builder) {
    return builder.addGetter(
        _blogsService::getEntry
    ).addRemover(
        idempotent(_blogsService::deleteEntry),
        _hasPermission::forDeleting(BlogsEntry.class)
    ).addUpdater(
        this::_updateBlogsEntry,
        _hasPermission::forUpdating(BlogsEntry.class),
        BlogPostingForm::buildForm
    ).build();
}
```

Routes for an item

---

```java, [.highlight: 2, 4, 7]
public ItemRoutes<BlogsEntry, Long> itemRoutes(Builder<BlogsEntry, Long> builder) {
    return builder.addGetter(
        _blogsService::getEntry
    ).addRemover(
        idempotent(_blogsService::deleteEntry),
        _hasPermission::forDeleting(BlogsEntry.class)
    ).addUpdater(
        this::_updateBlogsEntry,
        _hasPermission::forUpdating(BlogsEntry.class),
        BlogPostingForm::buildForm
    ).build();
}
```

Semantic DSL

---

```java, [.highlight: 3, 5, 8]
public ItemRoutes<BlogsEntry, Long> itemRoutes(Builder<BlogsEntry, Long> builder) {
    return builder.addGetter(
        _blogsService::getEntry
    ).addRemover(
        idempotent(_blogsService::deleteEntry),
        _hasPermission::forDeleting(BlogsEntry.class)
    ).addUpdater(
        this::_updateBlogsEntry,
        _hasPermission::forUpdating(BlogsEntry.class),
        BlogPostingForm::buildForm
    ).build();
}
```

Mapping semantic methods with service calls

---

```java, [.highlight: 6, 9]
public ItemRoutes<BlogsEntry, Long> itemRoutes(Builder<BlogsEntry, Long> builder) {
    return builder.addGetter(
        _blogsService::getEntry
    ).addRemover(
        idempotent(_blogsService::deleteEntry),
        _hasPermission::forDeleting(BlogsEntry.class)
    ).addUpdater(
        this::_updateBlogsEntry,
        _hasPermission::forUpdating(BlogsEntry.class),
        BlogPostingForm::buildForm
    ).build();
}
```

Permission checking!

---

```json, [.highlight: 10-17]
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

```java, [.highlight: 10]
public ItemRoutes<BlogsEntry, Long> itemRoutes(Builder<BlogsEntry, Long> builder) {
    return builder.addGetter(
        _blogsService::getEntry
    ).addRemover(
        idempotent(_blogsService::deleteEntry),
        _hasPermission::forDeleting(BlogsEntry.class)
    ).addUpdater(
        this::_updateBlogsEntry,
        _hasPermission::forUpdating(BlogsEntry.class),
        BlogPostingForm::buildForm
    ).build();
}
```

Form validation!

---

```java
public static Form<BlogPostingForm> buildForm(Builder<BlogPostingForm> formBuilder) {
	return formBuilder.title(
		language -> "The blog posting form"
	).description(
		language -> "This form can be used to create or update a blog posting"
	).constructor(
		BlogPostingForm::new
	).addRequiredDate(
		"displayDate", BlogPostingForm::_setDisplayDate
	).addRequiredString(
		"alternativeHeadline", BlogPostingForm::_setAlternativeHeadline
	).addRequiredString(
		"articleBody", BlogPostingForm::_setArticleBody
	).addRequiredString(
		"description", BlogPostingForm::_setDescription
	).addRequiredString(
		"headline", BlogPostingForm::_setHeadline
	).build();
}
```

Semantic DSL for forms

---

```java
public class BlogPostingForm {

	String alternativeHeadline;
	String articleBody;
	String description;
	Date displayDate;
	String headline;

}
```

It's just a POJO

---

```java
public NestedCollectionRoutes<BlogsEntry, Long> collectionRoutes(
    Builder<BlogsEntry, Long> builder) {

	return builder.addGetter(
		this::_getPageItems
	).addCreator(
		this::_addBlogsEntry,
		_hasPermission.forAddingEntries(BlogsEntry.class),
		BlogPostingForm::buildForm
	).build();
}
```

Routes for collections

---

# The future

---

# Any verb as an affordance

---

```java, [.highlight: 11, 12]
public ItemRoutes<BlogsEntry, Long> itemRoutes(Builder<BlogsEntry, Long> builder) {
    return builder.addGetter(
        _blogsService::getEntry
    ).addRemover(
        idempotent(_blogsService::deleteEntry),
        _hasPermission::forDeleting(BlogsEntry.class)
    ).addUpdater(
        this::_updateBlogsEntry,
        _hasPermission::forUpdating(BlogsEntry.class),
        BlogPostingForm::buildForm
    ).addOperation(
        this::_subscribe, SubscribeOperation.class
    ).build();
}
```

Any verb as an affordance

---

# Profile

---

```json
{
  "@context": "http://www.w3.org/ns/hydra/core#",
  "@id": "localhost:8080/vocab",
  "@type": "ApiDocumentation",
  "supportedClass": [
    {
      "@id": "http://schema.org/Person",
      "@type": "hydra:Class",
      "hydra:title": "Person",
      "hydra:description": "A person that uses the system",
      "supportedProperty": [
        {
          "property": "http://schema.org/givenName",
          "hydra:title": "givenName",
          "hydra:description": "The person's given name"
        },
        {
          "property": "http://schema.org/description",
          "hydra:title": "familyName",
          "hydra:description": "The person's family name"
        }
      ]
    },
    ...
  ]
}
```

Profile

---

# Queries & Ordering

---

# [fit] [modules/apps/headless-apio](https://github.com/liferay/liferay-portal/tree/master/modules/apps/headless-apio)

---

# [fit] [apiosample.wedeploy.io](https://apiosample.wedeploy.io)

![inline](https://raw.githubusercontent.com/ahdezma/talks/master/.images/apio_sample.png)

---

![inline right](https://raw.githubusercontent.com/ahdezma/talks/master/.images/slack.pdf)

# #apio-dev
# #apio-users

---

# [fit] Thank you!

---

# APIO Architect & Evolvable APIs

### [@alejandrohdezma](https://twitter.com/alejandrohdezma)