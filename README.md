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
* Although this is a frivolous example, the maze is a good metaphor for hypermedia
  applications in general. Any complex problem can be represented as a hypermedia maze
  that the client must navigate. If you’ve ever been trapped in a phone tree, or searched
  for products on an online store and then bought something from the search results,
  you’ve navigated a hypermedia maze.
* A link relation is a magical string associated with a hypermedia control like Maze+XML’s
  \<link\> tag. It explains the change in application state (for safe requests) or resource
  state (for unsafe requests) that will happen if the client triggers the control
* One of the most important web pages for a RESTful API developer is the registry of link
  relations managed by the Internet Assigned Numbers Authority (IANA).
  http://www.iana.org/assignments/link-relations/link-relations.xhtml
  I’ll be coming back to this registry throughout the book. It contains about 60 link relations that have
  been deemed to be generally useful and not tied to a particular data format. The simplest
  examples are the "next" and "previous" relations, for navigating a list.
* Chapter 9 includes a guide explaining when it’s OK to use the shorter names of registered
  relations. Here’s a summary:
* * You can use extension relations wherever you want.
* * You can use IANA-registered link relations whenever you want.
* * If a document’s media type defines some registered relations, you can use them
  within the document.
* * If a document includes a profile that defines some link relations (see Chapter 8),
  you can treat them as registered relations within that document.
* * Don’t give your link relations names that conflict with the names in the IANA
  registry
  (this refer to the string that is put in the "rel" attribute of the "link" html element)
  https://www.w3schools.com/tags/tag_link.asp
*  The URL to the collection of mazes is the proverbial “URL advertised on the billboard.”
   Starting with no information but this URL, you can do everything it’s possible to do
   with a Maze+XML API:
   1. Start off by GETting a representation of the collection of mazes. You know how to
   parse the representation, because you read the Maze+XML specification and pro‐
   grammed this knowledge into your client.
   2. Your client also knows that the link relation maze indicates an individual maze. This
   gives it a URL it can use in a second GET request. Sending that GET request gives
   you the representation of an individual maze.
   3. Your client knows how to parse the representation of an individual maze (because
   you programmed that knowledge into it), and it knows that the link relation start
   indicates an entrance into the maze. You can make a third GET request to enter the
   maze.
   4. Your client knows how to parse the representation of a maze cell. It knows what
   east, west, north, and south mean, so it can translate movement through an ab‐
   stract maze into a series of HTTP GET requests.
   5. Your client knows what exit means, so it knows when it’s completed a maze.
* Is Maze+XML an API?
  If you’ve got experience in this field, you may be wondering: where’s the API? A maze
  game isn’t a complex application, but even so, you may have expected more than a few
  XML tag names and link relations. The Maze+XML specification lacks the things you
  may be accustomed to. It doesn’t define any API calls or give any rules for constructing
  URLs. In fact, it barely mentions HTTP at all! I’ve shown some URLs in the example
  representations, but I deliberately made the URL formats internally inconsistent (com‐
  pare /beginner to /expert-maze/start) so you wouldn’t think URL formats were defined
  by the standard.
* But this book focuses on web APIs, which is to say, web-scale APIs (i.e., APIs where any
  member of the public can use a client, or write a client, or, in some cases, write a server).
  When you allow someone outside your organization to make API calls, you make that
  person a silent partner in the implementation of your server. It becomes very difficult
  to change anything on the server side without hurting this unknown customer
* Designs based on hypermedia have more flexibility. Every time the client makes an
  HTTP request, the server sends a response explaining which HTTP requests make the
  most sense as a next step. If the server-side options change, that document changes
  along with it. This doesn’t solve all of our API problems—the semantic gap is a huge
  problem!—but it solves the one we know how to solve.
* Imagine starting up a web browser that’s only ever been tested against one particular
  website. As soon as you send that browser to a site it wasn’t tested on, it’s going to crash.
  That’s the situation here. A standard like Maze+XML may have multiple server imple‐
  mentations. Client implementations need to be designed to work against all server im‐
  plementations, not just one.
* The similarity is no coincidence. As I said at the beginning of this chapter, the maze is
  a metaphor for hypermedia applications in general. Some “mazes” are tidy and wellbehaved. Others are chaotic
  and infinitely large. Thinking of a state diagram as a maze
  to be navigated will get you in the right frame of mind to understand hypermedia APIs.
* But just because a data format doesn’t include hypermedia controls doesn’t mean it’s
  useless. In Chapter 8, I’ll show you how JSON-LD can add basic hypermedia capabilities
  to any JSON format. In Chapter 10, I’ll show how XForms and XLink can do the same
  for XML. These technologies let you graft hypermedia controls onto an existing API
  that doesn’t include them.
* I showed in Chapter 4 how you can use the HTTP header Link to add simple hypermedia
  links and forms to documents that have no hypermedia controls of their own. Using
  these headers, you could conceivably design an API that served nothing but JPEG im‐
  ages, but I don’t recommend this.
* This is another reason why it’s important to look for domain-specific data formats before
  you set off to design your API. A standard like vCard represents a lot of time and money
  spent identifying the application-level semantics for a problem domain. You don’t need
  to start over just because vCard doesn’t have hypermedia controls
*  A hypermedia-aware script is less likely to break when something trivial happens, like
   the URL of a resource changing or new data being added to a representation. This means
   a hypermedia API has some room to change without breaking the scripts that depend
   on it. But a script is a playback of a human being’s thought process. If it encounters a
   situation the human didn’t originally consider, the script won’t be able to fill in the blanks.

## 6. The collection pattern
* Collection+JSON is one of several standards designed not to represent one specific
  problem domain (the way Maze+XML does), but to fit a pattern—the collection—that
  shows up over and over again, in all sorts of domains. This standard makes a good
  example, because it’s a formalized version of the JSON-based APIs that first-time de‐
  signers tend to come up with. Collection+JSON lets you follow your natural design
  inclinations without running afoul of the Fielding constraints.
* If there’s no domain-specific standard for your problem domain (and there probably
  isn’t), you may be able to use a collection-based standard instead. Instead of starting
  from nothing, you’ll be able to focus on adapting your application semantics to the
  collection pattern. Not only will you save time, you’ll get access to a preexisting base of
  client programs and server-side tools.
* A collection resource is a little more specific than that. It exists mainly to group other
  resources together. Its representation focuses on links to other resources, though it may
  also include snippets from the representations of those other resources. (Or even the
  full representations!)
* (See book notes for a nice description of Collection+JSON)
* There are a number of generic link relations for navigating paginated lists. These include
  "next", "previous", "first", "last", and "prev" (which is a synonym for “previous”).
  These link relations were originally defined for HTML, but now they’re registered with
  the IANA, so you can use them with any media type
* Google, the biggest corporate adopter of AtomPub, uses Atom
  documents to represent videos, calendar events, cells in a spreadsheet, places on a map,
  and more.
* But the AtomPub story also shows that “nothing wrong with the standard” isn’t good
  enough. People won’t go through the trouble of learning a standard unless it’s directly
  relevant to their needs. It’s easier to reinvent the “collection” pattern using a fiat standard
  based on JSON, so that’s what thousands of developers did—and continue to do
* But an "item" still isn’t anything in particular. It’s almost as vague a term as “resource.”
  In a microblogging API, an "item" will be a bit of text with a timestamp. In a payment
  processor, an "item" will include a creditor, a debitor, a method of payment, and an
  amount of money. There’s still an enormous gap between the application semantics of
  the collection pattern and the application semantics of your individual API.

## 7. Pure hypermedia designs
* Nothing says you have to use the collection pattern, but it is the most popular design
  pattern for APIs. If you want to implement some other pattern, or if your API design
  doesn’t fit any particular pattern, you can describe an API’s semantics using pure hy‐
  permedia. You don’t have to create an entirely new standard like Maze+XML, with its
  own media type. You can represent the state of your resources using a generic hypermedia language.
* HTML has distinct advantages even for an API designed to be consumed entirely by
  machines. HTML imposes more structure on a document than XML or JSON does, but
  not so much structure as to solve only one specific problem, the way Maze+XML does.
  HTML sits somewhere in the middle, like Collection+JSON.
* More important, HTML has built-in hypermedia controls. I mentioned these controls
  back in Chapter 4, but just to recap, here are the most important ones:
* * The /< link /> tag and /< a /> tag are simple outbound links, like the /< link /> tag in Maze
  +XML. They tell the client to make a GET request to a specific URL in order to get
  a representation. That representation becomes the current view.
* * The /< img /> tag and /< script /> tags are embedding links. They tell the client to au‐
  tomatically make a GET request to another resource, and to embed the represen‐
  tation of that resource in the current view.
* * When the /< form /> tag has the string "GET" as its method attribute (i.e. /< form meth
  od="GET">), it acts as a templated outbound link. This works like a URI Template,
  or the queries slot in Collection+JSON. The server provides the client with a base
  URL and some input fields (HTML /< input /> tags). The client plugs in values for
  those fields, combines them with the base URL to form a one-of-a-kind destination
  URL, and makes a GET request to that URL.
* * When the /<form/> tag has "POST" as its method attribute, it describes an HTTP POST
  request that can do anything at all. The /< input /> tags are still present, but instead
  of being used to create the request URL, they’re used to create an entity-body with
  the media type application/x-www-form-urlencoded. The request URL is hard‐
  coded in the action attribute of the <form> tag
* hCard is a microformat: a lightweight industry standard defined through informal collaboration on a
  wiki, rather than through the formal IETF process that results in RFCs.
* Microdata is a refinement of the microformat concept for HTML 5. You see, microfor‐
  mats are kind of a hack. HTML’s class attribute was designed to convey information
  about visual display (via CSS), not to convey bits of application semantics
* The main source of microdata items is schema.org, a project of four big search engines
  (Bing, Google, Yahoo!, and Yandex) to define application semantics for different prob‐
  lem domains. Search engines have an interest in understanding the high-level applica‐
  tion semantics of a web page—that is, whatever real-world thing the web page is talking
  about.
* you’ll start seeing examples that refer to schema.org microdata items. To a
  human being, it should be pretty obvious what they mean. The URL http://schema.org/Person
  points to the schema.org microdata item corresponding to our everyday notion
  of a “person.”
* What exactly should the HTML documents look like? I didn’t define this because I
  don’t care. Your hMaze implementation can serve full human-readable documents
  with lots of flavor text, or it can serve very compact documents optimized for au‐
  tomated clients. As long as you use the hMaze CSS classes and link relations cor‐
  rectly, your choice will have no effect on the application semantics of the maze.
* If you find yourself writing up (or generating) documentation like this example, you’re
  using human-readable documentation as a substitute for hypermedia. That’s unacceptable.
  You’re creating useless work for yourself and your users.
*  I don’t have to provide a template for constructing the action URL, or force the client
   to reckon with my internal concept of a “switch ID,” because the <form> tag for flipping
   a switch includes the actual URL the client should use. I don’t have to make caveats like
   “You can only flip a switch if you’re in the same cell as the switch,” because hypermedia
   controls are presented only when they can be used. If the submit button isn’t there, the
   state transition isn’t available
* I used to think that you should design APIs by identifying the resources and tying them
  together with hypermedia. This resource-oriented approach is good advice when you’re
  trying to move away from publishing all your internal methods as a huge list of API
  calls. Thinking in terms of resources will at least group the API calls together in sensible ways.
* But in a hypermedia-based design, resources don’t matter as much. The designer’s job
  is to identify all the state transitions. A resource-oriented design would focus heavily
  on the mysterious switch as a resource, as a thing in itself. But the switch itself isn’t all
  that important. My design focuses on the state transition, on what you can do with the
  switch.
* HTML includes a lot of hypermedia controls, but the controls can’t describe all of
  HTTP’s protocol semantics. There’s no way to tell an HTML client to make a PUT
  or DELETE request without using JavaScript.
* This is why I think HAL strips too much away from HTML. How are you supposed to
  know which of the infinite possibilities are present in a given link? The <link> tag with
  rel="east" should trigger a GET request that gives you a representation of the cell to
  the east. The /< link /> tag with rel="flip" should trigger a POST request that flips the
  switch. One of them is a safe operation that modifies application state; the other is an
  unsafe, non-idempotent operation that modifies resource state. In HAL, those two links
  look almost identical. The only real difference is the link relation.
* Siren - I’ll close out this chapter by taking a brief look at another general hypermedia format: Siren.
  Siren is a newer format than HAL, and although it’s based on JSON, it takes a
  more HTML-like approach to hypermedia than HAL’s minimalism.
* Siren is designed to represent abstract groupings of data it calls entities. A Siren “entity”
  is conceptually similar to HTML’s < di v> tag. It’s a convenient way of splitting up your
  data. An entity may be an HTTP resource with its own URL, but it doesn’t have to be.
* The Semantic Challenge: How Are We Doing?
* * We have a client-server Internet protocol, HTTP, which assigns very general meanings to different kinds of
requests: GET, POST, PUT, and so on.
* * We have the idea of hypermedia, which allows the server to tell the client which HTTP
requests it might want to make next. This frees the client from having to know the shape
of the API ahead of time. 
* * And we have a whole lot of standards for building APIs

## 8. Profiles
* Over the past three chapters, I’ve built up a set of rules for designing a brand new API.
  There’s still some work to do on these rules, but I can now present them in something
  approaching their complete form:
* * Is there a domain-specific standard for your problem? If so, use it. Document any
  application-specific extensions (Chapter 5).
* * Does your problem fit the collection pattern? If so, adopt one of the collection
  standards. Define an application-specific vocabulary and document it (Chapter 6).
* * If neither of those is true, choose a general hypermedia format. Break down your
  application into its state transitions. Document those state transitions (Chapter 7).
* * At this point, you have your protocol semantics nailed down. The application se‐
  mantics are all that remain. Are there existing microdata items or microformats
  that cover your problem domain? If so, use them. Otherwise, define an application specific vocabulary and
  document it (Chapter 7).
*  HTTP’s Content-Type header is the clearest example of this. The value of this header
   tells you how to parse the entity-body. Some examples:
* * Content-Type: text/html
* * Content-Type: application/json
* * Content-Type: application/atom+xml
* * Content-Type: application/vnd.collection+json
* * Content-Type: application/vnd.amundsen.maze+xml
* If the media type is one that defines hypermedia controls (like an HTML document),
  then parsing a response document lets you know what HTTP requests you can make
  next. You now understand the document’s protocol semantics.
* If the media type is a domain-specific format (like Maze+XML), then parsing the document also gives you
  an understanding of a state in the problem space (like a maze cell). You now understand
  the document’s application semantics.
  Once you understand both the protocol semantics and the application semantics, you’re done. You (or your
  software) can make a decision based on the available information.
* These “missing” specifications aren’t really missing. For hCard, the specification is at
  http://microformats.org/wiki/hcard. For Twitter, the specification is at https://dev.twit
  ter.com/docs. I’m going to call these “missing” specifications profiles. Documents like
  these are the main topic of this chapter.
* What is a profile? A profile is defined to not alter the semantics of the resource representation itself, but to
  allow clients to learn about additional semantics… associated with the resource repre‐
  sentation, in addition to those defined by the media type…
* This is why I recommend you choose a full-featured hypermedia format like HTML or
  Siren as your representation format. You’ll still have to write a profile, but the profile
  needn’t contain a lot of detail about your API’s protocol semantics. That stuff will be
  embedded in the representations themselves.
*  As the API designer, you are responsible for documenting all of your
   link relations ahead of time, in a profile document or in the defini‐
   tion of a custom media type. The only exceptions are link relations that
   you took from the IANA registry (see Chapter 10). You are not ex‐
   cused from this if you think your link relations are self-explanatory,
   because they never are.
* HTML is really, really popular. It’s the dominant representation format on the
  human web. But it’s not the dominant format for web APIs. That honor belongs to JSON,
  with XML a runner-up. HTML is a very distant third or fourth.
*  Any JSON object that defines the property @context can be a JSON-LD context. This
   particular context explains the application semantics of a JSON representation, in terms
   of human-readable API documentation.
* If you’re designing an API, and you know that all the decisions about state transitions
  will be made by human end users, you don’t need a profile at all. Websites don’t have
  profiles. If you know that all the decisions will be made by automated clients, you don’t
  need embedded documentation at all.
* In Summary
  We can solve the semantic challenge with a combination of a well-chosen media
  type and a profile that fills in the gaps. Here’s the essential information necessary to
  solve it:
* * A link relation is a string describing the state transition that will happen if the client
            triggers a hypermedia control. Example: Maze+XML’s east relation, which lets you
            know that a certain link points to something geographically east of the current
            resource. Traditionally, the state transition is a change in application state (triggered
            with a GET request), but it can also be a change in resource state (triggered by PUT,
            POST, DELETE, or PATCH).
* * A semantic descriptor is a short string that indicates what some part of a represen‐
            tation means. Example: hCard’s fn descriptor, which is used as a CSS class to mark
            up a person’s name in HTML. Unlike “link relation,” this is a term I made up for
            this book.
* * Although link relations and semantic descriptors are meaningless on their own,
            there’s always some document nearby that contains a human-readable explanation.
            We call this document a profile
* *  Profiles have traditionally taken the form of tedious “API documentation.” But if
            you chose a good hypermedia format for your representations, your profile will just
            be a list of link relations and semantic descriptors, with a prose explanation for
            each. This optimization lets you create a machine-readable profile using XMDP,
            ALPS, or JSON-LD.
* * A machine-readable profile allows a client to automatically look up the humanreadable definition of a link
            relation or semantic descriptor. Machine-readable
            profiles can be searched and remixed. The ALPS Registry contains a lot of ALPS
            profiles to work with.
* * JSON-LD contexts can take the ad hoc JSON documents served by today’s APIs,
            and describe their application and protocol semantics in a machine-readable way.
            You can use JSON-LD to retrofit a JSON API with simple hypermedia controls,
            without breaking the API’s existing clients.
* *  ALPS profiles are representation agnostic. One ALPS profile can be applied to an
            HTML document, a HAL document, a Collection+JSON document, an ad hoc
            JSON or XML document, and many others.
* *  Profiles are not a substitute for human-readable text embedded in hypermedia
            representations. There are two different use cases here. Profiles allow developers to
            write smart clients. Text embedded in a representation allows a human being to use
            an application through a client that faithfully renders representations.

## 9. The design procedure
* You need to design an API: what should it look like? In
  this chapter, I’ll lay out a procedure that begins with business requirements and ends
  with some software and some human-readable documentation.
* Two-Step Design Procedure.. In its simplest form, the procedure has two steps:
* * 1-Choose a media type to use in your representations. This puts constraints on your
    protocol semantics (the behavior of your API under the HTTP protocol) and your
    application semantics (the real-world things your representations can refer to).
* * 2-Write a profile that covers everything else.
* This won’t necessarily give you a good API. In fact, this version of the procedure de‐
  scribes every API ever designed. If you wanted a really generic design that’s hard to
  learn, you’d blaze through step 1 by choosing application/json as your representation
  format. Since JSON puts no constraints on your protocol or application semantics, you’d
  spend most of your time in step 2, defining a fiat standard and describing it with human readable API documentation.
  That’s what most APIs do today, and that’s what I’m trying to stop. A big chunk of the
  work that goes into creating a fiat standard is unnecessary, and client code based on a
  fiat standard can’t be reused. But doing anything else requires some preparatory thought
  and a willingness to reuse other people’s work when possible.
* Seven-Step Design Procedure:
* * List all the pieces of information a client might want to get out of your API or put
       into your API. These will become your semantic descriptors.
       Semantic descriptors tend to form hierarchies. A descriptor that refers to a real world object like a person will
       usually contain a number of more detailed, more abstract descriptors like givenName. Group your descriptors
       together in ways that make intuitive sense.
* * Draw a state diagram for your API. Each box on the diagram represents one kind
     of representation—a document that groups together some of your semantic de‐
     scriptors. Use arrows to connect representations in ways you think your clients will
     find natural. The arrows are your state transitions, triggered by HTTP requests.
     You don’t need to assign specific HTTP methods to your state transitions yet, but
     you should keep track of whether each state transition is safe, unsafe but idempo‐
     tent, or unsafe and nonidempotent.
     At this point, you may discover that something you put down as a semantic de‐
     scriptor (the customer of a business) makes more sense as a link relation (a busi
     ness links to a person or another business using the link relation customer). Iterate
     steps 1 and 2 until you’re satisfied with your semantic descriptors and link relations
     Now you understand your API’s protocol semantics (which HTTP requests a client will
       be making) and its application semantics (which bits of data will be sent back and forth).
       You’ve come up with a list of magic strings (semantic descriptors and link relations)
       that make your API unique, and you know roughly how those magic strings will be
       incorporated into HTTP requests and responses. You can then move on to the following steps:
* * Try to reconcile your magic strings with strings from existing profiles. I list some
     places to look in “The Semantic Zoo” on page 230. Think about IANA-registered link
     relations, semantic descriptors from schema.org or alps.io, names from domain specific media types, and so on.
     This may change your protocol semantics! In particular, unsafe link relations may
     switch back and forth between being idempotent and not being idempotent.
     Iterate steps 1 through 3 until you’re satisfied with your names and with the layout
     of your state diagram.
* * You’re now ready to choose a media type (or define a new one). The media type
     must be compatible with your protocol semantics and your application semantics.
     If you’re lucky, you may find a domain-specific media type that already covers some
     of your application semantics. If you define your own media type, you can make it
     do exactly what you need.
     If you choose a domain-specific media type, you may need to go back to step 3, and
     reconcile your names for semantic descriptors and link relations with the names
     defined by that media type.
* *  Write a profile that documents your application semantics. The profile should ex‐
     plain all of your magic strings, other than IANA-registered link relations and strings
     explained by the media type.
     I recommend you write the profile as an ALPS document, but a JSON-LD context
     or a normal web page will also work. The more semantics you borrowed from other
     people in step 4, the less work you’ll have to do here.
     If you defined your own media type, you may be able to skip this step, depending
     on how much of this information you put in the media type specification
* *  Now it’s time to write some code. Develop an HTTP server that implements the
     state diagram from step 3. A client that sends a certain HTTP request should trigger
     the appropriate state transition and get a certain representation in response.
     Each representation will use the media type you chose in step 4, and link to the
     profile you defined in step 5. Its data payload will convey values for the semantic
     descriptors you defined in step 1. It will include hypermedia controls to show the
     client how to trigger the further state transitions you defined in state 2.
* * Publish your billboard URL. If you’ve done the first five steps correctly, this is the
     only information your users will need to know to get started with your API. You
     can write alternate human-readable profiles (API documentation), tutorials, and
     example clients to help your users get started, but that’s not part of the design.
* (following this the book goes deeper into each of these points..)
* If an API is described only in prose, then changing the prose means rewriting all the clients. That’s
  a big problem with current APIs, and it’s a problem I’m trying to mitigate with this book
* Example: You Type It, We Post It
  (very nice example in which the author goes through all the steps described in this chapter)
*  Some Design Advice
   Hopefully by this point you have a good idea of how I go through my design process.
   Now I’d like to bring up some practical lessons I’ve learned from developing and ap‐
   plying this process:
* *  Resources Are Implementation Details - 
     Most procedures for designing RESTful web APIs focus on resource design. But there
     are no resources here. The boxes in the state diagrams aren’t resources, they’re repre‐
     sentations of resources—the actual documents sent back and forth between client and
     server.
* *  Don’t Fall into the Collection Trap - 
     Don’t use your database schema as the basis for your API design.
     Draw a state diagram instead. Why ? When you publish an API based on a database schema, changes to the schema
  become basically impossible. You’ve given a software dependency on your database
  schema to thousands of people you’ve never heard of. These people are your clients and
  supporting them is your responsibility. Changes to the schema, changes that your web‐
  site users won’t even notice, will cause big problems for your API users.
* * Don’t Start with the Representation Format
    It’s fine to use a general format like HTML for doodling, but I recommend you hold off on
    a decision until you’ve gotten, however tentatively, to step 4. That’s because represen‐
    tation formats aren’t just passive containers for data. They introduce assumptions about
    protocol and application semantics into every API that uses them. These assumptions
    may conflict with your business requirements.
* * URL Design Doesn’t Matter
    Some API design guides, including the original RESTful Web Services, spend a lot of
    time talking about the URLs you should assign to your resources. Each URL you serve
    should clearly identify the resource in such a way that a human being looking at the
    URL can figure out what’s on the other end. Again, there’s nothing wrong with nice-looking URLs. Nice-looking URLs are great!
    But they’re cosmetic. They look nice. They don’t do anything. Your API clients should
    continue to work even if all of the nice-looking URLs were suddenly replaced with
    randomly generated URLs
* * Standard Names Are Probably Better Than Your Names
    Let’s say that your application semantics include “a person’s first name.” You’d write that
    down in step 1, you’d try to fit it into a hierarchy, and you’d give it a temporary name
    based on the corresponding field in your database schema or your data model. Some‐
    thing like first_name, firstname, first-name, fn, first name, first, fname, or giv
    en_name. That’s fine for step 1. But when you get to step 3 you need to look around,
    notice that there are lots of existing profiles for describing peoples’ first names, and
    adopt one of them .You may be attached to the names you chose in step 1. But you’re not doing this API
    for yourself. You’re doing this for your users.
    Over their careers, users will consume lots of different APIs, and they’ll benefit from not having
    to learn 20 slightly different names for the same thing.
* *  When Your API Changes
     One of today’s most hotly debated topics in the API community is versioning. It’s an
     enormous problem. Most companies that put out an API never change that API after
     its initial release. They can’t do it. To be blunt, they can’t do it because they ignored the hypermedia constraint. Most APIs
     put their protocol and application semantics into human-readable documentation. The
     users of those APIs then write a bunch of client software based on that documentation.
     Now the API providers are stuck. They can change the documentation, but doing so
     won’t automatically change the behavior of all those clients. They’ve given their users
     veto power over any change in their design.
     If you change a resource and your clients can’t automatically adapt to the change, you’ll
     need to spend some period of time effectively publishing two different resources—the
     old one and the new one—with different application or protocol semantics. There are
     three common strategies for doing this.
* * * Partitioning the URL space
      In the most common versioning technique, the entire API is split into two disjoint APIs.
      Sometimes the two APIs have different billboard URLs, like http://api-v1.example.com/ and http://api-v2.example.com/.
* * * Versioning the media type
      If you defined a domain-specific media type for your application, you can give it a
      version parameter. Clients can then use content negotiation (see Chapter 10) to ask for
      one version or the other:
      Accept: application/vnd.myapi.document?version=2
      I don’t think you should define a domain-specific media type in the first place, but even
      if you do, this is a bad idea. Your media type is not your API. Here’s a thought experiment:
      could another company use your media type in their own, unrelated API?
* * * Versioning the profile
      I recommended that you base your API around a standardized media type, and obvi‐
      ously you can’t go in and declare a new version of someone else’s media type. But I also
      recommended that you define your application semantics in a machine-readable profile,
      and you can declare a new version of a profile.
* (more ideas about API versioning follow this in the book)
*  Adding Hypermedia to an Existing API
   You should be able to get your API up to the level of quality I advocate in this book,
   without breaking your existing clients. Here’s a modified version of the seven-step pro‐
   cess I laid out earlier, for fixing up a JSON-based API:
* * Document all your existing representations. Each one will contain a number of
     semantic descriptors. You can’t change these, but you should be able to add new
     ones.
* * Draw a state diagram for your API. The boxes on the diagram are your existing
    representations. You probably won’t have any state transitions, because most exist‐
    ing APIs don’t have any hypermedia links. Now’s the time to add some. Use arrows
    to connect representations in ways that make sense. The names of the arrows are
    your link relations. At this point it may turn out that some of your semantic descriptors are actually
    link relations:
    { "homepage": "http://example.com" }
    You can convert them to link relations at this point, but be sure not to rename them
    when you get to the next step.
* * You can’t change the name of anything you wrote down in step 1, because that would
    break your existing clients. But you can go through the link relations you created
    in step 2, and make sure their names come from the IANA and other well-known
    sources whenever possible.
* * You can’t change your media type, because that would break your clients. It’ll have
     to stay application/json (or whatever it is now).
* * Since you can’t change the media type, all your application semantics and protocol
    semantics must be defined somewhere else. You’ve got two choices: an ALPS profile
    or a JSON-LD context.
    If you wrote down any unsafe link relations in step 2, your best choice is JSON-LD
    with Hydra (see Chapter 12). You should be able to take your human-readable
    descriptions of API calls and convert them into machine-readable Hydra operations.
* * You’ve already got most of the code written. You’ll just need to extend each repre‐
   sentation by serving appropriate links.
* * Your billboard URL will be the same as before. If you didn’t have one before, because
    your API was a group of discrete API calls, you can create a new resource to act as
    your home page, and know that only hypermedia-aware clients will access it.

## 10. The Hypermedia zoo
## 11. HTTP for APIs
## 11. Resource description and linked data
## 12. CoAP: REST for embedded systems
