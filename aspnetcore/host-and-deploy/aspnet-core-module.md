---
title: "Referência de configuração de módulo principal do ASP.NET"
author: guardrex
description: "Como configurar o módulo do núcleo do ASP.NET para hospedar aplicativos do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 7bb7e5b9c821f87e73763f5f5c4f9fbcd751235f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="ec630-103">Referência de configuração de módulo principal do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ec630-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="ec630-104">Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="ec630-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="ec630-105">Este documento fornece detalhes sobre como configurar o módulo do núcleo do ASP.NET para hospedar aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ec630-105">This document provides details on how to configure the ASP.NET Core Module for hosting ASP.NET Core applications.</span></span> <span data-ttu-id="ec630-106">Para obter uma introdução ao ASP.NET Core Module e instruções de instalação, consulte o [visão geral do módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="ec630-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-via-webconfig"></a><span data-ttu-id="ec630-107">Configuração por meio do Web. config</span><span class="sxs-lookup"><span data-stu-id="ec630-107">Configuration via web.config</span></span>

<span data-ttu-id="ec630-108">O módulo do ASP.NET Core é configurado por meio de um site ou aplicativo *Web. config* de arquivo e tem seu próprio `aspNetCore` seção de configuração em `system.webServer`.</span><span class="sxs-lookup"><span data-stu-id="ec630-108">The ASP.NET Core Module is configured via a site or application *web.config* file and has its own `aspNetCore` configuration section within `system.webServer`.</span></span> <span data-ttu-id="ec630-109">Aqui está um exemplo *Web. config* de arquivos que o `Microsoft.NET.Sdk.Web` SDK serão fornecidas quando o projeto é publicado para uma [implantação dependente do framework](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) com espaços reservados para o `processPath` e `arguments`:</span><span class="sxs-lookup"><span data-stu-id="ec630-109">Here's an example *web.config* file that the `Microsoft.NET.Sdk.Web` SDK will provide when the project is published for a [framework-dependent deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) with placeholders for the `processPath` and `arguments`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="ec630-110">O *Web. config* exemplo a seguir é para um [implantação autossuficiente](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) para o [do serviço de aplicativo do Azure](https://azure.microsoft.com/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="ec630-110">The *web.config* example below is for a [self-contained deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) to the [Azure App Service](https://azure.microsoft.com/services/app-service/).</span></span> <span data-ttu-id="ec630-111">Para obter mais informações, consulte [Host no Windows com o IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="ec630-111">For more information, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="ec630-112">Consulte [configuração dos aplicativos sub](xref:host-and-deploy/iis/index#configuration-of-sub-applications) para uma nota importante relativas à configuração do *Web. config* arquivos em subdiretórios.</span><span class="sxs-lookup"><span data-stu-id="ec630-112">See [Configuration of sub-applications](xref:host-and-deploy/iis/index#configuration-of-sub-applications) for an important note pertaining to the configuration of *web.config* files in sub-applications.</span></span>

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="ec630-113">Atributos do elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="ec630-113">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="ec630-114">Atributo</span><span class="sxs-lookup"><span data-stu-id="ec630-114">Attribute</span></span> | <span data-ttu-id="ec630-115">Descrição</span><span class="sxs-lookup"><span data-stu-id="ec630-115">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ec630-116">processPath</span><span class="sxs-lookup"><span data-stu-id="ec630-116">processPath</span></span> | <p><span data-ttu-id="ec630-117">Atributo de cadeia de caracteres obrigatória.</span><span class="sxs-lookup"><span data-stu-id="ec630-117">Required string attribute.</span></span></p><p><span data-ttu-id="ec630-118">Caminho para o executável que iniciará um processo que escuta solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="ec630-118">Path to the executable that will launch a process listening for HTTP requests.</span></span> <span data-ttu-id="ec630-119">Há suporte para caminhos relativos.</span><span class="sxs-lookup"><span data-stu-id="ec630-119">Relative paths are supported.</span></span> <span data-ttu-id="ec630-120">Se o caminho começa com '.', o caminho é considerado em relação à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="ec630-120">If the path begins with '.', the path is considered to be relative to the site root.</span></span></p><p><span data-ttu-id="ec630-121">Não há nenhum valor padrão.</span><span class="sxs-lookup"><span data-stu-id="ec630-121">There's no default value.</span></span></p> |
| <span data-ttu-id="ec630-122">arguments</span><span class="sxs-lookup"><span data-stu-id="ec630-122">arguments</span></span> | <p><span data-ttu-id="ec630-123">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="ec630-123">Optional string attribute.</span></span></p><p><span data-ttu-id="ec630-124">Argumentos para o executável especificado em **processPath**.</span><span class="sxs-lookup"><span data-stu-id="ec630-124">Arguments to the executable specified in **processPath**.</span></span></p><p><span data-ttu-id="ec630-125">O valor padrão é uma cadeia de caracteres vazia.</span><span class="sxs-lookup"><span data-stu-id="ec630-125">The default value is an empty string.</span></span></p> |
| <span data-ttu-id="ec630-126">startupTimeLimit</span><span class="sxs-lookup"><span data-stu-id="ec630-126">startupTimeLimit</span></span> | <p><span data-ttu-id="ec630-127">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="ec630-127">Optional integer attribute.</span></span></p><p><span data-ttu-id="ec630-128">Duração em segundos que o módulo irá aguardar o executável iniciar um processo que escuta na porta.</span><span class="sxs-lookup"><span data-stu-id="ec630-128">Duration in seconds that the module will wait for the executable to start a process listening on the port.</span></span> <span data-ttu-id="ec630-129">Se esse tempo limite for excedido, o módulo irá interromper o processo.</span><span class="sxs-lookup"><span data-stu-id="ec630-129">If this time limit is exceeded, the module will kill the process.</span></span> <span data-ttu-id="ec630-130">O módulo tentará iniciar o processo novamente quando ele recebe uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo falhar ao iniciar **rapidFailsPerMinute** número vezes no último minuto sem interrupção.</span><span class="sxs-lookup"><span data-stu-id="ec630-130">The module will attempt to launch the process again when it receives a new request and will continue to attempt to restart the process on subsequent incoming requests unless the application fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="ec630-131">O valor padrão é 120.</span><span class="sxs-lookup"><span data-stu-id="ec630-131">The default value is 120.</span></span></p> |
| <span data-ttu-id="ec630-132">shutdownTimeLimit</span><span class="sxs-lookup"><span data-stu-id="ec630-132">shutdownTimeLimit</span></span> | <p><span data-ttu-id="ec630-133">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="ec630-133">Optional integer attribute.</span></span></p><p><span data-ttu-id="ec630-134">Duração em segundos para o qual o módulo irá aguardar o executável para desligar normalmente quando o *app_offline.htm* arquivo é detectado.</span><span class="sxs-lookup"><span data-stu-id="ec630-134">Duration in seconds for which the module will wait for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p><p><span data-ttu-id="ec630-135">O valor padrão é 10.</span><span class="sxs-lookup"><span data-stu-id="ec630-135">The default value is 10.</span></span></p> |
| <span data-ttu-id="ec630-136">rapidFailsPerMinute</span><span class="sxs-lookup"><span data-stu-id="ec630-136">rapidFailsPerMinute</span></span> | <p><span data-ttu-id="ec630-137">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="ec630-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="ec630-138">Especifica o número de vezes que o processo especificado na **processPath** pode falhar por minuto.</span><span class="sxs-lookup"><span data-stu-id="ec630-138">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="ec630-139">Se esse limite for excedido, o módulo interromperá a inicialização do processo para o restante do minuto.</span><span class="sxs-lookup"><span data-stu-id="ec630-139">If this limit is exceeded, the module will stop launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="ec630-140">O valor padrão é 10.</span><span class="sxs-lookup"><span data-stu-id="ec630-140">The default value is 10.</span></span></p> |
| <span data-ttu-id="ec630-141">requestTimeout</span><span class="sxs-lookup"><span data-stu-id="ec630-141">requestTimeout</span></span> | <p><span data-ttu-id="ec630-142">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="ec630-142">Optional timespan attribute.</span></span></p><p><span data-ttu-id="ec630-143">Especifica a duração para a qual o módulo do ASP.NET Core aguardará por uma resposta do processo que escuta em % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="ec630-143">Specifies the duration for which the ASP.NET Core Module will wait for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="ec630-144">O valor padrão é "00: 02:00".</span><span class="sxs-lookup"><span data-stu-id="ec630-144">The default value is "00:02:00".</span></span></p><p><span data-ttu-id="ec630-145">O `requestTimeout` deve ser especificado em minutos inteiros, caso contrário, o padrão é 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="ec630-145">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> |
| <span data-ttu-id="ec630-146">stdoutLogEnabled</span><span class="sxs-lookup"><span data-stu-id="ec630-146">stdoutLogEnabled</span></span> | <p><span data-ttu-id="ec630-147">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="ec630-147">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="ec630-148">Se true, **stdout** e **stderr** para o processo especificado na **processPath** será redirecionado para o arquivo especificado em **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="ec630-148">If true, **stdout** and **stderr** for the process specified in **processPath** will be redirected to the file specified in **stdoutLogFile**.</span></span></p><p><span data-ttu-id="ec630-149">O valor padrão é false.</span><span class="sxs-lookup"><span data-stu-id="ec630-149">The default value is false.</span></span></p> |
| <span data-ttu-id="ec630-150">stdoutLogFile</span><span class="sxs-lookup"><span data-stu-id="ec630-150">stdoutLogFile</span></span> | <p><span data-ttu-id="ec630-151">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="ec630-151">Optional string attribute.</span></span></p><p><span data-ttu-id="ec630-152">Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado na **processPath** será registrado.</span><span class="sxs-lookup"><span data-stu-id="ec630-152">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** will be logged.</span></span> <span data-ttu-id="ec630-153">Caminhos relativos são relativo à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="ec630-153">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="ec630-154">Qualquer caminho começando com '.' será relativo à raiz do site e todos os outros caminhos serão tratados como caminhos absolutos.</span><span class="sxs-lookup"><span data-stu-id="ec630-154">Any path starting with '.' will be relative to the site root and all other paths will be treated as absolute paths.</span></span> <span data-ttu-id="ec630-155">As pastas fornecidas no caminho devem existir para que o módulo criar o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="ec630-155">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="ec630-156">A ID de processo, o carimbo de hora (*yyyyMdhms*) e a extensão de arquivo (*. log*) com sublinhado delimitadores são adicionados para o último segmento do **stdoutLogFile** fornecido.</span><span class="sxs-lookup"><span data-stu-id="ec630-156">The process ID, timestamp (*yyyyMdhms*), and file extension (*.log*) with underscore delimiters are added to the last segment of the **stdoutLogFile** provided.</span></span></p><p><span data-ttu-id="ec630-157">O valor padrão é `aspnetcore-stdout`.</span><span class="sxs-lookup"><span data-stu-id="ec630-157">The default value is `aspnetcore-stdout`.</span></span></p> |
| <span data-ttu-id="ec630-158">forwardWindowsAuthToken</span><span class="sxs-lookup"><span data-stu-id="ec630-158">forwardWindowsAuthToken</span></span> | <span data-ttu-id="ec630-159">verdadeiro ou falso.</span><span class="sxs-lookup"><span data-stu-id="ec630-159">true or false.</span></span></p><p><span data-ttu-id="ec630-160">Se for true, o token será encaminhado para o processo filho escuta em % ASPNETCORE_PORT % como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação.</span><span class="sxs-lookup"><span data-stu-id="ec630-160">If true, the token will be forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="ec630-161">É responsabilidade do processo para chamar CloseHandle neste token por solicitação.</span><span class="sxs-lookup"><span data-stu-id="ec630-161">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p><p><span data-ttu-id="ec630-162">O valor padrão é true.</span><span class="sxs-lookup"><span data-stu-id="ec630-162">The default value is true.</span></span></p> |
| <span data-ttu-id="ec630-163">disableStartUpErrorPage</span><span class="sxs-lookup"><span data-stu-id="ec630-163">disableStartUpErrorPage</span></span> | <span data-ttu-id="ec630-164">verdadeiro ou falso.</span><span class="sxs-lookup"><span data-stu-id="ec630-164">true or false.</span></span></p><p><span data-ttu-id="ec630-165">Se for true, o **502.5 - falha do processo** página será eliminada, e a página de código de 502 status configuradas no seu *Web. config* terá precedência.</span><span class="sxs-lookup"><span data-stu-id="ec630-165">If true, the **502.5 - Process Failure** page will be suppressed, and the 502 status code page configured in your *web.config* will take precedence.</span></span></p><p><span data-ttu-id="ec630-166">O valor padrão é false.</span><span class="sxs-lookup"><span data-stu-id="ec630-166">The default value is false.</span></span></p> |

### <a name="setting-environment-variables"></a><span data-ttu-id="ec630-167">Definir variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="ec630-167">Setting environment variables</span></span>

<span data-ttu-id="ec630-168">O módulo de núcleo do ASP.NET permite que você especificar variáveis de ambiente para o processo especificado no `processPath` atributo especificando-os em uma ou mais `environmentVariable` elementos filho de um `environmentVariables` elemento de coleção no `aspNetCore` elemento.</span><span class="sxs-lookup"><span data-stu-id="ec630-168">The ASP.NET Core Module allows you specify environment variables for the process specified in the `processPath` attribute by specifying them in one or more `environmentVariable` child elements of an `environmentVariables` collection element under the `aspNetCore` element.</span></span> <span data-ttu-id="ec630-169">Variáveis de ambiente definidas nesta seção têm precedência sobre o sistema, variáveis de ambiente para o processo.</span><span class="sxs-lookup"><span data-stu-id="ec630-169">Environment variables set in this section take precedence over system environment variables for the process.</span></span>

<span data-ttu-id="ec630-170">O exemplo a seguir define duas variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="ec630-170">The example below sets two environment variables.</span></span> <span data-ttu-id="ec630-171">`ASPNETCORE_ENVIRONMENT`Configurar o ambiente do aplicativo para `Development`.</span><span class="sxs-lookup"><span data-stu-id="ec630-171">`ASPNETCORE_ENVIRONMENT` will configure the application's environment to `Development`.</span></span> <span data-ttu-id="ec630-172">Um desenvolvedor pode definir esse valor temporariamente *Web. config* arquivo para forçar o [página de exceção de desenvolvedor](xref:fundamentals/error-handling) carregar ao depurar uma exceção de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ec630-172">A developer may temporarily set this value in the *web.config* file in order to force the [developer exception page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="ec630-173">`CONFIG_DIR`é um exemplo de uma variável de ambiente definidas pelo usuário, em que o desenvolvedor tenha gravado código que lê o valor na inicialização para formar um caminho para carregar o arquivo de configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ec630-173">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that will read the value on startup to form a path in order to load the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

## <a name="appofflinehtm"></a><span data-ttu-id="ec630-174">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="ec630-174">app_offline.htm</span></span>

<span data-ttu-id="ec630-175">Se você colocar um arquivo com o nome *app_offline.htm* na raiz de um diretório de aplicativo web, o módulo do ASP.NET Core tentar desligar normalmente o aplicativo e parar o processamento de solicitações de entrada.</span><span class="sxs-lookup"><span data-stu-id="ec630-175">If you place a file with the name *app_offline.htm* at the root of a web application directory, the ASP.NET Core Module will attempt to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="ec630-176">Se o aplicativo ainda está em execução `shutdownTimeLimit` número de segundos, o módulo do ASP.NET Core finalizará o processo em execução.</span><span class="sxs-lookup"><span data-stu-id="ec630-176">If the app is still running after `shutdownTimeLimit` number of seconds, the ASP.NET Core Module will kill the running process.</span></span>

<span data-ttu-id="ec630-177">Enquanto o *app_offline.htm* arquivo estiver presente, o módulo do ASP.NET Core responderá a solicitações enviando o conteúdo de *app_offline.htm* arquivo.</span><span class="sxs-lookup"><span data-stu-id="ec630-177">While the *app_offline.htm* file is present, the ASP.NET Core Module will respond to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="ec630-178">Uma vez o *app_offline.htm* arquivo é removido, a próxima solicitação carrega o aplicativo, que responde às solicitações.</span><span class="sxs-lookup"><span data-stu-id="ec630-178">Once the *app_offline.htm* file is removed, the next request loads the application, which then responds to requests.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="ec630-179">Página de erro de inicialização</span><span class="sxs-lookup"><span data-stu-id="ec630-179">Start-up error page</span></span>

<span data-ttu-id="ec630-180">Se o módulo do ASP.NET Core falhar ao iniciar o processo de back-end ou o início do processo de back-end, mas não escutar na porta configurada, você verá uma página de código de status HTTP 502.5.</span><span class="sxs-lookup"><span data-stu-id="ec630-180">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, you will see an HTTP 502.5 status code page.</span></span> <span data-ttu-id="ec630-181">Para omitir esta página e reverter para a página de código de status de IIS 502 padrão, use o `disableStartUpErrorPage` atributo.</span><span class="sxs-lookup"><span data-stu-id="ec630-181">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="ec630-182">Para obter mais informações sobre como configurar mensagens de erro personalizadas, consulte [erros HTTP `<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="ec630-182">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).</span></span>

![Página de status 502](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="ec630-184">Criação de log e de redirecionamento</span><span class="sxs-lookup"><span data-stu-id="ec630-184">Log creation and redirection</span></span>

<span data-ttu-id="ec630-185">O módulo do ASP.NET Core redireciona `stdout` e `stderr` logs no disco se você definir o `stdoutLogEnabled` e `stdoutLogFile` atributos do `aspNetCore` elemento.</span><span class="sxs-lookup"><span data-stu-id="ec630-185">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if you set the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element.</span></span> <span data-ttu-id="ec630-186">As pastas no `stdoutLogFile` caminho deve existir para que o módulo criar o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="ec630-186">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="ec630-187">Uma extensão de arquivo e o carimbo de hora será adicionada automaticamente quando o arquivo de log é criado.</span><span class="sxs-lookup"><span data-stu-id="ec630-187">A timestamp and file extension will be added automatically when the log file is created.</span></span> <span data-ttu-id="ec630-188">Logs não são girados, a menos que ocorra a reciclagem de processo/reinício.</span><span class="sxs-lookup"><span data-stu-id="ec630-188">Logs are not rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="ec630-189">É responsabilidade do hoster para limitar o espaço em disco que consomem os logs.</span><span class="sxs-lookup"><span data-stu-id="ec630-189">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="ec630-190">Usando o `stdout` log só é recomendado para solução de problemas de inicialização do aplicativo e não gerais do aplicativo para fins de registro.</span><span class="sxs-lookup"><span data-stu-id="ec630-190">Using the `stdout` log is only recommended for troubleshooting application startup issues and not for general application logging purposes.</span></span>

<span data-ttu-id="ec630-191">O nome do arquivo de log é composto, acrescentando o ID de processo (PID), timestamp (*yyyyMdhms*) e a extensão de arquivo (*. log*) para o último segmento do `stdoutLogFile` caminho (normalmente *stdout* ) delimitados por sublinhados.</span><span class="sxs-lookup"><span data-stu-id="ec630-191">The log file name is composed by appending the process ID (PID), timestamp (*yyyyMdhms*), and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="ec630-192">Por exemplo se o `stdoutLogFile` caminho termina com *stdout*, um log para um aplicativo com uma identificação de 10652 criada em 10/8/2017 às 12:05:02 tem o nome de arquivo *stdout_10652_20178101252.log*.</span><span class="sxs-lookup"><span data-stu-id="ec630-192">For example if the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 10652 created on 8/10/2017 at 12:05:02 has the file name *stdout_10652_20178101252.log*.</span></span>

<span data-ttu-id="ec630-193">Aqui está um exemplo `aspNetCore` elemento configura `stdout` log.</span><span class="sxs-lookup"><span data-stu-id="ec630-193">Here's a sample `aspNetCore` element that configures `stdout` logging.</span></span> <span data-ttu-id="ec630-194">O `stdoutLogFile` mostrado no exemplo de caminho é apropriado para o serviço de aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="ec630-194">The `stdoutLogFile` path shown in the example is appropriate for the Azure App Service.</span></span> <span data-ttu-id="ec630-195">Um caminho local ou um caminho de compartilhamento de rede é aceitável para o registro local.</span><span class="sxs-lookup"><span data-stu-id="ec630-195">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="ec630-196">Confirme se a identidade do usuário AppPool tem permissão para gravar o caminho fornecido.</span><span class="sxs-lookup"><span data-stu-id="ec630-196">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```
<span data-ttu-id="ec630-197">Consulte [configuração por meio do Web. config](#configuration-via-webconfig) para obter um exemplo de `aspNetCore` elemento no *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="ec630-197">See [Configuration via web.config](#configuration-via-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="ec630-198">Configuração do módulo principal do ASP.NET com um IIS compartilhada</span><span class="sxs-lookup"><span data-stu-id="ec630-198">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="ec630-199">O instalador do módulo de núcleo do ASP.NET é executado com os privilégios do **sistema** conta.</span><span class="sxs-lookup"><span data-stu-id="ec630-199">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="ec630-200">Como a conta sistema local não ter modificar permissões para o caminho de compartilhamento que é usado pela configuração compartilhada de IIS, o instalador atingirá um erro de acesso negado ao tentar definir as configurações de módulo em  *applicationHost. config* no compartilhamento.</span><span class="sxs-lookup"><span data-stu-id="ec630-200">Because the local system account doesn't have modify permission for the share path which is used by the IIS Shared Configuration, the installer will hit an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span>

<span data-ttu-id="ec630-201">A solução sem suporte é desabilitar a configuração compartilhada de IIS, execute o instalador, exportar atualizado *applicationHost. config* para o compartilhamento de arquivos e habilitar novamente a configuração compartilhada de IIS.</span><span class="sxs-lookup"><span data-stu-id="ec630-201">The unsupported workaround is to disable the IIS Shared Configuration, run the installer, export the updated *applicationHost.config* file to the share, and re-enable the IIS Shared Configuration.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="ec630-202">Locais de arquivo do módulo, o esquema e a configuração</span><span class="sxs-lookup"><span data-stu-id="ec630-202">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="ec630-203">Módulo</span><span class="sxs-lookup"><span data-stu-id="ec630-203">Module</span></span>

<span data-ttu-id="ec630-204">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="ec630-204">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="ec630-205">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="ec630-205">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="ec630-206">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="ec630-206">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="ec630-207">**O IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="ec630-207">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="ec630-208">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="ec630-208">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="ec630-209">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="ec630-209">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="ec630-210">Esquema</span><span class="sxs-lookup"><span data-stu-id="ec630-210">Schema</span></span>

<span data-ttu-id="ec630-211">**IIS**</span><span class="sxs-lookup"><span data-stu-id="ec630-211">**IIS**</span></span>

   * <span data-ttu-id="ec630-212">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="ec630-212">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="ec630-213">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="ec630-213">**IIS Express**</span></span>

   * <span data-ttu-id="ec630-214">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="ec630-214">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="ec630-215">Configuração</span><span class="sxs-lookup"><span data-stu-id="ec630-215">Configuration</span></span>

<span data-ttu-id="ec630-216">**IIS**</span><span class="sxs-lookup"><span data-stu-id="ec630-216">**IIS**</span></span>

   * <span data-ttu-id="ec630-217">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="ec630-217">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="ec630-218">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="ec630-218">**IIS Express**</span></span>

   * <span data-ttu-id="ec630-219">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="ec630-219">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="ec630-220">Você pode procurar *aspnetcore.dll* no *applicationHost. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="ec630-220">You can search for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="ec630-221">Para o IIS Express, o *applicationHost. config* arquivo não existe por padrão.</span><span class="sxs-lookup"><span data-stu-id="ec630-221">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="ec630-222">O arquivo é criado no *{raiz do aplicativo}\.vs\config* quando você iniciar qualquer projeto de aplicativo web na solução do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec630-222">The file is created at *{application root}\.vs\config* when you start any web application project in the Visual Studio solution.</span></span>