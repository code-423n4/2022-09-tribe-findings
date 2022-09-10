
In `MetadataRenderer.sol` we can cache `i` varible in first `for loop` in `addProperties` function for save gas.

## Current Implemation

```
for (uint256 i = 0; i < numNewProperties; ++i) {
    // Append storage space
    properties.push();

    // Get the new property id
    uint256 propertyId = numStoredProperties + i;

    // Store the property name
    properties[propertyId].name = _names[i];

    emit PropertyAdded(propertyId, _names[i]);
}
```

## Catching for loop i variable 

```
// cache the newPropertyID with i 
uint256 endArrayLoop = numNewProperties + numStoredProperties;
for (uint256 i = numStoredProperties; i < endArrayLoop; ++i) {
    // Append storage space
    properties.push();

    // Store the property name
    properties[i].name = _names[i];

    emit PropertyAdded(i, _names[i]);
}
```