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

```yaml
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
```
</details>

---

```sh
# Get all device properties as json
$ powershell.exe -Command "Get-PnpDevice | Where-Object { \$_.InstanceId -like '*YUBICO*' } | ForEach-Object { \$device=\$_; \$props=Get-PnpDeviceProperty -InstanceId \$device.InstanceId -ErrorAction SilentlyContinue; \$allProps=@{}; \$allProps['FriendlyName']=\$device.FriendlyName; \$allProps['InstanceId']=\$device.InstanceId; \$allProps['Class']=\$device.Class; \$allProps['Status']=\$device.Status; foreach (\$p in \$props) { if (\$p.Data -is [Array]) { \$allProps[\$p.KeyName]=\$p.Data -join ', ' } else { \$allProps[\$p.KeyName]=\$p.Data } }; [PSCustomObject]\$allProps } | ConvertTo-Json -Depth 99"
```
<details>
  <summary>Click to expand JSON output</summary>

```json
[
    {
        "DEVPKEY_Device_BaseContainerId":  "{A814DAAC-3F96-11F0-B890-A0B33979102C}",
        "DEVPKEY_Device_InstallDate":  "\/Date(1748951221946)\/",
        "Class":  null,
        "DEVPKEY_Device_CompatibleIds":  "SCFILTER\\CID_2777BE07-6993-4513-BD80-C184FCB0AB2D",
        "{83DA6326-97A6-4088-9453-A1923F573B29} 5":  3758096968,
        "DEVPKEY_NAME":  "Smart Card",
        "{80497100-8C73-48B9-AAD9-CE387E19C56E} 6":  0,
        "DEVPKEY_Device_SafeRemovalRequired":  false,
        "DEVPKEY_Device_HardwareIds":  "SCFILTER\\CID_8073c021c057597562694b6579",
        "DEVPKEY_Device_InstanceId":  "SCFILTER\\CID_8073C021C057597562694B6579\\7\u00262FE4C607\u00260\u0026YUBICO_YUBIKEY_OTP+FIDO+CCID_0_SCFILTER_CID_8073C021C057597562694B6579",
        "DEVPKEY_Device_ProblemCode":  28,
        "DEVPKEY_Device_Stack":  "\\Driver\\scfilter",
        "DEVPKEY_Device_Parent":  "USB\\VID_1050\u0026PID_0407\u0026MI_02\\6\u0026effba79\u00260\u00260002",
        "{3464F7A4-2444-40B1-980A-E0903CB6D912} 10":  3,
        "{83DA6326-97A6-4088-9453-A1923F573B29} 15":  true,
        "DEVPKEY_Device_LocationInfo":  "ScFilter",
        "DEVPKEY_Device_RemovalPolicyDefault":  3,
        "DEVPKEY_Device_PowerData":  "56, 0, 0, 0, 4, 0, 0, 0, 25, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0, 1, 0, 0, 0",
        "DEVPKEY_Device_InstallState":  2,
        "DEVPKEY_Device_DevNodeStatus":  25191424,
        "DEVPKEY_Device_EnumeratorName":  "SCFILTER",
        "DEVPKEY_Device_InLocalMachineContainer":  false,
        "FriendlyName":  "Smart Card",
        "DEVPKEY_Device_Siblings":  "{892EDE5E-BE49-443c-A0B3-005D74F2D69C}\\ScFilter\\7\u00262fe4c607\u00260\u002604",
        "DEVPKEY_Device_DeviceDesc":  "Smart Card",
        "DEVPKEY_Device_HasProblem":  true,
        "DEVPKEY_Device_IsRebootRequired":  false,
        "Status":  "Error",
        "DEVPKEY_Device_LastArrivalDate":  "\/Date(1748952255288)\/",
        "DEVPKEY_Device_ReportedDeviceIdsHash":  3328081164,
        "DEVPKEY_Device_FirstInstallDate":  "\/Date(1748950570277)\/",
        "InstanceId":  "SCFILTER\\CID_8073C021C057597562694B6579\\7\u00262FE4C607\u00260\u0026YUBICO_YUBIKEY_OTP+FIDO+CCID_0_SCFILTER_CID_8073C021C057597562694B6579",
        "DEVPKEY_Device_Capabilities":  132,
        "DEVPKEY_Device_ProblemStatus":  0,
        "DEVPKEY_Device_PDOName":  "\\Device\\0000012d",
        "{A8B865DD-2E3D-4094-AD97-E593A70C75D6} 26":  false,
        "DEVPKEY_Device_RemovalPolicy":  3,
        "DEVPKEY_Device_ContainerId":  "{A814DAAC-3F96-11F0-B890-A0B33979102C}",
        "{83DA6326-97A6-4088-9453-A1923F573B29} 10":  "USB\\VID_1050\u0026PID_0407\u0026MI_02\\6\u0026effba79\u00260\u00260002",
        "DEVPKEY_Device_IsPresent":  true,
        "DEVPKEY_Device_ConfigFlags":  64,
        "DEVPKEY_Device_BusReportedDeviceDesc":  "Smart Card"
    },
    {
        "DEVPKEY_Device_BaseContainerId":  "{00000000-0000-0000-FFFF-FFFFFFFFFFFF}",
        "DEVPKEY_Device_LastArrivalDate":  "\/Date(1748952254736)\/",
        "DEVPKEY_Device_SessionId":  1,
        "Class":  null,
        "DEVPKEY_Device_CompatibleIds":  "SWD\\GenericRaw, SWD\\Generic",
        "DEVPKEY_Device_CreatorProcessId":  16748,
        "DEVPKEY_NAME":  "Yubico YubiKey OTP+FIDO+CCID 0",
        "{80497100-8C73-48B9-AAD9-CE387E19C56E} 6":  0,
        "DEVPKEY_Device_SafeRemovalRequired":  false,
        "DEVPKEY_Device_HardwareIds":  "ScDeviceInformationNode\\node",
        "DEVPKEY_Device_InstanceId":  "SWD\\SCDEVICEENUM\\1_YUBICO_YUBIKEY_OTP+FIDO+CCID_0",
        "DEVPKEY_Device_ProblemCode":  28,
        "{83DA6326-97A6-4088-9453-A1923F573B29} 10":  "SWD\\ScDeviceEnumBus\\0",
        "DEVPKEY_Device_Stack":  "\\Driver\\SoftwareDevice",
        "DEVPKEY_Device_Parent":  "SWD\\ScDeviceEnumBus\\0",
        "{3464F7A4-2444-40B1-980A-E0903CB6D912} 10":  3,
        "{83DA6326-97A6-4088-9453-A1923F573B29} 15":  true,
        "DEVPKEY_Device_RemovalPolicyDefault":  1,
        "DEVPKEY_Device_PowerData":  "56, 0, 0, 0, 1, 0, 0, 0, 9, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0, 4, 0, 0, 0, 0, 0, 0, 0",
        "DEVPKEY_Device_InstallState":  2,
        "DEVPKEY_Device_DevNodeStatus":  1098916874,
        "DEVPKEY_Device_EnumeratorName":  "SWD",
        "DEVPKEY_Device_FriendlyName":  "Yubico YubiKey OTP+FIDO+CCID 0",
        "DEVPKEY_Device_InLocalMachineContainer":  true,
        "FriendlyName":  "Yubico YubiKey OTP+FIDO+CCID 0",
        "DEVPKEY_Device_Siblings":  "SWD\\ScDeviceEnum\\1_Windows_Hello_for_Business_1, SWD\\ScDeviceEnum\\1_Alcorlink_USB_Smart_Card_Reader_0",
        "DEVPKEY_Device_DeviceDesc":  "Smart Card Device Information Node",
        "DEVPKEY_Device_HasProblem":  false,
        "DEVPKEY_Device_IsRebootRequired":  false,
        "Status":  "Error",
        "DEVPKEY_Device_LegacyBusType":  15,
        "DEVPKEY_Device_BusTypeGuid":  "{06D10322-7DE0-4CEF-8E25-197D0E7442E2}",
        "DEVPKEY_Device_ReportedDeviceIdsHash":  40132414,
        "InstanceId":  "SWD\\SCDEVICEENUM\\1_YUBICO_YUBIKEY_OTP+FIDO+CCID_0",
        "DEVPKEY_Device_Capabilities":  240,
        "DEVPKEY_Device_ProblemStatus":  0,
        "DEVPKEY_Device_PDOName":  "\\Device\\0000012c",
        "{A8B865DD-2E3D-4094-AD97-E593A70C75D6} 26":  false,
        "DEVPKEY_Device_RemovalPolicy":  1,
        "DEVPKEY_Device_ContainerId":  "{00000000-0000-0000-FFFF-FFFFFFFFFFFF}",
        "DEVPKEY_Device_BusNumber":  0,
        "DEVPKEY_Device_IsPresent":  true,
        "DEVPKEY_Device_ConfigFlags":  64,
        "DEVPKEY_Device_BusReportedDeviceDesc":  "Smart Card Device Information Node"
    }
]
```
</details>

# Key management
## Generate
```sh
```
## Show keys
```sh
```
## Import key
```sh
```
## Delete key
```sh
```

# Using the key

```sh
```
