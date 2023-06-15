# Experiments with RDA PIDINST and PIDINST-SCHEMA

<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

This repository contains some simple experiments with the RDA PIDINST
schema and the creation of a simple ontology based on it. The schema is
available at
[rdawg-pidinst](https://github.com/rdawg-pidinst/schema/blob/master/schema.rst).
The ontology is available at [pidinst.ttl](./data/pidinst.ttl) as well
as SHACL shapes at [pidinst-shapes.ttl](./data/pidinst-shapes.ttl). It
contains a sample pidinst JSON schema extracted from the [ODIS
Architecture GitHub
Issue](https://github.com/iodepo/odis-arch/issues/68#issuecomment-1353075204)
in the file [pidinst-schema.json](./data/pidinst-schema.json) and
[pidinst-example.json](./data/pidinst-example.json) which is a sample
instance of the schema. A data prototype is in the Sarowar Hossain
[Github repository](https://github.com/shmishkat/DataPrototype).
Analysis was done with ChatGPT GPT-4 with bing web plugin for Retrieval
Augmented Generation. Guidance for Dataset follows [ESIP Science on
schema.org](https://github.com/ESIPFed/science-on-schema.org) (SOSO) and
[Google Dataset
Search](https://developers.google.com/search/docs/data-types/dataset).

## Tools

- [devcontainers](https://code.visualstudio.com/docs/remote/containers)
  for VSCode
- [kglab](https://github.com/DerwenAI/kglab) for graph manipulation
  using:
  - [pyshacl](https://github.com/RDFLib/pySHACL) for SHACL validation
  - [rdflib](https://github.com/RDFLib/rdflib) for RDF manipulation
- [nbdev](https://nbdev.fast.ai/)

## Sample Use Case

The owner of the instrument is the NSF facility MagLab
(https://ror.org/03s53g630). Josiah Carberry’s
(https://orcid.org/0000-0002-1825-0097) data acquisition info:

The experimental data (spectra) are acquired by a spectrometer (#1
HRS750, \#2 IsoPlane, Teledyne Princeton Instruments). The spectrometers
are (almost) fully automated and controlled via the LightField software
(Teledyne Princeton Instruments). LightField automatically saves the
acquired data and all experiment settings (spectrometer settings) in one
file.
https://www.princetoninstruments.com/products/software-family/lightfield

LightField saves files in \*.SPE format (whatever it means).

DS’ file- / folder- name convention:

Folder name: PI name_Experiment ID_Magnet system-Instrument_Start date

      Josiah_Carberry_P19401-E011-DC_SCM3-HRS750_02-12-2023
      Josiah_Carberry_none_B114-IsoPlane_02-12-2023

File name: Type of the experiment: PL, Ra(man), Re(flectance),
Tr(ansmittance) Sample short name: \*\*\*\* Magnetic field: ***T (or
from to ) Temperature: ***K Light source: SC, 532nm, 785nm, … - Power:
***mW or uW, or percentage Central frequency / wl/energy: ***cm-1, nm,
eV Slit: value: \*\*\* um Acq.time: ***min or sec Objective NA: ***NA
Other: gate voltage, pressure, …

PL_WSe2-MoSe2_00.0T_to_05.2T\_
10K_633nm-100uW_720nm_30um_2min_0.65NA.SPE
Ra_CsPr_30T_7.2K_532nm-2mW_550cm-1_30um_3x2min_0.82NA.SPE
Re_InSe_0T_5K_SC-20%*600meV_50um_5sec* 0.65NA_Gate Sweep -10V to
+20V.SPE

## Thematic references in sample data

ChatGPT was used to do the thematic analysis of the sample data.
[PIDINST JSON
Chat](https://chat.openai.com/share/2e4ef2c4-23e8-4f14-aca5-96bb2d0346af)
is the link to the chat.

The sample data provided has the following semantic themes that
potentially map to persistent identifiers:

- Observation
  ([sosa:Observation](https://www.w3.org/TR/vocab-ssn/#SOSAObservation),
  [schema:Observation](https://schema.org/Observation))
- Experiment (Activity)
- Instrument (PIDINST, sosa, [handle.net registry](https://handle.net/))
- Sample (RRID)
- Agent (Person, Organization, Software)
  - Person (ORCID)
  - Organization (ROR)
  - Software ([SBoM](https://spdx.dev/),
    [Codemeta](https://github.com/codemeta/codemeta) )
- Location ([w3c locn](https://www.w3.org/ns/locn),
  [schema.org](https://schema.org/location))
- Dataset (DOI)
- Physical Parameters: There are various physical parameters involved in
  the experiments, such as type of experiment (PL, Ra, Re, Tr), sample
  name, magnetic field, temperature, light source, power, central
  frequency/wavelength/energy, slit value, acquisition time, objective
  numerical aperture (NA), and others like gate voltage, pressure.
  Ontology patterns representing these physical parameters, their units
  of measurement (T, K, nm, mW/uW/%, cm-1/nm/eV, um, min/sec, NA, V),
  and their roles in the experiment would be necessary.
- Experimental Procedure: The narrative also implies a certain procedure
  or workflow that is followed in conducting the experiments and saving
  the data. This could be represented by an ontology pattern describing
  scientific procedures or workflows.

Thematically, we have something that is defined as an
[Affordance](https://w3c.github.io/wot-architecture/#sec-affordances) in
terms of variables that we can measure (this is confusing LLM agents and
potential instrument settings. We need a specification of these
“affordances” in the PIDINST doc. This is separate from the the settings
that were used in the “Observation” which is an instantiation of a
particular set of affordances in a “Activity” that is an experiment.

``` mermaid
graph LR
    subgraph "Agent"
        P[Person] -. "rdfs:subClassOf" .-> A[Agent]
        O[Organization] -. "rdfs:subClassOf" .-> A
        S[Software] -. "rdfs:subClassOf" .-> A
    end
    subgraph "Entity"
        I[Instrument] -. "rdfs:subClassOf" .-> E[Entity]
        Sm[Sample] -. "rdfs:subClassOf" .-> E
        D[Dataset] -. "rdfs:subClassOf" .-> E
        PP[PhysicalParameters] -. "rdfs:subClassOf" .-> E
    end
    subgraph "Activity"
        Ob[Observation] -. "rdfs:subClassOf" .-> Act[Activity]
        Ex[Experiment] -. "rdfs:subClassOf" .-> Act
    end
    subgraph "Plan"
        EP[ExperimentalProcedure] -. "rdfs:subClassOf" .-> Pl[Plan]
    end

    P -- "prov:wasAssociatedWith" --> Ob
    Ob -- "prov:wasPartOf" --> Ex
    I -- "prov:used" --> Ob
    Sm -- "prov:used" --> Ob
    D -- "prov:wasGeneratedBy" --> Ob
    O -- "prov:actedOnBehalfOf" --> P
    S -- "prov:actedOnBehalfOf" --> I
    L[Location] -- "prov:atLocation" --> Ob
    L -- "prov:atLocation" --> I
    L -- "prov:atLocation" --> Sm
    PP -- "prov:used" --> Ob
    EP -- "prov:wasUsedBy" --> Ex
```

The narrative describes a complex experimental setup involving various
physical parameters, instruments, and naming conventions. Here are the
key themes or components present in the narrative, along with
corresponding ontology patterns that might be necessary to describe
them:

1.  **Instruments**: Spectrometers (#1 HRS750, \#2 IsoPlane, Teledyne
    Princeton Instruments) and the controlling software (LightField
    software). This would require an ontology pattern that describes
    scientific instruments, their models, manufacturers, and associated
    software.

2.  **Experimental Data**: The acquired data and experiment settings
    saved in a `.spe` file. An ontology pattern representing data
    objects, their formats (`.spe`), and the parameters/settings they
    contain would be necessary.

3.  **File and Directory Naming Conventions**: The narrative describes a
    detailed naming convention for both directories and files. This
    could be represented using an ontology pattern that encapsulates the
    structure and semantics of these naming conventions.

4.  **Physical Parameters**: There are various physical parameters
    involved in the experiments, such as type of experiment (PL, Ra, Re,
    Tr), sample name, magnetic field, temperature, light source, power,
    central frequency/wavelength/energy, slit value, acquisition time,
    objective numerical aperture (NA), and others like gate voltage,
    pressure. Ontology patterns representing these physical parameters,
    their units of measurement (T, K, nm, mW/uW/%, cm-1/nm/eV, um,
    min/sec, NA, V), and their roles in the experiment would be
    necessary.

5.  **Experimental Procedure**: The narrative also implies a certain
    procedure or workflow that is followed in conducting the experiments
    and saving the data. This could be represented by an ontology
    pattern describing scientific procedures or workflows.

6.  **Samples**: Different samples are used in the experiments, as seen
    in the file naming convention. An ontology pattern representing
    scientific samples, their types, and characteristics would be
    needed.

Remember, the role of an ontology is to provide a common framework for
representing knowledge. Depending on the complexity and specific
requirements of your use case, you might use existing ontologies (like
the OBO Foundry ontologies), or develop custom ontologies tailored to
your needs.

## References

- [NSF Open Knowledge Network
  Roadmap](https://nsf-gov-resources.nsf.gov/2022-09/OKN%20Roadmap%20-%20Report_v03.pdf?VersionId=xmU9ccfd_nWRlDDSUE9HrOCvbM1fny24)
- [RDA Instrument Identifiers](https://pidinst.org)
  - [rdawg-pidinst/schema: RDA WG PIDINST Metadata Schema
    (github.com)](https://github.com/rdawg-pidinst/schema)
  - [\# Metadata Schema for the Persistent Identification of
    Instruments](https://zenodo.org/record/6396467#.ZFOkCIrMKcJ)
- [Data
  Prototype](https://docs.google.com/document/d/1RRKxzwOW2SMn2bRAESBvnf7MmAmYy4itTrxaCFQrSOM/edit#heading=h.x8a40zxbykh3)
- [12. Linking instrument PIDs to datasets — PIDINST
  1.0b2.dev32+g510a615
  documentation](https://docs.pidinst.org/en/latest/white-paper/linking-datasets.html)
- [rdawg-pidinst/usage
  (github.com)](https://github.com/rdawg-pidinst/usage)
- [schema/schema.rst at master · rdawg-pidinst/schema ·
  GitHub](https://github.com/rdawg-pidinst/schema/blob/master/schema.rst)
- [Persistent Identification of Instruments - Data Science Journal
  (codata.org)](https://datascience.codata.org/articles/10.5334/dsj-2020-018)
- [RDA Physical Samples and
  Collections](https://www.rd-alliance.org/groups/physical-samples-and-collections-research-data-ecosystem-ig)
  - [ESIP and RDA Joint
    Webinar](https://www.rd-alliance.org/physical-sample-webinar1-June2021)
  - [Supporting Reproducibility by Capturing Physical Sample Data and
    Metadata in a Connected Electronic Lab
    Notebook](https://www.rd-alliance.org/sites/default/files/RDA%20%26%20ESIP%20Physical%20Samples%20Webinar%20June%202021.pdf)
  - [RRIDs: A Way to Track Samples Through the Scientific
    Literature](https://www.rd-alliance.org/PS_RRIDs_August2021_webinar)
- [RDA
  iAdopt](https://www.rd-alliance.org/group/interoperable-descriptions-observable-property-terminology-wg-i-adopt-wg/wiki/i-adopt)
  - [RDA iAdopt Github](https://github.com/i-adopt)
  - [RDA iAdopt Github - Ontology](https://github.com/i-adopt/ontology)

## Development of the [Ocean Data and Information System (ODIS)](https://github.com/iodepo/odis-arch) architecture

- [International Oceanographic Data and Information
  Exchange](https://www.iode.org/)
- [PIDInst JSON
  Schema](https://github.com/iodepo/odis-arch/issues/68#issuecomment-1353075204)
- [EarthCube p418 JSON Schema to
  JSON-LD](https://github.com/earthcubearchitecture-project418/p418Docs/blob/master/publishing.md#developing-a-workflow-for-non-technical-authors)

## Science on Schema.org

- [Science on Schema.org](https://science-on-schema.org/)
- [Experimental](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Experimental.md)
- [DataSet](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md)

## Schema.org IoT

- [IoT and Schema.org: Getting
  Started](https://schema.org/docs/iot-gettingstarted.html)

## BioSchemas

- [BioSchemas](https://bioschemas.org/)
- [BioSchemas Github](https://github.com/BioSchemas/specifications)
- [BioSchemas Github
  LabProtocol](https://github.com/BioSchemas/specifications/tree/master/LabProtocol)
- [BioSchemas
  LabProtocol](https://bioschemas.org/profiles/LabProtocol/0.7-DRAFT)

## Other Ontologies

- [Sosa](https://www.w3.org/TR/vocab-ssn/)
- [schema.org](https://schema.org/)
- [schema.org/Product](https://schema.org/Product)
- [W3C DCAT](https://www.w3.org/TR/vocab-dcat-3/)
- [W3C Prov-o](https://www.w3.org/TR/prov-o/)
- [W3C locn](https://www.w3.org/ns/locn)
- [W3C Time Ontology](https://www.w3.org/TR/owl-time/)
- [scientific instrument](https://www.wikidata.org/wiki/Q3099911)
- [RoR](https://ror.org/)
- [MagLab RoR](https://ror.org/03s53g630)
- [ORCID](https://orcid.org/)
- [Josiah Carberry - Fictional
  Orcid](https://orcid.org/0000-0002-1825-0097)
- [SPDX](https://spdx.org/)
- [SPDX License List](https://spdx.org/licenses/)
- [SPDX Model Version 3 Github](https://github.com/spdx/spdx-3-model)
- [Quantities, Units, Dimensions and Time](https://www.qudt.org/)
- [SBoMs](https://www.ntia.gov/sbom)
- [CyberSecurity and SBoMs](https://www.cisa.gov/sbom)

## Refactor Use case Narrative

> The experimental data (spectra) are acquired by a spectrometer (#1
> HRS750, \#2 IsoPlane, Teledyne Princeton Instruments). The
> spectrometers are (almost) fully automated and controlled via the
> LightField software (Teledyne Princeton Instruments). LightField
> automatically saves the acquired data and all experiment settings
> (spectrometer settings) in one file (spe file format). Each experiment
> is saved in a folder with the following directory naming convention:
> “PI name_Experiment ID_Magnet system-Instrument_Start date”. Inside
> the folder is stored the spe file with the following file naming
> convention:

> Type of the experiment: PL, Ra(man), Re(flectance), Tr(ansmittance): -
> Sample short name: \*\*\*\* - Magnetic field: ***T (or from to ) -
> Temperature: ***K - Light source: SC, 532nm, 785nm, … - Power: ***mW
> or uW, or percentage - Central frequency / wl/energy: ***cm-1, nm,
> eV - Slit: value: \*\*\* um - Acq.time: ***min or sec - Objective NA:
> ***NA - Other: gate voltage, pressure, …

> For example: PL_WSe2-MoSe2_00.0T_to_05.2T\_
> 10K_633nm-100uW_720nm_30um_2min_0.65NA.SPE
> Ra_CsPr_30T_7.2K_532nm-2mW_550cm-1_30um_3x2min_0.82NA.SPE
> Re_InSe_0T_5K_SC-20%*600meV_50um_5sec* 0.65NA_Gate Sweep -10V to
> +20V.SPE

## LLMs

- Use Keywords and descriptions for parameters
- Model the instrument as a product
- Model the settings more formally.

## ChatGPT summary

Certainly, the following provides a range of options that the MagLab
could consider when developing their semantic infrastructure, taking
into consideration both minimal and more comprehensive approaches.

1.  **Minimal Approach - Schema.org metadata for experiments:**
    - *Description:* Use Schema.org terms to describe the key features
      of your experiments. This involves adding appropriate Schema.org
      types (like `Dataset`, `Person`, `Organization`, etc.) and
      properties to your HTML metadata.
    - *FAIR perspective:* This approach would increase the Findability
      and Interoperability of your datasets, as Schema.org is widely
      recognized and used by major search engines, aiding in data
      discovery. However, this is a general-purpose vocabulary and may
      lack the specificity required to fully describe scientific
      experiments.
2.  **Mid-level Approach - Schema.org + ESIP Science-on-Schema.org
    guidelines:**
    - *Description:* Extend the minimal approach by following ESIP’s
      guidelines for using Schema.org in a scientific context. This
      provides a richer description of your data and includes terms from
      QUDT to quantify units and measuredVariables.
    - *FAIR perspective:* By adhering to a community-accepted standard
      like ESIP, you enhance the Interoperability and Reusability of
      your data, as others in the scientific community will have a
      better understanding of the metadata and can use it more
      effectively.
3.  **Advanced Approach - Adopt a more descriptive Ontology Design
    Pattern (ODP):**
    - *Description:* Implement a more formal ontology structure by
      creating a pattern that separates `Experiment`, `Instrument`, and
      `Sample` as distinct entities that participate in the `Experiment`
      process. This can be achieved with Semanticscience Integrated
      Ontology (SIO), which provides a rich semantic framework for
      describing scientific entities and processes. Here, `Instrument`
      and `Sample` participate in `Experiment`, which is a subclass of
      `SIO:Process` (or `prov:Activity` for alignment with PROV-O).
    - *FAIR perspective:* This approach would greatly enhance the
      Interoperability and Reusability of your data by providing a
      robust and expressive semantic model for your experiments. This
      detailed semantic information can improve data discovery and
      understanding, aiding both human users and automated processes.
4.  **Comprehensive Approach - Integrate with existing ontologies and
    identifiers:**
    - *Description:* Further expand the advanced approach by aligning
      with existing ontologies and identifier systems. This could
      include leveraging the PhysicalSamples ontology and Instrument
      identifiers (PIDINST) from RDA, BioSchemas recommendations, and
      RRIDs for tracking samples. Use OWL-Time and W3C locn for
      representing time and location, respectively.
    - *FAIR perspective:* This approach ensures maximal Findability,
      Accessibility, Interoperability, and Reusability of your data by
      adhering to well-established standards and practices. It promotes
      data integration, comparison, and reuse across different studies
      and scientific fields.

In summary, the chosen approach depends on the specific needs and
resources of the MagLab. A more comprehensive approach offers greater
FAIR compliance and utility for data users but requires more effort to
implement and maintain. Consider the FAIR principles as guiding
concepts, promoting maximum use and reuse of your valuable data.
Remember that being FAIR is a journey, not a destination, and even small
steps towards better metadata can have a significant impact on data
utility.

## Work

- We should probably use <https://w3id.org/pidinst/schema/> for
  namespace
- Sanity check against [need “platform” / “instrument” / “sensor” field
  in top-level JSON-LD](https://github.com/iodepo/odis-arch/issues/68)
- Sample PIDINST landing Page + Metadata
- Guidance Document Instrument.md
- PIDINST Schema dereference via w3id.org
- ODP for “experiment” leveraging SIO and Prov-O
- RO-Crate for experiment
- HDF5 for experiment
- Complete Wikidata for Princeton Instruments HRS750
- Complete Wikidata for Princeton Instruments IsoPlane
- Complete Wikidata for LightField
- Prompt engineering for @context
