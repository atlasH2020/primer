# ATLAS Primer for Software Developers

## Pre-requisites
This document assumes that you have a basic understanding of the ATLAS Network and will provide a short overview on how to get started to integrate your software with ATLAS.
You must also have a thorough understanding of OIDC/Oauth2.

## Introduction
A key design goal for ATLAS is to facilitate integration of existing software, as an add-on, without requiring a rewrite. Integrating with ATLAS means extending your software's capabilities by easily leveraging third-party ATLAS services. Integration is typically done in such a way that your software may still be usable without ATLAS features for those users that do not need them.

As a software provider, you remain fully in control of your software, it's development/release life-cycle, it's hosting and it's commercial strategy and pricing.

Depending on the nature of your software, there are two types of integration, non-exclusive, that may be suitable to your needs: leveraging third-party ATLAS services within your existing solution and/or exposing parts of your on-line solution as ATLAS services.

Note that most of the ATLAS examples in the atlasH2020 projects are provided in Python, but any language (C#, Java, Javascript, Ruby, PHP, etc.) with support for RESTful services and Oauth2 may be used.

## Registering as an ATLAS Participant
Whether you want to consume or provide ATLAS services, you will have to register your organization as an ATLAS participant on the ATLAS Participant Portal (https://participants-portal.iais.fraunhofer.de/). This process involves a background check of your organization; it is a necessary precaution to reduce the risk of rogue and mal-intentioned parties joining the ATLAS Network.

Upon success verification of your organization's existence and reputation, you will have access to your ATLAS Participant Portal in which you'll find your ATLAS participant credentials in the form of Oauth2 client_id/secret. These credentials are necessary to access the ATLAS registry api (https://github.com/atlasH2020/atlas-registry-api).

## I want to integrate existing ATLAS services into my software
The first step is to identify the kind of ATLAS Service Templates matches your needs and to explore the ATLAS Service Catalogue (https://sensorsystems.iais.fraunhofer.de/service-catalogue/service_catalogue) to check whether existing implementations may be of value to your organization or to your users.

Assuming you have found a service template (and implementing services) that can match your requirements, your next step is to implement a generic service client for the selected ATLAS service template. It is essentially a wrapper that implements the service template's API calls on the basis of the dynamic implementation's details which are retrieved from the ATLAS registry api. A sample service client implemented in python is provided in the atlas_hello_world_service_client project (https://github.com/atlasH2020/atlas_hello_world_service_client).

From the perspective of your software, the service client module you will develop for a given ATLAS service template will be able to transparently access any implementation available in ATLAS that may be selected.

While not mandatory, and only where relevant, it is preferable to let your users select the ATLAS service they prefer for the ATLAS template you have integrated; this is particularly important since the pairing to a service is done on a user-per-user basis, i.e. different users could use different services, and the user must have an account/subscription on the system providing the ATLAS service he selects. That means that your software should provide a suitable UI enabling the user to select his prefered service from a list of suitable services. Either operationm, listing and selection/pairing, can only be performed with the assistance of the ATLAS registry API; a sample Python implementation of an an ATLAS registry client is provided in https://github.com/atlasH2020/atlas_registry_client.

Your service client implementation must handle the persistence, on a user basis, of the pairing information and must handle refreshing of OAuth2 access token when needed. Again, Python examples of this are provided in the atlas_hello_world_service_client project and more specifically, in the atlas_service_client library (https://github.com/atlasH2020/atlas_service_client). As mentioned earlier, while these projects may be used for production implementations, they mainly serve as illustrations. You may implement the same functionality with different programming languages or using a architecture altogether with reasonable effort.

## I want to make parts of my software available as an ATLAS service
The first step, assuming you have a proper business case, is to identify which parts of your software could be mapped to one or more ATLAS service templates and to assess the level of effort required to map your existing data structures to those of service templates of interest. This type of integration may require more or less effort depending on the architecture of your existing software. In the context of this document, we assume you already have a proprietary API providing access to your functionality. 

We recommend implementation strategies that minimize the effort and disruption on your existing software.  The recommended integration approach is to use the facade pattern. You implement the API endpoints of the service template(s) in a module in which you handle the structure/format conversions that are needed to interface with your existing proprietary API.

An important requirement for ATLAS service providers is that they MUST support OAuth2 authentication. If you use a proprietary user/password database, you should consider moving this to an authentication server; to experiment evaluate this effort, you may use the Cognito Tutorial (https://github.com/atlasH2020/Cognito-Tutorial) which helps you setting up a fully functioning authentication server for development purposes in about 30 minutes. Of course, feel free to use other systems such as Google OAuth2, Azure, Okta, Auth0, Keycloak, etc

When your ATLAS service implementation ready, you must submit it via the ATLAS Participant Portal. Once submitted, a validation will be performed to insure that the pairing mechanism is properly implemented and that the API implementation is compliant with the selected ATLAS service template. At the end of the validation process, your ATLAS service will be published in the ATLAS registry and will become to potential consumers in the ATLAS Service Catalogue.


