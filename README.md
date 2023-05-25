# Experiments with RDA PIDINST and PIDINST-SCHEMA

::: {.cell 0=‘h’ 1=‘i’ 2=‘d’ 3=‘e’}

``` python
from pidinst_experiments.core import *
```

:::

> Some thinking using ChatGPT on what a Experiment Bill of Materials
> (EBoM) might look like as an analogy to Software Bill of Materials
> (SBoM) and how it might be used to support the RDA PIDINST and
> PIDINST-SCHEMA working groups.

This repository contains some simple experiments with the RDA PIDINST
schema and the creation of a simple ontology based on it. The schema is
available at
[rdawg-pidinst](https://github.com/rdawg-pidinst/schema/blob/master/schema.rst).
The ontology is available at [pidinst.ttl](./data/pidinst.ttl) as well
as SHACL shapes at [pidinst-shapes.ttl](./data/pidinst-shapes.ttl).

## Tools

- [devcontainers](https://code.visualstudio.com/docs/remote/containers)
  for VSCode
- [kglab](https://github.com/DerwenAI/kglab) for graph manipulation
  using:
  - [pyshacl](https://github.com/RDFLib/pySHACL) for SHACL validation
  - [rdflib](https://github.com/RDFLib/rdflib) for RDF manipulation

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
- [PIDInst JSON
  Schema](https://github.com/iodepo/odis-arch/issues/68#issuecomment-1353075204)
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

## Science on Schema.org

- [Science on Schema.org](https://science-on-schema.org/)
- [Experimental](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Experimental.md)
- [DataSet](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md)

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
