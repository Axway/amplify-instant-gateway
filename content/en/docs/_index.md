---
title: Welcome to Amplify Instant Gateway Documentation
linkTitle: Documentation
no_list: true
weight: 20
menu:
  main:
    weight: 20
---
## Overview

An API gateway is at the core of any API strategy and is the component that routes the traffic from your clients to your backend services and provides authentication, authorization, threat protection and multiple other security mechanisms.

The Amplify Instant Gateway or Instant Gateway is a modern and lightweight gateway that is used to instantly secure modern API assets wherever they are to accelerate productization, consumption, and business value delivery for API programs.

It enables API Providers to quickly secure their backend APIs using a range of authentication mechanisms, including OAuth, JWT Bearer Tokens and API Keys. Securing backend APIs via Amplify Gateway allows both data and business processes to be exposed securely for many use cases including point to point integrations as well as data exposed via the public web. In order to use a backend API proxied through and mediated by the Instant Gateway, consumers must first be granted access to the API and then have a credential  provisioned to enable them to securely call the API at runtime. 

For more information, use the navigation menu on the left to browse all documentation. You can also search for specific terms using the search field in the top right corner of any page.

## Key Benefits

These are some of the key benefits for your enterprise that can be achieved by the Instant Gateway:

* Be ahead of competition by ensuring a **faster time-to-market of your APIs**. Secure, govern and monetize new APIs through one easy interface. Allow users with a limited background in API Management to quickly manage and expose an API for consumption by interested parties.
* **Increase innovation** by offering new capabilities to your customers and your internal stakeholders. The Instant Gateway allows you to manage APIs with new protocols such as GraphQL or gRPC.
* **Grow** your setup along **with your customers**. Easily and automatically scale the Instant Gateway to allow expansion of traffic of existing customers.
* The Instant Gateway ensures you choose for a **future proof solution**. The solution is Fully cloud native, and incorporates the latest protocols (GraphQL, gRPC, HTTP/2, …). It is also designed in a manner that allows it to stay up to date with any new protocols that will be released in the future.
* **Less need for specialized resources** for maintenance and configuration. The lightweight approach and the use of a single interface for all API Lifecycle tasks ensures self-service by project teams or business departments. The design time is fully Axway managed and for the runtime you can choose to have Axway host the solution for you.
* **Automate everything.** The setup and configuration of the Instant Gateway is fully integrated in your CI/CD pipeline. All capabilities of the Instant Gateway are accessible through the UI, API and CLI. 
* **Save hosting costs** by using a modern architecture that helps you to ensure that there is no unused infrastructure. Start small, scale easily up and down and this in fully automated fashion

# Functional Capabilities

![Amplify Instant Gateway Overview](/Images/overview.png "Amplify Instant Gateway Overview")

The Instant Gateway consists out of 2 core components:

* **Governance Plane:** The governance plane is an integral part of the Amplify API Management Platform. The Amplify API Management Platform is a SaaS offering and hides integration complexity, enforces IT policy and scales at will, providing centralized visibility, control and a marketplace for all your integration assets. 

  The Instant Gateway is entirely integrated with this platform and provides these additional capabilities:

  * **API Lifecycle:** Managing the lifecycle of the APIs by governing, securing, deploying, and consuming the APIs running on the Instant Gateway Data Plane.
  * **Data Planes:** Manage the different Instant Gateway data planes.
  * **Usage:** Monitor the usage of the different APIs, such as number of calls, average/min/max response time.
  * **Traffic:** Provide traffic logs based on data sampling. The full traffic logs can easily be integrated with an external traffic monitoring tool.

  All capabilities offered by the Governance Plane are available through UI, CLI and API.
* **Data Plane:** The data plane is a containerized component used to process the API traffic. It is the engine that is used for:

  * **Authentication/Authorization:** ensure that only the correct clients and users have access to your APIs. The Instant Gateway supports amongst others API Key, JWT, OAuth, mTLS.
  * **Threat Protection:** Protect your backend from attacks and unwanted traffic. The instant gateway offers advanced rate limiting capabilities to protect your backend from DDoS attacks.
  * **Other security mechanisms:** Beside the AuthN/AuthZ and threat protection mechanism mentioned above, the Instant Gateway provides additional capabilities such as CORS (Cross Origin Resource Sharing), TLS, and certificate verification.
  * **Usage and Metrics tracking:** Usage and metrics can be tracked through the governance plane and the data plane communicates with the governance plane to provide the usage and metrics.
  * **Support for multiple protocols:** REST, HTTP 1.1, HTTP 2.0, OAS3 (3.0.1, 3.0.2), GraphQL and gRPC.
