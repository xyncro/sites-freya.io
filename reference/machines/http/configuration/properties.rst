Properties
==========

Properties are a slight addition to the machine model as described in the topic on the design of :doc:`/topics/design/machines`. The model there only included a relatively "pure" implementation, containing nothing but decisions and terminals (handlers in an HTTP context). However, in the HTTP context it makes the model significantly neater if configuration can also provide some information to use for decision making (and otherwise) which is richer than simply ``true|false``.

Consider the case of configuring which methods are allowed for a resource. With only simple decisions available, a whole chain of *"is GET allowed?"*, *"is POST allowed?"*, *"is HEAD allowed?"* etc. decisions would need to be configured simply to configure a basic aspect of an HTTP resource. It would be much simpler to allow the resource to be configured with a more complex property -- the set of methods allowed, in this case, which could then be referenced by a decision within the machine (instead asking something like *"is the current request method a member of the set of allowed methods?"*).

For this reason the HTTP machine (and extensions to the HTTP machine) define a small number of properties which can be set, which will be used by decisions built in to the machine.

Reference and Types
-------------------

As each of the properties of an HTTP machine is of a different type (unlike decisions or handlers), each property will be described individually. In addition, use of static type inference is again common to allow simpler syntax where possible.

Acceptable Media Types
^^^^^^^^^^^^^^^^^^^^^^

The ``acceptableMediaTypes`` configuration property allows the specification of media types which will be acceptable for a request payload (when relevant). If you don't specify any acceptable media types, the machine will assume that any media type is acceptable.

Acceptable media types may be configured in any of several forms:

.. code-block:: fsharp

   // A simple machine with a single media type

   let singleMachine =
       freyaMachine {
           acceptableMediaTypes MediaType.Json }

   // A simple machine with a list of media types

   let listMachine =
       freyaMachine {
           acceptableMediaTypes [ MediaType.Json; MediaType.Xml ] }

   // A dynamic machine with a list of media types

   let mediaTypes =
       freya {
           // request logic
           return [ MediaType.Json ] }

   let dynamicMachine =
       freyaMachine {
           acceptableMediaTypes mediaTypes }

As you can see, the machine syntax does not need to change to accomodate different ways to specify the configuration property, while the last example of a dynamic property (``Freya<MediaType list>``) shows how the media types to be accepted can be determined at runtime (perhaps in response to other information about the client, or more complex logic about available transformations of the existing resource, for example).

Available Character Sets
^^^^^^^^^^^^^^^^^^^^^^^^

The ``availableCharsets`` configuration property allows the specification of character sets which are available for a representation of the resource (where one is relevant). If you don't specify any character sets, the machine will assume that character sets will be determined by the handler without content negotiation.

Available character sets may be configured in any of several forms:

.. code-block:: fsharp

   // A simple machine with a single character set

   let singleMachine =
       freyaMachine {
           availableCharsets Charset.Utf8 }

   // A simple machine with a list of character sets

   let listMachine =
       freyaMachine {
           availableCharsets [ Charset.Utf8; Charset.Unicode ] }

   // A dynamic machine with a list of character sets

   let charsets =
       freya {
           // request logic
           return [ Charset.Utf8 ] }

   let dynamicMachine =
       freyaMachine {
           availableCharsets charsets }

As you can see, the machine syntax does not need to change to accomodate different ways to specify the configuration property, while the last example of a dynamic property (``Freya<Charset list>``) shows how the character sets to be made available can be determined at runtime (perhaps based on available representations of the resource).

Available Content Encodings
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``availableContentCodings`` configuration property allows the specification of content encodings which are available for a representation of the resource (where one is relevant). If you don't specify any content encodings, the machine will assume that content encoding will be determined by the handler without content negotiation.

Available content encodings may be configured in any of several forms:

.. code-block:: fsharp

   // A simple machine with a single content encoding

   let singleMachine =
       freyaMachine {
           availableContentCodings ContentCoding.Deflate }

   // A simple machine with a list of content encodings

   let listMachine =
       freyaMachine {
           availableContentCodings [
               ContentCoding.Deflate
               ContentCoding.GZip ] }

   // A dynamic machine with a list of content encodings

   let codings =
       freya {
           // request logic
           return [ ContentCoding.Deflate ] }

   let dynamicMachine =
       freyaMachine {
           availableContentCodings codings }

As you can see, the machine syntax does not need to change to accomodate different ways to specify the configuration property, while the last example of a dynamic property (``Freya<ContentCoding list>``) shows how the content encodings to be made available can be determined at runtime (perhaps based on available transformations of the resource).

Available Languages
^^^^^^^^^^^^^^^^^^^

The ``availableLanguages`` configuration property allows the specification of languages which are available for a representation of the resource (where one is relevant). If you don't specify any languages, the machine will assume that language will be determined by the handler without content negotiation.

Available languages may be configured in any of several forms:

.. code-block:: fsharp

   // A simple machine with a single language

   let singleMachine =
       freyaMachine {
           availableLanguages (LanguageTag.parse "en-GB") }

   // A simple machine with a list of languages

   let listMachine =
       freyaMachine {
           availableLanguages [
               LanguageTag.parse "en-GB"
               LanguageTag.parse "fr-FR" ] }

   // A dynamic machine with a list of languages

   let languages =
       freya {
           // request logic
           return [ LanguageTag.parse "en-GB" ] }

   let dynamicMachine =
       freyaMachine {
           availableLanguages languages }

As you can see, the machine syntax does not need to change to accomodate different ways to specify the configuration property, while the last example of a dynamic property (``Freya<LanguageTag list>``) shows how the languages to be made available can be determined at runtime (perhaps based on available translations of the resource).

Available Media Types
^^^^^^^^^^^^^^^^^^^^^

The ``availableMediaTypes`` configuration property allows the specification of media types which are available for a representation of the resource (where one is relevant). If you don't specify any media types, the machine will assume that media type will be determined by the handler without content negotiation.

Available media types may be configured in any of several forms:

.. code-block:: fsharp

   // A simple machine with a single media type

   let singleMachine =
       freyaMachine {
           availableMediaTypes MediaType.Json }

   // A simple machine with a list of media types

   let listMachine =
       freyaMachine {
           availableMediaTypes [
               MediaType.Json
               MediaType.Text ] }

   // A dynamic machine with a list of media types

   let mediaTypes =
       freya {
           // request logic
           return [ MediaType.Json ] }

   let dynamicMachine =
       freyaMachine {
           availableMediaTypes mediaTypes }

As you can see, the machine syntax does not need to change to accomodate different ways to specify the configuration property, while the last example of a dynamic property (``Freya<MediaType list>``) shows how the media types to be made available can be determined at runtime (perhaps based on available forms of the resource).

Entity Tag
^^^^^^^^^^

The ``entityTag`` configuration property allows the specification of an entity tag (ETag) for the resource. This property (if specified) will be returned with the resource, and may also be used to negotiate with the client if the client sends ``IfMatch`` or ``IfNoneMatch`` headers for tag based preconditions on a request.

The entity tag can be defined statically or dynamically (as a simple value or a Freya function):

.. code-block:: fsharp

   // A simple machine with a static entity tag

   let staticMachine =
       freyaMachine {
           entityTag (Strong "foo") }

   // A simple machine with a dynamic entity tag

   let etag =
       freya {
           // request logic
           return (Strong "foo") }

   let dynamicMachine =
       freyaMachine {
           entityTag etag }

As with other configuration properties, the machine configuration syntax will accomodate either way of specifying the entity tag without syntax change.

Last Modified
^^^^^^^^^^^^^

The ``lastModified`` configuration property allows the specification of a last modified date/time for the resource. This property (if specified) will be returned with the resource, and may also be used to negotiate with the client if the client sends ``IfModifiedSince`` or ``IfNotModifiedSince`` headers for time based preconditions on a request.

The date/time can be defined statically or dynamically (as a simple value or a Freya function):

.. code-block:: fsharp

   // A simple machine with a static date/time

   let staticMachine =
       freyaMachine {
           lastModified (DateTime (2000, 1, 1)) }

   // A simple machine with a dynamic date/time

   let modified =
       freya {
           // request logic
           return (DateTime (2000, 1, 1)) }

   let dynamicMachine =
       freyaMachine {
           lastModified modified }

As with other configuration properties, the machine configuration syntax will accomodate either way of specifying the date/time without syntax change.

Methods
^^^^^^^
