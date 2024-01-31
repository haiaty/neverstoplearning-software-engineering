## DESCRIPTION

combine Modular monolilith + the lucid variant present in this repo.

Each module is developed as described in the lucid variant.

Modules can communicate between them only by using 'services' or features from another module

Drivers should be done for each module. Don't try to reuse them. If you need a common driver
or commmon code, create a separated module, maybe called 'shared' or somenthing like this,
and create features and services that can be reused from other modules. But try to avoid 
these as much as possible since these introduce coupling between more parts.
