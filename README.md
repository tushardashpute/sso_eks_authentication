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

Goto --> IAM Identity Center --> Settings --> Choose Identity source as External Identity Provider

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/6a49dfd6-681f-43a7-9f2f-1514aa6b1382)

Click Next --> Download service Provider Metadata file (This will be used when we integrate AWS SSO in azure side). This will be used to create trust between Azure AD and AWS.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/b33725d0-c4f0-4bec-b0c8-c00e460437e0)

Keep this window open and goto Azure Enterprize Application, there will add AWS SSO application:

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/b7f23792-80ea-48d7-9703-b8894b33d511)

Click Add new application

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/edd10cb1-7c25-48af-99c0-1ff6df42bcd6)

Select AWS Single Sing-On, give some name and click on create.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/57a54607-aad0-4b0c-a88f-2af8dd12cbda)

Once the Application is created click on set up single sign on

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/d369f06b-0074-4948-9a6a-53ac09148d09)

Select SAML option:

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/35ccb41e-91d8-4ac7-8f9a-aba8e714cac4)

Uploca the YAML file which we downloaded from AWS Identity Center:

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/1d59cb95-e212-44fd-b0e3-d8151f8961a0)

Once uploaded click on Save.

Now download the Federation Metadata XML and Certificate (Base64) file.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/0f4a4b0f-68d2-464e-aa73-678dbd7b0b66)

Move back to AWS IAM Identity Center --> Upload the above files as IdP SAML metadata and IdP certificate respectively.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/6f05b125-3fc8-438b-af30-324554ab98cd)

Enter ACCEPT and proceed with the change identity source:

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/7b596171-46b6-40b0-943c-388dff3b85a9)

Post This click on enable automatic provision, which means if we create users in Azure AD it will be automatically created inside AWS.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/71578ddc-d27c-4a19-8eb1-473f18b3b6e1)

Copy the URL and Token.

2. Add users to azure AWSSSO application

I have one user created, let's add one new user to Azure AD.

Goto Azure console --> users --> add new user

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/d29b7d22-c13c-4a3b-b185-518d06322f9a)


![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/c781f36c-4151-468f-82f7-b42ce5ba587b)

Click review and create.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/25a784a7-8795-48e5-a18b-611a7ac9402a)

Let's add both users to AWSSSO app in the Azure portal.

Goto Azure Enterprize Application --> click on AWSSSSO app --> click add users

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/6a70d42d-8a7d-48a7-bd8f-d3385431a4f7)

3. Add users to AWS

Now click on provisioning under AWS SSO application. 

**Note : With this step we are enabling automatically provisioning of new users to AWS if they are added to the Azure AD.**

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/82f1e8af-c6b1-454f-932c-83140764b05d)

Select Automatic and enter the URL and Token which we copied in the above step.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/62ec054f-1788-454a-8084-1e38a30f2933)

Click on Test connection.

Click on start provisioning, it will show 2 users created.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/8f4c701a-feeb-4216-ac54-22a2becf47b9)

4. verify users at AWS end

    ![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/870e2959-57d2-4131-a4f0-e8ce7318fe54)


5. Assign Users to AWS accounts

goto AWS IAM Identity Center --> AWS account 
![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/c73edb7c-954e-4896-8915-596e7b158fce)

Click on next, we can either add users or groups. I am adding users

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/c9419285-56f4-4faf-b606-1352a8322f3d)

Assigning admin access, click on next --> submit.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/e23a0ff7-d406-4d98-8bb3-2386498b8257)


6. Test SSO 

Goto IAM Identity Center --> copy aWS access portal URL.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/b296f436-3f32-48da-8f07-551bc1b60f94)

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/27c1279e-59e3-46d8-8c52-b927772d152f)

Enter the username and password for AD user.

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/4136beff-4dee-45d6-8ac4-22581d0a9122)

In Windows system, to configure SSO you have to install windows terminal :

- https://github.com/microsoft/terminal

![image](https://github.com/tushardashpute/sso_eks_authentication/assets/74225291/ff8c911b-6cc0-4566-a4cd-91403bdeddc6)


Reference Link : 
- https://www.youtube.com/watch?v=x7TCs9HxRFg&t=29s
- https://docs.aws.amazon.com/singlesignon/latest/userguide/idp-microsoft-entra.html
