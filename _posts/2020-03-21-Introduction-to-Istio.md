**What is Istio?**
When applications are broken into small chunks to simplify development, deployment, updates, and scaling, you end up with more responsibility to look after than before.


Luckily, developers now have access to a tool called Istio, an open source service mesh, to help address these challenges before it becomes a bit of a mess.

**What is a Service Mesh?**

In a microservice architecture, a service mesh is a term that is used to describe a network of microservices that make up an application and interactions they have between them. As soon as a service mesh grows in size and complexity, which it will since you will add new features and functionalities over time, it becomes harder to manage and understand.

A service mesh often has complex operational requirements which include A/B testing, canary releases, access control, and rate limiting. These are in addition to its standard requirements of load balancing, discovering, failure recovery, end-to-end authentication, monitoring, and metrics.

**How is a Service Mesh Different to an API Gateway?**

A question that I get asked a lot when I talk to people about service mesh is, “How is a service mesh different to an API gateway?”. And it’s a good question.

There is significant overlap between an API gateway and a service mesh. They both handle service discovery, authentication, monitoring, and rate limiting, but where they differ lies in their architecture and intention.

The purpose of a service mesh is to manage the internal service-to-service interaction, whereas an API gateway is mainly concerned about looking after the external client-to-service communication.

alt text Source: DZone

In case if you’re wondering if you need both, you probably do. API gateways accept traffic from outside your network for it to be distributed internally, and a service mesh helps to route and manage traffic internally. When working together, a service mesh can take advantage of the API gateway’s functionality of accepting the external traffic so that it can effectively route it to all the services in the mesh.

That said, we reckon service mesh will evolve and incorporate much of the functions that you get in an API gateway.
What is Istio?

Istio is an open source service mesh that is developed by Google. It provides all the fundamental tools to help you run a distributed microservice architecture.

The Istio service mesh architecture is logically split into two; the data plane and the control plane.

alt text Source: Istio.io

The data plane is responsible for handling the network traffic between the services in the mesh. It is made up with a set of intelligent proxies, known as Envoy, which are deployed as sidecars which mediate and control all the communication between the microservices along with the Mixer, the telemetry and general policy hub (we will explain what these components are in a bit more detail later).

As for the control plane, it manages and configures the proxies, via the control plane API, to route traffic. The control plane also configures the Mixer to collect telemetry and enforce policies.
Istio Components Explained

The following explains all the different components in the Istio architecture.
Envoy

Istio utilizes an extended version of the Envoy proxy, a high-performance proxy that is written in C++ to facilitate all inbound and outbound traffic of all the services within the mesh. Istio actually leverages many of Envoy’s built-in features, which consists of dynamic service discovery, load balancing, TLS termination, health checks, and rich metrics to name a few.

As mentioned, the Envoy proxy is deployed as a sidecar. This deployment allows Istio to extract a wealth of information from the signal such as traffic behavior and attributes.
Mixer

The Mixer is a component that is platform-independent. It enforces usage policy and access control rights across the mesh and also retrieves telemetry data from Envoy and other services.

The component also includes a flexible plugin model, which enables Istio to be used in various host environments and infrastructures.
Pilot

The Pilot component incorporates the rules provided by the control plane regarding traffic behavior and converts them into configurations which are applied by the Envoy. Pilot also provides service discovery for Envoy sidecars and intelligent routing to assist with traffic management.
Citadel

Citadel manages and controls end-user authentication with built-in identity management. Citadel can be used to upgrade any unencrypted traffic in the service mesh.
Galley

Galley validates user-specified configurations on behalf of the control plane. This allows Istio to be used transparently across different orchestration systems.
Istio Features & Benefits Explained

Now that you’re familiar with Istio, and its architecture, let’s take a look at the core features and capabilities that it provides.
Better Traffic Management

Istio’s accessible configuration rules and traffic routing helps you better manage the flow of traffic internally between services as well as between API calls and services. In terms of configuration, Istio lets you easily adjust several service-level properties like timeouts and circuit breakers, and you can also carry out tasks like A/B testing, canary rollouts, and staged rollouts.
Security

Istio’s ability to manage authentication, authorization, and encryption of microservice interaction at scale frees up developers so they can focus on maintaining security on an application level. Plus, Istio secures service communication by default.
Observability

Istio’s monitoring and logging capabilities provide deeper insights into how your service mesh deployment operates. This will help you gain a better understanding of how your service performance is impacting your other processes.

Thanks to the Mixer component, users can get absolute control over all interactions that happen within the mesh.
Integration and Customization

Thanks to its open source license, Istio can be customized and extended to integrate with legacy systems for ACLs, monitoring, logging, quotas, auditing, and more.
Platform Support

Istio was designed to be platform-independent and can be used in a host of different environments including cloud and on-premise. Istio can be deployed on Kubernetes, Mesos, Consul, and more.
At BoxBoat, We know Istio

Istio helps streamline traffic management, security, and observability issues—all common obstacles when it comes to building and scaling a microservice architecture.

