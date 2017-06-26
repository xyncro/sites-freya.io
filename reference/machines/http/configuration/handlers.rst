Handlers
========

Definition
----------

Defining handlers to provide responses is slightly more complex than defining :doc:`decisions`, but is still a well-typed and straightforward process. The definition of handlers gains some extra complexity due to support for (optional) content-negotiation, and this leads to more potential types for a handler.

Types
^^^^^

As with :doc:`decisions`, the handler can be defined as multiple types due to the use of static type inference in the HTTP machine computation expression. This allows for different types to be provided transparently, based on your goal for the handler.

Static
""""""

The simplest handler implementation is to provide nothing but a ``Representation``. A Representation defines the data to be returned as the response, both the payload itself (Data, as a byte array) and any information on media type, language, etc. which is relevant (as the type ``Description``). Here is an example of a handler of this type which will always return a very simple Hello World representation.

.. code-block:: fsharp

   // Static handler (Representation)

   let hello =
      { Description =
          { Charset = Some Charset.Utf8
            Encodings = None
            MediaType = Some MediaType.Text
            Languages = None }
        Data = Encoding.UTF8.GetBytes "Hello World" }

   // A simple HTTP machine with a static handler

   let staticHttp =
       freyaMachine {
           handleOk hello }

This will return the Hello World content with a text media type and a defined UTF-8 character set, but no definition for encodings or languages.

Dynamic
"""""""

The static handler is only of use for very simple content requirements, and does not allow you to change any additional properties of the response other than through the representation returned. A dynamic handler allows you to work with request and response (and other) data as you normally would with Freya functions -- a dynamic handler is simply a ``Freya<Representation>``.

As the handler is the last function to be executed, you can also set properties on the response inside the handler -- for example, adding or overriding response headers, status codes, etc. In this way you can easily customise the response where your requirements are outside of the HTTP implementation of the HTTP machine.

Here is an example of a machine with a dynamic handler, which also overrides the response reason phrase:

.. code-block:: fsharp

   // Dynamic handler

   let hello =
       freya {
           do! Freya.Optic.set Response.reasonPhrase_ (Some "Custom Phrase!")

           return {
               Description =
                 { Charset = Some Charset.Utf8
                   Encodings = None
                   MediaType = Some MediaType.Text
                   Languages = None }
               Data = Encoding.UTF8.GetBytes "Hello World" } }

   // A simple HTTP machine with a dynamic handler

   let dynamicHttp =
       freyaMachine {
           handleOk hello }

As you can see, there are no syntax changes required within the HTTP machine configuration when working with different handler types.

Negotiated Dynamic
""""""""""""""""""

The third, and most powerful handler type allows for a more controlled approach to negotiation. In the previous handler types, you've seen that the ``Representation`` returned defines the media type, encoding, etc. all of which could be the result of content negotiation during the execution of the machine. For this reason, the third handler type is ``Acceptable -> Freya<Representation>``, a function which takes an ``Acceptable`` type, which is the result of any content negotiation which may have occurred during the execution of the machine.

The ``Acceptable`` type gives enough information to determine an appropriate representation:

.. code-block:: fsharp

   type Acceptable =
       { Charsets: Acceptance<Charset>
         Encodings: Acceptance<ContentCoding>
         MediaTypes: Acceptance<MediaType>
         Languages: Acceptance<LanguageTag> }

    and Acceptance<'a> =
        | Acceptable of 'a list
        | Free

Using the information contained within the ``Acceptable`` type, and the individual ``Acceptance<_>`` value for each negotiable aspect, the handler can determine the representation to send, and construct it appropriately. The ``Acceptable`` case of the ``Acceptance<_>`` type is an **ordered** list of matched values, with the most preferred first.

Usage of the the negotiated dynamic handler is similar to the dynamic handler, but with the opportunity to determine the most appropriate representation after content negotiation.

.. code-block:: fsharp

   // Negotiated Dynamic handler

   let hello acceptable =
       freya {
           do! Freya.Optic.set Response.reasonPhrase_ (Some "Custom Phrase!")

           // Logic to determine and construct appropriate representation ...

           return { ... } }

   // A simple HTTP machine with a dynamic handler

   let dynamicHttp =
       freyaMachine {
           handleOk hello }
        
Reference
---------

The basic response properties will be set by default, but may be overridden by supplying any of the following handlers, which will also return an appropriate representation. Where no representation is to be returned, a supplied handler can return ``Representation.empty``.

2xx
^^^

.. raw:: html

   <table class="handlers">
     <thead>
       <tr>
         <th>Handler</th>
         <th>Status</th>
         <th>Notes</th>
       </tr>
     </thead>
     <tbody class="two">
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleOk</a></td>
         <td>200</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleOptions</a></td>
         <td>200</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.fallback.html">handleFallback</a></td>
         <td>200</td>
         <td>The fallback handler is not HTTP semantic, but provides a handler to deal with cases not covered by the HTTP machine in normal operation.</td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleCreated</a></td>
         <td>201</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.operation.html">handleAccepted</a></td>
         <td>202</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleNoContent</a></td>
         <td>204</td>
         <td></td>
       </tr>
     </tbody>
   </table>

3xx
^^^

.. raw:: html

   <table class="handlers">
     <thead>
       <tr>
         <th>Handler</th>
         <th>Status</th>
         <th>Notes</th>
       </tr>
     </thead>
     <tbody class="three">
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleMultipleChoices</a></td>
         <td>300</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleMovedPermanently</a></td>
         <td>301</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleFound</a></td>
         <td>302</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleSeeOther</a></td>
         <td>303</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.preconditions.html">handleNotModified</a></td>
         <td>304</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleTemporaryRedirect</a></td>
         <td>307</td>
         <td></td>
       </tr>
     </tbody>
   </table>

4xx
^^^

.. raw:: html

   <table class="handlers">
     <thead>
       <tr>
         <th>Handler</th>
         <th>Status</th>
         <th>Notes</th>
       </tr>
     </thead>
     <tbody class="four">
       <tr>
         <td><a href="../../../../_static/specifications.validations.html">handleBadRequest</a></td>
         <td>400</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.permissions.html">handleUnauthorized</a></td>
         <td>401</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.permissions.html">handleForbidden</a></td>
         <td>403</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleNotFound</a></td>
         <td>404</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.validations.html">handleMethodNotAllowed</a></td>
         <td>405</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.negotiations.html">handleNotAcceptable</a></td>
         <td>406</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.conflict.html">handleConflict</a></td>
         <td>409</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.responses.html">handleGone</a></td>
         <td>410</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.content.html">handleLengthRequired</a></td>
         <td>411</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.preconditions.html">handlePreconditionFailed</a></td>
         <td>411</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.validations.html">handleUriTooLong</a></td>
         <td>414</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.content.html">handleUnsupportedMediaType</a></td>
         <td>415</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.validations.html">handleExpectationFailed</a></td>
         <td>417</td>
         <td></td>
       </tr>
     </tbody>
   </table>

5xx
^^^

.. raw:: html

   <table class="handlers">
     <thead>
       <tr>
         <th>Handler</th>
         <th>Status</th>
         <th>Notes</th>
       </tr>
     </thead>
     <tbody class="five">
       <tr>
         <td><a href="../../../../_static/specifications.operation.html">handleInternalServerError</a></td>
         <td>500</td>
         <td>The internal server error handler will be invoked when an operation decision returns false.</td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.assertions.html">handleNotImplemented</a></td>
         <td>501</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.assertions.html">handleServiceUnavailable</a></td>
         <td>503</td>
         <td></td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.assertions.html">handleHttpVersionNotSupported</a></td>
         <td>505</td>
         <td></td>
       </tr>
     </tbody>
   </table>
