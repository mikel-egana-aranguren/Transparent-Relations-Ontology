[![License](https://img.shields.io/badge/license-Apache2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![DOI](https://zenodo.org/badge/476618172.svg)](https://zenodo.org/badge/latestdoi/476618172)
![Quality](https://github.com/mikel-egana-aranguren/Transparent-Relations-Ontology/actions/workflows/quality.yml/badge.svg)
![Widoco](https://github.com/mikel-egana-aranguren/Transparent-Relations-Ontology/actions/workflows/static.yml/badge.svg)

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

## HTTP Access

TRO has a persistent URI thanks to [W3ID](https://github.com/perma-id/w3id.org/tree/master/TRO):

* Machines: `curl -sH "Accept: text/turtle" -L https://w3id.org/TRO`
* Humans: https://w3id.org/TRO

## Best practices

Feel free to contribute in any way, with pull requests, issues, etc. We try to follow the best practices from [Best Practices for Implementing FAIR Vocabularies and Ontologies on the Web](https://arxiv.org/abs/2003.13084) and other resources as described in the guidelines bellow.

### Development

We loosely stick to [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/) and [Semantic Versioning](https://semver.org/) so basically:

* Work on a `feature_*` branch listing the changes in the `RELEASES.md` file under the section `## Changes (No release yet)`.
* When are you are done merge (No fast forward) `feature_*` into `develop`, preferably with a pull request.
* To create a release after major changes:
  * Change the `owl:priorVersion` to the current version; `owl:versionInfo`, `owl:versionIRI` and `owl:schemaVersion` values to the version that will be released.
  * Make sure all [Quality tests pass](https://github.com/mikel-egana-aranguren/Transparent-Relations-Ontology/actions) (See "Ontology quality" section bellow).
  * Create the documentation (See "Documentation" section bellow).
  * Merge (No fast forward) from `develop` to a new `release_*` branch and edit the `RELEASES.md` file changing `## Changes (No release yet)` to the release number (e.g. `## RELEASE 0.1.2`) and adding any new changes to the list that were made in the `develop` branch.
* Merge (No fast forward) `release_*` to `main` (With a pull request) and `develop`.
* If a major version bump has happened, create both a release and a tag in GitHub pointing to the commit in `main` resulting from pulling `release_*`.

### Ontology quality

The main [OWL](ontology) file in [Turtle](https://www.w3.org/TR/turtle/) lives at `development/TransparentRelationsOntology.ttl`, and it is produced using [Protégé](https://protege.stanford.edu/).

The quality is checked by executing different tests:

* [ROBOT](https://github.com/ontodev/robot) processes that are defined in a Makefile (`robot/Makefile`):
  * [Report](http://robot.obolibrary.org/report#report-level-error): this is based on [SPARQL](https://www.w3.org/TR/sparql11-query/) [queries](http://robot.obolibrary.org/report_queries/) that are collected in an specific profile (`robot/tro_report_profile`) with their respective log level (Error, Warn, Info). To execute locally (Specially to see the HTML report generated), run `make report --directory ./robot`.
  * [Verify](http://robot.obolibrary.org/verify) is used to define quality rules as SPARQL queries, see for example `robot/verify-comment.rq`. To execute locally run `make verify --directory ./robot` 
  * [Reason](http://robot.obolibrary.org/reason) applies automatic inference to the ontology, which is used in this case to simply ensure the ontology is consistent. To execute locally, run `make reason --directory ./robot`.
* [OQuaRE metrics report](https://github.com/tecnomod-um/oquare-metrics), based on the [OQuaRE framework](https://semantics.inf.um.es/oquare/), in `oquare/`.

A [GitHub actions](https://github.com/mikel-egana-aranguren/Transparent-Relations-Ontology/actions) workflow checks the quality after every push or pull request to `develop` with changes in the `development/` directory as defined in the GitHub Actions YAML file: `.github/workflows/quality.yml` (If you want to skip quality checks, add `[skip actions]` to the commit message). **Important note**: every time OQuaRE is executed new files appear in GitHub, so pull to your local repo regularly.

### Documentation

The documentation is generated by the [Docker](https://www.docker.com/) version of [Widoco](https://dgarijo.github.io/Widoco/). Execute `widoco.sh`: it will take `development/TransparentRelationsOntology.ttl` as input and it will generate the HTML files in `release/`.

### References

Aranguren, M. E. (2024). The Transparent Relations Ontology (TRO): a vocabulary to describe conflicts of interest. arXiv preprint [arXiv:2410.07154](https://arxiv.org/abs/2410.07154).
