# What is Component-Based Architecture?
Paul Gillin
Paul Gillin
January 15, 2024
4 minute read
what is component architecture
Component-based architecture is a framework for building software based on reusable parts. Each component contains well-defined functionality into a binary unit that is stored in a library and then dropped into an application without modifying other components.

The benefits include reduced development and testing time, enhanced reliability (because components are pre-tested), and the flexibility to change applications by adding or replacing components without significant disruption.

What is component-based architecture?
Think of a component as a Lego block.

You can choose various shapes, sizes, and colors when building a structure out of Legos. Some blocks are purpose-built to be doors, windows, and other structural elements. Each block has all the features it needs to connect to others, and adding or subtracting blocks generally has minimal impact on the structure.

Software components are much more complex than little pieces of plastic, but the concept is similar.

Each element performs a task in an architecturally defined way. For simplicity, components are stored in a library, assembled by developers, and talk to each other through APIs.



An object request broker, sometimes called a “software bus,” facilitates communication by providing a single communications plane that all components use. Communication can occur in several ways, such as asynchronously, through broadcast, through a message-driven system, or as part of an ongoing data stream.

The component-based architecture concept isn’t new. You can find mentions of it in academic papers from as early as the late 1960s.

IBM introduced its System Object Model in the early 1990s, the first commercial effort to define a way to build software from components. Microsoft’s Component Object Model and Object Linking and Embedding, introduced around the same time, provided the first frameworks for commercial deployment.

Related Content
What Are Composite Apps?
Further reading

What Are Composite Applications? Use Cases, Benefits & More

5 features of components
Software components have five common characteristics;

1. Reusability
Components plug into a variety of applications without the need for modification or special accommodations.

2. Extensibility
Components combine with other components to create new behaviors.

3. Replaceability
Components with similar functionality can be swapped.

4. Encapsulation
Components are self-contained and expose functionality through interfaces while hiding the details of internal processes.

5. Independence
Components have minimal dependencies on other components and can operate in different environments and contexts.

Related Content

Further reading

5 Benefits of Developing with Containers

Examples of components
Components are the building blocks of service-oriented and microservices architectures, which assemble applications from loosely coupled and discrete services.

Microservices is the dominant architecture used in DevOps and cloud-native development. It’s valued for its productivity benefits, since developers assemble most of the application rather than building it from scratch.

One example of a component is the spreadsheet functionality in Microsoft PowerPoint. Users work with a spreadsheet that looks and acts like Microsoft Excel when editing the data underlying a chart or graph. While it’s not a full version of Excel, the spreadsheet widget has enough functionality to support essential functions.

Other examples of components include a function that calculates tax in an eCommerce transaction or one that challenges the user to answer questions upon login.

Tradeoffs with components
A component-based architecture can have drawbacks because it isn’t appropriate for every scenario.

For one thing, the application must be broken down into modular and functionally separate pieces, which can be a challenge when applications are large. Also, the need for a component’s reusability can limit its customization options.

Finding a component that exactly matches the requirements of an application can be challenging, too. Many components may need monitoring in a given application, and updates and maintenance of component libraries can be complex.

Alternatives to components
There are numerous alternatives to component-based development and architecture. These include:

Microkernel architecture comprises a core processing component and independent plug-in modules with specific functions. Components don’t communicate with each other—only with the microkernel.
Client-server architecture has two components that exchange requests for data, services, and content: client and server. Otherwise, they operate mostly independently.
Event-driven architecture consists of decoupled, purpose-built software modules that go into action in response to an event, such as swiping a credit card or generating an alert with a sensor.
Components have become a fundamental building block of modern software. Although the term is 20+ years old, the concept is sound and has proven to be adaptable to a constantly changing technology landscape.


source:
https://www.mendix.com/blog/what-is-component-based-architecture