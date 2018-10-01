# Cutting down the APIO

[_@alejandrohdezma_](https://twitter.com/alejandrohdezma)

![inline](../.images/cutting_celery.gif)

---

### What is APIO Architect?

---

### What is APIO Architect?

- Java 8 

---

### What is APIO Architect?

- Java 8 
- OSGi

---

### What is APIO Architect?

- Java 8 
- OSGi
- Liferay Portal Independent

---

### What is APIO Architect?

- Java 8 
- OSGi
- Liferay Portal Independent
- JAX-RS

---

### What is APIO Architect?

- Java 8 
- OSGi
- Liferay Portal Independent
- JAX-RS
- Functional Programming

---

![inline](../.images/github.pdf)

# [com-liferay-apio-architect](https://github.com/liferay/com-liferay-apio-architect)

---

Pure classes/functions

---

```java
public class URLCreator {
   
	public static String createBinaryURL(
		ServerURL serverURL, String binaryId, Path path) {

		return String.join("/", serverURL.get(), "b", path.asURI(), binaryId);
	}

	public static String createCollectionPageURL(
		String collectionURL, Page page, PageType pageType) {

		return UriBuilder.fromUri(
			collectionURL
		).queryParam(
			"page", pageType.getPageNumber(page)
		).queryParam(
			"per_page", page.getItemsPerPage()
		).build(
		).toString();
	}
}
```

---

Javadocs!

---

```java
/**
 * Manages the creation of URLs, and has all their necessary information.
 *
 * <p>
 * This class shouldn't be instantiated.
 * </p>
 *
 * @author Alejandro Hernández
 */
public final class URLCreator {

	/**
	 * Returns the URL for a binary resource.
	 *
	 * @param  serverURL the server URL
	 * @param  binaryId the binary resource's ID
	 * @param  path the resource's {@code com.liferay.apio.architect.uri.Path}
	 * @return the URL for a binary resource
	 */
	public static String createBinaryURL(
		ServerURL serverURL, String binaryId, Path path) {

		return String.join("/", serverURL.get(), "b", path.asURI(), binaryId);
	}
}
```

---

## Divided into several modules

---

Module | Semantic
---|---
apio-architect-api | Cornerstone module, all the framework API
apio-architect-application | Contains the JAX-RS application/resources
apio-architect-error | Contains the exception converters
apio-architect-error-problem-json | Mapper for `application/problem+json`
apio-architect-jaxrs-json | Contains the JAX-RS specific classes: writers, readers, filters...
apio-architect-message-hal | Mappers for `application/hal+json`
apio-architect-message-json-ld | Mappers for `application/ld+json`
apio-architect-message-json-plain | Mapper for `application/json`
apio-architect-response-control | Response controls implementations: embedded, fields, pagination
apio-architect-sample | Contains the sample APIs ([apiosample.wedeploy.io](https://apiosample.wedeploy.io)
apio-architect-sample-liferay-portal | Contains the Liferay Portal Sample APIs
apio-architect-test-util | Utility methods for unit testing
apio-architect-wiring-osgi-api | Internal framework API, interfaces that handle other interfaces
apio-architect-wiring-osgi-impl | Implementation of `apio-architect-wiring-osgi-api`
apio-architect-writer-api | Classes for writting instances into representation

---

# Cutting down the APIO

[_@alejandrohdezma_](https://twitter.com/alejandrohdezma)

![inline](../.images/cutting_celery.gif)
