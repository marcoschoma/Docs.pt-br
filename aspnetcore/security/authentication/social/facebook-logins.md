---
title: "Configuração de logon externo do Facebook no núcleo do ASP.NET"
author: rick-anderson
description: "Configuração de logon externo do Facebook no núcleo do ASP.NET"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 8/1/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: da019ad3fd6cefa23b8331c98cc36e50ac9c1105
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="configuring-facebook-authentication"></a><span data-ttu-id="e3349-104">Configurar a autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="e3349-104">Configuring Facebook authentication</span></span>

<a name=security-authentication-facebook-logins></a>

<span data-ttu-id="e3349-105">Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e3349-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e3349-106">Este tutorial mostra como habilitar os usuários entrar com sua conta do Facebook usando um projeto do ASP.NET Core 2.0 de exemplo criado no [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="e3349-106">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="e3349-107">Vamos começar criando um Facebook App ID seguindo o [etapas oficiais](https://www.facebook.com/unsupportedbrowser).</span><span class="sxs-lookup"><span data-stu-id="e3349-107">We start by creating a Facebook App ID by following the [official steps](https://www.facebook.com/unsupportedbrowser).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="e3349-108">Criar o aplicativo no Facebook</span><span class="sxs-lookup"><span data-stu-id="e3349-108">Create the app in Facebook</span></span>

*  <span data-ttu-id="e3349-109">Navegue até o [Facebook para desenvolvedores](https://www.facebook.com/unsupportedbrowser) página e entre.</span><span class="sxs-lookup"><span data-stu-id="e3349-109">Navigate to the [Facebook for Developers](https://www.facebook.com/unsupportedbrowser) page and sign in.</span></span> <span data-ttu-id="e3349-110">Se você ainda não tiver uma conta do Facebook, use o **inscrever-se para o Facebook** link na página de logon para criar uma.</span><span class="sxs-lookup"><span data-stu-id="e3349-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="e3349-111">Toque na **criar aplicativo** botão no canto superior direito para criar uma nova ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e3349-111">Tap the **Create App** button in the upper right corner to create a new App ID.</span></span>

   ![Abra o Facebook para o portal de desenvolvedores no Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="e3349-113">Preencha o formulário e toque no **criar ID do aplicativo** botão.</span><span class="sxs-lookup"><span data-stu-id="e3349-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Criar um formulário de nova ID de aplicativo](index/_static/FBNewAppId.png)

* <span data-ttu-id="e3349-115">Quando for apresentado **selecionar um produto** prompt, clique em **Set Up** no **logon do Facebook** cartão.</span><span class="sxs-lookup"><span data-stu-id="e3349-115">When presented with **Select a product** prompt, Click **Set Up** on the **Facebook Login** card.</span></span>

   ![Página de instalação do produto](index/_static/FBProductSetup.png)

* <span data-ttu-id="e3349-117">O **Quickstart** assistente iniciará com **escolher uma plataforma** como a primeira página.</span><span class="sxs-lookup"><span data-stu-id="e3349-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="e3349-118">Ignorar o assistente agora clicando o **configurações** link no menu à esquerda:</span><span class="sxs-lookup"><span data-stu-id="e3349-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Início rápido do Skip](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="e3349-120">Você verá o **configurações do cliente OAuth** página:</span><span class="sxs-lookup"><span data-stu-id="e3349-120">You are presented with the **Client OAuth Settings** page:</span></span>

![Página de configurações de OAuth do cliente](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="e3349-122">Insira o URI de desenvolvimento com */signin-facebook* acrescentados no **válido URIs de redirecionamento OAuth** campo (por exemplo: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="e3349-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="e3349-123">A autenticação do Facebook configurada mais tarde neste tutorial automaticamente manipulará as solicitações no */signin-facebook* rota para implementar o fluxo do OAuth.</span><span class="sxs-lookup"><span data-stu-id="e3349-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="e3349-124">Clique em **salvar alterações**.</span><span class="sxs-lookup"><span data-stu-id="e3349-124">Click **Save Changes**.</span></span>

* <span data-ttu-id="e3349-125">Clique o **painel** link no painel de navegação esquerdo.</span><span class="sxs-lookup"><span data-stu-id="e3349-125">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="e3349-126">Nessa página, anote o `App ID` e `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="e3349-126">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="e3349-127">Você adicionará ambos em seu aplicativo ASP.NET Core na próxima seção:</span><span class="sxs-lookup"><span data-stu-id="e3349-127">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Painel do desenvolvedor do Facebook](index/_static/FBDashboard.png)

* <span data-ttu-id="e3349-129">Ao implantar o site que você precise revisá o **logon do Facebook** página de instalação e registrar um novo URI público.</span><span class="sxs-lookup"><span data-stu-id="e3349-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="e3349-130">Armazenar a ID do aplicativo Facebook e o segredo do aplicativo</span><span class="sxs-lookup"><span data-stu-id="e3349-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="e3349-131">Vincular as configurações confidenciais como Facebook `App ID` e `App Secret` para sua configuração de aplicativo usando o [Manager segredo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="e3349-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="e3349-132">Para os fins deste tutorial, nomeie os tokens `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="e3349-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

## <a name="configure-facebook-authentication"></a><span data-ttu-id="e3349-133">Configurar a autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="e3349-133">Configure Facebook Authentication</span></span>

<span data-ttu-id="e3349-134">O modelo de projeto usado neste tutorial garante que [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacote já está instalado.</span><span class="sxs-lookup"><span data-stu-id="e3349-134">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package is already installed.</span></span>

* <span data-ttu-id="e3349-135">Para instalar este pacote com 2017 do Visual Studio, clique com botão direito no projeto e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e3349-135">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="e3349-136">Para instalar o .NET Core CLI, execute o seguinte no diretório do projeto:</span><span class="sxs-lookup"><span data-stu-id="e3349-136">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e3349-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e3349-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e3349-138">Adicione o serviço do Facebook no `ConfigureServices` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="e3349-138">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

<span data-ttu-id="e3349-139">O `AddAuthentication` método só deve ser chamado uma vez ao adicionar vários provedores de autenticação.</span><span class="sxs-lookup"><span data-stu-id="e3349-139">The `AddAuthentication` method should only be called once when adding multiple authentication providers.</span></span> <span data-ttu-id="e3349-140">Chamadas subsequentes para que ele tem o potencial de substituição qualquer configurado anteriormente [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) propriedades.</span><span class="sxs-lookup"><span data-stu-id="e3349-140">Subsequent calls to it have the potential of overriding any previously configured [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) properties.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e3349-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e3349-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e3349-142">Adicionar o middleware do Facebook no `Configure` método *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="e3349-142">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="e3349-143">Consulte o [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) referência de API para obter mais informações sobre opções de configuração com suporte a autenticação do Facebook.</span><span class="sxs-lookup"><span data-stu-id="e3349-143">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="e3349-144">Opções de configuração podem ser usadas para:</span><span class="sxs-lookup"><span data-stu-id="e3349-144">Configuration options can be used to:</span></span>

* <span data-ttu-id="e3349-145">Solicite informações diferentes sobre o usuário.</span><span class="sxs-lookup"><span data-stu-id="e3349-145">Request different information about the user.</span></span>
* <span data-ttu-id="e3349-146">Adicione argumentos de cadeia de caracteres de consulta para personalizar a experiência de logon.</span><span class="sxs-lookup"><span data-stu-id="e3349-146">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="e3349-147">Entrar com o Facebook</span><span class="sxs-lookup"><span data-stu-id="e3349-147">Sign in with Facebook</span></span>

<span data-ttu-id="e3349-148">Execute o aplicativo e clique em **login**.</span><span class="sxs-lookup"><span data-stu-id="e3349-148">Run your application and click **Log in**.</span></span> <span data-ttu-id="e3349-149">Você verá uma opção para entrar com o Facebook.</span><span class="sxs-lookup"><span data-stu-id="e3349-149">You see an option to sign in with Facebook.</span></span>

![Aplicativo Web: usuário não autenticado](index/_static/DoneFacebook.png)

<span data-ttu-id="e3349-151">Quando você clica na **Facebook**, você será redirecionado para o Facebook para autenticação:</span><span class="sxs-lookup"><span data-stu-id="e3349-151">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Página de autenticação do Facebook](index/_static/FBLogin.png)

<span data-ttu-id="e3349-153">Endereço de email e o perfil público de solicitações de autenticação do Facebook por padrão:</span><span class="sxs-lookup"><span data-stu-id="e3349-153">Facebook authentication requests public profile and email address by default:</span></span>

![Página de autenticação do Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="e3349-155">Depois que você insira suas credenciais de Facebook, que você será redirecionado para o site onde você pode definir seu email.</span><span class="sxs-lookup"><span data-stu-id="e3349-155">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="e3349-156">Agora você está conectado usando suas credenciais do Facebook:</span><span class="sxs-lookup"><span data-stu-id="e3349-156">You are now logged in using your Facebook credentials:</span></span>

![Aplicativo Web: usuário autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="e3349-158">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="e3349-158">Troubleshooting</span></span>

* <span data-ttu-id="e3349-159">**ASP.NET Core 2. x somente:** identidade se não está configurada por meio da chamada `services.AddIdentity` na `ConfigureServices`, tentar autenticar resultará em *ArgumentException: A opção 'SignInScheme' deve ser fornecida*.</span><span class="sxs-lookup"><span data-stu-id="e3349-159">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="e3349-160">O modelo de projeto usado neste tutorial garante que isso é feito.</span><span class="sxs-lookup"><span data-stu-id="e3349-160">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="e3349-161">Se o banco de dados do site não tiver sido criado, aplicando a migração inicial, você obtém *uma operação de banco de dados falhou ao processar a solicitação* erro.</span><span class="sxs-lookup"><span data-stu-id="e3349-161">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="e3349-162">Toque em **aplicar migrações** para criar o banco de dados e a atualização para continuar após o erro.</span><span class="sxs-lookup"><span data-stu-id="e3349-162">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3349-163">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="e3349-163">Next steps</span></span>

* <span data-ttu-id="e3349-164">Este artigo mostrou como você pode autenticar com o Facebook.</span><span class="sxs-lookup"><span data-stu-id="e3349-164">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="e3349-165">Você pode seguir uma abordagem semelhante para autenticar com outros provedores listados no [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="e3349-165">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="e3349-166">Depois de publicar seu site da web para o aplicativo web do Azure, você deve redefinir o `AppSecret` no portal do desenvolvedor do Facebook.</span><span class="sxs-lookup"><span data-stu-id="e3349-166">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="e3349-167">Definir o `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` como configurações de aplicativo no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="e3349-167">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="e3349-168">O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="e3349-168">The configuration system is set up to read keys from environment variables.</span></span>