```bash
cd /dmo_source/DMO.Performance/Master
```
Generally speaking:
1. collect data
2. plonk in db
3. use the data analysis tools in /Business Logic/
4. plonk back into db
5. fe takes from db
## `OPCLogic.cs`
### Overview
The OPCLogic class provides methods for managing OPC-related operations, including updating downtime line collection states.
- `UpdateDowntimeLineCollectionState` method
- `UpdateDowntimeEquipmentCollectionState` method
### `UpdateDowntimeLineCollectionState`
#### Overview
Updates the downtime event collection state for a specified production line.
#### Parameters
- `lineRowId`: The unique identifier of the production line.
- `collectionEnabled`: A boolean indicating whether downtime event collection should be enabled or disabled.
#### Summary
This method performs the following actions:
- Retrieves the production line equipment associated with the provided `lineRowId`.
- Identifies equipment with active status, enabled performance functions, and matching downtime modes.
- Creates a list of `OPCWritePropertyValueParams` objects to update the downtime event collection enabled property.
- If `collectionEnabled` is false, also updates the current PLC code property to "-1".
- Writes the updated property values to the OPC server using `OPCWebApiService`.
- Saves the updated downtime event collection enabled property value to the equipment.
- Logs an audit entry for the change.
#### Exceptions
- Throws an exception if writing property values to the OPC server fails.
### `UpdateDowntimeEquipmentCollectionState`
#### Overview
Updates the downtime event collection state for a specified equipment.
#### Parameters
- `equipmentRowId`: The unique identifier of the equipment.
- `collectionEnabled`: A boolean indicating whether downtime event collection should be enabled or disabled.
#### Summary
This method:
1. Retrieves the equipment details using `DMO.Core.BLL.EquipmentLogic.GetEquipmentDetails`.
2. Identifies active equipment with enabled performance functions and matching downtime event collection properties.
3. Creates a list of `OPCWritePropertyValueParams` objects to update the downtime event collection enabled property.
4. If `collectionEnabled` is false, also updates the current PLC code property to "-1".
5. Writes the updated property values to the OPC server using `OPCWebApiService`.
6. Saves the updated downtime event collection enabled property value to the equipment.
7. Logs an audit entry for the change.
#### Exceptions
- Throws an exception if writing property values to the OPC server fails.
## `POConfirmationLogic.cs`
