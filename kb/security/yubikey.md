---
layout: page
ptitle: The yubico YubiKey 5 NFC
---

---

[product page](
   https://www.yubico.com/ro/product/yubikey-5-series/yubikey-5-nfc/)

[wikipedia](https://en.wikipedia.org/wiki/YubiKey)

# Detecting the hardware
```sh
# In Linux:

# In Windows 11 (WSL)

# Get device ID (with wmic)
$ wmic.exe path Win32_PnPEntity get Name,DeviceID | grep -i yubi
SCFILTER\CID_8073C021C057597562694B6579\7&2FE4C607&0&YUBICO_YUBIKEY_OTP+FIDO+CCID_0_SCFILTER_CID_8073C021C057597562694B6579  Smart Card
SWD\SCDEVICEENUM\1_YUBICO_YUBIKEY_OTP+FIDO+CCID_0                                                                            Yubico YubiKey OTP+FIDO+CCID 0

# Get device info as json object(to yaml):
$ powershell.exe -Command "Get-PnpDevice | Where-Object { \$_.InstanceId -like '*YUBICO*' } | ConvertTo-Json -Depth 99" | yq -yr
```
<details>
  <summary>Click to expand YAML output</summary>
{% highlight txt %}
- CimClass:
    CimSuperClassName: CIM_LogicalDevice
    CimSuperClass:
      CimSuperClassName: CIM_LogicalElement
      CimSuperClass:
        CimSuperClassName: CIM_ManagedSystemElement
        CimSuperClass:
          CimSuperClassName: null
          CimSuperClass: null
          CimClassProperties:
            - Name: Caption
              Value: null
              CimType: 14
              Flags: Property, ReadOnly, NullValue
              Qualifiers:
                - Name: MaxLen
                  Value: 64
                  CimType: 7
                  Flags: EnableOverride, ToSubclass
                - Name: read
                  Value: true
                  CimType: 1
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
            - Name: Description
              Value: null
              CimType: 14
              Flags: Property, ReadOnly, NullValue
              Qualifiers:
                - Name: read
                  Value: true
                  CimType: 1
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
            - Name: InstallDate
              Value: null
              CimType: 13
              Flags: Property, ReadOnly, NullValue
              Qualifiers:
                - Name: MappingStrings
                  Value:
                    - MIF.DMTF|ComponentID|001.5
                  CimType: 30
                  Flags: EnableOverride, ToSubclass
                - Name: read
                  Value: true
                  CimType: 1
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
            - Name: Name
              Value: null
              CimType: 14
              Flags: Property, ReadOnly, NullValue
              Qualifiers:
                - Name: read
                  Value: true
                  CimType: 1
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
            - Name: Status
              Value: null
              CimType: 14
              Flags: Property, ReadOnly, NullValue
              Qualifiers:
                - Name: MaxLen
                  Value: 10
                  CimType: 7
                  Flags: EnableOverride, ToSubclass
                - Name: read
                  Value: true
                  CimType: 1
                  Flags: EnableOverride, ToSubclass
                - Name: ValueMap
                  Value:
                    - OK
                    - Error
                    - Degraded
                    - Unknown
                    - Pred Fail
                    - Starting
                    - Stopping
                    - Service
                    - Stressed
                    - NonRecover
                    - No Contact
                    - Lost Comm
                  CimType: 30
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
          CimClassQualifiers:
            - Name: Abstract
              Value: true
              CimType: 1
              Flags: EnableOverride, Restricted
            - Name: Locale
              Value: 1033
              CimType: 7
              Flags: EnableOverride, ToSubclass
            - Name: UUID
              Value: '{8502C517-5FBB-11D2-AAC1-006008C78BC7}'
              CimType: 14
              Flags: EnableOverride, ToSubclass
          CimClassMethods: []
          CimSystemProperties:
            Namespace: ROOT/cimv2
            ServerName: TANCREDI-L-W11
            ClassName: CIM_ManagedSystemElement
            Path: null
        CimClassProperties:
          - Name: Caption
            Value: null
            CimType: 14
            Flags: Property, ReadOnly, NullValue
            Qualifiers:
              - Name: MaxLen
                Value: 64
                CimType: 7
                Flags: EnableOverride, ToSubclass
              - Name: read
                Value: true
                CimType: 1
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
          - Name: Description
            Value: null
            CimType: 14
            Flags: Property, ReadOnly, NullValue
            Qualifiers:
              - Name: read
                Value: true
                CimType: 1
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
          - Name: InstallDate
            Value: null
            CimType: 13
            Flags: Property, ReadOnly, NullValue
            Qualifiers:
              - Name: MappingStrings
                Value:
                  - MIF.DMTF|ComponentID|001.5
                CimType: 30
                Flags: EnableOverride, ToSubclass
              - Name: read
                Value: true
                CimType: 1
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
          - Name: Name
            Value: null
            CimType: 14
            Flags: Property, ReadOnly, NullValue
            Qualifiers:
              - Name: read
                Value: true
                CimType: 1
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
          - Name: Status
            Value: null
            CimType: 14
            Flags: Property, ReadOnly, NullValue
            Qualifiers:
              - Name: MaxLen
                Value: 10
                CimType: 7
                Flags: EnableOverride, ToSubclass
              - Name: read
                Value: true
                CimType: 1
                Flags: EnableOverride, ToSubclass
              - Name: ValueMap
                Value:
                  - OK
                  - Error
                  - Degraded
                  - Unknown
                  - Pred Fail
                  - Starting
                  - Stopping
                  - Service
                  - Stressed
                  - NonRecover
                  - No Contact
                  - Lost Comm
                CimType: 30
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
        CimClassQualifiers:
          - Name: Locale
            Value: 1033
            CimType: 7
            Flags: EnableOverride, ToSubclass
          - Name: UUID
            Value: '{8502C518-5FBB-11D2-AAC1-006008C78BC7}'
            CimType: 14
            Flags: EnableOverride, ToSubclass
          - Name: Abstract
            Value: true
            CimType: 1
            Flags: EnableOverride, Restricted
        CimClassMethods: []
        CimSystemProperties:
          Namespace: ROOT/cimv2
          ServerName: TANCREDI-L-W11
          ClassName: CIM_LogicalElement
          Path: null
      CimClassProperties:
        - Name: Caption
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: MaxLen
              Value: 64
              CimType: 7
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: Description
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: InstallDate
          Value: null
          CimType: 13
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: MappingStrings
              Value:
                - MIF.DMTF|ComponentID|001.5
              CimType: 30
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: Name
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: Status
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: MaxLen
              Value: 10
              CimType: 7
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: ValueMap
              Value:
                - OK
                - Error
                - Degraded
                - Unknown
                - Pred Fail
                - Starting
                - Stopping
                - Service
                - Stressed
                - NonRecover
                - No Contact
                - Lost Comm
              CimType: 30
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: Availability
          Value: null
          CimType: 4
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: MappingStrings
              Value:
                - MIF.DMTF|Operational State|003.5
                - MIB.IETF|HOST-RESOURCES-MIB.hrDeviceStatus
              CimType: 30
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: ValueMap
              Value:
                - '1'
                - '2'
                - '3'
                - '4'
                - '5'
                - '6'
                - '7'
                - '8'
                - '9'
                - '10'
                - '11'
                - '12'
                - '13'
                - '14'
                - '15'
                - '16'
                - '17'
                - '18'
                - '19'
                - '20'
                - '21'
              CimType: 30
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: ConfigManagerErrorCode
          Value: null
          CimType: 6
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: Schema
              Value: Win32
              CimType: 14
              Flags: EnableOverride, ToSubclass
            - Name: ValueMap
              Value:
                - '0'
                - '1'
                - '2'
                - '3'
                - '4'
                - '5'
                - '6'
                - '7'
                - '8'
                - '9'
                - '10'
                - '11'
                - '12'
                - '13'
                - '14'
                - '15'
                - '16'
                - '17'
                - '18'
                - '19'
                - '20'
                - '21'
                - '22'
                - '23'
                - '24'
                - '25'
                - '26'
                - '27'
                - '28'
                - '29'
                - '30'
                - '31'
              CimType: 30
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: ConfigManagerUserConfig
          Value: null
          CimType: 1
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: Schema
              Value: Win32
              CimType: 14
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: CreationClassName
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: CIM_Key
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: DeviceID
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: CIM_Key
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: ErrorCleared
          Value: null
          CimType: 1
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: ErrorDescription
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: LastErrorCode
          Value: null
          CimType: 6
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: PNPDeviceID
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: Schema
              Value: Win32
              CimType: 14
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: PowerManagementCapabilities
          Value: null
          CimType: 20
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: PowerManagementSupported
          Value: null
          CimType: 1
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: StatusInfo
          Value: null
          CimType: 4
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: MappingStrings
              Value:
                - MIF.DMTF|Operational State|003.3
              CimType: 30
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: ValueMap
              Value:
                - '1'
                - '2'
                - '3'
                - '4'
                - '5'
              CimType: 30
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: SystemCreationClassName
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: CIM_Key
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: Propagated
              Value: CIM_System.CreationClassName
              CimType: 14
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: SystemName
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: CIM_Key
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: Propagated
              Value: CIM_System.Name
              CimType: 14
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
      CimClassQualifiers:
        - Name: Locale
          Value: 1033
          CimType: 7
          Flags: EnableOverride, ToSubclass
        - Name: UUID
          Value: '{8502C529-5FBB-11D2-AAC1-006008C78BC7}'
          CimType: 14
          Flags: EnableOverride, ToSubclass
        - Name: Abstract
          Value: true
          CimType: 1
          Flags: EnableOverride, Restricted
      CimClassMethods:
        - Name: SetPowerState
          ReturnType: 6
          Parameters:
            - Name: PowerState
              CimType: 4
              Qualifiers:
                - Name: ID
                  Value: 0
                  CimType: 7
                  Flags: DisableOverride, ToSubclass
                - Name: IN
                  Value: true
                  CimType: 1
                  Flags: DisableOverride, ToSubclass
                - Name: ValueMap
                  Value:
                    - '1'
                    - '2'
                    - '3'
                    - '4'
                    - '5'
                    - '6'
                  CimType: 30
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
            - Name: Time
              CimType: 13
              Qualifiers:
                - Name: ID
                  Value: 1
                  CimType: 7
                  Flags: DisableOverride, ToSubclass
                - Name: IN
                  Value: true
                  CimType: 1
                  Flags: DisableOverride, ToSubclass
              ReferenceClassName: null
          Qualifiers: []
        - Name: Reset
          ReturnType: 6
          Parameters: []
          Qualifiers: []
      CimSystemProperties:
        Namespace: ROOT/cimv2
        ServerName: TANCREDI-L-W11
        ClassName: CIM_LogicalDevice
        Path: null
    CimClassProperties:
      - Name: Caption
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MaxLen
            Value: 64
            CimType: 7
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Description
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: InstallDate
        Value: null
        CimType: 13
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - MIF.DMTF|ComponentID|001.5
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Name
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Status
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MaxLen
            Value: 10
            CimType: 7
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: ValueMap
            Value:
              - OK
              - Error
              - Degraded
              - Unknown
              - Pred Fail
              - Starting
              - Stopping
              - Service
              - Stressed
              - NonRecover
              - No Contact
              - Lost Comm
            CimType: 30
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Availability
        Value: null
        CimType: 4
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - MIF.DMTF|Operational State|003.5
              - MIB.IETF|HOST-RESOURCES-MIB.hrDeviceStatus
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: ValueMap
            Value:
              - '1'
              - '2'
              - '3'
              - '4'
              - '5'
              - '6'
              - '7'
              - '8'
              - '9'
              - '10'
              - '11'
              - '12'
              - '13'
              - '14'
              - '15'
              - '16'
              - '17'
              - '18'
              - '19'
              - '20'
              - '21'
            CimType: 30
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: ConfigManagerErrorCode
        Value: null
        CimType: 6
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: Schema
            Value: Win32
            CimType: 14
            Flags: EnableOverride, ToSubclass
          - Name: ValueMap
            Value:
              - '0'
              - '1'
              - '2'
              - '3'
              - '4'
              - '5'
              - '6'
              - '7'
              - '8'
              - '9'
              - '10'
              - '11'
              - '12'
              - '13'
              - '14'
              - '15'
              - '16'
              - '17'
              - '18'
              - '19'
              - '20'
              - '21'
              - '22'
              - '23'
              - '24'
              - '25'
              - '26'
              - '27'
              - '28'
              - '29'
              - '30'
              - '31'
            CimType: 30
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: ConfigManagerUserConfig
        Value: null
        CimType: 1
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: Schema
            Value: Win32
            CimType: 14
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: CreationClassName
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: CIM_Key
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: DeviceID
        Value: null
        CimType: 14
        Flags: Property, Key, ReadOnly, NullValue
        Qualifiers:
          - Name: CIM_Key
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: key
            Value: true
            CimType: 1
            Flags: DisableOverride, ToSubclass
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: Override
            Value: DeviceId
            CimType: 14
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: ErrorCleared
        Value: null
        CimType: 1
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: ErrorDescription
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: LastErrorCode
        Value: null
        CimType: 6
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: PNPDeviceID
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: Schema
            Value: Win32
            CimType: 14
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: PowerManagementCapabilities
        Value: null
        CimType: 20
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: PowerManagementSupported
        Value: null
        CimType: 1
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: StatusInfo
        Value: null
        CimType: 4
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - MIF.DMTF|Operational State|003.3
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: ValueMap
            Value:
              - '1'
              - '2'
              - '3'
              - '4'
              - '5'
            CimType: 30
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: SystemCreationClassName
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: CIM_Key
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: Propagated
            Value: CIM_System.CreationClassName
            CimType: 14
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: SystemName
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: CIM_Key
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: Propagated
            Value: CIM_System.Name
            CimType: 14
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: ClassGuid
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: CompatibleID
        Value: null
        CimType: 30
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: HardwareID
        Value: null
        CimType: 30
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Manufacturer
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: PNPClass
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Present
        Value: null
        CimType: 1
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Service
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
    CimClassQualifiers:
      - Name: Locale
        Value: 1033
        CimType: 7
        Flags: EnableOverride, ToSubclass
      - Name: UUID
        Value: '{FE28FD98-C875-11d2-B352-00104BC97924}'
        CimType: 14
        Flags: EnableOverride, ToSubclass
      - Name: dynamic
        Value: true
        CimType: 1
        Flags: EnableOverride, ToSubclass
      - Name: provider
        Value: CIMWin32
        CimType: 14
        Flags: EnableOverride, ToSubclass
    CimClassMethods:
      - Name: SetPowerState
        ReturnType: 6
        Parameters:
          - Name: PowerState
            CimType: 4
            Qualifiers:
              - Name: ID
                Value: 0
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: IN
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
              - Name: ValueMap
                Value:
                  - '1'
                  - '2'
                  - '3'
                  - '4'
                  - '5'
                  - '6'
                CimType: 30
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
          - Name: Time
            CimType: 13
            Qualifiers:
              - Name: ID
                Value: 1
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: IN
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
            ReferenceClassName: null
        Qualifiers: []
      - Name: Reset
        ReturnType: 6
        Parameters: []
        Qualifiers: []
      - Name: Enable
        ReturnType: 6
        Parameters:
          - Name: rebootNeeded
            CimType: 1
            Qualifiers:
              - Name: ID
                Value: 0
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: OUT
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
            ReferenceClassName: null
        Qualifiers:
          - Name: Implemented
            Value: true
            CimType: 1
            Flags: EnableOverride, Restricted
      - Name: Disable
        ReturnType: 6
        Parameters:
          - Name: rebootNeeded
            CimType: 1
            Qualifiers:
              - Name: ID
                Value: 0
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: OUT
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
            ReferenceClassName: null
        Qualifiers:
          - Name: Implemented
            Value: true
            CimType: 1
            Flags: EnableOverride, Restricted
      - Name: GetDeviceProperties
        ReturnType: 6
        Parameters:
          - Name: devicePropertyKeys
            CimType: 30
            Qualifiers:
              - Name: ID
                Value: 0
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: IN
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
              - Name: optional
                Value: true
                CimType: 1
                Flags: EnableOverride, Restricted
            ReferenceClassName: null
          - Name: deviceProperties
            CimType: 32
            Qualifiers:
              - Name: EmbeddedInstance
                Value: Win32_PnPDeviceProperty
                CimType: 14
                Flags: EnableOverride, ToSubclass
              - Name: ID
                Value: 1
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: OUT
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
            ReferenceClassName: null
        Qualifiers:
          - Name: Implemented
            Value: true
            CimType: 1
            Flags: EnableOverride, Restricted
    CimSystemProperties:
      Namespace: ROOT/cimv2
      ServerName: TANCREDI-L-W11
      ClassName: Win32_PnPEntity
      Path: null
  CimInstanceProperties:
    - Name: Caption
      Value: Smart Card
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: Description
      Value: Smart Card
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: InstallDate
      Value: null
      CimType: 13
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: Name
      Value: Smart Card
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: Status
      Value: Error
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: Availability
      Value: null
      CimType: 4
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: ConfigManagerErrorCode
      Value: 28
      CimType: 6
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: ConfigManagerUserConfig
      Value: false
      CimType: 1
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: CreationClassName
      Value: Win32_PnPEntity
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: DeviceID
      Value: SCFILTER\CID_8073C021C057597562694B6579\7&2FE4C607&0&YUBICO_YUBIKEY_OTP+FIDO+CCID_0_SCFILTER_CID_8073C021C057597562694B6579
      CimType: 14
      Flags: Property, Key, ReadOnly, NotModified
      IsValueModified: false
    - Name: ErrorCleared
      Value: null
      CimType: 1
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: ErrorDescription
      Value: null
      CimType: 14
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: LastErrorCode
      Value: null
      CimType: 6
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: PNPDeviceID
      Value: SCFILTER\CID_8073C021C057597562694B6579\7&2FE4C607&0&YUBICO_YUBIKEY_OTP+FIDO+CCID_0_SCFILTER_CID_8073C021C057597562694B6579
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: PowerManagementCapabilities
      Value: null
      CimType: 20
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: PowerManagementSupported
      Value: null
      CimType: 1
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: StatusInfo
      Value: null
      CimType: 4
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: SystemCreationClassName
      Value: Win32_ComputerSystem
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: SystemName
      Value: TANCREDI-L-W11
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: ClassGuid
      Value: null
      CimType: 14
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: CompatibleID
      Value:
        - SCFILTER\CID_2777BE07-6993-4513-BD80-C184FCB0AB2D
      CimType: 30
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: HardwareID
      Value:
        - SCFILTER\CID_8073c021c057597562694b6579
      CimType: 30
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: Manufacturer
      Value: null
      CimType: 14
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: PNPClass
      Value: null
      CimType: 14
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: Present
      Value: true
      CimType: 1
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: Service
      Value: null
      CimType: 14
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
  CimSystemProperties:
    Namespace: ROOT/cimv2
    ServerName: TANCREDI-L-W11
    ClassName: Win32_PnPEntity
    Path: null
  Class: null
  FriendlyName: Smart Card
  InstanceId: SCFILTER\CID_8073C021C057597562694B6579\7&2FE4C607&0&YUBICO_YUBIKEY_OTP+FIDO+CCID_0_SCFILTER_CID_8073C021C057597562694B6579
  Problem: 28
  ConfigManagerErrorCode: 28
  ProblemDescription: ''
  Caption: Smart Card
  Description: Smart Card
  InstallDate: null
  Name: Smart Card
  Status: Error
  Availability: null
  ConfigManagerUserConfig: false
  CreationClassName: Win32_PnPEntity
  DeviceID: SCFILTER\CID_8073C021C057597562694B6579\7&2FE4C607&0&YUBICO_YUBIKEY_OTP+FIDO+CCID_0_SCFILTER_CID_8073C021C057597562694B6579
  ErrorCleared: null
  ErrorDescription: null
  LastErrorCode: null
  PNPDeviceID: SCFILTER\CID_8073C021C057597562694B6579\7&2FE4C607&0&YUBICO_YUBIKEY_OTP+FIDO+CCID_0_SCFILTER_CID_8073C021C057597562694B6579
  PowerManagementCapabilities: null
  PowerManagementSupported: null
  StatusInfo: null
  SystemCreationClassName: Win32_ComputerSystem
  SystemName: TANCREDI-L-W11
  ClassGuid: null
  CompatibleID:
    - SCFILTER\CID_2777BE07-6993-4513-BD80-C184FCB0AB2D
  HardwareID:
    - SCFILTER\CID_8073c021c057597562694b6579
  Manufacturer: null
  PNPClass: null
  Present: true
  Service: null
  PSComputerName: null
- CimClass:
    CimSuperClassName: CIM_LogicalDevice
    CimSuperClass:
      CimSuperClassName: CIM_LogicalElement
      CimSuperClass:
        CimSuperClassName: CIM_ManagedSystemElement
        CimSuperClass:
          CimSuperClassName: null
          CimSuperClass: null
          CimClassProperties:
            - Name: Caption
              Value: null
              CimType: 14
              Flags: Property, ReadOnly, NullValue
              Qualifiers:
                - Name: MaxLen
                  Value: 64
                  CimType: 7
                  Flags: EnableOverride, ToSubclass
                - Name: read
                  Value: true
                  CimType: 1
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
            - Name: Description
              Value: null
              CimType: 14
              Flags: Property, ReadOnly, NullValue
              Qualifiers:
                - Name: read
                  Value: true
                  CimType: 1
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
            - Name: InstallDate
              Value: null
              CimType: 13
              Flags: Property, ReadOnly, NullValue
              Qualifiers:
                - Name: MappingStrings
                  Value:
                    - MIF.DMTF|ComponentID|001.5
                  CimType: 30
                  Flags: EnableOverride, ToSubclass
                - Name: read
                  Value: true
                  CimType: 1
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
            - Name: Name
              Value: null
              CimType: 14
              Flags: Property, ReadOnly, NullValue
              Qualifiers:
                - Name: read
                  Value: true
                  CimType: 1
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
            - Name: Status
              Value: null
              CimType: 14
              Flags: Property, ReadOnly, NullValue
              Qualifiers:
                - Name: MaxLen
                  Value: 10
                  CimType: 7
                  Flags: EnableOverride, ToSubclass
                - Name: read
                  Value: true
                  CimType: 1
                  Flags: EnableOverride, ToSubclass
                - Name: ValueMap
                  Value:
                    - OK
                    - Error
                    - Degraded
                    - Unknown
                    - Pred Fail
                    - Starting
                    - Stopping
                    - Service
                    - Stressed
                    - NonRecover
                    - No Contact
                    - Lost Comm
                  CimType: 30
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
          CimClassQualifiers:
            - Name: Abstract
              Value: true
              CimType: 1
              Flags: EnableOverride, Restricted
            - Name: Locale
              Value: 1033
              CimType: 7
              Flags: EnableOverride, ToSubclass
            - Name: UUID
              Value: '{8502C517-5FBB-11D2-AAC1-006008C78BC7}'
              CimType: 14
              Flags: EnableOverride, ToSubclass
          CimClassMethods: []
          CimSystemProperties:
            Namespace: ROOT/cimv2
            ServerName: TANCREDI-L-W11
            ClassName: CIM_ManagedSystemElement
            Path: null
        CimClassProperties:
          - Name: Caption
            Value: null
            CimType: 14
            Flags: Property, ReadOnly, NullValue
            Qualifiers:
              - Name: MaxLen
                Value: 64
                CimType: 7
                Flags: EnableOverride, ToSubclass
              - Name: read
                Value: true
                CimType: 1
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
          - Name: Description
            Value: null
            CimType: 14
            Flags: Property, ReadOnly, NullValue
            Qualifiers:
              - Name: read
                Value: true
                CimType: 1
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
          - Name: InstallDate
            Value: null
            CimType: 13
            Flags: Property, ReadOnly, NullValue
            Qualifiers:
              - Name: MappingStrings
                Value:
                  - MIF.DMTF|ComponentID|001.5
                CimType: 30
                Flags: EnableOverride, ToSubclass
              - Name: read
                Value: true
                CimType: 1
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
          - Name: Name
            Value: null
            CimType: 14
            Flags: Property, ReadOnly, NullValue
            Qualifiers:
              - Name: read
                Value: true
                CimType: 1
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
          - Name: Status
            Value: null
            CimType: 14
            Flags: Property, ReadOnly, NullValue
            Qualifiers:
              - Name: MaxLen
                Value: 10
                CimType: 7
                Flags: EnableOverride, ToSubclass
              - Name: read
                Value: true
                CimType: 1
                Flags: EnableOverride, ToSubclass
              - Name: ValueMap
                Value:
                  - OK
                  - Error
                  - Degraded
                  - Unknown
                  - Pred Fail
                  - Starting
                  - Stopping
                  - Service
                  - Stressed
                  - NonRecover
                  - No Contact
                  - Lost Comm
                CimType: 30
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
        CimClassQualifiers:
          - Name: Locale
            Value: 1033
            CimType: 7
            Flags: EnableOverride, ToSubclass
          - Name: UUID
            Value: '{8502C518-5FBB-11D2-AAC1-006008C78BC7}'
            CimType: 14
            Flags: EnableOverride, ToSubclass
          - Name: Abstract
            Value: true
            CimType: 1
            Flags: EnableOverride, Restricted
        CimClassMethods: []
        CimSystemProperties:
          Namespace: ROOT/cimv2
          ServerName: TANCREDI-L-W11
          ClassName: CIM_LogicalElement
          Path: null
      CimClassProperties:
        - Name: Caption
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: MaxLen
              Value: 64
              CimType: 7
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: Description
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: InstallDate
          Value: null
          CimType: 13
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: MappingStrings
              Value:
                - MIF.DMTF|ComponentID|001.5
              CimType: 30
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: Name
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: Status
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: MaxLen
              Value: 10
              CimType: 7
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: ValueMap
              Value:
                - OK
                - Error
                - Degraded
                - Unknown
                - Pred Fail
                - Starting
                - Stopping
                - Service
                - Stressed
                - NonRecover
                - No Contact
                - Lost Comm
              CimType: 30
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: Availability
          Value: null
          CimType: 4
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: MappingStrings
              Value:
                - MIF.DMTF|Operational State|003.5
                - MIB.IETF|HOST-RESOURCES-MIB.hrDeviceStatus
              CimType: 30
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: ValueMap
              Value:
                - '1'
                - '2'
                - '3'
                - '4'
                - '5'
                - '6'
                - '7'
                - '8'
                - '9'
                - '10'
                - '11'
                - '12'
                - '13'
                - '14'
                - '15'
                - '16'
                - '17'
                - '18'
                - '19'
                - '20'
                - '21'
              CimType: 30
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: ConfigManagerErrorCode
          Value: null
          CimType: 6
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: Schema
              Value: Win32
              CimType: 14
              Flags: EnableOverride, ToSubclass
            - Name: ValueMap
              Value:
                - '0'
                - '1'
                - '2'
                - '3'
                - '4'
                - '5'
                - '6'
                - '7'
                - '8'
                - '9'
                - '10'
                - '11'
                - '12'
                - '13'
                - '14'
                - '15'
                - '16'
                - '17'
                - '18'
                - '19'
                - '20'
                - '21'
                - '22'
                - '23'
                - '24'
                - '25'
                - '26'
                - '27'
                - '28'
                - '29'
                - '30'
                - '31'
              CimType: 30
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: ConfigManagerUserConfig
          Value: null
          CimType: 1
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: Schema
              Value: Win32
              CimType: 14
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: CreationClassName
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: CIM_Key
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: DeviceID
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: CIM_Key
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: ErrorCleared
          Value: null
          CimType: 1
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: ErrorDescription
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: LastErrorCode
          Value: null
          CimType: 6
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: PNPDeviceID
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: Schema
              Value: Win32
              CimType: 14
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: PowerManagementCapabilities
          Value: null
          CimType: 20
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: PowerManagementSupported
          Value: null
          CimType: 1
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: StatusInfo
          Value: null
          CimType: 4
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: MappingStrings
              Value:
                - MIF.DMTF|Operational State|003.3
              CimType: 30
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: ValueMap
              Value:
                - '1'
                - '2'
                - '3'
                - '4'
                - '5'
              CimType: 30
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: SystemCreationClassName
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: CIM_Key
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: Propagated
              Value: CIM_System.CreationClassName
              CimType: 14
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
        - Name: SystemName
          Value: null
          CimType: 14
          Flags: Property, ReadOnly, NullValue
          Qualifiers:
            - Name: CIM_Key
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
            - Name: Propagated
              Value: CIM_System.Name
              CimType: 14
              Flags: EnableOverride, ToSubclass
            - Name: read
              Value: true
              CimType: 1
              Flags: EnableOverride, ToSubclass
          ReferenceClassName: null
      CimClassQualifiers:
        - Name: Locale
          Value: 1033
          CimType: 7
          Flags: EnableOverride, ToSubclass
        - Name: UUID
          Value: '{8502C529-5FBB-11D2-AAC1-006008C78BC7}'
          CimType: 14
          Flags: EnableOverride, ToSubclass
        - Name: Abstract
          Value: true
          CimType: 1
          Flags: EnableOverride, Restricted
      CimClassMethods:
        - Name: SetPowerState
          ReturnType: 6
          Parameters:
            - Name: PowerState
              CimType: 4
              Qualifiers:
                - Name: ID
                  Value: 0
                  CimType: 7
                  Flags: DisableOverride, ToSubclass
                - Name: IN
                  Value: true
                  CimType: 1
                  Flags: DisableOverride, ToSubclass
                - Name: ValueMap
                  Value:
                    - '1'
                    - '2'
                    - '3'
                    - '4'
                    - '5'
                    - '6'
                  CimType: 30
                  Flags: EnableOverride, ToSubclass
              ReferenceClassName: null
            - Name: Time
              CimType: 13
              Qualifiers:
                - Name: ID
                  Value: 1
                  CimType: 7
                  Flags: DisableOverride, ToSubclass
                - Name: IN
                  Value: true
                  CimType: 1
                  Flags: DisableOverride, ToSubclass
              ReferenceClassName: null
          Qualifiers: []
        - Name: Reset
          ReturnType: 6
          Parameters: []
          Qualifiers: []
      CimSystemProperties:
        Namespace: ROOT/cimv2
        ServerName: TANCREDI-L-W11
        ClassName: CIM_LogicalDevice
        Path: null
    CimClassProperties:
      - Name: Caption
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MaxLen
            Value: 64
            CimType: 7
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Description
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: InstallDate
        Value: null
        CimType: 13
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - MIF.DMTF|ComponentID|001.5
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Name
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Status
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MaxLen
            Value: 10
            CimType: 7
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: ValueMap
            Value:
              - OK
              - Error
              - Degraded
              - Unknown
              - Pred Fail
              - Starting
              - Stopping
              - Service
              - Stressed
              - NonRecover
              - No Contact
              - Lost Comm
            CimType: 30
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Availability
        Value: null
        CimType: 4
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - MIF.DMTF|Operational State|003.5
              - MIB.IETF|HOST-RESOURCES-MIB.hrDeviceStatus
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: ValueMap
            Value:
              - '1'
              - '2'
              - '3'
              - '4'
              - '5'
              - '6'
              - '7'
              - '8'
              - '9'
              - '10'
              - '11'
              - '12'
              - '13'
              - '14'
              - '15'
              - '16'
              - '17'
              - '18'
              - '19'
              - '20'
              - '21'
            CimType: 30
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: ConfigManagerErrorCode
        Value: null
        CimType: 6
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: Schema
            Value: Win32
            CimType: 14
            Flags: EnableOverride, ToSubclass
          - Name: ValueMap
            Value:
              - '0'
              - '1'
              - '2'
              - '3'
              - '4'
              - '5'
              - '6'
              - '7'
              - '8'
              - '9'
              - '10'
              - '11'
              - '12'
              - '13'
              - '14'
              - '15'
              - '16'
              - '17'
              - '18'
              - '19'
              - '20'
              - '21'
              - '22'
              - '23'
              - '24'
              - '25'
              - '26'
              - '27'
              - '28'
              - '29'
              - '30'
              - '31'
            CimType: 30
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: ConfigManagerUserConfig
        Value: null
        CimType: 1
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: Schema
            Value: Win32
            CimType: 14
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: CreationClassName
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: CIM_Key
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: DeviceID
        Value: null
        CimType: 14
        Flags: Property, Key, ReadOnly, NullValue
        Qualifiers:
          - Name: CIM_Key
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: key
            Value: true
            CimType: 1
            Flags: DisableOverride, ToSubclass
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: Override
            Value: DeviceId
            CimType: 14
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: ErrorCleared
        Value: null
        CimType: 1
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: ErrorDescription
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: LastErrorCode
        Value: null
        CimType: 6
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: PNPDeviceID
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: Schema
            Value: Win32
            CimType: 14
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: PowerManagementCapabilities
        Value: null
        CimType: 20
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: PowerManagementSupported
        Value: null
        CimType: 1
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: StatusInfo
        Value: null
        CimType: 4
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - MIF.DMTF|Operational State|003.3
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: ValueMap
            Value:
              - '1'
              - '2'
              - '3'
              - '4'
              - '5'
            CimType: 30
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: SystemCreationClassName
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: CIM_Key
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: Propagated
            Value: CIM_System.CreationClassName
            CimType: 14
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: SystemName
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: CIM_Key
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
          - Name: Propagated
            Value: CIM_System.Name
            CimType: 14
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: ClassGuid
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: CompatibleID
        Value: null
        CimType: 30
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: HardwareID
        Value: null
        CimType: 30
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Manufacturer
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: PNPClass
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Present
        Value: null
        CimType: 1
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
      - Name: Service
        Value: null
        CimType: 14
        Flags: Property, ReadOnly, NullValue
        Qualifiers:
          - Name: MappingStrings
            Value:
              - WMI
            CimType: 30
            Flags: EnableOverride, ToSubclass
          - Name: read
            Value: true
            CimType: 1
            Flags: EnableOverride, ToSubclass
        ReferenceClassName: null
    CimClassQualifiers:
      - Name: Locale
        Value: 1033
        CimType: 7
        Flags: EnableOverride, ToSubclass
      - Name: UUID
        Value: '{FE28FD98-C875-11d2-B352-00104BC97924}'
        CimType: 14
        Flags: EnableOverride, ToSubclass
      - Name: dynamic
        Value: true
        CimType: 1
        Flags: EnableOverride, ToSubclass
      - Name: provider
        Value: CIMWin32
        CimType: 14
        Flags: EnableOverride, ToSubclass
    CimClassMethods:
      - Name: SetPowerState
        ReturnType: 6
        Parameters:
          - Name: PowerState
            CimType: 4
            Qualifiers:
              - Name: ID
                Value: 0
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: IN
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
              - Name: ValueMap
                Value:
                  - '1'
                  - '2'
                  - '3'
                  - '4'
                  - '5'
                  - '6'
                CimType: 30
                Flags: EnableOverride, ToSubclass
            ReferenceClassName: null
          - Name: Time
            CimType: 13
            Qualifiers:
              - Name: ID
                Value: 1
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: IN
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
            ReferenceClassName: null
        Qualifiers: []
      - Name: Reset
        ReturnType: 6
        Parameters: []
        Qualifiers: []
      - Name: Enable
        ReturnType: 6
        Parameters:
          - Name: rebootNeeded
            CimType: 1
            Qualifiers:
              - Name: ID
                Value: 0
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: OUT
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
            ReferenceClassName: null
        Qualifiers:
          - Name: Implemented
            Value: true
            CimType: 1
            Flags: EnableOverride, Restricted
      - Name: Disable
        ReturnType: 6
        Parameters:
          - Name: rebootNeeded
            CimType: 1
            Qualifiers:
              - Name: ID
                Value: 0
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: OUT
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
            ReferenceClassName: null
        Qualifiers:
          - Name: Implemented
            Value: true
            CimType: 1
            Flags: EnableOverride, Restricted
      - Name: GetDeviceProperties
        ReturnType: 6
        Parameters:
          - Name: devicePropertyKeys
            CimType: 30
            Qualifiers:
              - Name: ID
                Value: 0
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: IN
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
              - Name: optional
                Value: true
                CimType: 1
                Flags: EnableOverride, Restricted
            ReferenceClassName: null
          - Name: deviceProperties
            CimType: 32
            Qualifiers:
              - Name: EmbeddedInstance
                Value: Win32_PnPDeviceProperty
                CimType: 14
                Flags: EnableOverride, ToSubclass
              - Name: ID
                Value: 1
                CimType: 7
                Flags: DisableOverride, ToSubclass
              - Name: OUT
                Value: true
                CimType: 1
                Flags: DisableOverride, ToSubclass
            ReferenceClassName: null
        Qualifiers:
          - Name: Implemented
            Value: true
            CimType: 1
            Flags: EnableOverride, Restricted
    CimSystemProperties:
      Namespace: ROOT/cimv2
      ServerName: TANCREDI-L-W11
      ClassName: Win32_PnPEntity
      Path: null
  CimInstanceProperties:
    - Name: Caption
      Value: Yubico YubiKey OTP+FIDO+CCID 0
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: Description
      Value: Smart Card Device Information Node
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: InstallDate
      Value: null
      CimType: 13
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: Name
      Value: Yubico YubiKey OTP+FIDO+CCID 0
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: Status
      Value: Error
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: Availability
      Value: null
      CimType: 4
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: ConfigManagerErrorCode
      Value: 28
      CimType: 6
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: ConfigManagerUserConfig
      Value: false
      CimType: 1
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: CreationClassName
      Value: Win32_PnPEntity
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: DeviceID
      Value: SWD\SCDEVICEENUM\1_YUBICO_YUBIKEY_OTP+FIDO+CCID_0
      CimType: 14
      Flags: Property, Key, ReadOnly, NotModified
      IsValueModified: false
    - Name: ErrorCleared
      Value: null
      CimType: 1
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: ErrorDescription
      Value: null
      CimType: 14
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: LastErrorCode
      Value: null
      CimType: 6
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: PNPDeviceID
      Value: SWD\SCDEVICEENUM\1_YUBICO_YUBIKEY_OTP+FIDO+CCID_0
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: PowerManagementCapabilities
      Value: null
      CimType: 20
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: PowerManagementSupported
      Value: null
      CimType: 1
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: StatusInfo
      Value: null
      CimType: 4
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: SystemCreationClassName
      Value: Win32_ComputerSystem
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: SystemName
      Value: TANCREDI-L-W11
      CimType: 14
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: ClassGuid
      Value: null
      CimType: 14
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: CompatibleID
      Value:
        - SWD\GenericRaw
        - SWD\Generic
      CimType: 30
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: HardwareID
      Value:
        - ScDeviceInformationNode\node
      CimType: 30
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: Manufacturer
      Value: null
      CimType: 14
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: PNPClass
      Value: null
      CimType: 14
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
    - Name: Present
      Value: true
      CimType: 1
      Flags: Property, ReadOnly, NotModified
      IsValueModified: false
    - Name: Service
      Value: null
      CimType: 14
      Flags: Property, ReadOnly, NotModified, NullValue
      IsValueModified: false
  CimSystemProperties:
    Namespace: ROOT/cimv2
    ServerName: TANCREDI-L-W11
    ClassName: Win32_PnPEntity
    Path: null
  Class: null
  FriendlyName: Yubico YubiKey OTP+FIDO+CCID 0
  InstanceId: SWD\SCDEVICEENUM\1_YUBICO_YUBIKEY_OTP+FIDO+CCID_0
  Problem: 28
  ConfigManagerErrorCode: 28
  ProblemDescription: ''
  Caption: Yubico YubiKey OTP+FIDO+CCID 0
  Description: Smart Card Device Information Node
  InstallDate: null
  Name: Yubico YubiKey OTP+FIDO+CCID 0
  Status: Error
  Availability: null
  ConfigManagerUserConfig: false
  CreationClassName: Win32_PnPEntity
  DeviceID: SWD\SCDEVICEENUM\1_YUBICO_YUBIKEY_OTP+FIDO+CCID_0
  ErrorCleared: null
  ErrorDescription: null
  LastErrorCode: null
  PNPDeviceID: SWD\SCDEVICEENUM\1_YUBICO_YUBIKEY_OTP+FIDO+CCID_0
  PowerManagementCapabilities: null
  PowerManagementSupported: null
  StatusInfo: null
  SystemCreationClassName: Win32_ComputerSystem
  SystemName: TANCREDI-L-W11
  ClassGuid: null
  CompatibleID:
    - SWD\GenericRaw
    - SWD\Generic
  HardwareID:
    - ScDeviceInformationNode\node
  Manufacturer: null
  PNPClass: null
  Present: true
  Service: null
  PSComputerName: null
{% endhighlight %}
</details>


---

```sh
# Get all device properties as json
$ powershell.exe -Command "Get-PnpDevice | Where-Object { \$_.InstanceId -like '*YUBICO*' } | ForEach-Object { \$device=\$_; \$props=Get-PnpDeviceProperty -InstanceId \$device.InstanceId -ErrorAction SilentlyContinue; \$allProps=@{}; \$allProps['FriendlyName']=\$device.FriendlyName; \$allProps['InstanceId']=\$device.InstanceId; \$allProps['Class']=\$device.Class; \$allProps['Status']=\$device.Status; foreach (\$p in \$props) { if (\$p.Data -is [Array]) { \$allProps[\$p.KeyName]=\$p.Data -join ', ' } else { \$allProps[\$p.KeyName]=\$p.Data } }; [PSCustomObject]\$allProps } | ConvertTo-Json -Depth 99" | yq -yr
```
<details>
  <summary>Click to expand YAML output</summary>
{% highlight txt %}
- DEVPKEY_Device_BaseContainerId: "{A814DAAC-3F96-11F0-B890-A0B33979102C}"
  DEVPKEY_Device_InstallDate: "/Date(1748951221946)/"
  Class: 
  DEVPKEY_Device_CompatibleIds: SCFILTER\CID_2777BE07-6993-4513-BD80-C184FCB0AB2D
  "{83DA6326-97A6-4088-9453-A1923F573B29} 5": 3758096968
  DEVPKEY_NAME: Smart Card
  "{80497100-8C73-48B9-AAD9-CE387E19C56E} 6": 0
  DEVPKEY_Device_SafeRemovalRequired: false
  DEVPKEY_Device_HardwareIds: SCFILTER\CID_8073c021c057597562694b6579
  DEVPKEY_Device_InstanceId: SCFILTER\CID_8073C021C057597562694B6579\7&2FE4C607&0&YUBICO_YUBIKEY_OTP+FIDO+CCID_0_SCFILTER_CID_8073C021C057597562694B6579
  DEVPKEY_Device_ProblemCode: 28
  DEVPKEY_Device_Stack: "\\Driver\\scfilter"
  DEVPKEY_Device_Parent: USB\VID_1050&PID_0407&MI_02\6&effba79&0&0002
  "{3464F7A4-2444-40B1-980A-E0903CB6D912} 10": 3
  "{83DA6326-97A6-4088-9453-A1923F573B29} 15": true
  DEVPKEY_Device_LocationInfo: ScFilter
  DEVPKEY_Device_RemovalPolicyDefault: 3
  DEVPKEY_Device_PowerData: 56, 0, 0, 0, 4, 0, 0, 0, 25, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0,
    4, 0, 0, 0, 4, 0, 0, 0, 1, 0, 0, 0
  DEVPKEY_Device_InstallState: 2
  DEVPKEY_Device_DevNodeStatus: 25191424
  DEVPKEY_Device_EnumeratorName: SCFILTER
  DEVPKEY_Device_InLocalMachineContainer: false
  FriendlyName: Smart Card
  DEVPKEY_Device_Siblings: "{892EDE5E-BE49-443c-A0B3-005D74F2D69C}\\ScFilter\\7&2fe4c607&0&04"
  DEVPKEY_Device_DeviceDesc: Smart Card
  DEVPKEY_Device_HasProblem: true
  DEVPKEY_Device_IsRebootRequired: false
  Status: Error
  DEVPKEY_Device_LastArrivalDate: "/Date(1748952255288)/"
  DEVPKEY_Device_ReportedDeviceIdsHash: 3328081164
  DEVPKEY_Device_FirstInstallDate: "/Date(1748950570277)/"
  InstanceId: SCFILTER\CID_8073C021C057597562694B6579\7&2FE4C607&0&YUBICO_YUBIKEY_OTP+FIDO+CCID_0_SCFILTER_CID_8073C021C057597562694B6579
  DEVPKEY_Device_Capabilities: 132
  DEVPKEY_Device_ProblemStatus: 0
  DEVPKEY_Device_PDOName: "\\Device\\0000012d"
  "{A8B865DD-2E3D-4094-AD97-E593A70C75D6} 26": false
  DEVPKEY_Device_RemovalPolicy: 3
  DEVPKEY_Device_ContainerId: "{A814DAAC-3F96-11F0-B890-A0B33979102C}"
  "{83DA6326-97A6-4088-9453-A1923F573B29} 10": USB\VID_1050&PID_0407&MI_02\6&effba79&0&0002
  DEVPKEY_Device_IsPresent: true
  DEVPKEY_Device_ConfigFlags: 64
  DEVPKEY_Device_BusReportedDeviceDesc: Smart Card
- DEVPKEY_Device_BaseContainerId: "{00000000-0000-0000-FFFF-FFFFFFFFFFFF}"
  DEVPKEY_Device_LastArrivalDate: "/Date(1748952254736)/"
  DEVPKEY_Device_SessionId: 1
  Class: 
  DEVPKEY_Device_CompatibleIds: SWD\GenericRaw, SWD\Generic
  DEVPKEY_Device_CreatorProcessId: 16748
  DEVPKEY_NAME: Yubico YubiKey OTP+FIDO+CCID 0
  "{80497100-8C73-48B9-AAD9-CE387E19C56E} 6": 0
  DEVPKEY_Device_SafeRemovalRequired: false
  DEVPKEY_Device_HardwareIds: ScDeviceInformationNode\node
  DEVPKEY_Device_InstanceId: SWD\SCDEVICEENUM\1_YUBICO_YUBIKEY_OTP+FIDO+CCID_0
  DEVPKEY_Device_ProblemCode: 28
  "{83DA6326-97A6-4088-9453-A1923F573B29} 10": SWD\ScDeviceEnumBus\0
  DEVPKEY_Device_Stack: "\\Driver\\SoftwareDevice"
  DEVPKEY_Device_Parent: SWD\ScDeviceEnumBus\0
  "{3464F7A4-2444-40B1-980A-E0903CB6D912} 10": 3
  "{83DA6326-97A6-4088-9453-A1923F573B29} 15": true
  DEVPKEY_Device_RemovalPolicyDefault: 1
  DEVPKEY_Device_PowerData: 56, 0, 0, 0, 1, 0, 0, 0, 9, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0,
    4, 0, 0, 0, 4, 0, 0, 0, 0, 0, 0, 0
  DEVPKEY_Device_InstallState: 2
  DEVPKEY_Device_DevNodeStatus: 1098916874
  DEVPKEY_Device_EnumeratorName: SWD
  DEVPKEY_Device_FriendlyName: Yubico YubiKey OTP+FIDO+CCID 0
  DEVPKEY_Device_InLocalMachineContainer: true
  FriendlyName: Yubico YubiKey OTP+FIDO+CCID 0
  DEVPKEY_Device_Siblings: SWD\ScDeviceEnum\1_Windows_Hello_for_Business_1, SWD\ScDeviceEnum\1_Alcorlink_USB_Smart_Card_Reader_0
  DEVPKEY_Device_DeviceDesc: Smart Card Device Information Node
  DEVPKEY_Device_HasProblem: false
  DEVPKEY_Device_IsRebootRequired: false
  Status: Error
  DEVPKEY_Device_LegacyBusType: 15
  DEVPKEY_Device_BusTypeGuid: "{06D10322-7DE0-4CEF-8E25-197D0E7442E2}"
  DEVPKEY_Device_ReportedDeviceIdsHash: 40132414
  InstanceId: SWD\SCDEVICEENUM\1_YUBICO_YUBIKEY_OTP+FIDO+CCID_0
  DEVPKEY_Device_Capabilities: 240
  DEVPKEY_Device_ProblemStatus: 0
  DEVPKEY_Device_PDOName: "\\Device\\0000012c"
  "{A8B865DD-2E3D-4094-AD97-E593A70C75D6} 26": false
  DEVPKEY_Device_RemovalPolicy: 1
  DEVPKEY_Device_ContainerId: "{00000000-0000-0000-FFFF-FFFFFFFFFFFF}"
  DEVPKEY_Device_BusNumber: 0
  DEVPKEY_Device_IsPresent: true
  DEVPKEY_Device_ConfigFlags: 64
  DEVPKEY_Device_BusReportedDeviceDesc: Smart Card Device Information Node
{% endhighlight %}
</details>

# Installing the drivers
Under Windows 11 the device might show up in `Device Manager` at `Other devices`
-> `Smart Card`, having a warning/exclamation mark, because the driver is not
installed. You can just right-click it and select `Update driver`, then select
`Search automatically for drivers`. Windows should automatically detect the
correct driver, and install it. Once installed, the device will be moved into
the `Smart cards` section, and it might be named: `Identity Device (NIST SP
800-73 [PIV])`.

Once the Driver is installed, this patch will be applied to the
yaml above:


<details>
  <summary>Click to expand diff output</summary>
{% highlight diff %}
1198c1198
<       Value: Smart Card
---
>       Value: Identity Device (NIST SP 800-73 [PIV])
1203c1203
<       Value: Smart Card
---
>       Value: Identity Device (NIST SP 800-73 [PIV])
1213c1213
<       Value: Smart Card
---
>       Value: Identity Device (NIST SP 800-73 [PIV])
1218c1218
<       Value: Error
---
>       Value: OK
1228c1228
<       Value: 28
---
>       Value: 0
1293c1293
<       Value: null
---
>       Value: '{990a2bd7-e738-46c7-b26f-1cf8fb9f1391}'
1295c1295
<       Flags: Property, ReadOnly, NotModified, NullValue
---
>       Flags: Property, ReadOnly, NotModified
1310c1310
<       Value: null
---
>       Value: Microsoft
1312c1312
<       Flags: Property, ReadOnly, NotModified, NullValue
---
>       Flags: Property, ReadOnly, NotModified
1315c1315
<       Value: null
---
>       Value: SmartCard
1317c1317
<       Flags: Property, ReadOnly, NotModified, NullValue
---
>       Flags: Property, ReadOnly, NotModified
1325c1325
<       Value: null
---
>       Value: UmPass
1327c1327
<       Flags: Property, ReadOnly, NotModified, NullValue
---
>       Flags: Property, ReadOnly, NotModified
1334,1335c1334,1335
<   Class: null
<   FriendlyName: Smart Card
---
>   Class: SmartCard
>   FriendlyName: Identity Device (NIST SP 800-73 [PIV])
1337,1341c1337,1341
<   Problem: 28
<   ConfigManagerErrorCode: 28
<   ProblemDescription: ''
<   Caption: Smart Card
<   Description: Smart Card
---
>   Problem: 0
>   ConfigManagerErrorCode: 0
>   ProblemDescription: null
>   Caption: Identity Device (NIST SP 800-73 [PIV])
>   Description: Identity Device (NIST SP 800-73 [PIV])
1343,1344c1343,1344
<   Name: Smart Card
<   Status: Error
---
>   Name: Identity Device (NIST SP 800-73 [PIV])
>   Status: OK
1358c1358
<   ClassGuid: null
---
>   ClassGuid: '{990a2bd7-e738-46c7-b26f-1cf8fb9f1391}'
1363,1364c1363,1364
<   Manufacturer: null
<   PNPClass: null
---
>   Manufacturer: Microsoft
>   PNPClass: SmartCard
1366c1366
<   Service: null
---
>   Service: UmPass
{% endhighlight %}
</details>

## Sharing it with WSL
The USB device needs to be shared with the VM where the WSL kernel is running.
To do that, we will use this project: https://github.com/dorssel/usbipd-win .
```bat
:: Download
curl "https://github.com/dorssel/usbipd-win/releases/download/v5.1.0/usbipd-win_5.1.0_x64.msi" -o D:\data\programs\usbipd-win_5.1.0_x64.msi
:: Start powershell as admin:
:: Elevate through password
:: runas /user:Administrator "powershell.exe"
:: OR,Elevate through UAC(User Account Control), just saying yes
powershell -Command "Start-Process -Verb RunAs powershell"
```
Then, in the elevated powershell, run:
```powershell
# Quiet (non-interactive) install
PS C:\WINDOWS\system32> powershell -Command "Start-Process msiexec.exe -ArgumentList '/i \"D:\data\programs\usbipd-win_5.1.0_x64.msi\" /qn' -Verb RunAs"
# OR, UI / guided installer
# powershell -Command "Start-Process msiexec.exe -ArgumentList '/i \"D:\data\programs\usbipd-win_5.1.0_x64.msi\"' -Verb RunAs"

# Then list the USB devices:
PS C:\WINDOWS\system32> usbipd list
Connected:
BUSID  VID:PID    DEVICE                                                        STATE
2-7    1050:0407  USB Input Device, Microsoft Usbccid Smartcard Reader (WUDF)   Not shared

# Share it (this will change State to Shared)
PS C:\WINDOWS\system32> usbipd bind --busid 2-7
# Later you can unbind it using:
# PS C:\WINDOWS\system32> usbipd unbind --busid 2-7

# Then just attach it to the WSL 2. This will make it unavailable for the
# Windows operating system.
PS C:\WINDOWS\system32> usbipd attach --busid 2-7 --wsl
usbipd: info: Using WSL distribution 'Ubuntu-24.04' to attach; the device will be available in all WSL 2 distributions.
usbipd: info: Detected networking mode 'nat'.
usbipd: info: Using IP address 172.21.176.1 to reach the host.
# Later you can detach it using:
# PS C:\WINDOWS\system32> usbipd detach --busid 2-7

# Exit elevated session
PS C:\WINDOWS\system32> exit
```
But now, you can see it in GNU/Linux:
```sh
$ lsusb
Bus 001 Device 005: ID 1050:0407 Yubico.com Yubikey 4/5 OTP+U2F+CCID

$ usb-devices
T:  Bus=01 Lev=01 Prnt=01 Port=00 Cnt=01 Dev#=  5 Spd=12   MxCh= 0
D:  Ver= 2.00 Cls=00(>ifc ) Sub=00 Prot=00 MxPS=64 #Cfgs=  1
P:  Vendor=1050 ProdID=0407 Rev=05.43
S:  Manufacturer=Yubico
S:  Product=YubiKey OTP+FIDO+CCID
C:  #Ifs= 3 Cfg#= 1 Atr=80 MxPwr=30mA
I:  If#= 0 Alt= 0 #EPs= 1 Cls=03(HID  ) Sub=01 Prot=01 Driver=usbhid
E:  Ad=81(I) Atr=03(Int.) MxPS=   8 Ivl=10ms
I:  If#= 1 Alt= 0 #EPs= 2 Cls=03(HID  ) Sub=00 Prot=00 Driver=usbhid
E:  Ad=04(O) Atr=03(Int.) MxPS=  64 Ivl=2ms
E:  Ad=84(I) Atr=03(Int.) MxPS=  64 Ivl=2ms
I:  If#= 2 Alt= 0 #EPs= 3 Cls=0b(scard) Sub=00 Prot=00 Driver=(none)
E:  Ad=02(O) Atr=02(Bulk) MxPS=  64 Ivl=0ms
E:  Ad=82(I) Atr=02(Bulk) MxPS=  64 Ivl=0ms
E:  Ad=83(I) Atr=03(Int.) MxPS=   8 Ivl=32ms
```

## Installing GNU/Linux software
Will use the [`yubikey-manager`](
  https://developers.yubico.com/yubikey-manager/), as I am not currently
interested in the GUI [Yubico Authenticator](
  https://developers.yubico.com/yubioath-flutter/). You can consult the [`ykman`
manual page](
  https://docs.yubico.com/software/yubikey/tools/ykman/Base_Commands.html) for
details on how to use the tool. For Android, there is a Yubico Authenticator
[build](https://play.google.com/store/apps/details?id=com.yubico.yubioath).
```sh
# Install PC/SC Smart Card Daemon, YubiKey manager, and other tools
$ sudo apt install -y libusb-1.0-0 pcscd scdaemon gnupg2 yubikey-manager \
  pcsc-tools libfido2-1 libfido2-dev libu2f-udev fido2-tools
$ sudo usermod -aG plugdev ${USER} # Restart WSL

$ ( cat - <<EOF | sudo tee /etc/udev/rules.d/70-u2f.rules
# YubiKey and other FIDO2 devices
KERNEL=="hidraw*", SUBSYSTEM=="hidraw", ATTRS{idVendor}=="1050", MODE="0660", GROUP="plugdev"
KERNEL=="hidraw*", SUBSYSTEM=="hidraw", ATTRS{idVendor}=="20a0", MODE="0660", GROUP="plugdev"
EOF
)
$ sudo udevadm control --reload-rules
$ sudo udevadm trigger
# Unplug-replug YubiKey USB device

# Now fido2-token -L should work without sudo
$ fido2-token -L
/dev/hidraw1: vendor=0x1050, product=0x0407 (Yubico YubiKey OTP+FIDO+CCID)

# Start daemon
$ sudo service pcscd start

# Use pcsc-tools package to scan for your device
$ sudo pcsc_scan
PC/SC device scanner
V 1.7.1 (c) 2001-2022, Ludovic Rousseau <ludovic.rousseau@free.fr>
Using reader plug'n play mechanism
Scanning present readers...
0: Yubico YubiKey OTP+FIDO+CCID 00 00

Thu Jun  5 08:59:59 2025
 Reader 0: Yubico YubiKey OTP+FIDO+CCID 00 00
  Event number: 0
  Card state: Card inserted,
  ATR: 3B FD 13 00 00 81 31 FE 15 80 73 C0 21 C0 57 59 75 62 69 4B 65 79 40

ATR: 3B FD 13 00 00 81 31 FE 15 80 73 C0 21 C0 57 59 75 62 69 4B 65 79 40
+ TS = 3B --> Direct Convention
+ T0 = FD, Y(1): 1111, K: 13 (historical bytes)
  TA(1) = 13 --> Fi=372, Di=4, 93 cycles/ETU
    43010 bits/s at 4 MHz, fMax for Fi = 5 MHz => 53763 bits/s
  TB(1) = 00 --> VPP is not electrically connected
  TC(1) = 00 --> Extra guard time: 0
  TD(1) = 81 --> Y(i+1) = 1000, Protocol T = 1
-----
  TD(2) = 31 --> Y(i+1) = 0011, Protocol T = 1
-----
  TA(3) = FE --> IFSC: 254
  TB(3) = 15 --> Block Waiting Integer: 1 - Character Waiting Integer: 5
+ Historical bytes: 80 73 C0 21 C0 57 59 75 62 69 4B 65 79
  Category indicator byte: 80 (compact TLV data object)
    Tag: 7, len: 3 (card capabilities)
      Selection methods: C0
        - DF selection by full DF name
        - DF selection by partial DF name
      Data coding byte: 21
        - Behaviour of write functions: proprietary
        - Value 'FF' for the first byte of BER-TLV tag fields: invalid
        - Data unit in quartets: 2
      Command chaining, length fields and logical channels: C0
        - Command chaining
        - Extended Lc and Le fields
        - Logical channel number assignment: No logical channel
        - Maximum number of logical channels: 1
    Tag: 5, len: 7 (card issuer's data)
      Card issuer data: 59 75 62 69 4B 65 79
+ TCK = 40 (correct checksum)

Possibly identified card (using /usr/share/pcsc/smartcard_list.txt):
3B FD 13 00 00 81 31 FE 15 80 73 C0 21 C0 57 59 75 62 69 4B 65 79 40
        Yubico YubiKey 5 NFC (PKI)
        https://www.yubico.com/product/yubikey-5-nfc



# Create a Polkit rule:
# If you want to allow all local Linux users to access the YubiKey, then you can
# use subject.isLocal in the if, instead of subject.user == "${USER}"
$ ( cat - <<EOF | sudo tee /etc/polkit-1/rules.d/49-allow-pcsc.rules
polkit.addRule(function(action, subject) {
    if ((action.id == "org.debian.pcsc-lite.access_pcsc" ||
         action.id == "org.debian.pcsc-lite.access_card") &&
        subject.user == "${USER}") {
        return polkit.Result.YES;
    }
});
EOF
)

# Restart pcscd
$ sudo systemctl restart pcscd

# Finally access your YubiKey:
$ ykman info
Device type: YubiKey 5 NFC
Serial number: 14874043
Firmware version: 5.4.3
Form factor: Keychain (USB-A)
Enabled USB interfaces: OTP, FIDO, CCID
NFC transport is enabled.

Applications    USB     NFC
OTP             Enabled Enabled
FIDO U2F        Enabled Enabled
FIDO2           Enabled Enabled
OATH            Enabled Enabled
PIV             Enabled Enabled
OpenPGP         Enabled Enabled
YubiHSM Auth    Enabled Enabled
```

# FIDO2
Before we use [FIDO2](https://fidoalliance.org/fido2/) to store credentials, we
need to protect the credentials with a [PIN](https://en.wikipedia.org/wiki/Personal_identification_number). The PIN should be between 4-63 characters, and
alphanumeric.
```sh
# Setting a PIN
$ ykman fido info
PIN is not set.
$ ykman fido access change-pin
Enter your new PIN:
Repeat for confirmation:
$ ykman fido info
PIN is set, with 8 attempt(s) remaining.
# PIN attempts are reset to 8 when it is correctly introduced, after some
# failures
```

You should know that (by default) most FIDO2 websites, when they are using the
WebAuthn protocol to **register** a new key, they will send a new credentials
request that will make the YubiKey generate(internally) a new private key, sign
the request with the private key, then the client (YubiKey) will return the
signed request and the public key to the server/backend. At **authentication**
time, the server side is using the public key to verify that the client has
access to the private key, by asking the client to sign some random data. So,
the private key of the credentials is generated, stored, and used safely on the
YubiKey hardware, without ever leaving the device.

However, there is a limit on the number of private keys you can store on the
YubiKey device, which is determined by the storage capacity it has. For the
YubiKey 5 NFC model, the limit is `~25` **resident credentials**. Resident
credentials are visible using the `credentials list` command below. However,
there are also **non-resident** credentials, that are not visible/listable, but
they also keep a private key, using the space of your YubiKey. While they don't
have a documented limit on the number of non-resident credentials, it appears to
be in terms of thousands. Anyway, you are able to reset the fido part of the key
to remove the non-resident credentials. For resident credentials you can list
and remove them through ykman. So, just remember that there are non-resident
credentials that will not show up in the credentials list.

## Reading resident credentials
```sh
# Assuming that your PIN is in: export my_fido_pin="SECRET_PIN"
$ ykman fido credentials list --pin ${my_fido_pin}
Credential ID  RP ID  Username  Display name

# After registering to https://demo.yubico.com/webauthn-technical/registration
# a non-resident credential is saved on your YubiKey, that doesn't show up in
# the credentials list command.

# After registering to https://webauthn.io/
$ ykman fido credentials list --pin ${my_fido_pin}
Credential ID  RP ID        Username  Display name
b03574fa...    webauthn.io  tedi      tedi
```
## Deleting resident credentials
```sh
$ ykman fido credentials delete b03574fa --force --pin ${my_fido_pin}
```

## FIDO2 SSH
```sh
# The generated ${HOME}/.ssh/id_ed25519_sk does not contain the full private key
# It contains only a reference to the key inside of the YubiKey, so you still
# can't login without the YubiKey.
$ ssh-keygen -t ed25519-sk -f ${HOME}/.ssh/id_ed25519_sk # enter FIDO PIN
$ cat ${HOME}/.ssh/id_ed25519_sk.pub # copy to remote host
$ ssh-copy-id -i ${HOME}/.ssh/id_ed25519_sk user@remote-server
$ ssh user@remote-server # Touch YubiKey to confirm user presence
Confirm user presence for key ED25519-SK SHA256:Jaw5yThf/XoOBLY+xdMop4NO0J3cUNqTuGYJS6cZM8g
User presence confirmed
Welcome to remote-server ...
```

# OATH
32 OATH accounts can be added on this YubiKey.
```sh
# Show OATH info
$ ykman oath info
OATH version: 5.4.3
Password protection: disabled

# List OATH accounts
$ ykman oath accounts list
GitHub:john.doe
Google:sign_into user@gmail.com
Google:sign_into_user@gmail.com
Google account:sign_into user@gmail.com
Google_account:sign_into_user@gmail.com
TestAccount:user@example.com

# Add OATH TOTP account - test with https://totp.danhersam.com/
$ ykman oath accounts add \
  --oath-type TOTP \
  --digits 6 \
  --period 30 \
  --algorithm SHA1 \
  --issuer "Google account" \
  "sign into GMail with user@gmail.com" \
  JBSWY3DPEHPK3PXP

# Getting the TOTP code - 617463 in this case
$ ykman oath accounts code \
  "Google account:sign into GMail with user@gmail.com"
Google account:sign into GMail with user@gmail.com  617463

# Delete OATH TOTP account
$ ykman oath accounts delete --force \
  "Google account:sign into GMail with user@gmail.com"
```

# PIV

```sh
$ sudo apt-get install yubico-piv-tool ykcs11 opensc

$ ykman piv info
PIV version:              5.4.3
PIN tries remaining:      3/3
PUK tries remaining:      3/3
Management key algorithm: TDES
WARNING: Using default PIN!
WARNING: Using default PUK!
WARNING: Using default Management key!
CHUID: No data available
CCC:   No data available

# ... to be continued ...
```
