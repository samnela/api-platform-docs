# Extending JSON-LD context

API Platform Core provides the possibility to extend the JSON-LD context of properties. This allows you to describe JSON-LD 
typed values, inverse properties using the `@reverse` keyword and you can even overwrite the `@id` property this way. Everything you define
within the following annotation, will be passed to the context, that provides a generic way to extend the context.

```php
<?php

// src/AppBundle/Entity/Book.php

namespace AppBundle\Entity;

use ApiPlatform\Core\Annotation\ApiProperty;
use ApiPlatform\Core\Annotation\ApiResource;

/**
 * @ApiResource(iri="http://schema.org/Book")
 */
class Book
{
    // ...

    /**
     * ...
     * @ApiProperty(
     *     iri="http://schema.org/name", 
     *     attributes={
     *         "jsonld_context"={
     *             "@id"="http://yourcustomid.com",
     *             "@type"="http://www.w3.org/2001/XMLSchema#string"
     *             "someProperty"={
     *                 "a"="textA",
     *                 "b"="textB"
     *             }
     *         }
     *     }
     * )
     */
    public $name;
}
```

The generated context will now have your custom attributes set:

`GET /contexts/Book`

```json
{
  "@context": {
    "@vocab": "http://example.com/apidoc#",
    "hydra": "http://www.w3.org/ns/hydra/core#",
    "name": {
      "@id": "http://yourcustomid.com",
      "@type": "http://www.w3.org/2001/XMLSchema#string",
      "someProperty": {
        "a": "textA",
        "b": "textB"
      }
    }
  }
}
```

Note that you do not have to provide the `@id` attribute, if you do not provide an `@id` attribute, the value from `iri` will be taken.

Previous chapter: [Using External JSON-LD Vocabularies](core/external-vocabularies.md)
Next chapter: [Data Providers](core/data-providers.md)