autoscale: true

# [fit] Evolvable APIs with APIO Architect

### Alejandro Hernandez _@alejandrohdezma_

---

# [fit] Ask any doubt!

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/doubt.jpg)

---

# Vulcan?
# APIO?
# Hypermedia?

---

# Liferay APIs in 2018

- SOAP
- JSONWS API (/api/jsonws)
- JAX-RS (7.0)
...


---

# APIO Goals

- Easy versions
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

# What's Hypermedia?

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

> REST "well" done (Richardson Maturity Model)
-- Martin Fowler's blog

![fit right](https://raw.githubusercontent.com/ahdezma/talks/master/.images/glory_of_rest.png)

---

# [fit] Code time!

---

# [fit] [apio-architect-workshop](https://github.com/ahdezma/apio-architect-workshop)

![inline](https://raw.githubusercontent.com/ahdezma/talks/master/.images/daftpunktocat-thomas.gif)

---

# [fit] Problems creating APIs?

---

# 1 - Which format is better?

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

HAL

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

SIREN

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

JSON-LD

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

# [fit] APIO Architect

---

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/old_ways.png)

---

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/old_ways.png)

# Write to one format

---

![](https://raw.githubusercontent.com/ahdezma/talks/master/.images/new_ways.png)

---

```java
@Component(immediate = true)
public class BlogPostingResource
	implements CollectionResource<BlogsEntry, Long, ??> {

	@Override
	public String getName() {
		return "blog-postings";
	}

	@Override
	public Representor<BlogsEntry, Long> representor(
		Builder<BlogsEntry, Long> builder) {

		return builder.types(
			"BlogPosting"
		).identifier(
			BlogsEntry::getEntryId
		).build();
	}

}
```

---

```java,[.highlight: 3]
@Component(immediate = true)
public class BlogPostingResource
	implements CollectionResource<BlogsEntry, Long, ??> {

	@Override
	public String getName() {
		return "blog-postings";
	}

	@Override
	public Representor<BlogsEntry, Long> representor(
		Builder<BlogsEntry, Long> builder) {

		return builder.types(
			"BlogPosting"
		).identifier(
			BlogsEntry::getEntryId
		).build();
	}

}
```

---

```java
public interface BlogPostingId extends Identifier<Long> {
}
```

---

```java,[.highlight: 3]
@Component(immediate = true)
public class BlogPostingResource
	implements CollectionResource<BlogsEntry, Long, BlogPostingId> {

	@Override
	public String getName() {
		return "blog-postings";
	}

	@Override
	public Representor<BlogsEntry, Long> representor(
		Builder<BlogsEntry, Long> builder) {

		return builder.types(
			"BlogPosting"
		).identifier(
			BlogsEntry::getEntryId
		).build();
	}

}
```

---

```java
public Representor<BlogsEntry, Long> representor(
	Builder<BlogsEntry, Long> builder) {

	return builder.types(
		"BlogPosting"
	).identifier(
		blogsEntry -> blogsEntry.getEntryId()
	).build();
}
```

The Representor

---

```java
public Representor<BlogsEntry, Long> representor(
	Builder<BlogsEntry, Long> builder) {

	return builder.types(
		"BlogPosting"
	).identifier(
		blogsEntry -> blogsEntry.getEntryId()
	).addDate(
		"datePublished", BlogsEntry::getDisplayDate
	).addLink(
		"license", "https://creativecommons.org/licenses/by/4.0"
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

The Representor

---

# Mappers:

- JSON

- HAL

- JSON+LD

- HTML?

- ...

---

# [fit] Problems creating APIs?

---

# 2 - Routes

---

# What do we want to achieve?

---

#### Decouple route and representation code
#### Reuse code to represent CRUD
#### Clearly express the supported routes

---

# [fit] CollectionRoutes.Builder

---

```java
@Override
public CollectionRoutes<BlogsEntry> collectionRoutes(
	CollectionRoutes.Builder<BlogsEntry> builder) {

	return builder.addGetter(
		this::_getPageItems
	).build();
}

private PageItems<BlogsEntry> _getPageItems(Pagination pagination) {
	List<BlogsEntry> blogsEntries = _blogsService.getGroupEntries(
		20143, 0, pagination.getStartPosition(),
		pagination.getEndPosition());

	int count = _blogsService.getGroupEntriesCount(20143, 0);

	return new PageItems<>(blogsEntries, count);
}

@Reference
private BlogsEntryService _blogsService;
```

---

```java
@Override
public CollectionRoutes<BlogsEntry> collectionRoutes(
	Builder<BlogsEntry, Long> builder) {

	return builder.addGetter(
		this::_getPageItems
	).addCreator(
		this::_addBlogsEntry, BlogPostingForm::buildForm
	).build();
}
```

---

```json
{
    "@context": "http://www.w3.org/ns/hydra/context.jsonld",
    "@id": "http://apiosample.wedeploy.io/f/c/blog-postings",
    "@type": "Class",
    "description": "This form can be used to create or update a blog posting",
    "supportedProperty": [
        {
            "@type": "SupportedProperty",
            "property": "#headline",
            "readable": false,
            "required": true,
            "writeable": true
        },
        ...
    ],
    "title": "The blog posting form"
}
```

---

# What about item routes?

---

# [fit] ItemRoutes.Builder

---

```java
@Override
public ItemRoutes<BlogsEntry> itemRoutes(
	Builder<BlogsEntry, Long> builder) {

	return builder.addGetter(
		this::_getBlogsEntry
	).addRemover(
		this::_deleteBlogsEntry
	).addUpdater(
		this::_updateBlogsEntry, BlogsEntryForm::buildForm
	).build();
}
```

---

# [fit] Problems creating APIs?

---

# 3 - Converting fields, links, dates

---

# Why?

---

### Types! (dates, strings, numbers)

### Links

---

# [fit] Shared vocabularies!

---

![70%](https://raw.githubusercontent.com/ahdezma/talks/master/.images/schema_org.png)

---

| BlogsEntry | BlogPosting |
| --- | --- |
| getTitle() | headline |
| getContent() | articleBody |
| getSubtitle() | alternativeHeadline |
| getDisplayDate() | datePublished |

## Mapping BlogsEntry to BlogPosting

---

```java
public Representor<BlogsEntry, Long> representor(
	Builder<BlogsEntry, Long> builder) {

	return builder.types(
		"BlogPosting"
	).identifier(
		blogsEntry -> blogsEntry.getEntryId()
	).addDate(
		"datePublished", BlogsEntry::getDisplayDate
	).addLink(
		"license", "https://creativecommons.org/licenses/by/4.0"
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

---

# [fit] Problems creating APIs?

---

# 4 - Relations between resources

---

#### Embedded resources (save requests)

#### Navigation between resources

---

```java
public class PersonResource
	implements CollectionResource<User, Long, PersonId> {

	@Override
	public String getName() {
		return "people";
	}

	@Override
	public Representor<User, Long> representor(
		Representor.Builder<User, Long> representorBuilder) {

		return representorBuilder.types(
			"Person"
		).identifier(
			User::getUserId
		).addString(
			"additionalName", User::getMiddleName
		).addString(
			"email", User::getEmailAddress
		).addString(
			"familyName", User::getLastName
		).addString(
			"givenName", User::getFirstName
		).addString(
			"jobTitle", User::getJobTitle
		).build();
	}
	
}
```

---

```java
public Representor<BlogsEntry, Long> representor(
	Builder<BlogsEntry, Long> builder) {

	return builder.types(
		"BlogPosting"
	).identifier(
		BlogsEntry::getEntryId
	).addDate(
		"displayDate", BlogsEntry::getDisplayDate
	).addLinkedModel(
		"creator", PersonId.class, blogsEntry -> blogsEntry.getUserId()
	).addString(
		"articleBody", BlogsEntry::getContent
	).addString(
		"headline", BlogsEntry::getTitle
	).build();
}
```

---

# What else?

## I want more

---

# [fit] APIO Consumer

---

```kotlin
val id =  "http://.../o/api/p/web-sites/5745/blogs"

thingScreenlet.load(id, credentials)

```

![90% right](https://raw.githubusercontent.com/ahdezma/talks/master/.images/apio_consumer.gif)

## Clients for Java (+Android), iOS and Javascript (coming...)

---

# #apio-users

![30%](https://raw.githubusercontent.com/ahdezma/talks/master/.images/slack.pdf)

---

# [fit] Questions?

---

# Evolvable APIs with APIO Architect

### Alejandro Hernández _@alejandrohdezma_
