# AWS SSO - Configure Azure Active Directory as external IDP | How to integrate Azure AD with AWS SSO

Pre-requisites:
1. You must have the custom domain (in my case I am having tushar.online)
2. EKS cluster
3. Azure account



Steps 
======

AWS SSO - Configure Azure Active Directory as external IDP | How to integrate Azure AD with AWS SSO

Integrate Azure Active Directory as an external Identity Provider for AWS SSO.

SAML: 
- Security Assertion Markup Language 2.0 (SAML) is an open federation standard that allows an Identity Provider (IdP) to authenticate users 
and pass identity and security information about them to a Service Provider(SP). This info is sent in an XML document.
- SAML performs two functions:
    A. Authentication: Determining that users are who they claim to be.
    B. Authorization: Pass user Authorization to the Service Providers.
- An identity Provider (IdP) is a service that stores, manages identities, performs user authentication, and passes authorization to service providers.
- Service Provider (SP) trusts the IdP, uses IdP for user authentication.

  ![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/73b59cf8-7665-45fa-b74c-513f15e715e9)

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/8a3815ef-6562-49f9-b066-e23018c5d2d2)

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/523d223a-a45c-4ee3-9a4a-3830d38a7576)

1. Configure AWS SSO:



2. 


![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/ff8c911b-6cc0-4566-a4cd-91403bdeddc6)
