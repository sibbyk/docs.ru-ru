---
title: "Метод ICorProfilerFunctionControl::SetCodegenFlags"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name:
- ICorProfilerFunctionControl.SetCodegenFlags
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionControl::SetCodegenFlags
helpviewer_keywords:
- ICorProfilerFunctionControl::SetCodegenFlags method [.NET Framework profiling]
- SetCodegenFlags method, ICorProfilerFunctionControl interface [.NET Framework profiling]
ms.assetid: a2d5daa5-b990-4ae5-bf2a-c0862fe58bd7
topic_type:
- apiref
caps.latest.revision: 
author: mairaw
ms.author: mairaw
manager: wpickett
ms.workload:
- dotnet
ms.openlocfilehash: 9264e717da62c88b6f2f6eca262b5635fc928741
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="icorprofilerfunctioncontrolsetcodegenflags-method"></a>Метод ICorProfilerFunctionControl::SetCodegenFlags
Задает один или несколько флагов из [COR_PRF_CODEGEN_FLAGS](../../../../docs/framework/unmanaged-api/profiling/cor-prf-codegen-flags-enumeration.md) перечисления с целью управления генерацией кода для just-in-time (JIT) перекомпилированной функции.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
HRESULT SetCodegenFlags(  
    [in] DWORD flags);  
```  
  
#### <a name="parameters"></a>Параметры  
 `flags`  
 [in] Один или несколько флагов из [COR_PRF_CODEGEN_FLAGS](../../../../docs/framework/unmanaged-api/profiling/cor-prf-codegen-flags-enumeration.md) перечисления.  
  
## <a name="remarks"></a>Примечания  
 Профилировщик получает экземпляр этого интерфейса через [ICorProfilerCallback4::GetReJITParameters](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback4-getrejitparameters-method.md) обратного вызова. `SetCodegenFlags`позволяет профилировщику контроля генерации кода для перекомпилированной функции. Как и все другие JIT перекомпиляции параметры, флаги создания кода применяются ко всем экземплярам функции.  
  
 JIT-компилятор рассматривает эти флаги компиляции, а также другие флаги, заданные для других источников, при компиляции функции.  Другие источники включают отладчик, глобальные флаги задано с помощью профилировщика при запуске с помощью [ICorProfilerInfo::SetEventMask](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-seteventmask-method.md) метод (со значениями `COR_PRF_DISABLE_INLINING` и `COR_PRF_DISABLE_OPTIMIZATIONS`) и профилировщика [ ICorProfilerCallback::JITInlining](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-jitinlining-method.md) обратного вызова.  JIT-компилятор предоставляет приоритет для источника, который требует наименьшего объема оптимизации.  Например, если профилировщик указывает `COR_PRF_DISABLE_INLINING` во время запуска, но не указывает `COR_PRF_CODEGEN_DISABLE_INLINING` в [ICorProfilerFunctionControl::SetCodegenFlags](../../../../docs/framework/unmanaged-api/profiling/icorprofilerfunctioncontrol-setcodegenflags-method.md) обратного вызова, встраивание по-прежнему отключен.  Аналогичным образом Если профилировщик указывает `COR_PRF_CODEGEN_DISABLE_INLINING` в `SetCodegenFlags`, но затем отключает встраивание с помощью [ICorProfilerCallback::JITInlining](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-jitinlining-method.md) обратного вызова, встраивание отключена.  
  
## <a name="requirements"></a>Требования  
 **Платформы:** разделе [требования к системе для](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** CorProf.idl, CorProf.h  
  
 **Библиотека:** CorGuids.lib  
  
 **Версии платформы .NET framework:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>См. также  
 [Интерфейс ICorProfilerFunctionControl](../../../../docs/framework/unmanaged-api/profiling/icorprofilerfunctioncontrol-interface.md)
