## User
I'm thining about something like a "Experiment Bill of Materials" similar to, and in fact would leverage, the "software bill of materials" idea using specifications like SPDX that have a semantic representation. EBoM's would leverage prov-o, schema.org, qudt, sosa as a base and link Experiments, People, Facilities, Instruments, Equipment, Supplies and potentiall procedure.
## Bing
That sounds like a very interesting and ambitious idea. What are you trying to achieve with the Experiment Bill of Materials?
## User
A couple of things, a basis for FAIR experiments. Leveraging Open Knowledge Networks (OKNs) and povide the same goals the SBoMs provide for software.
## Bing
I see. So you want to create a standard way of describing and documenting experiments that can be easily shared and reused by others, using existing semantic web technologies and vocabularies. That sounds very useful and innovative.
If I understand correctly, FAIR experiments are experiments that follow the FAIR principles of making data Findable, Accessible, Interoperable and Reusable. Open Knowledge Networks (OKNs) are networks of interconnected data and knowledge that can be queried and analyzed by machines and humans. And software bill of materials (SBoMs) are documents that list the components and dependencies of a software product.
Is that right?
## User
Yes! That is correct!
## Bing
Great! I'm glad I got it right.😊
So how do you plan to implement your idea? Do you have any examples of experiments that you want to describe with your EBoM?
## User
As JSON-LD manifests using schema.org markup for the experiment that can link to Instrument Landing Pages using RDA PIDINST schema and RRID's for samples. ORCIDs and RoR would identify the facility and researchers. Here is a use case from NSF's MagLab facility:
The owner of the instrument is the NSF facility MagLab (https://ror.org/03s53g630). Josiah Carberry’s (https://orcid.org/0000-0002-1825-0097) data acquisition info: 
The experimental data (spectra) are acquired by a spectrometer (#1 HRS750, #2 IsoPlane, Teledyne Princeton Instruments). The spectrometers are (almost) fully automated and controlled via the LightField software (Teledyne Princeton Instruments). LightField automatically saves the acquired data and all experiment settings (spectrometer settings) in one file. 
https://www.princetoninstruments.com/products/software-family/lightfield
LightField saves files in *.SPE format (whatever it means). 
DS’ file- / folder- name convention:
Folder name:
PI name_Experiment ID_Magnet system-Instrument_Start date
 Josiah_Carberry_P19401-E011-DC_SCM3-HRS750_02-12-2023
 Josiah_Carberry_none_B114-IsoPlane_02-12-2023
File name:
Type of the experiment: PL, Ra(man), Re(flectance), Tr(ansmittance)
Sample short name: ****
Magnetic field: ***T (or from to )
Temperature: ***K
Light source: SC, 532nm, 785nm, … - Power: ***mW or uW, or percentage
Central frequency / wl/energy: ***cm-1, nm, eV
Slit: value: *** um
Acq.time: ***min or sec
Objective NA: ***NA
Other: gate voltage, pressure, …
PL_WSe2-MoSe2_00.0T_to_05.2T_ 10K_633nm-100uW_720nm_30um_2min_0.65NA.SPE
Ra_CsPr_30T_7.2K_532nm-2mW_550cm-1_30um_3x2min_0.82NA.SPE
Re_InSe_0T_5K_SC-20%600meV_50um_5sec 0.65NA_Gate Sweep -10V to +20V.SPE
## Bing
Wow, that's very detailed and specific. I can see how using JSON-LD and schema.org would help to standardize and structure the information about the experiment. And using RDA PIDINST, RRID, ORCID and RoR would help to identify and link the different entities involved in the experiment.
I'm curious, how do you plan to use prov-o to capture the provenance of the experiment? Do you have a way to automatically generate the provenance graph from the JSON-LD manifest? And how do you plan to use qudt and sosa to describe the units and sensors used in the experiment?
## User
Prov-o would model the experiment as a subclass of Prov-o Activity. The Activity can have Agents (the researchers) Software Agents (that interface with the insrument and collect the data). In addition a activity supports the idea of Roles, so an Instrument and Sample have a role relationship to an 'analysis activity' experiment.
## Bing
I see. That makes sense. So you would use prov-o to model the relationships between the entities and activities involved in the experiment, and to track the sources and derivations of the data.
And what about qudt and sosa? How would you use them to describe the units and sensors used in the experiment?
## User
Instument would be a sosa:Platform, the that has a sosa:sensor that sosa:observs some property (Reflectance, Transmittence) etc of a 'feature of interest' which links to the sample. QUDT provides a way of describing the features of interest and the units. I need a way to connect back to schema:measuredVariable though.
## Bing
I understand. So you would use sosa to model the instruments and sensors as platforms and sensors that observe some properties of the samples, and qudt to model the units and dimensions of those properties. And you need a way to link those properties to the schema:measuredVariable property that describes what the experiment is measuring.
Maybe you could use schema:additionalType to link the schema:measuredVariable to a qudt:QuantityKind or qudt:Unit. For example:

```json
{
  "@context": {
    "schema": "http://schema.org/",
    "qudt": "http://qudt.org/schema/qudt/"
  },
  "@type": "Experiment",
  "measuredVariable": {
    "@type": "PropertyValue",
    "name": "Reflectance",
    "additionalType": {
      "@id": "qudt:Reflectance"
    }
  }
}
```
What do you think of this approach?
## User
Yes. I like this. Given the example experiment, could you generate what a EBoM might look like?
## Bing
Sure, I can try to generate a possible EBoM based on your example experiment. Here is what I came up with:

```json
{
  "@context": {
    "schema": "http://schema.org/",
    "qudt": "http://qudt.org/schema/qudt/",
    "sosa": "http://www.w3.org/ns/sosa/",
    "pidinst": "https://w3id.org/pidinst/schema/"
  },
  "@type": "Experiment",
  "name": "Re_InSe_0T_5K_SC-20%_600meV_50um_5sec_ 0.65NA_Gate Sweep -10V to +20V",
  "description": "An experiment to measure the reflectance of InSe at 0T and 5K using a supercontinuum light source at 20% power and 600 meV energy.",
  "experimentId": "P19401-E011-DC",
  "startDate": "2023-02-12",
  "instrument": {
    "@type": ["sosa:Platform", "pidinst:InstrumentInstance"],
    "name": "#1 HRS750",
    "identifier": {
      "@type": ["schema:PropertyValue", "pidinst:PID"],
      "@value": "<some PID>"
    },
    "instrumentType": {
      "@type": ["schema:Class", "pidinst:InstrumentType"],
      "@id": "<some URL>",
      "name": "<some name>",
      "instrumentTypeIdentifier": {
        "@type": ["schema:PropertyValue", "pidinst:PID"],
        "@value": "<some PID>"
      }
    },
    "manufacturer": {
      "@type": ["schema:Organization", "pidinst:Manufacturer"],
      "@id": "<some URL>",
      "name": "<some name>"
    },
    "softwareAgent": {
      "@type": ["schema:SoftwareApplication", "prov:SoftwareAgent"],
      "@id": "<some URL>",
      "name": "LightField",
      "version": "<some version>",
      "operatingSystem": "<some OS>",
      "applicationCategory": "<some category>"
    },
    "sensor": {
      "@type": ["sosa:Sensor", "pidinst:SensorInstance"],
      "@id": "<some URL>",
      "name": "<some name>",
      "identifier": {
        "@type": ["schema:PropertyValue", "pidinst:PID"],
        "@value": "<some PID>"
      },
      "sensorType": {
        "@type":["schema:Class", pidinst:"SensorType"],
        "@id":"<some URL>",
        name":"<some name>",
        sensorTypeIdentifier":{
          "@type":["schema:PropertyValue", pidinst:"PID"],
          "@value":"<some PID>"
        }
      },
      manufacturer":{
        "@type":["schema:Organization", pidinst:"Manufacturer"],
        "@id":"<some URL>",
        name":"<some name>"
      },
      observes:{
        "@type":["qudt:QuantityKind", schema:"Class"],
        additionalType:{
          @id":"qudt:Reflectance"
        }
      }
    }
  },
  measuredVariable":{
    @type":"schema:PropertyValue",
    name":"Reflectance",
    additionalType:{
      @id":"qudt:Reflectance"
    }
  },
  measuredValue":{
    @type":"qudt:QuantityValue",
    numericValue":"<some value>",
    unit":{
      @id":"qudt:<some unit>"
    }
  },
  experimentalCondition:{
    @type":"schema:Intangible",
    name":"Magnetic field",
    value":{
      @type":"qudt:QuantityValue",
      numericValue":"0",
      unit":{
        @id":"qudt:Tesla"
      }
    }
  },
  experimentalCondition:{
    @type":"schema:Intangible",
    name":"Temperature",
    value":{
      @type":"qudt:QuantityValue",
      numericValue":"5",
      unit":{
        @id":"qudt:Kelvin"
      }
    }
  },
  experimentalCondition:{
    @type":"schema:Intangible",
    name":"Light source",
    value":{
      @type":"schema:Intangible",
      name":"Supercontinuum light source",
      additionalProperty:{
        @type":"schema:Intangible",
        name":"Power",
        value":{
          @type":"qudt:QuantityValue",
          numericValue":"20",
          unit":{
            @id":"qudt:<some unit>"
          }
        }
      },
       additionalProperty:{
         @type:"schema:Intangible",
         name:"Energy",
         value:{
           @type:"qudt:QuantityValue",
           numericValue:"600",
           unit:{
             @id:"qudt:<some unit>"
           }
         }
       }
     }
   },
   experimentalCondition:{
     @type:"schema:Intangible",
     name:"Slit width",
     value:{
       @type:"qudt:QuantityValue",
       numericValue:"50",
       unit:{
         @id:"qudt:<some unit>"
       }
     }
   },
   experimentalCondition:{
     @type:"schema:Intangible",
     name:"Acquisition time",
     value:{
       @type:"qudt:QuantityValue",
       numericValue:"5",
       unit:{
         @id:"qudt:<some unit>"
       }
     }
   },
   experimentalCondition:{
     @type:"schema:Intangible",
     name:"Objective NA",
     value:{
       @type:"qudt:QuantityValue",
       numericValue:"0.65"
     }
   },
   experimentalCondition:{
     @type:"schema:Intangible",
     name:"Gate voltage sweep range"
     value:{
       @type:"qudt:QuantityRange"
       minInclusiveNumericValue:"-10"
       maxInclusiveNumericValue:"+20"
       unit:{
         @id:"qudt:<some unit>"
       }
     }
   },
   sample:{
     @type:["sosa:FeatureOfInterest", schema:"Product"],
     identifier:{
       type:["RRID", schema:"PropertyValue"]
       value"<some RRID>"
     },
     additionalProperty:{
       type:["RRID", schema"PropertyValue"]
       value"<some RRID>"
     },
     additionalProperty:{
       type:["RRID", schema"PropertyValue"]
       value"<some RRID>"
     }
   },
   agent:[{
     type:["ORCID", schema"Person"]
     identifier{
       type:["ORCID", schema"PropertyValue"]
       value"<Josiah Carberry's ORCID>"
     },
     role{
       type:["schema"Role"]
       roleName"Principal Investigator"
     }
   }],
   location{
     type:["ROR", schema"Place"]
     identifier{
       type:["ROR", schema"PropertyValue"]
       value"<MagLab's ROR>"
     }
   } 
}
```
This is just a rough sketch of what a possible EBoM might look like. I'm not sure if I got all the details right, and I'm sure there are ways to improve it. But I hope it gives you an idea of how to use JSON-LD and the various schemas and vocabularies to describe your experiment.
## User
No. This is really, really good. You came up with the entity experimentalConditions that I have been trying to figure out how to model. Do you have any ideas?
## Bing
Thank you. I'm glad you like it.😊
For the experimentalConditions entity, I just used a generic schema:Intangible type to represent any condition that affects the experiment, and then used schema:name and schema:value to describe the condition. For the value, I used qudt:QuantityValue or qudt:QuantityRange to specify the numeric value and unit of the condition. I also used schema:additionalProperty to add more details to some conditions, such as the power and energy of the light source.
I'm not sure if this is the best way to model the experimental conditions, but it seemed to work for your example. Maybe there are other schemas or vocabularies that could be more suitable for describing different types of conditions. Do you have any suggestions?
## User
Could you search. I think the Semantic Science Integrated Ontology might be a good starting point.
## Bing
OK, I searched for Semantic Science Integrated Ontology and found some relevant results. According to the first result, the Semantic Science Integrated Ontology (SIO) is an ontology to facilitate biomedical knowledge discovery. SIO features a simple upper level comprised of essential types and relations for the rich description of arbitrary (real, hypothesized, virtual, fictional) objects, processes and their attributes.
I also found a GitHub repository that contains the OWL2 ontology file for SIO, as well as a BioPortal page that provides more details and documentation about SIO.
It seems that SIO could be a good starting point for describing your experimental conditions, as it provides a lot of general and biomedical terms and relations. For example, you could use sio:has-attribute to relate an object or process to its condition, and sio:has-value to specify the value of the condition. You could also use sio:has-unit to link the value to a qudt:Unit.
For example, you could describe the temperature condition as:

```json
{
  "@context": {
    "sio": "http://semanticscience.org/resource/",
    "qudt": "http://qudt.org/schema/qudt/"
  },
  "@type": "sio:Process",
  "sio:has-attribute": {
    "@type": "sio:Temperature",
    "sio:has-value": {
      "@type": "sio:Value",
      "@value": "5",
      "sio:has-unit": {
        "@id": "qudt:Kelvin"
      }
    }
  }
}
```
What do you think of this approach?
## User
This is good! I think the nanomine ontology also uses SIO for material science which would let us describe sample in greater detail, so I think it makse sense to leverage SIO.
## Bing
I'm glad you like it.😊
I searched for the nanomine ontology and found that it is an ontology for polymer nanocomposites that extends SIO and other ontologies. It seems that it could be useful for describing your samples in more detail, as it provides terms and relations for materials, properties, structures, and processes.
For example, you could describe your sample of WSe2-MoSe2 as:

```json
{
  "@context": {
    "sio": "http://semanticscience.org/resource/",
    "nm": "http://nanomine.org/nm/"
  },
  "@type": ["sio:Material", "nm:PolymerNanocomposite"],
  "sio:has-attribute": {
    "@type": ["sio:MaterialAttribute", "nm:Composition"],
    "sio:has-value": {
      "@type": ["sio:Value", "nm:Component"],
      "@value": "WSe2-MoSe2",
      "sio:has-unit": {
        "@id": "nm:MoleFraction"
      }
    }
  }
}
```

Is this what you had in mind?
## User
Yes! Could you create an example for the other materials?
## Bing
Sure, I can try to create some more examples for the other materials. Here are some possible ones:

```json
{
  "@context": {
    "sio": "http://semanticscience.org/resource/",
    "nm": "http://nanomine.org/nm/"
  },
  "@type": ["sio:Material", "nm:PolymerNanocomposite"],
  "sio:has-attribute": {
    "@type": ["sio:MaterialAttribute", "nm:Composition"],
    "sio:has-value": {
      "@type": ["sio:Value", "nm:Component"],
      "@value": "CsPr",
      "sio:has-unit": {
        "@id": "nm:MoleFraction"
      }
    }
  }
}

{
  "@context": {
    "sio": "http://semanticscience.org/resource/",
    "nm": "http://nanomine.org/nm/"
  },
  "@type": ["sio:Material", "nm:PolymerNanocomposite"],
  "sio:has-attribute": {
    "@type": ["sio:MaterialAttribute", "nm:Composition"],
    "sio:has-value": {
      "@type": ["sio:Value", "nm:Component"],
      "@value": "InSe",
      "sio:has-unit": {
        "@id": "nm:MoleFraction"
      }
    }
  }
}
```

I hope these examples are helpful. Please let me know if you have any questions or feedback.😊
## User
Ok. One last thought experiment. How would this work with a RO-Crate that contained each of the spe files?
## Bing
OK, I searched for RO-Crate and found some relevant results. According to the first result, an RO-Crate is a directory tree with an ro-crate-metadata.json file at the top level. This file contains metadata about the other files and directories, represented by data entities. These metadata consist both of properties of the data entities themselves and of other, non-digital entities called contextual entities.
I also found a website that provides more information and documentation about RO-Crate, as well as a GitHub repository that contains the specification and examples of RO-Crate.
It seems that RO-Crate could be a good way to package your experiment data and metadata in a FAIR way. You could use the ro-crate-metadata.json file to describe your EBoM and link it to the spe files that contain the experimental data. You could also use schema:hasPart and schema:isPartOf to relate the EBoM to the spe files.
For example, you could have something like this:

```json
{
  "@context": [
    "https://w3id.org/ro/crate/1.1/context",
    {
      "sio": "http://semanticscience.org/resource/",
      "qudt": "http://qudt.org/schema/qudt/",
      "sosa": "http://www.w3.org/ns/sosa/",
      "pidinst": "https://w3id.org/pidinst/schema/",
      "nm": "http://nanomine.org/nm/"
    }
  ],
  "@graph": [
    {
      "@type": "CreativeWork",
      "@id": "./",
      "name": "Experiment P19401-E011-DC",
      "description": "An experiment to measure the reflectance of InSe at 0T and 5K using a supercontinuum light source at 20% power and 600 meV energy.",
      "hasPart": [
        {
          "@id": "#ebom"
        },
        {
          "@id": "./Re_InSe_0T_5K_SC-20%_600meV_50um_5sec_0.65NA_Gate Sweep -10V to +20V.SPE"
        }
      ]
    },
    {
      "@type": ["Dataset", "Experiment"],
      "@id": "#ebom",
      "name": "Re_InSe_0T_5K_SC-20%_600meV_50um_5sec_ 0.65NA_Gate Sweep -10V to +20V",
      "description": "An experiment to measure the reflectance of InSe at 0T and 5K using a supercontinuum light source at 20% power and 600 meV energy.",
      "experimentId": "P19401-E011-DC",
      "startDate": "2023-02-12",
      // ... rest of EBoM metadata ...
      // ... use schema:isPartOf to link back to parent crate ...
      "isPartOf": {
        "@id": "./"
      }
    },
    {
      "@type": ["File", "DataDownload"],
      "@id": "./Re_InSe_0T_5K_SC-20%_600meV_50um_5sec_0.65NA_Gate Sweep -10V to +20V.SPE",
      // ... describe file properties ...
      // ... use schema:isPartOf to link back to parent crate ...
      "isPartOf": {
        "@id": "./"
      }
    }
  ]
}
```

Is this what you had in mind?
## User
Yes. There is also a possibility of using HDF5 data containers for the experiments. I know there is support for HDF5 linked data and a python library for linked data in HDF5 that is in the HDF5 gitub repo. What would that look like for our use case?
## Bing
OK, I searched for HDF5 linked data and found some relevant results. According to the first result, an HDF5 file is organized as a rooted, directed graph using HDF5 group objects. The named data objects are the nodes of the graph, and the links are the directed arcs. Each arc of the graph has a name, with the special name / reserved for the root group.
I also found a GitHub repository that contains a Python reader for Linked Data in HDF5 files. It supports reading RDF data stored in HDF5 files using different formats, such as Allotrope Data Format (ADF) and HDF5-JSON-LD.
It seems that HDF5 could be another way to package your experiment data and metadata in a FAIR way. You could use HDF5 groups and links to organize your data and metadata in a hierarchical way. You could also use one of the supported RDF formats to store your EBoM and link it to the spe files that contain the experimental data.
For example, you could have something like this:

```
{
  "@context": [
    "https://w3id.org/ro/crate/1.1/context",
    {
      "sio": "http://semanticscience.org/resource/",
      "qudt": "http://qudt.org/schema/qudt/",
      "sosa": "http://www.w3.org/ns/sosa/",
      "pidinst": "https://w3id.org/pidinst/schema/",
      "nm": "http://nanomine.org/nm/"
    }
  ],
  "@graph": [
    {
      "@type": "CreativeWork",
      "@id": "./",
      "name": "Experiment P19401-E011-DC",
      "description": "An experiment to measure the reflectance of InSe at 0T and 5K using a supercontinuum light source at 20% power and 600 meV energy.",
      // ... use HDF5 groups and links to organize data and metadata ...
      // ... use HDF5-JSON-LD format to store EBoM ...
      // ... use schema:hasPart and schema:isPartOf to relate EBoM to spe files ...
    }
  ]
}
```

Is this what you had in mind?
## User
Yes. This has been a very good and useful conversation. Thank you! If I want to work on this later with Bing Chat, could you create a summary prompt with enough detail and context for Bing Chat to help me flush out more details?
## Bing
You're very welcome. I'm happy to help.😊
Sure, I can create a summary prompt for you. Here is what I came up with:
Summary prompt:
The user wants to create an Experiment Bill of Materials (EBoM) to describe and document experiments in a FAIR way, using JSON-LD and various schemas and vocabularies such as schema.org, prov-o, qudt, sosa, pidinst, and nm. The user also wants to package the experiment data and metadata in a FAIR way, using either RO-Crate or HDF5 linked data. The user has provided an example of an experiment that measures the reflectance of InSe at 0T and 5K using a supercontinuum light source at 20% power and 600 meV energy. The user has also provided some details about the instruments, sensors, samples, and experimental conditions involved in the experiment. The assistant has generated some possible examples of EBoM and RO-Crate or HDF5 linked data for the user's experiment.
The user wants to continue working on this project with Bing Chat and needs more help to flush out more details. The assistant should help the user by:
 - Asking clarifying questions about the user's goals and requirements
 - Providing feedback and suggestions on the user's EBoM and data packaging
 - Searching for relevant information and resources on the web
 - Generating more examples or templates for EBoM and data packaging
 - Explaining the benefits and challenges of using different schemas, vocabularies, and formats
 - Encouraging the user to share their work with others and get feedback

The assistant should also follow the general rules and instructions for Bing Chat mode.
## User
This is perfect. Thank you so much!
## Bing
You're very welcome. I'm glad I could help.😊
I hope you have a great day and good luck with your project.👍
If you want to chat with me again, just say hello. I'll be here.😉
