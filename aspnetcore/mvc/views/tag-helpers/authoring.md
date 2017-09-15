---
title: "Criação de auxiliares de marcação no núcleo do ASP.NET"
author: rick-anderson
description: "Saiba como criar auxiliares de marcação no núcleo do ASP.NET."
keywords: "ASP.NET Core, auxiliares de marcação"
ms.author: riande
manager: wpickett
ms.date: 6/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 97013d06273c0993b74cdacfa16cb0d655c73667
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="db5e8-104">Auxiliares de marcação no núcleo do ASP.NET, um passo a passo com exemplos de criação</span><span class="sxs-lookup"><span data-stu-id="db5e8-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="db5e8-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="db5e8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="db5e8-106">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="db5e8-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="db5e8-107">Guia de Introdução com auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="db5e8-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="db5e8-108">Este tutorial fornece uma introdução à programação auxiliares de marcação.</span><span class="sxs-lookup"><span data-stu-id="db5e8-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="db5e8-109">[Introdução ao auxiliares de marcação](intro.md) descreve os benefícios que fornecem auxiliares de marcação.</span><span class="sxs-lookup"><span data-stu-id="db5e8-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="db5e8-110">Um auxiliar de marca é qualquer classe que implementa o `ITagHelper` interface.</span><span class="sxs-lookup"><span data-stu-id="db5e8-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="db5e8-111">No entanto, quando você cria um auxiliar de marca, você geralmente deriva `TagHelper`, fornece acesso ao `Process` método.</span><span class="sxs-lookup"><span data-stu-id="db5e8-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="db5e8-112">Criar um novo projeto ASP.NET Core chamado **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="db5e8-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="db5e8-113">Você não precisa de autenticação para este projeto.</span><span class="sxs-lookup"><span data-stu-id="db5e8-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="db5e8-114">Criar uma pasta para armazenar os auxiliares de marca chamado *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="db5e8-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="db5e8-115">O *TagHelpers* pasta é *não* necessário, mas é uma convenção razoável.</span><span class="sxs-lookup"><span data-stu-id="db5e8-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="db5e8-116">Agora vamos começar a escrever alguns auxiliares de marca simples.</span><span class="sxs-lookup"><span data-stu-id="db5e8-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="db5e8-117">Um auxiliar de marca mínima</span><span class="sxs-lookup"><span data-stu-id="db5e8-117">A minimal Tag Helper</span></span>

<span data-ttu-id="db5e8-118">Nesta seção, você escreve um auxiliar de marca que atualiza uma marca de email.</span><span class="sxs-lookup"><span data-stu-id="db5e8-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="db5e8-119">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="db5e8-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="db5e8-120">O servidor usará nosso auxiliar de marca de email para converter essa marcação como a seguir:</span><span class="sxs-lookup"><span data-stu-id="db5e8-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="db5e8-121">Ou seja, uma marca de âncora que torna isso um link de email.</span><span class="sxs-lookup"><span data-stu-id="db5e8-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="db5e8-122">Você talvez queira fazer isso se você estiver escrevendo um mecanismo de blog e precisa enviar email de marketing, suporte e outros contatos, todos ao mesmo domínio.</span><span class="sxs-lookup"><span data-stu-id="db5e8-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="db5e8-123">Adicione o seguinte `EmailTagHelper` de classe para o *TagHelpers* pasta.</span><span class="sxs-lookup"><span data-stu-id="db5e8-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="db5e8-124">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-124">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]</span></span>
    
    <span data-ttu-id="db5e8-125">**Observações:**</span><span class="sxs-lookup"><span data-stu-id="db5e8-125">**Notes:**</span></span>
    
    * <span data-ttu-id="db5e8-126">Auxiliares de marcação usam uma convenção de nomenclatura que tem como alvo os elementos do nome da classe raiz (menos o *TagHelper* parte do nome de classe).</span><span class="sxs-lookup"><span data-stu-id="db5e8-126">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="db5e8-127">Neste exemplo, o nome da raiz **Email**TagHelper é *email*, portanto, o `<email>` marca será direcionada.</span><span class="sxs-lookup"><span data-stu-id="db5e8-127">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="db5e8-128">Essa convenção de nomenclatura deve funcionar para a maioria dos auxiliares de marcação, posteriormente, mostrarei como substituí-la.</span><span class="sxs-lookup"><span data-stu-id="db5e8-128">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="db5e8-129">O `EmailTagHelper` classe deriva de `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="db5e8-129">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="db5e8-130">O `TagHelper` classe fornece métodos e propriedades para gravar auxiliares de marcação.</span><span class="sxs-lookup"><span data-stu-id="db5e8-130">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="db5e8-131">Substituído `Process` método controla o que faz o auxiliar de marca quando executado.</span><span class="sxs-lookup"><span data-stu-id="db5e8-131">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="db5e8-132">O `TagHelper` classe também fornece uma versão assíncrona (`ProcessAsync`) com os mesmos parâmetros.</span><span class="sxs-lookup"><span data-stu-id="db5e8-132">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="db5e8-133">O parâmetro de contexto para `Process` (e `ProcessAsync`) contém informações associadas com a execução da marca HTML atual.</span><span class="sxs-lookup"><span data-stu-id="db5e8-133">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="db5e8-134">O parâmetro de saída para `Process` (e `ProcessAsync`) contém um elemento HTML com monitoração de estado representa a fonte original usada para gerar uma marca HTML e o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="db5e8-134">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="db5e8-135">Nome da nossa classe possui um sufixo de **TagHelper**, que é *não* necessário, mas é considerada uma convenção de prática recomendada.</span><span class="sxs-lookup"><span data-stu-id="db5e8-135">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="db5e8-136">Você pode declarar a classe como:</span><span class="sxs-lookup"><span data-stu-id="db5e8-136">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="db5e8-137">Para fazer o `EmailTagHelper` classe disponíveis a todos os nossos modos de exibição do Razor, adicione o `addTagHelper` diretiva para o *Views/_ViewImports.cshtml* arquivo: [!code-html [principal](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-137">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="db5e8-138">O código anterior usa a sintaxe de curinga para especificar que todos os auxiliares de marca no nosso assembly estarão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="db5e8-138">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="db5e8-139">A primeira cadeia de caracteres após `@addTagHelper` Especifica o auxiliar de marca para carregar (Use "*" para todos os auxiliares de marcação), e a segunda cadeia de caracteres "AuthoringTagHelpers" Especifica o assembly de auxiliar de marca está em.</span><span class="sxs-lookup"><span data-stu-id="db5e8-139">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="db5e8-140">Além disso, observe que a segunda linha coloca nos auxiliares de marca do MVC do ASP.NET Core usando a sintaxe de curinga (os auxiliares são discutidos em [Introdução ao auxiliares de marcação](intro.md).) É o `@addTagHelper` diretiva que disponibiliza o auxiliar de marca para o modo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="db5e8-140">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="db5e8-141">Como alternativa, você pode fornecer o nome totalmente qualificado (FQN) de um auxiliar de marca, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="db5e8-141">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
    <span data-ttu-id="db5e8-142">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-142">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]</span></span>
    
    <span data-ttu-id="db5e8-143">Para adicionar um auxiliar de marca para uma exibição usando um FQN, primeiro adicione o FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) e, em seguida, o nome do assembly (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="db5e8-143">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="db5e8-144">A maioria dos desenvolvedores vão preferir usar a sintaxe de curinga.</span><span class="sxs-lookup"><span data-stu-id="db5e8-144">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="db5e8-145">[Introdução ao auxiliares de marcação](intro.md) apresenta detalhes sobre a sintaxe de adição, remoção, hierarquia e curinga do auxiliar de marca.</span><span class="sxs-lookup"><span data-stu-id="db5e8-145">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="db5e8-146">Atualizar a marcação no *Views/Home/Contact.cshtml* arquivos com essas alterações:</span><span class="sxs-lookup"><span data-stu-id="db5e8-146">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    <span data-ttu-id="db5e8-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span></span>

4.  <span data-ttu-id="db5e8-148">Execute o aplicativo e usar seu navegador favorito para exibir o código-fonte HTML para verificar se as marcas de email são substituídas com marcação de âncora (por exemplo, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="db5e8-148">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="db5e8-149">*Suporte* e *Marketing* são renderizados como links, mas não têm um `href` atributo para torná-los funcional.</span><span class="sxs-lookup"><span data-stu-id="db5e8-149">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="db5e8-150">Corrigiremos que na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="db5e8-150">We'll fix that in the next section.</span></span>

<span data-ttu-id="db5e8-151">Observação: Como marcas e atributos, marcas, nomes de classes e atributos HTML no Razor e c# não são diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="db5e8-151">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="db5e8-152">SetAttribute e SetContent</span><span class="sxs-lookup"><span data-stu-id="db5e8-152">SetAttribute and SetContent</span></span>

<span data-ttu-id="db5e8-153">Nesta seção, vamos atualizar o `EmailTagHelper` para que ele cria uma marca de âncora válido para email.</span><span class="sxs-lookup"><span data-stu-id="db5e8-153">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="db5e8-154">Vamos atualizar para obter informações de um modo de exibição do Razor (na forma de um `mail-to` atributo) e usá-lo a gerar a âncora.</span><span class="sxs-lookup"><span data-stu-id="db5e8-154">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="db5e8-155">Atualização de `EmailTagHelper` classe com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="db5e8-155">Update the `EmailTagHelper` class with the following:</span></span>

<span data-ttu-id="db5e8-156">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-156">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]</span></span>

<span data-ttu-id="db5e8-157">**Observações:**</span><span class="sxs-lookup"><span data-stu-id="db5e8-157">**Notes:**</span></span>

* <span data-ttu-id="db5e8-158">Nomes de classe e a propriedade de maiusculas e minúsculas de Pascal para os auxiliares de marca são convertidos em seus [inferior caso kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="db5e8-158">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="db5e8-159">Portanto, para usar o `MailTo` atributo, você usará `<email mail-to="value"/>` equivalente.</span><span class="sxs-lookup"><span data-stu-id="db5e8-159">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="db5e8-160">A última linha define o conteúdo concluído para nosso auxiliar de marca minimamente funcional.</span><span class="sxs-lookup"><span data-stu-id="db5e8-160">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="db5e8-161">A linha realçada mostra a sintaxe para adicionar atributos:</span><span class="sxs-lookup"><span data-stu-id="db5e8-161">The highlighted line shows the syntax for adding attributes:</span></span>

<span data-ttu-id="db5e8-162">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-162">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]</span></span>

<span data-ttu-id="db5e8-163">Essa abordagem funciona para o atributo "href" como no momento, ele não existe na coleção de atributos.</span><span class="sxs-lookup"><span data-stu-id="db5e8-163">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="db5e8-164">Você também pode usar o `output.Attributes.Add` para adicionar um atributo do auxiliar de marca ao final da coleção de atributos de marca.</span><span class="sxs-lookup"><span data-stu-id="db5e8-164">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="db5e8-165">Atualizar a marcação no *Views/Home/Contact.cshtml* arquivos com essas alterações: [!code-html [principal](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-165">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="db5e8-166">Execute o aplicativo e verificar que ele gera os links corretos.</span><span class="sxs-lookup"><span data-stu-id="db5e8-166">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="db5e8-167">Se você escrever o email marca de fechamento automático (`<email mail-to="Rick" />`), a saída final também seriam fechamento automático.</span><span class="sxs-lookup"><span data-stu-id="db5e8-167">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="db5e8-168">Para habilitar a capacidade de gravar a marca com uma marca de início (`<email mail-to="Rick">`) deve decorar a classe com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="db5e8-168">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > <span data-ttu-id="db5e8-169">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-169">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]</span></span>
    
    <span data-ttu-id="db5e8-170">Com um fechamento automático auxiliar de marca de email, a saída será `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="db5e8-170">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="db5e8-171">Marcas de âncora de fechamento automático não são HTML válido, portanto, você não quiser criar um, mas talvez você queira criar um auxiliar de marca é fechamento automático.</span><span class="sxs-lookup"><span data-stu-id="db5e8-171">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="db5e8-172">Auxiliares de marcação definir o tipo do `TagMode` propriedade depois de ler uma marca.</span><span class="sxs-lookup"><span data-stu-id="db5e8-172">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="db5e8-173">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="db5e8-173">ProcessAsync</span></span>

<span data-ttu-id="db5e8-174">Nesta seção, vamos escrever um auxiliar de email assíncrona.</span><span class="sxs-lookup"><span data-stu-id="db5e8-174">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="db5e8-175">Substitua o `EmailTagHelper` classe com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="db5e8-175">Replace the `EmailTagHelper` class with the following code:</span></span>

    <span data-ttu-id="db5e8-176">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-176">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]</span></span>

    <span data-ttu-id="db5e8-177">**Observações:**</span><span class="sxs-lookup"><span data-stu-id="db5e8-177">**Notes:**</span></span>

    * <span data-ttu-id="db5e8-178">Essa versão usa o assíncrona `ProcessAsync` método.</span><span class="sxs-lookup"><span data-stu-id="db5e8-178">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="db5e8-179">O assíncrona `GetChildContentAsync` retorna um `Task` que contém o `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="db5e8-179">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="db5e8-180">Use o `output` para obter o conteúdo do elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="db5e8-180">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="db5e8-181">Faça a seguinte alteração para o *Views/Home/Contact.cshtml* de arquivo para o auxiliar de marca possa obter o email de destino.</span><span class="sxs-lookup"><span data-stu-id="db5e8-181">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    <span data-ttu-id="db5e8-182">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-182">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span></span>

3.  <span data-ttu-id="db5e8-183">Execute o aplicativo e verificar que ele gera links de email válido.</span><span class="sxs-lookup"><span data-stu-id="db5e8-183">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="db5e8-184">RemoveAll, PreContent.SetHtmlContent e PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="db5e8-184">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="db5e8-185">Adicione o seguinte `BoldTagHelper` de classe para o *TagHelpers* pasta.</span><span class="sxs-lookup"><span data-stu-id="db5e8-185">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="db5e8-186">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-186">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]</span></span>

    <span data-ttu-id="db5e8-187">**Observações:**</span><span class="sxs-lookup"><span data-stu-id="db5e8-187">**Notes:**</span></span>
    
    * <span data-ttu-id="db5e8-188">O `[HtmlTargetElement]` atributo passa um parâmetro de atributo que especifica que qualquer elemento HTML que contém um atributo HTML denominado "bold" fará a correspondência, e o `Process` substituição do método na classe será executado.</span><span class="sxs-lookup"><span data-stu-id="db5e8-188">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="db5e8-189">Em nosso exemplo, o `Process` método Remove o atributo "bold" e envolve a marcação que contém com `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="db5e8-189">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="db5e8-190">Porque você não deseja substituir a conteúdo de marca existente, você deve escrever a abertura `<strong>` marca com o `PreContent.SetHtmlContent` método e o fechamento `</strong>` marca com o `PostContent.SetHtmlContent` método.</span><span class="sxs-lookup"><span data-stu-id="db5e8-190">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="db5e8-191">Modificar o *About.cshtml* exibição para conter um `bold` valor do atributo.</span><span class="sxs-lookup"><span data-stu-id="db5e8-191">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="db5e8-192">O código completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="db5e8-192">The completed code is shown below.</span></span>

    <span data-ttu-id="db5e8-193">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-193">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]</span></span>

3.  <span data-ttu-id="db5e8-194">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="db5e8-194">Run the app.</span></span> <span data-ttu-id="db5e8-195">Você pode usar o seu navegador favorito para inspecionar a origem e verifique se a marcação.</span><span class="sxs-lookup"><span data-stu-id="db5e8-195">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="db5e8-196">O `[HtmlTargetElement]` atributo acima se destina somente a marcação HTML que fornece um nome de atributo de "bold".</span><span class="sxs-lookup"><span data-stu-id="db5e8-196">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="db5e8-197">O `<bold>` elemento não foi modificado pelo auxiliar de marca.</span><span class="sxs-lookup"><span data-stu-id="db5e8-197">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="db5e8-198">Comente o `[HtmlTargetElement]` linha de atributo e o padrão serão direcionamento `<bold>` marcas, ou seja, a marcação HTML do formulário `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="db5e8-198">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="db5e8-199">Lembre-se de que a convenção de nomenclatura padrão corresponderá ao nome da classe **negrito**TagHelper para `<bold>` marcas.</span><span class="sxs-lookup"><span data-stu-id="db5e8-199">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="db5e8-200">Execute o aplicativo e verificar se o `<bold>` a marca é processada pelo auxiliar de marca.</span><span class="sxs-lookup"><span data-stu-id="db5e8-200">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="db5e8-201">Decoração de uma classe com vários `[HtmlTargetElement]` resulta em um OR lógico de destinos de atributos.</span><span class="sxs-lookup"><span data-stu-id="db5e8-201">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="db5e8-202">Por exemplo, usando o código a seguir, uma marca em negrito ou um atributo em negrito fará a correspondência.</span><span class="sxs-lookup"><span data-stu-id="db5e8-202">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

<span data-ttu-id="db5e8-203">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-203">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]</span></span>

<span data-ttu-id="db5e8-204">Quando vários atributos são adicionados à mesma instrução, o tempo de execução trata como uma lógica AND.</span><span class="sxs-lookup"><span data-stu-id="db5e8-204">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="db5e8-205">Por exemplo, no código a seguir, um elemento HTML deve ser nomeado "bold" com um atributo chamado "bold" (`<bold bold />`) para fazer a correspondência.</span><span class="sxs-lookup"><span data-stu-id="db5e8-205">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="db5e8-206">Você também pode usar o `[HtmlTargetElement]` para alterar o nome do elemento de destino.</span><span class="sxs-lookup"><span data-stu-id="db5e8-206">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="db5e8-207">Por exemplo, se você quisesse o `BoldTagHelper` destino `<MyBold>` marcas, você usaria o seguinte atributo:</span><span class="sxs-lookup"><span data-stu-id="db5e8-207">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="db5e8-208">Passando um modelo para um auxiliar de marca</span><span class="sxs-lookup"><span data-stu-id="db5e8-208">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="db5e8-209">Adicionar um *modelos* pasta.</span><span class="sxs-lookup"><span data-stu-id="db5e8-209">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="db5e8-210">Adicione o seguinte `WebsiteContext` de classe para o *modelos* pasta:</span><span class="sxs-lookup"><span data-stu-id="db5e8-210">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    <span data-ttu-id="db5e8-211">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-211">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]</span></span>

3.  <span data-ttu-id="db5e8-212">Adicione o seguinte `WebsiteInformationTagHelper` de classe para o *TagHelpers* pasta.</span><span class="sxs-lookup"><span data-stu-id="db5e8-212">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="db5e8-213">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-213">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]</span></span>
    
    <span data-ttu-id="db5e8-214">**Observações:**</span><span class="sxs-lookup"><span data-stu-id="db5e8-214">**Notes:**</span></span>
    
    * <span data-ttu-id="db5e8-215">Conforme mencionado anteriormente, auxiliares de marcação converte maiusculas e minúsculas de Pascal c# nomes de classes e propriedades para os auxiliares de marca em [inferior caso kebab](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="db5e8-215">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="db5e8-216">Portanto, para usar o `WebsiteInformationTagHelper` Razor, você escreverá `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="db5e8-216">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="db5e8-217">Você não explicitamente identificando o elemento de destino com o `[HtmlTargetElement]` de atributo, isso o padrão de `website-information` será direcionada.</span><span class="sxs-lookup"><span data-stu-id="db5e8-217">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="db5e8-218">Se você aplicou o seguinte atributo (Observação não for o caso de kebab, mas corresponde ao nome de classe):</span><span class="sxs-lookup"><span data-stu-id="db5e8-218">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="db5e8-219">A marca de caso kebab inferior `<website-information />` não serão compatíveis.</span><span class="sxs-lookup"><span data-stu-id="db5e8-219">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="db5e8-220">Se você deseja usar o `[HtmlTargetElement]` atributo, você usaria caso kebab conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="db5e8-220">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="db5e8-221">Elementos de fechamento automático não têm nenhum conteúdo.</span><span class="sxs-lookup"><span data-stu-id="db5e8-221">Elements that are self-closing have no content.</span></span> <span data-ttu-id="db5e8-222">Neste exemplo, a marcação Razor usará uma marca de fechamento automático, mas o auxiliar de marca criará um [seção](http://www.w3.org/TR/html5/sections.html#the-section-element) elemento (que não é fechamento automático e você estiver escrevendo o conteúdo dentro do `section` elemento).</span><span class="sxs-lookup"><span data-stu-id="db5e8-222">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="db5e8-223">Portanto, você precisa definir `TagMode` para `StartTagAndEndTag` para gravar a saída.</span><span class="sxs-lookup"><span data-stu-id="db5e8-223">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="db5e8-224">Como alternativa, você pode comentar a linha que define `TagMode` e escrita de marcação com uma marca de fechamento.</span><span class="sxs-lookup"><span data-stu-id="db5e8-224">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="db5e8-225">(Marcação de exemplo é fornecida posteriormente neste tutorial.)</span><span class="sxs-lookup"><span data-stu-id="db5e8-225">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="db5e8-226">O `$` (cifrão) na linha a seguir usa uma [interpolados cadeia de caracteres](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="db5e8-226">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="db5e8-227">Adicione a seguinte marcação para o *About.cshtml* exibição.</span><span class="sxs-lookup"><span data-stu-id="db5e8-227">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="db5e8-228">A marcação realçada exibe as informações do site da web.</span><span class="sxs-lookup"><span data-stu-id="db5e8-228">The highlighted markup displays the web site information.</span></span>
    
    <span data-ttu-id="db5e8-229">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-229">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="db5e8-230">Na marcação Razor mostrada abaixo:</span><span class="sxs-lookup"><span data-stu-id="db5e8-230">In the Razor markup shown below:</span></span>
    >
    ><span data-ttu-id="db5e8-231">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-231">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]</span></span>
    > 
    ><span data-ttu-id="db5e8-232">O Razor Saiba o `info` atributo é uma classe, não uma cadeia de caracteres, e você quiser escrever código c#.</span><span class="sxs-lookup"><span data-stu-id="db5e8-232">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="db5e8-233">Qualquer atributo do auxiliar de marca de cadeia de caracteres não deve ser escrito sem a `@` caracteres.</span><span class="sxs-lookup"><span data-stu-id="db5e8-233">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="db5e8-234">Executar o aplicativo e navegue até o modo de exibição sobre para ver as informações do site da web.</span><span class="sxs-lookup"><span data-stu-id="db5e8-234">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="db5e8-235">Você pode usar a seguinte marcação com uma marca de fechamento e remova a linha com `TagMode.StartTagAndEndTag` no auxiliar de marca:</span><span class="sxs-lookup"><span data-stu-id="db5e8-235">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    ><span data-ttu-id="db5e8-236">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-236">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]</span></span>

## <a name="condition-tag-helper"></a><span data-ttu-id="db5e8-237">Auxiliar de marca de condição</span><span class="sxs-lookup"><span data-stu-id="db5e8-237">Condition Tag Helper</span></span>

<span data-ttu-id="db5e8-238">O auxiliar de marca de condição renderiza a saída quando passado um valor true.</span><span class="sxs-lookup"><span data-stu-id="db5e8-238">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="db5e8-239">Adicione o seguinte `ConditionTagHelper` de classe para o *TagHelpers* pasta.</span><span class="sxs-lookup"><span data-stu-id="db5e8-239">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="db5e8-240">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-240">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]</span></span>

2.  <span data-ttu-id="db5e8-241">Substitua o conteúdo do *Views/Home/Index.cshtml* arquivo com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="db5e8-241">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    <!-- literal_block {"xml:space": "preserve", "source": "mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->
    
    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="db5e8-242">Substitua o `Index` método o `Home` controlador com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="db5e8-242">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    <span data-ttu-id="db5e8-243">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-243">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]</span></span>

4.  <span data-ttu-id="db5e8-244">Executar o aplicativo e navegue até a página inicial.</span><span class="sxs-lookup"><span data-stu-id="db5e8-244">Run the app and browse to the home page.</span></span> <span data-ttu-id="db5e8-245">A marcação na condicional `div` não será renderizado.</span><span class="sxs-lookup"><span data-stu-id="db5e8-245">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="db5e8-246">Acrescente a cadeia de caracteres de consulta `?approved=true` para a URL (por exemplo, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="db5e8-246">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="db5e8-247">`approved`é definido como true e a condicional marcação será exibida.</span><span class="sxs-lookup"><span data-stu-id="db5e8-247">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="db5e8-248">Use o [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operador para especificar o atributo de destino em vez de especificar uma cadeia de caracteres como você fez com o auxiliar de marca em negrito:</span><span class="sxs-lookup"><span data-stu-id="db5e8-248">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
><span data-ttu-id="db5e8-249">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-249">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]</span></span>
>
><span data-ttu-id="db5e8-250">O [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operador protegerá o código deve ele nunca ser refatorado (queremos alterar o nome para `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="db5e8-250">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="db5e8-251">Evitando conflitos de auxiliar de marca</span><span class="sxs-lookup"><span data-stu-id="db5e8-251">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="db5e8-252">Nesta seção, você escreve um par de auxiliares de marcação de vinculação automática.</span><span class="sxs-lookup"><span data-stu-id="db5e8-252">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="db5e8-253">O primeiro substituirá a marcação que contém uma URL iniciada por HTTP para um HTML âncora marca que contém a mesma URL (e, portanto, resultando em um link de URL).</span><span class="sxs-lookup"><span data-stu-id="db5e8-253">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="db5e8-254">O segundo fará o mesmo para uma URL começando com WWW.</span><span class="sxs-lookup"><span data-stu-id="db5e8-254">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="db5e8-255">Como esses dois auxiliares estão intimamente relacionados e pode refatorá-los no futuro, vamos mantê-los no mesmo arquivo.</span><span class="sxs-lookup"><span data-stu-id="db5e8-255">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="db5e8-256">Adicione o seguinte `AutoLinkerHttpTagHelper` de classe para o *TagHelpers* pasta.</span><span class="sxs-lookup"><span data-stu-id="db5e8-256">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="db5e8-257">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-257">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]</span></span>

    >[!NOTE]
    ><span data-ttu-id="db5e8-258">O `AutoLinkerHttpTagHelper` classe destinos `p` elementos e usa [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) para criar a âncora.</span><span class="sxs-lookup"><span data-stu-id="db5e8-258">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="db5e8-259">Adicione a seguinte marcação para o fim do *Views/Home/Contact.cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="db5e8-259">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    <span data-ttu-id="db5e8-260">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-260">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]</span></span>

3.  <span data-ttu-id="db5e8-261">Execute o aplicativo e verificar se o auxiliar de marca renderizado corretamente a âncora.</span><span class="sxs-lookup"><span data-stu-id="db5e8-261">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="db5e8-262">Atualização de `AutoLinker` classe para incluir o `AutoLinkerWwwTagHelper` que converterá www texto em uma marca de âncora que contém o texto original da Web.</span><span class="sxs-lookup"><span data-stu-id="db5e8-262">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="db5e8-263">O código atualizado é realçado abaixo:</span><span class="sxs-lookup"><span data-stu-id="db5e8-263">The updated code is highlighted below:</span></span>

    <span data-ttu-id="db5e8-264">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-264">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]</span></span>

5.  <span data-ttu-id="db5e8-265">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="db5e8-265">Run the app.</span></span> <span data-ttu-id="db5e8-266">Observe o texto www é renderizado como um link, mas o texto HTTP não está.</span><span class="sxs-lookup"><span data-stu-id="db5e8-266">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="db5e8-267">Se você colocar um ponto de interrupção em ambas as classes, você pode ver que a classe do auxiliar de marca HTTP é executado primeira.</span><span class="sxs-lookup"><span data-stu-id="db5e8-267">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="db5e8-268">O problema é que a saída do auxiliar de marca é armazenado em cache e quando o auxiliar de marca da Web é executado, ele substitui a saída do cache do auxiliar de marca HTTP.</span><span class="sxs-lookup"><span data-stu-id="db5e8-268">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="db5e8-269">Posteriormente no tutorial, veremos como controlar a ordem de auxiliares de marcação executados no.</span><span class="sxs-lookup"><span data-stu-id="db5e8-269">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="db5e8-270">Corrigiremos o código com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="db5e8-270">We'll fix the code with the following:</span></span>

    <span data-ttu-id="db5e8-271">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-271">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]</span></span>

    >[!NOTE]
    ><span data-ttu-id="db5e8-272">Na primeira edição os auxiliares de marca vinculação automática, você obteve o conteúdo do destino com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="db5e8-272">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    ><span data-ttu-id="db5e8-273">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-273">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]</span></span>
    >
    ><span data-ttu-id="db5e8-274">Ou seja, você chama `GetChildContentAsync` usando o `TagHelperOutput` passado para o `ProcessAsync` método.</span><span class="sxs-lookup"><span data-stu-id="db5e8-274">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="db5e8-275">Como mencionado anteriormente, porque a saída é armazenada em cache, a última marca auxiliar para executar o wins.</span><span class="sxs-lookup"><span data-stu-id="db5e8-275">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="db5e8-276">Corrigido o problema com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="db5e8-276">You fixed that problem with the following code:</span></span>
    >
    ><span data-ttu-id="db5e8-277">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-277">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]</span></span>
    >
    ><span data-ttu-id="db5e8-278">O código acima verifica para ver se o conteúdo tiver sido modificado e, em caso afirmativo, ele obtém o conteúdo do buffer de saída.</span><span class="sxs-lookup"><span data-stu-id="db5e8-278">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="db5e8-279">Execute o aplicativo e verifique se os dois links funcionam como esperado.</span><span class="sxs-lookup"><span data-stu-id="db5e8-279">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="db5e8-280">Enquanto pode aparecer que nosso auxiliar de marca de vinculador automática está correto e completa, ela tem um problema sutil.</span><span class="sxs-lookup"><span data-stu-id="db5e8-280">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="db5e8-281">Se o auxiliar de marca da Web é executado primeiro, os links da Web não estarão corretos.</span><span class="sxs-lookup"><span data-stu-id="db5e8-281">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="db5e8-282">Atualize o código adicionando o `Order` sobrecarga para controlar a ordem em que a marca é executado no.</span><span class="sxs-lookup"><span data-stu-id="db5e8-282">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="db5e8-283">O `Order` propriedade determina a ordem de execução em relação a outros auxiliares de marcação direcionando o mesmo elemento.</span><span class="sxs-lookup"><span data-stu-id="db5e8-283">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="db5e8-284">O valor de ordem padrão é zero e instâncias com valores mais baixos são executadas primeiro.</span><span class="sxs-lookup"><span data-stu-id="db5e8-284">The default order value is zero and instances with lower values are executed first.</span></span>

    <span data-ttu-id="db5e8-285">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-285">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]</span></span>
    
    <span data-ttu-id="db5e8-286">O código acima, será possível garantir que o auxiliar de marca HTTP é executada antes do auxiliar de marca da Web.</span><span class="sxs-lookup"><span data-stu-id="db5e8-286">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="db5e8-287">Alterar `Order` para `MaxValue` e verifique se a marcação gerada para a marca WWW está incorretova.</span><span class="sxs-lookup"><span data-stu-id="db5e8-287">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="db5e8-288">Inspecionar e recuperando conteúdo filho</span><span class="sxs-lookup"><span data-stu-id="db5e8-288">Inspecting and retrieving child content</span></span>

<span data-ttu-id="db5e8-289">Os auxiliares de marca fornecem várias propriedades para recuperar o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="db5e8-289">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="db5e8-290">O resultado de `GetChildContentAsync` pode ser anexada à `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="db5e8-290">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="db5e8-291">Você pode inspecionar o resultado de `GetChildContentAsync` com `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="db5e8-291">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="db5e8-292">Se você modificar `output.Content`, o corpo da TagHelper não será executado ou renderizado a menos que você chame `GetChildContentAsync` como em nosso exemplo de vinculador automática:</span><span class="sxs-lookup"><span data-stu-id="db5e8-292">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

<span data-ttu-id="db5e8-293">[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]</span><span class="sxs-lookup"><span data-stu-id="db5e8-293">[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]</span></span>

-  <span data-ttu-id="db5e8-294">Diversas chamadas para `GetChildContentAsync` retornará o mesmo valor e não será executado novamente o `TagHelper` corpo, a menos que você passar um parâmetro false indicando que não use o resultado em cache.</span><span class="sxs-lookup"><span data-stu-id="db5e8-294">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>