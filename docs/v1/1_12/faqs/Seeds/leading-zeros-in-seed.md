# How do I preserve leading zeros in a seed?

If you need to preserve leading zeros (for example in a zipcode or mobile number), include leading zeros in your seed file, and use the `column_types` [configuration](https://docs.getdbt.com/reference/resource-configs/column_types.md) with a varchar datatype of the correct length.
