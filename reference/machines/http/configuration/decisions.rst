Decisions
=========

Definition
----------

Defining decisions to configure the HTTP machine is simple. All decisions are optional (defaults for all decisions, whether simple default values or a default function) are built in to the HTTP machine, so you only need to implement decisions which are relevant for your resource.

Types
^^^^^

Decisions can be implemented as one of two types due the static type inference used as part of the HTTP machine computation expression. This is a useful feature, as it enables the optimisation of machines (see the topic on the design of machines, particularly :doc:`/topics/design/machines/decisions` and :doc:`/topics/design/machines/optimisation` for relevant details).

You can configure decisions in the HTTP machine as either static or dynamic (i.e. as either ``bool`` or ``Freya<bool>`` in this case). No special syntax is needed, type inference will allow them to be used interchangeably. However, it should be noted that decisions defined statically -- as ``bool`` -- will be optimised and removed from the decision tree underlying the machine.

.. code-block:: fsharp

   // A simple HTTP machine with a static decision

   let staticHttp =
       freyaMachine {
           serviceAvailable true }

   // A dynamic (Freya<'a>) function and a machine with a
   // dynamic decision

   let available =
       freya {
           <some health check logic>
           return [true|false] }
   
   let dynamicHttp =
       freyaMachine {
           serviceAvailable available }

As you can see from the above example, the computation expression will accept decision values of either type transparently.
   
Reference
---------

As shown in the clickable `HTTP Model Diagrams <../../../../_static/components.core.html>`_ the decisions are defined within re-usable functional blocks. A full reference for any block which contains decisions follows, giving the decision name, default values where applicable, and an explanation of the purpose and/or implementation of logic associated with each block and individual decision.

Assertions
^^^^^^^^^^

Assertions decisions handle basic decisions about the state of the system as a whole, and basic properties of the HTTP request being handled.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.assertions.html">serviceAvailable</a></td>
         <td>true</td>
         <td>Is the service available?</td>
       </tr>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.assertions.html">httpVersionSupported</a></td>
         <td>n/a</td>
         <td>Is the HTTP version (1.0, 1.1, etc.) supported? The default implementation will allow any HTTP versions of 1.1 or higher.</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.assertions.html">methodImplemented</a></td>
         <td>n/a</td>
         <td>Is the request method implemented/known about? The default implementation will compare the method with the basic sets of core HTTP methods, and any custom methods included in the <code>method</code> property.</td>
       </tr>
     </tbody>
   </table>

Conflict
^^^^^^^^

Conflict decisions handle decisions about the resource supplied by the client and whether it conflicts with the state of the resource known by the server.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.conflict.html">conflict</a></td>
         <td>false</td>
         <td>Does the resource supplied conflict with the server resource?</td>
       </tr>
     </tbody>
   </table>

Content
^^^^^^^

Content decisions handle decisions about the resource (and the representation of the resource) supplied by the client, such as whether the resource is of an acceptable media type (if a media type is specified).

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr>
         <td><a href="../../../../_static/specifications.content.html">lengthDefined</a></td>
         <td>n/a</td>
         <td>Is the length of the content defined?</td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.content.html">hasMediaType</a></td>
         <td>n/a</td>
         <td>Is the media type of the included representation defined?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.content.html">mediaTypeSupported</a></td>
         <td>n/a</td>
         <td>Is the media type of the included representation an acceptable media type? The implementation will refer to the <code>acceptableMediaTypes</code> property if it is defined. If the <code>acceptableMediaTypes</code> property is not configured, any media type will be accepted.</td>
       </tr>
     </tbody>
   </table>

Existence
^^^^^^^^^

Existence decisions handle decisions about the resource and whether it currently exists on the server.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.existence.html">exists</a></td>
         <td>true</td>
         <td>Does the resource currently exist on the server?</td>
       </tr>
     </tbody>
   </table>

Method
^^^^^^

Method decisions handle decisions about whether the request method matches a specified method (used to route request handling to a set of method specific decisions).

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.method.html">methodMatches</a></td>
         <td>n/a</td>
         <td>Does the request method match the method configured? The decision will always return <code>false</code> (and thus be pruned from the final decision tree) if the method is not configured to be allowed via the <code>methods</code> property if configured or the default methods allowed if not.</td>
       </tr>
     </tbody>
   </table>

Negotiations
^^^^^^^^^^^^

Negotiations decisions handle decisions about the requested representation (if any) requested by the client, and whether such a representation can be negotiated.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr>
         <td><a href="../../../../_static/specifications.negotiation.html">hasAccept</a></td>
         <td>n/a</td>
         <td>Does the request define acceptable media types?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.negotiation.html">acceptMatches</a></td>
         <td>n/a</td>
         <td>Is at least one acceptable media type available? The implementation will refer to the <code>availableMediaTypes</code> property if configured, otherwise it is assumed that the server will decide on an appropriately typed representation via a different process.</td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.negotiation.html">hasAcceptLanguage</a></td>
         <td>n/a</td>
         <td>Does the request define acceptable languages?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.negotiation.html">acceptLanguageMatches</a></td>
         <td>n/a</td>
         <td>Is at least one acceptable language available? The implementation will refer to the <code>availableLanguages</code> property if configured, otherwise it is assumed that the server will decide on an appropriate language via a different process.</td>
       </tr>       
       <tr>
         <td><a href="../../../../_static/specifications.negotiation.html">hasAcceptCharset</a></td>
         <td>n/a</td>
         <td>Does the request define acceptable character sets?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.negotiation.html">acceptCharsetMatches</a></td>
         <td>n/a</td>
         <td>Is at least one acceptable character set available? The implementation will refer to the <code>availableCharsets</code> property if configured, otherwise it is assumed that the server will decide on an appropriate character set via a different process.</td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.negotiation.html">hasAcceptEncoding</a></td>
         <td>n/a</td>
         <td>Does the request define acceptable encodings?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.negotiation.html">acceptEncodingMatches</a></td>
         <td>n/a</td>
         <td>Is at least one acceptable encoding available? The implementation will refer to the <code>availableEncodings</code> property if configured, otherwise it is assumed that the server will decide on an appropriate encoding via a different process.</td>
       </tr>
     </tbody>
   </table>

Operation
^^^^^^^^^

Operation decisions are slightly different to ordinary decision, as the operation decision (named ``do<Method>``, e.g. ``doPost``) is expected to have side effects. This is the right place to implement your behavioural logic, to take the action that the request was intended to trigger.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.operation.html">do&lt;Method&gt;</a></td>
         <td>true</td>
         <td>Has the action succeeded without error?</td>
       </tr>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.operation.html">completed</a></td>
         <td>true</td>
         <td>Has the action completed?</td>
       </tr>
     </tbody>
   </table>
   
Permissions
^^^^^^^^^^^

Permissions decisions handle basic decisions about the client and whether they are authorized and allowed to make the request.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.permissions.html">authorized</a></td>
         <td>true</td>
         <td>Is the request authorized? This might involve validating credentials, etc.</td>
       </tr>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.permissions.html">allowed</a></td>
         <td>true</td>
         <td>Is the request allowed? This might involve checking permissions, etc.</td>
       </tr>
     </tbody>
   </table>

Preconditions
^^^^^^^^^^^^^

Common
""""""

Common precondition decisions handle basic decisions about the resource and the knowledge that the client has of that resource, to determine whether an action should proceed, for any action.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr>
         <td><a href="../../../../_static/specifications.preconditions.html">hasIfMatch</a></td>
         <td>n/a</td>
         <td>Does the request define an if-match value?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.preconditions.html">ifMatchMatches</a></td>
         <td>n/a</td>
         <td>Does the if-match value supplied match an Entity Tag given for the resource? The implementation will refer to the <code>entityTag</code> property if configured, otherwise it will assume a successful match.</td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.preconditions.html">hasIfUnmodifiedSince</a></td>
         <td>n/a</td>
         <td>Does the request define an if-unmodified-since value?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.preconditions.html">ifUnmodifiedSinceMatches</a></td>
         <td>n/a</td>
         <td>Does the if-unmodified-since value supplied match an last modified time given for the resource? The implementation will refer to the <code>lastModified</code> property if configured, otherwise it will assume a successful match.</td>
       </tr>
     </tbody>
   </table>

Safe
""""

Safe precondition decisions handle basic decisions about the resource and the knowledge that the client has of that resource, to determine whether an action should proceed, for a safe action.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr>
         <td><a href="../../../../_static/specifications.preconditions.html">hasIfNoneMatch</a></td>
         <td>n/a</td>
         <td>Does the request define an if-none-match value?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.preconditions.html">ifNoneMatchMatches</a></td>
         <td>n/a</td>
         <td>Does the if-none-match value supplied (not) match any Entity Tags given for the resource? The implementation will refer to the <code>entityTag</code> property if configured, otherwise it will assume a successful (non-)match.</td>
       </tr>
       <tr>
         <td><a href="../../../../_static/specifications.preconditions.html">hasIfModifiedSince</a></td>
         <td>n/a</td>
         <td>Does the request define an if-modified-since value?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.preconditions.html">ifModifiedSinceMatches</a></td>
         <td>n/a</td>
         <td>Does the if-modified-since value supplied match an last modified time given for the resource? The implementation will refer to the <code>lastModified</code> property if configured, otherwise it will assume a successful match.</td>
       </tr>
     </tbody>
   </table>

Unsafe
""""""

Unsafe precondition decisions handle basic decisions about the resource and the knowledge that the client has of that resource, to determine whether an action should proceed, for an unsafe action.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr>
         <td><a href="../../../../_static/specifications.preconditions.html">hasIfNoneMatch</a></td>
         <td>n/a</td>
         <td>Does the request define an if-none-match value?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.preconditions.html">ifNoneMatchMatches</a></td>
         <td>n/a</td>
         <td>Does the if-none-match value supplied (not) match any Entity Tags given for the resource? The implementation will refer to the <code>entityTag</code> property if configured, otherwise it will assume a successful (non-)match.</td>
       </tr>
     </tbody>
   </table>

Responses
^^^^^^^^^

Created
"""""""

Created decisions handle decisions about the resource and whether it was created as part of the request processing.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.responses.html">created</a></td>
         <td>false</td>
         <td>Was the resource created in response to this request?</td>
       </tr>
     </tbody>
   </table>

Common
""""""

Commons decisions handle basic decisions about the resource and the representation (if any) that should be returned to the client.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.responses.html">noContent</a></td>
         <td>false</td>
         <td>Should (no) representation be returned to the client?</td>
       </tr>
     </tbody>
   </table>

Moved
"""""

Moved decisions handle basic decisions about the resource in situations where it is no longer present at the current URI.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.responses.html">gone</a></td>
         <td>false</td>
         <td>Has the resource gone from this URI?</td>
       </tr>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.responses.html">movedTemporarily</a></td>
         <td>false</td>
         <td>Has the resource been moved temporarily from this URI?</td>
       </tr>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.responses.html">movedPermanently</a></td>
         <td>false</td>
         <td>Has the resource been moved permanently from this URI?</td>
       </tr>
     </tbody>
   </table>

Other
"""""

Other decisions handle basic decisions about the resource where the resource should not be represented directly, but should refer to another resource or location for this resource.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.responses.html">seeOther</a></td>
         <td>false</td>
         <td>Should another resource be used instead of this one?</td>
       </tr>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.responses.html">found</a></td>
         <td>false</td>
         <td>Has this resource been found?</td>
       </tr>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.responses.html">multipleChoices</a></td>
         <td>false</td>
         <td>Are there multiple forms of this resource which may be relevant to the client?</td>
       </tr>
     </tbody>
   </table>

Validations
^^^^^^^^^^^

Validations decisions handle basic decisions about the request and whether it is of an appropriate form to be handled by the server.

.. raw:: html

   <table class="decisions">
     <thead>
       <tr>
         <th>Decision</th>
         <th>Default</th>
         <th>Description</th>
       </tr>
     </thead>
     <tbody>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.validations.html">expectationMet</a></td>
         <td>true</td>
         <td>Have implied HTTP expectations been met?</td>
       </tr>
       <tr class="influence">
         <td><a href="../../../../_static/specifications.validations.html">methodAllowed</a></td>
         <td>true</td>
         <td>Is the request method allowed for this resource? The implementation will refer to the <code>methods</code> property if configured, or the default set of methods if not.</td>
       </tr>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.validations.html">uriTooLong</a></td>
         <td>false</td>
         <td>Is the URI too long?</td>
       </tr>
       <tr class="configure">
         <td><a href="../../../../_static/specifications.validations.html">badRequest</a></td>
         <td>false</td>
         <td>Is the request bad or malformed in some way?</td>
       </tr>
     </tbody>
   </table>
