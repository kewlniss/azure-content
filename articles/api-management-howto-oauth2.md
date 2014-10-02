<properties pageTitle="How to authorize developer accounts using OAuth 2.0 in Azure API Management" metaKeywords="" description="Learn how to authorize users using OAuth 2.0 in API Management." metaCanonical="" services="api-management" documentationCenter="API Management" title="How to authorize developer accounts using OAuth 2.0 in Azure API Management" authors="sdanie" solutions="" manager="dwrede" editor="" />

<tags ms.service="api-management" ms.workload="mobile" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="01/01/1900" ms.author="sdanie" />

# How to authorize developer accounts using OAuth 2.0 in Azure API Management

API Management provides the capability for users to access the developer portal using OAuth 2.0 authorization. This guide shows how to configure an API Management service instance to use OAuth 2.0 authorization.

>For more information about OAuth 2.0, see the specification at [http://oauth.net/2/][].

## In this topic

-   [Prerequisites][]
-   [Configure an OAuth 2.0 authorization server in API Management][]
-   [Configure an API to use OAuth 2.0 user authorization][]
-   [Test the OAuth 2.0 user authorization in the Developer Portal][]

## <a name="prerequisites"> </a>Prerequisites

This guide shows you how to configure your API Management service instance to use OAuth 2.0 authorization for developer accounts, but does not show you how to configure an OAuth 2.0 provider. The configuration for each OAuth 2.0 provider is different, although the steps are similar, and the required pieces of information used in configuring OAuth 2.0 in your API Management service instance are the same. This topic shows examples using Azure Active Directory as an OAuth 2.0 provider.

>For more information on configuring OAuth 2.0 using Azure Active Directory, see the [WebApp-GraphAPI-DotNet][] sample.

## <a name="step1"> </a>Configure an OAuth 2.0 authorization server in API Management

To get started, click **Management Console** in the Azure Portal for your API Management service. This takes you to the API Management administrative portal.

![API Management console][api-management-management-console]

>If you have not yet created an API Management service instance, see [Create an API Management service instance][] in the [Get started with Azure API Management][] tutorial.

Click **Security** from the **API Management** menu on the left, click **OAuth 2.0**, and then click **Add authorization server**.

![OAuth 2.0][api-management-oauth2]

After clicking **Add authorization server**, the new authorization server form is displayed.

![New server][api-management-oauth2-server-1]

Enter a name and an optional description in the **Name** and **Description** fields. 

>These fields are used to identify the OAuth 2.0 authorization server within the current API Management service instance and their values do not come from the OAuth 2.0 server.

Enter the **Client registration page URL**. This page is where users can create and manage their accounts, and varies depending on the OAuth 2.0 provider used.

The next section of the form contains the **Authorization code grant types**, **Authorization endpoint URL**, and **Authorization request method** settings.

![New server][api-management-oauth2-server-2]

Specify the **Authorization code grant types** by checking the desired types. **Authorization code** is specified by default.

Enter the **Authorization endpoint URL**. For Azure Active Directory, this URL will be similar to the following URL, where `<client_id>` is replaced with the client id that identifies your application to the OAuth 2.0 server.

    https://login.windows.net/<client_id>/oauth2/authorize

The **Authorization request method** specifies how the authorization request is sent to the OAuth 2.0 server. By default **GET** is selected.

The next section is where the **Token endpoint URL**, **Client authentication methods**, **Access token sending method**, and **Default scope** are specified.

![New server][api-management-oauth2-server-3]

For an Azure Active Directory OAuth 2.0 server, the **Token endpoint URL** will have the following format, where `<APPID>`  has the format of `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

The default setting for **Client authentication methods** is **Basic**, and  **Access token sending method** is **Authorization header**. These values are configured on this section of the form, along with the **Default scope**.

The **Client credentials** section contains the **Client ID** and **Client secret**, which are obtained during the creation and configuration process of your OAuth 2.0 server. Once the **Client ID** and **Client secret** are specified, the **redirect_uri** for the **authorization code** is generated. This URI is used to configure the reply URL in your OAuth 2.0 server configuration.

![New server][api-management-oauth2-server-4]

If **Authorization code grant types** is set to **Resource owner password**, the **Resource owner password credentials** section is used to specify those credentials; otherwise you can leave it blank.

![New server][api-management-oauth2-server-5]

Once the form is complete, click **Save** to save the API Management OAuth 2.0 authorization server configuration. Once the server configuration is saved, you can configure APIs to use this configuration, as shown in the next section.

## <a name="step2"> </a>Configure an API to use OAuth 2.0 user authorization

Click **APIs** from the **API Management** menu on the left, click the name of the desired API, click **Security**, and then check the box for **OAuth 2.0**.

![User authorization][api-management-user-authorization]

Select the desired **Authorization server** from the drop-down list, and click **Save**.

![User authorization][api-management-user-authorization-save]

## <a name="step3"> </a>Test the OAuth 2.0 user authorization in the Developer Portal

Once you have configured your OAuth 2.0 authorization server and configured your API to use that server, you can test it by going to the Developer Portal and calling an API.  Click **Developer portal** in the top right menu.

![Developer portal][api-management-developer-portal-menu]

Click **APIs** in the top menu and select **Echo API**.

![Echo API][api-management-apis-echo-api]

>If you have only one API configured or visible to your account, then clicking APIs takes you directly to the operations for that API.

Select the **GET Resource** operation, click **Open Console**, and then select **Authorization code** from the drop-down.

![Open console][api-management-open-console]

When **Authorization code** is selected, a pop-up window is displayed with the sign-in form of the OAuth 2.0 provider. In this example the sign-in form is provided by Azure Active Directory.

>If you have pop-ups disabled you will be prompted to enable them by the browser. After you enable them, select **Authorization code** again and the sign-in form will be displayed.

![Sign in][api-management-oauth2-signin]

Once you have signed in, the **Request headers** are populated with an `Authorization : Bearer` header that authorizes the request.

![Request header token][api-management-request-header-token]

At this point you can configure the desired values for the remaining parameters, and submit the request. 



[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png




[How to add operations to an API]: ../api-management-howto-add-operations
[How to add and publish a product]: ../api-management-howto-add-products
[Monitoring and analytics]: ../api-management-monitoring
[Add APIs to a product]: ../api-management-howto-add-products/#add-apis
[Publish a product]: ../api-management-howto-add-products/#publish-product
[Get started with Azure API Management]: ../api-management-get-started
[Get started with advanced API configuration]: ../api-management-get-started-advanced
[API Management policy reference]: ../api-management-policy-reference
[Caching policies]: ../api-management-policy-reference/#caching-policies
[Create an API Management service instance]: ../api-management-get-started/#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps
