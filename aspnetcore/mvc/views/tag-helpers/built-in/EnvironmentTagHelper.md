---
title: "Auxiliar de marca de ambiente no núcleo do ASP.NET"
author: pkellner
description: Auxiliar de marca de ambiente do ASP.Net Core definidos incluindo todas as propriedades
keywords: ASP.NET Core, o auxiliar de marca
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper
ms.openlocfilehash: 5870d9ebd02becf29f892c91310022d3b9a6b7af
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="fe270-104">Auxiliar de marca de ambiente no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fe270-104">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="fe270-105">Por [Peter Kellner](http://peterkellner.net) e [Ateya Hisham Bin](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="fe270-105">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="fe270-106">O auxiliar de marca de ambiente condicionalmente renderiza o conteúdo incluído com base no ambiente de hospedagem atual.</span><span class="sxs-lookup"><span data-stu-id="fe270-106">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="fe270-107">O único atributo `names` é uma lista separada por vírgulas de ambiente nomes, que se qualquer correspondência com o ambiente atual, irá disparar o conteúdo entre a ser renderizado.</span><span class="sxs-lookup"><span data-stu-id="fe270-107">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="fe270-108">Atributos de auxiliar de marca de ambiente</span><span class="sxs-lookup"><span data-stu-id="fe270-108">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="fe270-109">nomes</span><span class="sxs-lookup"><span data-stu-id="fe270-109">names</span></span>

<span data-ttu-id="fe270-110">Aceita um único nome de ambiente de hospedagem ou uma lista separada por vírgulas de nomes de ambiente que disparam o processamento do conteúdo do anexo de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="fe270-110">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="fe270-111">Esses valores são comparados com o valor retornado da propriedade estática ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="fe270-111">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="fe270-112">Esse valor é um dos seguintes: **preparo**; **Desenvolvimento** ou **produção**.</span><span class="sxs-lookup"><span data-stu-id="fe270-112">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="fe270-113">A comparação diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="fe270-113">The comparison ignores case.</span></span>

<span data-ttu-id="fe270-114">Um exemplo de uma opção válida `environment` auxiliar de marca é:</span><span class="sxs-lookup"><span data-stu-id="fe270-114">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="fe270-115">incluir e excluir atributos</span><span class="sxs-lookup"><span data-stu-id="fe270-115">include and exclude attributes</span></span>

<span data-ttu-id="fe270-116">ASP.NET Core 2. x adiciona o `include`  &  `exclude` atributos.</span><span class="sxs-lookup"><span data-stu-id="fe270-116">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="fe270-117">Esses atributos controlam a processar o conteúdo incluído com base nos nomes de ambiente hospedagem incluído ou excluído.</span><span class="sxs-lookup"><span data-stu-id="fe270-117">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="fe270-118">incluir Core ASP.NET 2.0 e posterior</span><span class="sxs-lookup"><span data-stu-id="fe270-118">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="fe270-119">O `include` propriedade tem um comportamento semelhante do `names` atributo no ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="fe270-119">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="fe270-120">excluir o núcleo do ASP.NET 2.0 e posterior</span><span class="sxs-lookup"><span data-stu-id="fe270-120">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="fe270-121">Em contraste, o `exclude` permite que a propriedade de `EnvironmentTagHelper` renderizar o conteúdo entre todos os nomes de ambiente de hospedagem exceto a (s) que você especificou.</span><span class="sxs-lookup"><span data-stu-id="fe270-121">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="fe270-122">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="fe270-122">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>