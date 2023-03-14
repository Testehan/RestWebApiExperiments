## Introduction
* Of course, Twitter, Facebook, and Google are big companies that compete with each
  other. They don’t want to make it easy for you to learn their competitors’ APIs. But small
  companies and nonprofits do the same thing. They design their APIs as though nobody
  else had ever had a similar idea. This interferes with their goal of getting people to
  actually use their APIs.
* When navigating the forest of standards, it’s useful to keep in mind that not all standards
  have equal force. Some are extremely well established, used by everyone, and if you go
  against them you’re causing a lot of trouble for yoursef. Other standards are just one
  person’s opinion, and that opinion might be no better than yours.
  I find it helpful to divide standards into four categories: fiat standards, personal stand‐
  ards, corporate standards, and open standards. I’ll be using these terms throughout the
  book, so let me explain each one in a bit more depth before we move on.
* Designing a new API today means reinventing a long series of wheels. Once your API
  is finished, your client developers have to reinvent corresponding wheels on the client side.
  Even under ideal circumstances, your API will be a fiat standard, since your business
  requirements will be slightly different from everyone else’s. But ideally a fiat standard
  would be just a light gloss over a number of other standards.

## 1. Surfing the web
* Resources = The URL http://www.youtypeitwepostit.com/ identifies a resource—probably the home
  page of the website advertised on the billboard
* Representations = When a web browser sends an HTTP request for a resource, the server sends a document
  in response (usually an HTML document, but sometimes a binary image or something
  else). Whatever document the server sends, we call that document a representation of the resource.
* So each URL identifies a resource. When a client makes an HTTP request to a URL, it
  gets a representation of the underlying resource. The client never sees a resource directly
* This principle is sometimes called statelessness. I think this is a confusing term because
  the client and the server in this system both keep state; they just keep different kinds of
  state. The term “statelessness” is getting at the fact that the server doesn’t care what state
  the client is in.
* Application state = Every state in this diagram corresponds to a particular page (or to no page at all) being
  open in Alice’s browser window. In REST terms, we call this bit of information—which
  page are you on—the application state.
*  Resource State=When the story begins, there are two messages in the message list: “Hello” and “Later.”
   Sending a GET to the home page doesn’t change resource state, since the home page is
   a static document that never changes. Sending a GET to the message list won’t change the state either.
   But when Alice sends a POST to the message list, it puts the server in a new state. Now
   the message list contains three messages: “Hello,” “Later,” and “Test.”
* Connectedness = The strands of the web are the HTML \<a\> tags and \<form\>
  tags, each describing a GET or POST HTTP request Alice might decide to make. I call
  this the principle of connectedness: each web page tells you how to get to the adjoining pages.
* The Web as a whole works on the principle of connectedness (see above), which is better known as
  “hypermedia as the engine of application state,” sometimes abbreviated HATEOAS. I
  prefer “connectedness” or “the hypermedia constraint,” because “hypermedia as the en‐
  gine of application state” sounds intimidating. But at this point, you should have no
  reason to find it intimidating. You know what application state is—it’s which web page
  a client is on. Hypermedia is the general term for things like HTML links and forms:
  the techniques a server uses to explain to a client what it can do next.
  To say that hypermedia is the engine of application state is to say that we all navigate
  the Web by filling out forms and following links.
* Web APIs Lag Behind the Web
* * Web APIs frequently have human-readable documentation that explains how to
  construct URLs for all the different resources. This is like writing English prose
  explaining how to find a particular file on an FTP server. If websites did this, no
  one would bother to use the Web.
  Instead of telling you what URLs to type in, websites embed URLs in \<a\> tags and
  \<form\> tags—hypermedia controls that you can activate by clicking a link or a button.
  In REST terms, putting information about URL construction in separate human readable documents violates the
  principles of connectedness and self-descriptive messages.
* * Lots of websites have help docs, but when was the last time you used them? Unless
    there’s a serious problem (you bought something and it was never delivered), it’s
    easier to click around and figure out how the site works by exploring the connected,
    self-descriptive HTML documents it sends you.
    Today’s APIs present their resources in a big menu of options instead of an inter‐
    connected web. This makes it difficult to see what one resource has to do with another.
* * Integrating with a new API inevitably requires writing custom software, or instal‐
    ling a one-off library written by someone else. But you don’t need to write custom
    software to use a new website. You see a URL on a billboard and plug it into your
    web browser—the same client you use for every other website in the world.
* * When APIs change, custom API clients break and have to be fixed. But when a
    website undergoes a redesign, the site’s users grumble about the redesign and then
    they adapt. Their browsers don’t stop working.
    In REST terms, the website redesign is entirely encapsulated in the self-descriptive
    HTML documents served by the website. A client that could understand the old
    HTML documents can understand the new ones

## 2. A Simple API
* The ideal API would have the same characteristics that make the World Wide Web easy
  to use. As a developer, you would be able to figure out how to use it, starting with nothing
  but a URL you saw on a billboard.
* How to Read an HTTP Response ? Well, every HTTP response can be split into three parts:
* * The status code, sometimes called the response code
    This is a three-digit number that summarizes how the request went. The response
    code is the first thing an API client sees, and it sets the tone for the rest of the
    response. Here, the status code was 200 (OK). This is the status code a client hopes
    for—it means that everything went fine.
* * The entity-body, sometimes called just the body
    This is a document written in some data format, which the client is expected to
    understand. If you think of a GET request as a request for a representation, you can
    think of the entity-body as the representation (technically, the entire HTTP response is the
    ‘representation’, but the important information is usually in the entitybody).
* * The response headers 
    These are a series of key-value pairs describing the entity-body and the HTTP re‐
    sponse in general. Response headers are sent between the status code and the entitybody
* The most important HTTP header is Content-Type, which tells the HTTP client
  how to understand the entity-body. It’s so important that its value has a special
  name. We say the value of the Content-Type header is the entity-body’s media
  type. (It’s also called the MIME type or the content type. Sometimes “media type” is
  hyphenated: media-type.)
  (example : the most common media types are text/html (for HTML) and image types like image/jpeg)
* Collection+JSON is a way of serving lists—not lists of data structures, which you can
  do with normal JSON, but lists that describe HTTP resources
* A document that doesn’t follow these rules isn’t a Collection+JSON document: it’s just
  some JSON. By allowing yourself to be bound by Collection+JSON’s constraints, you
  gain the ability to talk about concepts like resources and URLs. These concepts are not
  defined in JSON, which can only talk about simple things like strings and lists.
* If the publishers of microblogging APIs got together and agreed to use a common set
  of application semantics, the semantic gap for microblogging would disappear almost
  entirely. (This would be a profile, and I’ll cover this idea in Chapter 8.) The more con‐
  straints we share and the more compatible our designs, the smaller the semantic gap
  and the more our users benefit.

## 3. Resources and representations
* REST is not a protocol, a file format, or a development framework. It’s a set of design
  constraints: statelessness, hypermedia as the engine of application state, and so on. Col‐
  lectively, we call these the Fielding constraints, because they were first identified in Roy
  T. Fielding’s 2000 dissertation on software architecture, which gathered them together
  under the name “REST.
* From the client’s perspective, it doesn’t matter what a resource is, because the client
  never sees a resource. All it ever sees are URLs and representations.
* A representation can be any machine-readable document containing any information about a resource.
* We think of representations as something the server sends to the client. That’s because
  when we surf the Web, most of our requests are GET requests. We’re asking for repre‐
  sentations. But in a POST, PUT, or PATCH request, the client sends a representation to
  the server. The server’s job is then to change the resource state so it reflects the incoming
  representation
* The server sends a representation describing the state of a resource. The client sends a
  representation describing the state it would like the resource to have. That’s represen‐
  tational state transfer (REST).
* The Protocol Semantics of HTTP
* * GET - Get a representation of this resource.
* * DELETE - Destroy this resource.
* * POST - Create a new resource underneath this one, based on the given representation.
* * PUT - Replace this state of this resource with the one described in the given representation.
* * HEAD - Get the headers that would be sent along with a representation of this resource, but
    not the representation itself.
* * OPTIONS - Discover which HTTP methods this resource responds to.
* Sending a GET request to the server should have the same effect on re‐
  source state as not sending a GET request—that is, no effect at all. Incidental side effects
  like logging and rate limiting are OK, but a client should never make a GET request
  hoping that it will change the resource state.
* idempotence = Sending a request twice has the same effect on resource state as sending it once.
  The notion of idempotence comes from math. Multiplying a number by zero is an
  idempotent operation. 5 × 0 is zero, but 5 × 0 × 0 is also zero. 
  Multiplying by 1 is a safe operation, the way HTTP GET is supposed to be safe.
* OPTIONS is a good idea, but almost nobody uses it. Well-designed APIs advertise a
  resource’s capabilities by serving hypermedia documents (see Chapter 4) in response to
  GET requests. The links and forms in those documents explain what HTTP requests a
  client can make next. Poorly designed APIs use human-readable documentation to ex‐
  plain which HTTP requests a client can make.
* Now it’s time to reveal the skeleton in the HTTP closet. The HTTP POST method has
  a dirty secret, one you’ve certainly encountered if you’ve ever worked in web develop‐
  ment. POST is not solely used to create new resources. On the Web we surf with our
  browsers, HTTP POST is used to convey any kind of change. It’s PUT, DELETE, PATCH,
  LINK, and UNLINK all rolled into one.
* In terms of protocol semantics, this operation—“edit this blog post”—sounds like a PUT
  request. But an HTML form can’t trigger a PUT request. The HTML data format doesn’t
  allow it. So we use POST instead.
* The definition is so vague that a POST request really has no protocol semantics at all. POST doesn’t
  really mean “create a new resource”; it means “whatever!”
  I call this “whatever!” usage of POST overloaded POST. Because an overloaded POST
  request has no protocol semantics, you can only understand it in terms of its application
  semantics.
* The methods I recommend for use in most web APIs are GET, POST, PUT, DELETE,
  and PATCH(?..Before 2008, the PATCH method didn’t exist.)

## 4. Hypermedia
* The missing piece of the puzzle is hypermedia. Hypermedia connects resources to each
  other, and describes their capabilities in machine-readable ways. Properly used, hyper‐
  media can solve—or at least mitigate—the usability and stability problems found in
  today’s web APIs.
* Like REST, hypermedia isn’t a single technology described by a standards document
  somewhere. Hypermedia is a strategy, implemented in different ways by dozens of technologies
* The hypermedia strategy always has the same goal. Hypermedia is a way for the server
  to tell the client what HTTP requests the client might want to make in the future. It’s a
  menu, provided by the server, from which the client is free to choose. The server knows
  what might happen, but the client decides what actually happens.
* To sum up, the familiar HTML controls allow the server to describe four kinds of HTTP requests.
* * The /<a/> tag describes a GET request for one specific URL, which is made only if
  the user triggers the control.
* * The /<img/> tag describes a GET request for one specific URL, which happens auto‐
  matically, in the background.
* * The /<form/> tag with method="POST" describes a POST request to one specific URL,
  with a custom entity-body constructed by the client. The request is only made if
  the user triggers the control.
* * The /<form/> tag with method="GET" describes a GET request to a custom URL con‐
  structed by the client. The request is only made if the user triggers the control.
* Fielding (guy who first wrote about REST) dissertation:
  Hypermedia is defined by the presence of application control information embedded
  within, or as a layer above, the presentation of information.
* URI Versus URL ..What is the difference ?  As far as this book is concerned, the difference is this: there’s no
  guarantee that a URI has a representation. A URI is nothing but an identifier. A URL is
  an identifier that can be dereferenced. That is, a computer can somehow take a URL and
  get a representation of the underlying resource.  Here’s a URI that’s not a URL: urn:isbn:9781449358063. It designates a resource: the
  print edition of this book.
* Without a URL, you can’t get a representation. Without representations, there can be
  no representational state transfer. In general, when your web API refers to a resource, it should use a URL with the http
  or https scheme, and that URL should work: it should serve a useful representation in
  response to a GET request.
* That’s why API designers shouldn’t design APIs that serve plain JSON. You should use
  a media type that has real support for hypermedia. Your users will thank you. They’ll
  be able to use preexisting libraries written against the media type, rather than writing
  new ones specifically for your API.
* Providing the links is a step in the right direction. Out of the infinite set of legal HTTP
  requests, a hypermedia document explains which requests might be useful right now,
  on this particular site. The client doesn’t have to guess.
* A hypermedia format doesn’t have to be generic like HTML. It can be defined in enough
  detail to convey the application semantics of a wiki or a store. In the next chapter, I’ll
  talk about hypermedia formats that are designed to represent one specific type of prob‐
  lem. Outside that problem space, they’re practically useless. But within their limits, they
  meet the semantic challenge very well.

## 5. Domain specific designs
## 6. The collection pattern
## 7. Pure hypermedia designs
## 8. Profiles
## 9. The design procedure
## 10. The Hypermedia zoo
## 11. HTTP for APIs
## 11. Resource description and linked data
## 12. CoAP: REST for embedded systems
