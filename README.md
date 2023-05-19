[![License](https://img.shields.io/badge/license-Apache2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
![ROBOT results](https://github.com/mikel-egana-aranguren/Transparent-Relations-Ontology/actions/workflows/robot.yml/badge.svg)

# Transparent Relations Ontology

## About

Transparent Relations Ontology (TRO) offers a vocabulary to publish information about relations that should be more transparent, usually between powerful parties, to detect potential conflicts of interests: for example, governments and their providers, or politicians and their personal relationships. Research journalism and Open Data are two areas in which this sort of data is growing, and new methods and standards are needed to cope with the ([FAIR](https://www.go-fair.org/fair-principles/)) publication and consumption of such data.

TRO provides the vocabulary for the following projects:

* [Basque Country Institutions Transparent Relations Graph](https://github.com/mikel-egana-aranguren/BasqueCountryInstitutionsTransparentRelationsGraph).
* [Jon Ander Asua Gradu Amaierako Lana: KG Relaciones Clientelares](https://github.com/JonAnderAsua/TFG-KG-RelacionesClientelares).

Related projects:

* [La donación](https://ladonacion.es/).
* [OffShore Leaks DataBase](https://offshoreleaks.icij.org/).
* [TheyBuyForYou](https://github.com/TBFY).
* [Public Procurement Ontology](http://contsem.unizar.es/def/sector-publico/pproc).
* [eProcurement ontology](https://joinup.ec.europa.eu/collection/eprocurement/solution/eprocurement-ontology).

## Access

TRO provides a persistent URI namespace thanks to [W3ID](https://github.com/perma-id/w3id.org/tree/master/TRO):

* Machines: `curl -sH "Accept: text/turtle" -L https://w3id.org/TRO`
* Humans: https://w3id.org/TRO

## Development

### Ontology files

The main [OWL](ontology) file in [Turtle](https://www.w3.org/TR/turtle/) lives at `development/TransparentRelationsOntology.ttl`. It is produced using [Protégé](https://protege.stanford.edu/).

### Methods

We loosely stick to the [GitFlow methodology](https://nvie.com/posts/a-successful-git-branching-model/), so basically:

* Work on a `feature_*` branch listing the changes in the `RELEASES.md` file under the section `## Changes (No release yet)`.
* When are you are done merge `feature_*` into `develop`, preferably with a pull request.
* To create a release after major changes:
  * Change the `owl:priorVersion` to the current version, `owl:versionInfo` and `owl:schemaVersion` values to the version that will be released.
  * Make sure all [ROBOT tests pass](https://github.com/mikel-egana-aranguren/Transparent-Relations-Ontology/actions) (See "Quality tests" section bellow).
  * Create the documentation (See "Documentation" section bellow).
  * Merge from `develop` to a new `release_*` branch and edit the `RELEASES.md` file changing `## Changes (No release yet)` to the release number (e.g. `## RELEASE 0.1.2`) and adding any new changes to the list that were made in the `develop` branch.
* Merge `release_*` to `main` (With a pull request), `gh-pages` and `develop`.
* If a major version bump has happened, create both a release and a tag in GitHub pointing to the commit in `main` resulting from pulling `release_*`.

We also try to follow the best practices described in [Best Practices for Implementing FAIR Vocabularies and Ontologies on the Web](https://arxiv.org/abs/2003.13084).

### Quality tests

The quality tests are defined as [SPARQL](https://www.w3.org/TR/sparql11-query/) queries to be executed by [ROBOT](https://github.com/ontodev/robot) through [GitHub actions](https://github.com/mikel-egana-aranguren/Transparent-Relations-Ontology/actions):

* GitHub Actions YAML file: `.github/workflows/robot.yml`.
* ROBOT Makefile: `robot/Makefile`.
* SPARQL files should be defined in `robot/` with the `.rq` suffix and comments at the beggining stating the test's purpose. For example, `verify-label.rq` verifies that all the OWL classes have an `rdfs:label`: if there is one class without label the GitHub Actions pipeline will fail.

### Documentation

The documentation is generated by the [Docker](https://www.docker.com/) version of [Widoco](https://dgarijo.github.io/Widoco/). Execute `widoco.sh`: it will take `development/TransparentRelationsOntology.ttl` as input and it will generate the HTML files in `release/`.
