---
title: "Конфигурация клиента"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 5da5bd3b-65d9-43b7-91b9-cc9e989b1350
caps.latest.revision: "8"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: 75b594d01c8a9297f3383c2648b3853c2c024b9b
ms.sourcegitcommit: c0dd436f6f8f44dc80dc43b07f6841a00b74b23f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="client-configuration"></a>Конфигурация клиента
Конфигурацию клиента [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] можно использовать для задания адреса, привязки, поведения и контракта - основополагающих свойств конечной точки клиента, которая используется клиентом для подключения к конечным точкам службы. [ \<Клиента >](../../../../docs/framework/configure-apps/file-schema/wcf/client.md) элемент имеет [ \<endpoint >](http://msdn.microsoft.com/library/13aa23b7-2f08-4add-8dbf-a99f8127c017) элемент, атрибуты которого используются для настройки основополагающих свойств конечной точки. Эти атрибуты обсуждаются в подразделе "Настройка конечных точек" данного раздела.  
  
 [ \<Endpoint >](http://msdn.microsoft.com/library/13aa23b7-2f08-4add-8dbf-a99f8127c017) элемент также содержит [ \<метаданных >](../../../../docs/framework/configure-apps/file-schema/wcf/metadata.md) элемент, используемый для задания параметров импорта и экспорта метаданных, [ \<заголовки >](../../../../docs/framework/configure-apps/file-schema/wcf/headers.md) элемент, содержащий коллекцию заголовков настраиваемый адрес, и [ \<удостоверение >](../../../../docs/framework/configure-apps/file-schema/wcf/identity.md) элемент, который позволяет выполнять проверку подлинности конечной точки другими конечными точками обмен сообщениями с ним. [ \<Заголовки >](../../../../docs/framework/configure-apps/file-schema/wcf/headers.md) и [ \<удостоверение >](../../../../docs/framework/configure-apps/file-schema/wcf/identity.md) элементы являются частью <xref:System.ServiceModel.EndpointAddress> и обсуждаются в [адреса](../../../../docs/framework/wcf/feature-details/endpoint-addresses.md) раздела. Ссылки на разделы, в которых рассматривается использование расширений метаданных, приведены в подразделе "Настройка метаданных" данного раздела.  
  
## <a name="configuring-endpoints"></a>Настройка конечных точек  
 Конфигурация клиента позволяет клиенту задавать одну или несколько конечных точек, каждая со своим именем, адресом и контрактом, ссылающихся на [ \<привязки >](../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md) и [ \< поведения >](../../../../docs/framework/configure-apps/file-schema/wcf/behaviors.md) элементов в конфигурации клиента, который будет использоваться для настройки этих конечных точек. Файл конфигурации клиента должен называться "App.config", так как это имя ожидается средой выполнения [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]. В следующем примере показан файл конфигурации клиента.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
        <client>  
          <endpoint  
            name="endpoint1"  
            address="http://localhost/ServiceModelSamples/service.svc"  
            binding="wsHttpBinding"  
            bindingConfiguration="WSHttpBinding_IHello"  
            behaviorConfiguration="IHello_Behavior"  
            contract="IHello" >  
  
            <metadata>  
              <wsdlImporters>  
                <extension  
                  type="Microsoft.ServiceModel.Samples.WsdlDocumentationImporter, WsdlDocumentation"/>  
              </wsdlImporters>  
            </metadata>  
  
            <identity>  
              <servicePrincipalName value="host/localhost" />  
            </identity>  
          </endpoint>  
// Add another endpoint by adding another <endpoint> element.  
          <endpoint  
            name="endpoint2">  
           //Configure another endpoint here.  
          </endpoint>  
        </client>  
  
//The bindings section references by the bindingConfiguration endpoint attribute.  
    <bindings>  
      <wsHttpBinding>  
        <binding name="WSHttpBinding_IHello"   
                 bypassProxyOnLocal="false"   
                 hostNameComparisonMode="StrongWildcard">  
          <readerQuotas maxDepth="32"/>  
          <reliableSession ordered="true"   
                           enabled="false" />  
          <security mode="Message">  
           //Security settings go here.  
          </security>  
        </binding>  
        <binding name="Another Binding"  
        //Configure this binding here.  
        </binding>  
          </wsHttpBinding>  
        </bindings>  
  
//The behavior section references by the behaviorConfiguration endpoint attribute.  
        <behaviors>  
            <endpointBehaviors>  
                <behavior name=" IHello_Behavior ">  
                    <clientVia />  
                </behavior>  
            </endpointBehaviors>  
        </behaviors>  
  
    </system.serviceModel>  
</configuration>  
```  
  
 Необязательный атрибут `name` уникальным образом идентифицирует конечную точку для данного контракта. Он используется методом <xref:System.ServiceModel.ChannelFactory%601.%23ctor%2A> или <xref:System.ServiceModel.ClientBase%601.%23ctor%2A> для задания целевой конечной точки в конфигурации клиента, которая должна быть загружена при создании канала к службе. Предусмотрено подстановочное имя "*" конечной точки в конфигурации, которое указывает методу <xref:System.ServiceModel.ChannelFactory.ApplyConfiguration%2A>, что следует загрузить любую конфигурацию конечной точки из файла, при условии, что имеется ровно одна конфигурация, а в противном случае создать исключение. Если этот атрибут опущен, соответствующая конечная точка используется как конечная точка по умолчанию, связанная с заданным типом контракта. Значением по умолчанию для атрибута `name` является пустая строка, соответствие для которой проверяется так же, как и для любого другого имени.  
  
 С каждой конечной точкой связан адрес, который используется для поиска и идентификации этой конечной точки. Атрибут `address` может использоваться для указания URL-адреса, задающего расположение конечной точки. Однако адрес для конечной точки службы может также быть задан в коде путем создания универсального кода ресурса (URI), и он добавляется в <xref:System.ServiceModel.ServiceHost> с помощью одного из методов <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A>. [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][Адреса](../../../../docs/framework/wcf/feature-details/endpoint-addresses.md). При введении указывает, [ \<заголовки >](../../../../docs/framework/configure-apps/file-schema/wcf/headers.md) и [ \<удостоверение >](../../../../docs/framework/configure-apps/file-schema/wcf/identity.md) элементы являются частью <xref:System.ServiceModel.EndpointAddress> и обсуждаются также в [ Адреса](../../../../docs/framework/wcf/feature-details/endpoint-addresses.md) раздела.  
  
 Атрибут `binding` задает тип привязки, использование которого ожидается в конечной точке при подключении к службе. Для того чтобы на тип можно было ссылаться, он должен иметь зарегистрированный раздел конфигурации. В предыдущем примере это [ \<wsHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md) секции, которая указывает, что конечная точка использует <xref:System.ServiceModel.WSHttpBinding>. Однако могут существовать несколько привязок указанного типа, которые могут использоваться конечной точкой. Каждый из них имеет свой собственный [ \<привязки >](../../../../docs/framework/misc/binding.md) элемент в элементе типа (привязки). Для различения привязок одного типа служит атрибут `bindingconfiguration`. Его значение соответствует `name` атрибут [ \<привязки >](../../../../docs/framework/misc/binding.md) элемента. [!INCLUDE[crabout](../../../../includes/crabout-md.md)]Настройка клиента привязки с помощью конфигурации см. в разделе [как: Указание привязки клиента в конфигурации](../../../../docs/framework/wcf/how-to-specify-a-client-binding-in-configuration.md).  
  
 `behaviorConfiguration` Атрибут используется для указания [ \<поведение >](../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) из [ \<endpointBehaviors >](../../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md) конечная точка должна использовать. Его значение соответствует `name` атрибут [ \<поведение >](../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) элемента. Пример использования конфигурации для задания поведений клиента см. в разделе [Настройка поведений клиента](../../../../docs/framework/wcf/configuring-client-behaviors.md).  
  
 Атрибут `contract` задает контракт, который предоставляет данная конечная точка. Это значение соответствует свойству <xref:System.ServiceModel.ServiceContractAttribute.ConfigurationName%2A> атрибута <xref:System.ServiceModel.ServiceContractAttribute>. Значение по умолчанию - это полное имя типа класса, реализующего службу.  
  
### <a name="configuring-metadata"></a>Настройка метаданных  
 [ \<Метаданных >](../../../../docs/framework/configure-apps/file-schema/wcf/metadata.md) элемент используется для задания параметров, используемых для регистрации метаданных расширений импорта. [!INCLUDE[crabout](../../../../includes/crabout-md.md)]расширение системы метаданных см. в разделе[расширение системы метаданных](../../../../docs/framework/wcf/extending/extending-the-metadata-system.md).  
  
## <a name="see-also"></a>См. также  
 [Конечные точки: адреса, привязки и контракты](../../../../docs/framework/wcf/feature-details/endpoints-addresses-bindings-and-contracts.md)  
 [Настройка поведения клиентов](../../../../docs/framework/wcf/configuring-client-behaviors.md)
