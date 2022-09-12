TribeRedeemer.previewRedeem : Use ++i over i++ in for loop

i++ is generally more expensive because it must increment a value and "return" the old value, so it may require holding two numbers in memory. ++i only ever uses one number in memory. There are many cases where they are likely equivalent after compiler optimizations.