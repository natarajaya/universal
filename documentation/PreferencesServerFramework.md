# Preferences Server Framework

## Introduction

As the name suggests, the purpose of the preferences server framework is to server the user needs and preferences. Only
the preferences that are relevant and permitted to the consumer are provided, and they can be provided in the ontology
desired by the consumer, given that an ontology (or 'view') has been provided for this.

Currently, the preferences server Framework consist of two components:

* The Ontology Handler (see below)
* [The Preferences Server](PreferencesServer.md)

_The Ontology Handler_ is the component containing the functionality for translating between one ontology (or 'view')
and another. Certain consumers prefer viewing the preferences in one view more suitable for them (i.e. ISO-24751) and
the Ontology Handler is able to translate these settings (given that an ontology transformation has been provided) from
one view to another.

_The Preferences Server_ is the public facing component of the preferences server framework. It provides APIs, allowing
requests for preference sets in some desired view (via API parameters), that only gives the consumer access to the
relevant settings. It is responsible for (via the ontology handler) to translate the settings to the desired view,
ensuring that no duplicate preferences are stored, etc. For a full description of the Preferences Server, see: [The
Preferences Server](PreferencesServer.md).

## Motivation and rules

We have identified a set of requirements to a new Preferences Framework and Ontology Handler implementations, and the
components using it:

* **Each ontology should have a (ontology-global) unique ID** for the framework to be able to identify it
* **preference sets should consist of containers keyed by ontology IDs** - as opposed to before, where it's guessed by
  the pattern of the preferences what ontology is used, the preference sets should explicitly declare what ontology
  they're in.
* **A single preference safe can contain multiple ontologies.**
* **A single (transformable) value will only be present once in the preference safe**
* **Ontology, preference safes and security gateway should always be set up together** - seeing how we would now have
  the preferences keyed by ontology, any instance of the preferences server would need to be deployed along with a
  ontology server, as well as a little front-end allowing the transformation of the preferences into a desired ontology.
  This will allow anyone who needs to read the preferences to get them in a format they understand. This has some
  implications:
* **Any consumer of preferences is expected to declare what format it wants them in** (or absense thereof implies the
  use of the 'flat' ontology) - this includes the MMs and preference editors

## Ontology Handler:

Component with the ability of performing various operations on preference sets. It is NOT implemented as a REST API
service, as it is expected to be present on all any instance running the Preferences Server. In other words, either the
ontology handling is expected to be done based on parameters given when talking to the [Preferences
Server](PreferencesServer.md) or by using the public functions of the OntologyHandler directly.

### Ontologies:

The ontology transform specs are stored in the `testData/ontologies` folder of the `universal` repository. The filename
of a transformation spec file should describe what transformation that file contains. More specifically, a filename for
ontology transformations should have the following format: <from>-<to>.json where <from> should be replaced with the
name of the ontology that is expected as the input model, and <to> should be the name of the ontology that will be
output from the transformation.
