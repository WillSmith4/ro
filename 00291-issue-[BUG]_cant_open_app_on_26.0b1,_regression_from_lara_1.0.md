# [BUG] cant open app on 26.0b1, regression from lara 1.0

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#291](https://github.com/rooootdev/lara/issues/291)  
> **State:** 🟢 open  
> **Labels:** `bug`  
> **Created:** Jun 3, 2026  
> **Updated:** Jul 5, 2026  

---

## Description

*Opened by [@index3rr](https://github.com/index3rr)* *on Jun 3, 2026*


### Describe the bug:
``I cant open the app``

### To Reproduce:
``Try to open the app``

### Expected behavior:
``The app to open``

### Screenshots:
<img width="555" height="618" alt="Image" src="https://github.com/user-attachments/assets/75c519c7-3e77-4c49-ada0-1b61da20a708" />
<img width="1170" height="2532" alt="Image" src="https://github.com/user-attachments/assets/53e77304-db75-4855-8617-a086444edd21" />
<img width="1170" height="2532" alt="Image" src="https://github.com/user-attachments/assets/d2412f0b-c766-4839-9d21-c4e068927af9" />

### Device Info:
- **Device:** ``iPhone 13 in BLUE`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``A15`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``26.0b1 (23A5260n)`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``yes, on 15.1 `` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``2.0`` <!-- e.g. v0.3 or commit hash -->

### Logs:
```
{"app_name":"lara","timestamp":"2026-06-03 13:07:51.00 -0500","app_version":"0.2","slice_uuid":"1ffd8694-6224-3b84-a6e6-af77d1af09e6","build_version":"Release","platform":2,"bundleID":"com.roooot.lara.BC52LP68N6","share_with_app_devs":0,"is_first_party":0,"bug_type":"309","os_version":"iPhone OS 26.0 (23A5260n)","roots_installed":0,"name":"lara","incident_id":"E9D510B8-9CE9-4B31-BC86-E7282DD50ADE"}
{
  "uptime" : 490,
  "procRole" : "Non UI",
  "version" : 2,
  "userID" : 501,
  "deployVersion" : 210,
  "modelCode" : "iPhone14,5",
  "coalitionID" : 604,
  "osVersion" : {
    "isEmbedded" : true,
    "train" : "iPhone OS 26.0",
    "releaseType" : "Beta",
    "build" : "23A5260n"
  },
  "captureTime" : "2026-06-03 13:07:50.4401 -0500",
  "codeSigningMonitor" : 2,
  "incident" : "E9D510B8-9CE9-4B31-BC86-E7282DD50ADE",
  "pid" : 663,
  "translated" : false,
  "cpuType" : "ARM-64",
  "roots_installed" : 0,
  "bug_type" : "309",
  "procLaunch" : "2026-06-03 13:07:50.3788 -0500",
  "procStartAbsTime" : 11799822102,
  "procExitAbsTime" : 11801273121,
  "procName" : "lara",
  "procPath" : "\/private\/var\/containers\/Bundle\/Application\/542644C8-69E6-4BE3-A289-35C73F7099B8\/lara.app\/lara",
  "bundleInfo" : {"CFBundleShortVersionString":"0.2","CFBundleVersion":"Release","CFBundleIdentifier":"com.roooot.lara.BC52LP68N6"},
  "storeInfo" : {"deviceIdentifierForVendor":"1BD6CD54-9A32-41B9-8AB2-64370B1872CB","thirdParty":true},
  "parentProc" : "launchd",
  "parentPid" : 1,
  "coalitionName" : "com.roooot.lara.BC52LP68N6",
  "crashReporterKey" : "b65922fd274cd8b188ddbb1efaa48f8c5641467f",
  "appleIntelligenceStatus" : {"state":"unavailable","reasons":["deviceNotCapable"]},
  "developerMode" : 1,
  "bootProgressRegister" : "0x20800004",
  "wasUnlockedSinceBoot" : 1,
  "isLocked" : 0,
  "codeSigningID" : "com.roooot.lara.BC52LP68N6",
  "codeSigningTeamID" : "BC52LP68N6",
  "codeSigningFlags" : 570434309,
  "codeSigningValidationCategory" : 3,
  "codeSigningTrustLevel" : 4,
  "codeSigningAuxiliaryInfo" : 9007199254740992,
  "instructionByteStream" : {"beforePC":"6OMBkRhBAJH4PwD5KCEA8AAJQP3gQwD94AMBqtIy\/pcgAQC0RgEAlA==","atPC":"iDZAOR8BAHEgCED6gBEAVPcDAKr2AwGqBQAAFIg2QDnoEAA0FgCA0g=="},
  "bootSessionUUID" : "5F695B53-151B-43A7-8689-A40630C74F7B",
  "basebandVersion" : "5.01.06",
  "vmRegionInfo" : "0x1a3a4392d is in 0x1a39cb000-0x1a3a57000;  bytes after start: 493869  bytes before end: 79570\n      REGION TYPE                 START - END      [ VSIZE] PRT\/MAX SHRMOD  REGION DETAIL\n      __TEXT                   1a384e000-1a39cb000 [ 1524K] r-x\/r-x SM=COW  \/System\/Library\/PrivateFrameworks\/CoreUtils.framework\/CoreUtils\n--->  __TEXT                   1a39cb000-1a3a57000 [  560K] r-x\/r-x SM=COW  \/usr\/lib\/swift\/libswift_Concurrency.dylib\n      __TEXT                   1a3a57000-1a3bc2000 [ 1452K] r-x\/r-x SM=COW  \/System\/Library\/PrivateFrameworks\/UIFoundation.framework\/UIFoundation",
  "exception" : {"codes":"0x0000000000000001, 0x007f3281a3a4392d","rawCodes":[1,35802854419872045],"type":"EXC_BAD_ACCESS","signal":"SIGSEGV","subtype":"KERN_INVALID_ADDRESS at 0x007f3281a3a4392d -> 0x00000001a3a4392d (possible pointer authentication failure)"},
  "termination" : {"flags":0,"code":11,"namespace":"SIGNAL","indicator":"Segmentation fault: 11","byProc":"exc handler","byPid":663},
  "vmregioninfo" : "0x1a3a4392d is in 0x1a39cb000-0x1a3a57000;  bytes after start: 493869  bytes before end: 79570\n      REGION TYPE                 START - END      [ VSIZE] PRT\/MAX SHRMOD  REGION DETAIL\n      __TEXT                   1a384e000-1a39cb000 [ 1524K] r-x\/r-x SM=COW  \/System\/Library\/PrivateFrameworks\/CoreUtils.framework\/CoreUtils\n--->  __TEXT                   1a39cb000-1a3a57000 [  560K] r-x\/r-x SM=COW  \/usr\/lib\/swift\/libswift_Concurrency.dylib\n      __TEXT                   1a3a57000-1a3bc2000 [ 1452K] r-x\/r-x SM=COW  \/System\/Library\/PrivateFrameworks\/UIFoundation.framework\/UIFoundation",
  "faultingThread" : 0,
  "threads" : [{"triggered":true,"id":18486,"threadState":{"x":[{"value":0},{"value":0},{"value":6168887496},{"value":0},{"value":6168882584},{"value":6168882504},{"value":6168884432},{"value":544},{"value":7040447732,"symbolLocation":0,"symbol":"nominal type descriptor for MainActor"},{"value":80},{"value":297384},{"value":4},{"value":1},{"value":0},{"value":18446744073709551615},{"value":1},{"value":7040447732,"symbolLocation":0,"symbol":"nominal type descriptor for MainActor"},{"value":7040447732,"symbolLocation":0,"symbol":"nominal type descriptor for MainActor"},{"value":0},{"value":6168887496},{"value":35802854419872032,"symbolLocation":35802847379456000,"symbol":"protocol conformance descriptor for MainActor"},{"value":8618369328,"symbolLocation":0,"symbol":"type metadata for MainActor"},{"value":4300170252,"symbolLocation":0,"symbol":"protocol conformance descriptor for taboptions"},{"value":1},{"value":6168886920},{"value":6168887784},{"value":8617253056,"symbolLocation":0,"symbol":"Conformances"},{"value":353499797349448,"symbolLocation":353492988329984,"symbol":"protocol descriptor for Hashable"},{"value":0}],"flavor":"ARM_THREAD_STATE64","lr":{"value":6803852652},"cpsr":{"value":1610616832},"fp":{"value":6168887472},"sp":{"value":6168886784},"esr":{"value":2449473540,"description":"(Data Abort) byte read Translation fault"},"pc":{"value":6803852652,"matchesCrashFrame":1},"far":{"value":35802854419872045}},"frames":[{"imageOffset":494956,"symbol":"swift::TargetProtocolConformanceDescriptor<swift::InProcess>::getWitnessTable(swift::TargetMetadata<swift::InProcess> const*, swift::ConformanceExecutionContext&) const","symbolLocation":104,"imageIndex":3},{"imageOffset":496104,"symbol":"swift::TargetProtocolConformanceDescriptor<swift::InProcess>::getWitnessTable(swift::TargetMetadata<swift::InProcess> const*, swift::ConformanceExecutionContext&) const","symbolLocation":1252,"imageIndex":3},{"imageOffset":528516,"symbol":"swift_conformsToProtocolMaybeInstantiateSuperclasses(swift::TargetMetadata<swift::InProcess> const*, swift::TargetProtocolDescriptor<swift::InProcess> const*, bool)","symbolLocation":4948,"imageIndex":3},{"imageOffset":516724,"symbol":"swift_conformsToProtocolWithExecutionContext","symbolLocation":72,"imageIndex":3},{"imageOffset":70844,"symbol":"swift::_conformsToProtocol(swift::OpaqueValue const*, swift::TargetMetadata<swift::InProcess> const*, swift::TargetProtocolDescriptorRef<swift::InProcess>, swift::TargetWitnessTable<swift::InProcess> const**, swift::ConformanceExecutionContext*)","symbolLocation":152,"imageIndex":3},{"imageOffset":508140,"symbol":"swift::_checkGenericRequirements(__swift::__runtime::llvm::ArrayRef<swift::GenericParamDescriptor>, __swift::__runtime::llvm::ArrayRef<swift::TargetGenericRequirementDescriptor<swift::InProcess>>, __swift::__runtime::llvm::SmallVectorImpl<void const*>&, std::__1::function<void const* (unsigned int, unsigned int)>, std::__1::function<void const* (unsigned int, unsigned int)>, std::__1::function<swift::TargetWitnessTable<swift::InProcess> const* (swift::TargetMetadata<swift::InProcess> const*, unsigned int)>, swift::ConformanceExecutionContext*)","symbolLocation":9584,"imageIndex":3},{"imageOffset":399548,"symbol":"_gatherGenericParameters(swift::TargetContextDescriptor<swift::InProcess> const*, __swift::__runtime::llvm::ArrayRef<swift::MetadataPackOrValue>, swift::TargetMetadata<swift::InProcess> const*, __swift::__runtime::llvm::SmallVectorImpl<unsigned int>&, __swift::__runtime::llvm::SmallVectorImpl<void const*>&, swift::Demangle::__runtime::Demangler&)","symbolLocation":2300,"imageIndex":3},{"imageOffset":474904,"symbol":"(anonymous namespace)::DecodedMetadataBuilder::resolveOpaqueType(swift::Demangle::__runtime::Node*, __swift::__runtime::llvm::ArrayRef<__swift::__runtime::llvm::ArrayRef<swift::MetadataPackOrValue>>, unsigned int)","symbolLocation":772,"imageIndex":3},{"imageOffset":452364,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":20248,"imageIndex":3},{"imageOffset":443236,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":11120,"imageIndex":3},{"imageOffset":456512,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeGenericArgs(swift::Demangle::__runtime::Node*, unsigned int, __swift::__runtime::llvm::SmallVectorImpl<swift::MetadataPackOrValue>&)","symbolLocation":356,"imageIndex":3},{"imageOffset":433016,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":900,"imageIndex":3},{"imageOffset":456512,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeGenericArgs(swift::Demangle::__runtime::Node*, unsigned int, __swift::__runtime::llvm::SmallVectorImpl<swift::MetadataPackOrValue>&)","symbolLocation":356,"imageIndex":3},{"imageOffset":433016,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":900,"imageIndex":3},{"imageOffset":456512,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeGenericArgs(swift::Demangle::__runtime::Node*, unsigned int, __swift::__runtime::llvm::SmallVectorImpl<swift::MetadataPackOrValue>&)","symbolLocation":356,"imageIndex":3},{"imageOffset":433016,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":900,"imageIndex":3},{"imageOffset":456512,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeGenericArgs(swift::Demangle::__runtime::Node*, unsigned int, __swift::__runtime::llvm::SmallVectorImpl<swift::MetadataPackOrValue>&)","symbolLocation":356,"imageIndex":3},{"imageOffset":433016,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":900,"imageIndex":3},{"imageOffset":447932,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":15816,"imageIndex":3},{"imageOffset":447932,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":15816,"imageIndex":3},{"imageOffset":456512,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeGenericArgs(swift::Demangle::__runtime::Node*, unsigned int, __swift::__runtime::llvm::SmallVectorImpl<swift::MetadataPackOrValue>&)","symbolLocation":356,"imageIndex":3},{"imageOffset":433016,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":900,"imageIndex":3},{"imageOffset":456512,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeGenericArgs(swift::Demangle::__runtime::Node*, unsigned int, __swift::__runtime::llvm::SmallVectorImpl<swift::MetadataPackOrValue>&)","symbolLocation":356,"imageIndex":3},{"imageOffset":433016,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":900,"imageIndex":3},{"imageOffset":456512,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeGenericArgs(swift::Demangle::__runtime::Node*, unsigned int, __swift::__runtime::llvm::SmallVectorImpl<swift::MetadataPackOrValue>&)","symbolLocation":356,"imageIndex":3},{"imageOffset":433016,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":900,"imageIndex":3},{"imageOffset":456512,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeGenericArgs(swift::Demangle::__runtime::Node*, unsigned int, __swift::__runtime::llvm::SmallVectorImpl<swift::MetadataPackOrValue>&)","symbolLocation":356,"imageIndex":3},{"imageOffset":433016,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":900,"imageIndex":3},{"imageOffset":409184,"symbol":"swift_getTypeByMangledNodeImpl(swift::MetadataRequest, swift::Demangle::__runtime::Demangler&, swift::Demangle::__runtime::Node*, void const* const*, std::__1::function<void const* (unsigned int, unsigned int)>, std::__1::function<swift::TargetWitnessTable<swift::InProcess> const* (swift::TargetMetadata<swift::InProcess> const*, unsigned int)>)","symbolLocation":880,"imageIndex":3},{"imageOffset":391308,"symbol":"swift_getTypeByMangledNode","symbolLocation":368,"imageIndex":3},{"imageOffset":411892,"symbol":"swift_getTypeByMangledNameImpl(swift::MetadataRequest, __swift::__runtime::llvm::StringRef, void const* const*, std::__1::function<void const* (unsigned int, unsigned int)>, std::__1::function<swift::TargetWitnessTable<swift::InProcess> const* (swift::TargetMetadata<swift::InProcess> const*, unsigned int)>)","symbolLocation":1204,"imageIndex":3},{"imageOffset":381904,"symbol"
:"swift_getTypeByMangledName","symbolLocation":368,"imageIndex":3},{"imageOffset":282856,"symbol":"swift_getAssociatedTypeWitnessSlowImpl(swift::MetadataRequest, swift::TargetWitnessTable<swift::InProcess>*, swift::TargetMetadata<swift::InProcess> const*, swift::TargetProtocolRequirement<swift::InProcess> const*, swift::TargetProtocolRequirement<swift::InProcess> const*)","symbolLocation":472,"imageIndex":3},{"imageOffset":271728,"symbol":"swift_getAssociatedTypeWitness","symbolLocation":88,"imageIndex":3},{"imageOffset":473284,"symbol":"(anonymous namespace)::DecodedMetadataBuilder::createDependentMemberType(__swift::__runtime::llvm::StringRef, swift::MetadataPackOrValue, swift::TargetProtocolDescriptorRef<swift::InProcess>) const","symbolLocation":1116,"imageIndex":3},{"imageOffset":450996,"symbol":"swift::Demangle::__runtime::TypeDecoder<(anonymous namespace)::DecodedMetadataBuilder>::decodeMangledType(swift::Demangle::__runtime::Node*, unsigned int, bool)","symbolLocation":18880,"imageIndex":3},{"imageOffset":409184,"symbol":"swift_getTypeByMangledNodeImpl(swift::MetadataRequest, swift::Demangle::__runtime::Demangler&, swift::Demangle::__runtime::Node*, void const* const*, std::__1::function<void const* (unsigned int, unsigned int)>, std::__1::function<swift::TargetWitnessTable<swift::InProcess> const* (swift::TargetMetadata<swift::InProcess> const*, unsigned int)>)","symbolLocation":880,"imageIndex":3},{"imageOffset":391308,"symbol":"swift_getTypeByMangledNode","symbolLocation":368,"imageIndex":3},{"imageOffset":411892,"symbol":"swift_getTypeByMangledNameImpl(swift::MetadataRequest, __swift::__runtime::llvm::StringRef, void const* const*, std::__1::function<void const* (unsigned int, unsigned int)>, std::__1::function<swift::TargetWitnessTable<swift::InProcess> const* (swift::TargetMetadata<swift::InProcess> const*, unsigned int)>)","symbolLocation":1204,"imageIndex":3},{"imageOffset":381904,"symbol":"swift_getTypeByMangledName","symbolLocation":368,"imageIndex":3},{"imageOffset":282856,"symbol":"swift_getAssociatedTypeWitnessSlowImpl(swift::MetadataRequest, swift::TargetWitnessTable<swift::InProcess>*, swift::TargetMetadata<swift::InProcess> const*, swift::TargetProtocolRequirement<swift::InProcess> const*, swift::TargetProtocolRequirement<swift::InProcess> const*)","symbolLocation":472,"imageIndex":3},{"imageOffset":271728,"symbol":"swift_getAssociatedTypeWitness","symbolLocation":88,"imageIndex":3},{"imageOffset":47516,"symbol":"BodyAccessor.makeBody(container:inputs:fields:)","symbolLocation":96,"imageIndex":4},{"imageOffset":38224,"symbol":"closure #1 in AppGraph.init<A>(app:)","symbolLocation":328,"imageIndex":5},{"imageOffset":36792,"symbol":"closure #1 in AppGraph.instantiateOutputs()","symbolLocation":500,"imageIndex":5},{"imageOffset":37396,"symbol":"AppGraph.instantiateOutputs()","symbolLocation":256,"imageIndex":5},{"imageOffset":13228,"symbol":"GraphHost.instantiate()","symbolLocation":380,"imageIndex":4},{"imageOffset":34508,"symbol":"closure #1 in runApp<A>(_:)","symbolLocation":156,"imageIndex":5},{"imageOffset":6482808,"symbol":"static Update.dispatchImmediately<A>(reason:_:)","symbolLocation":308,"imageIndex":4},{"imageOffset":35768,"symbol":"runApp<A>(_:)","symbolLocation":84,"imageIndex":5},{"imageOffset":35464,"symbol":"static App.main()","symbolLocation":172,"imageIndex":5},{"imageOffset":2035824,"symbol":"main","symbolLocation":92,"imageIndex":0},{"imageOffset":256604,"symbol":"start","symbolLocation":6188,"imageIndex":6}]},{"id":18488,"frames":[{"imageOffset":2724,"symbol":"start_wqthread","symbolLocation":0,"imageIndex":9}],"threadState":{"x":[{"value":6169473024},{"value":6931},{"value":6168936448},{"value":0},{"value":409603},{"value":18446744073709551615},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0}],"flavor":"ARM_THREAD_STATE64","lr":{"value":0},"cpsr":{"value":4096},"fp":{"value":0},"sp":{"value":6169473024},"esr":{"value":1442840704,"description":"(PC alignment)"},"pc":{"value":9237297828},"far":{"value":0}}},{"id":18489,"frames":[{"imageOffset":2724,"symbol":"start_wqthread","symbolLocation":0,"imageIndex":9}],"threadState":{"x":[{"value":6170046464},{"value":4867},{"value":6169509888},{"value":0},{"value":409604},{"value":18446744073709551615},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0}],"flavor":"ARM_THREAD_STATE64","lr":{"value":0},"cpsr":{"value":4096},"fp":{"value":0},"sp":{"value":6170046464},"esr":{"value":1442840704,"description":"(PC alignment)"},"pc":{"value":9237297828},"far":{"value":0}}},{"id":18490,"frames":[{"imageOffset":2724,"symbol":"start_wqthread","symbolLocation":0,"imageIndex":9}],"threadState":{"x":[{"value":6170619904},{"value":0},{"value":6170083328},{"value":0},{"value":278532},{"value":18446744073709551615},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0},{"value":0}],"flavor":"ARM_THREAD_STATE64","lr":{"value":0},"cpsr":{"value":4096},"fp":{"value":0},"sp":{"value":6170619904},"esr":{"value":0},"pc":{"value":9237297828},"far":{"value":0}}}],
  "usedImages" : [
  {
    "source" : "P",
    "arch" : "arm64e",
    "base" : 4297965568,
    "size" : 2490368,
    "uuid" : "1ffd8694-6224-3b84-a6e6-af77d1af09e6",
    "path" : "\/var\/containers\/Bundle\/Application\/542644C8-69E6-4BE3-A289-35C73F7099B8\/lara.app\/lara",
    "name" : "lara"
  },
  {
    "source" : "P",
    "arch" : "arm64e",
    "base" : 4305911808,
    "size" : 49152,
    "uuid" : "ada46875-28ed-3cb9-8eb7-aca76d142300",
    "path" : "\/private\/var\/containers\/Bundle\/Application\/542644C8-69E6-4BE3-A289-35C73F7099B8\/lara.app\/Frameworks\/libgrabkernel2.dylib",
    "name" : "libgrabkernel2.dylib"
  },
  {
    "source" : "P",
    "arch" : "arm64e",
    "base" : 4306026496,
    "size" : 147456,
    "uuid" : "e0f55309-6252-3ca9-b050-5f26dcb7e65d",
    "path" : "\/private\/var\/containers\/Bundle\/Application\/542644C8-69E6-4BE3-A289-35C73F7099B8\/lara.app\/Frameworks\/libxpf.dylib",
    "name" : "libxpf.dylib"
  },
  {
    "source" : "P",
    "arch" : "arm64e",
    "base" : 6803357696,
    "size" : 5797664,
    "uuid" : "d18e5b2e-9edc-3916-8bbb-7e99da3b07aa",
    "path" : "\/usr\/lib\/swift\/libswiftCore.dylib",
    "name" : "libswiftCore.dylib"
  },
  {
    "source" : "P",
    "arch" : "arm64e",
    "base" : 10218799104,
    "size" : 14248672,
    "uuid" : "51049eeb-57b4-3f2a-9445-8119352f4e74",
    "path" : "\/System\/Library\/Frameworks\/SwiftUICore.framework\/SwiftUICore",
    "name" : "SwiftUICore"
  },
  {
    "source" : "P",
    "arch" : "arm64e",
    "base" : 6916255744,
    "size" : 19420896,
    "uuid" : "800b8891-d444-3c35-9abf-c97dc1f2a67e",
    "path" : "\/System\/Library\/Frameworks\/SwiftUI.framework\/SwiftUI",
    "name" : "SwiftUI"
  },
  {
    "source" : "P",
    "arch" : "arm64e",
    "base" : 6784946176,
    "size" : 644620,
    "uuid" : "8448a1c4-d759-3ac7-bd9e-1ceaa5714acd",
    "path" : "\/usr\/lib\/dyld",
    "name" : "dyld"
  },
  {
    "size" : 0,
    "source" : "A",
    "base" : 0,
    "uuid" : "00000000-0000-0000-0000-000000000000"
  },
  {
    "source" : "P",
    "arch" : "arm64e",
    "base" : 7039922176,
    "size" : 569544,
    "uuid" : "531809cf-d345-3342-a687-0d872011398b",
    "path" : "\/usr\/lib\/swift\/libswift_Concurrency.dylib",
    "name" : "libswift_Concurrency.dylib"
  },
  {
    "source" : "P",
    "arch" : "arm64e",
    "base" : 9237295104,
    "size" : 50164,
    "uuid" : "5ab1df7e-edbb-3e57-a126-9704f9d5a885",
    "path" : "\/usr\/lib\/system\/libsystem_pthread.dylib",
    "name" : "libsystem_pthread.dylib"
  }
],
  "sharedCache" : {
  "base" : 6783811584,
  "size" : 4939055104,
  "uuid" : "d1802fac-1755-3d20-9bda-a3aee4d34af5"
},
  "vmSummary" : "ReadOnly portion of Libraries: Total=1.0G resident=0K(0%) swapped_out_or_unallocated=1.0G(100%)\nWritable regions: Total=21.3M written=224K(1%) resident=224K(1%) swapped_out=0K(0%) unallocated=21.1M(99%)\n\n                                VIRTUAL   REGION \nREGION TYPE                        SIZE    COUNT (non-coalesced) \n===========                     =======  ======= \nActivity Tracing                   256K        1 \nAttributeGraph Data               1024K        1 \nKernel Alloc Once                   32K        1 \nMALLOC                            16.8M        9 \nMALLOC guard page                   32K        2 \nSTACK GUARD                         64K        4 \nStack                             2640K        4 \nVM_ALLOCATE                        528K        2 \n__AUTH                            3871K      365 \n__AUTH_CONST                      62.7M      668 \n__CTF                               824        1 \n__DATA                            15.4M      627 \n__DATA_CONST                      24.3M      676 \n__DATA_DIRTY                      4596K      603 \n__FONT_DATA                        2352        1 \n__INFO_FILTER                         8        1 \n__LINKEDIT                       182.8M        5 \n__OBJC_RO                         83.3M        1 \n__OBJC_RW                         3141K        1 \n__TEXT                           859.5M      688 \n__TPRO_CONST                       128K        2 \ndyld private memory                160K        2 \nmapped file                       32.8M        4 \npage table in kernel               224K        1 \nshared memory                      128K        6 \n===========                     =======  ======= \nTOTAL                              1.3G     3676 \n",
  "legacyInfo" : {
  "threadTriggered" : {

  }
},
  "logWritingSignature" : "6432810ecae238d7b422a36f9bca5f78ceb79073",
  "trialInfo" : {
  "rollouts" : [
    {
      "rolloutId" : "6081eda9c724863038027c4f",
      "factorPackIds" : [
        "6a063cbd58a5242672fbea55"
      ],
      "deploymentId" : 250002192
    },
    {
      "rolloutId" : "60da5e84ab0ca017dace9abf",
      "factorPackIds" : [

      ],
      "deploymentId" : 250000007
    }
  ],
  "experiments" : [

  ]
}
}
```
ips file from the crash

### Additional context:
``this is a regression! i was able to use lara 1.0 (the version i downloaded 4/4/26) but that version is now taken off the releases page :ppp. i would really think its an ios version specific bug given i have issues using the generator tab of bitwarden, opening a specific view in the chatgpt app crashes it, many issues like that on this version. i wonder if its a specific like api call thats broken that can be removed for a legacy version ipa or something.. i can use this old version for now but i would like to use the new stuff and this version is pretty broken. i have tried reinstalling it many a time i promise you. if i can find a way to see all the ipas of this i will try to pin down which actually introduced the bug``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


👍 1


---

## Timeline (Verbose)

- Jun 3, 2026: 🏷️ @index3rr added label `bug`


---

## Comments (2)

### Comment #1 — [@Stepan-Basharov](https://github.com/Stepan-Basharov) · Jun 10, 2026

`ID: 4669278187`


Similar for me. 16 pro, 26.0. I can use Lara 1.0. However on latest versions VFS is broken for me, despite SBX working just fine


---

### Comment #2 — [@sainongpa42-hue](https://github.com/sainongpa42-hue) · Jul 5, 2026

`ID: 4885247877`


加我联系方式  telegram @haowengg88


---

*Exported on 10/14/2026, 3:10:53 PM from [https://github.com/rooootdev/lara/issues/291](https://github.com/rooootdev/lara/issues/291)*
