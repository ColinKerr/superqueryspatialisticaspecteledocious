# Introduction

## What is an iModel?

An iModel is a Sqlite file with a fixed table structure designed for performance and scalability and an extensible metadata layer on top providing friendly queries.

The extensible metadata layer, [BIS (Base Infrastructure Schema)](https://www.itwinjs.org/bis/), provides rich semantic meaning to data within an iModel and enables powerful queries that cross engineering disciplines and boundaries that previously existed due to data existing in different files and file formats.  Querying an iModel is done with [ECSQL](https://www.itwinjs.org/learning/ecsql/) a SQL like language where tables and columns are replaced with classes and properties.

Bentley's Open Source libraries allow powerful custom applications and services to be written using iModels.  In this case the free account and sample iModels available as part of the iTwin Platform will be used.

## Signing Up and getting the data

If you are not already signed up for a free iTwin Platform developer account [Sign up Here](https://developer.bentley.com/gettingstarted)

Once you have an account go to the [registration dashboard](https://www.itwinjs.org/getting-started/registration-dashboard/?tab=1) to add the sample iModels to your demo project.  We will use the `Stadium` sample.

We will be using The [iModel Console](https://imodelconsole.bentley.com), and [iTwin Design Review](https://www.bentley.com/en/products/product-line/digital-twins/itwin-design-review).

Other useful tools that we will not be using directly are [iModel Reporter](https://github.com/imodeljs/imodel-reporter) and the [iModel Schema Explorer](https://imodelschemaeditor.bentley.com/).

[Next: Design Review Tips](design-review-tips.md)
