# networkOntology: ontology for computing nodes, software and resources

**Namespace:** `https://www.ia.urjc.es/ontologies/networkOntology/` (prefix `:`)
**Format:** OWL 2 in Turtle syntax

## Description

networkOntology models computing nodes, the software deployed on them, and the resources they offer or require: communication (bandwidth), computation (disk, memory) and functional resources (sensors, cameras). It also describes communication between nodes and the physical location of nodes and resources.

The ontology is self-contained: it does not import other ontologies and does not declare version metadata.

## Classes

Main hierarchy:

```
:Resource
├── :CommunicationResource
│   └── :Bandwidth
├── :ComputationalResource
│   ├── :Disk
│   └── :Memory
└── :FunctionalResource
    ├── :Camera
    └── :Sensor
        ├── :HumiditySensor
        └── :TemperatureSensor

:Location
├── :Address
├── :GeoLocation
└── :Generic

:Software
└── :Service

:ComputingNode
:Shareable          (no declared relations)
```

## Properties

### Object properties

| Property | Domain | Range | Notes |
| --- | --- | --- | --- |
| `:connectedTo` | `:ComputingNode` | `:ComputingNode` | Connection between nodes |
| `:deployedIn` | `:Software` | `:ComputingNode` | Software deployed on a node |
| `:hasResource` | `:ComputingNode` | `:Resource` | Superproperty of a node's resources |
| `:hasCommunicationResource` | `:ComputingNode` | `:CommunicationResource` | Subproperty of `:hasResource` |
| `:hasComputationalResource` | `:ComputingNode` | `:ComputationalResource` | Subproperty of `:hasResource` |
| `:hasFunctionalResource` | `:ComputingNode` | `:FunctionalResource` | Subproperty of `:hasResource` |
| `:hasRequirement` | `:Software` | `:Resource` | Superproperty of a software's requirements |
| `:hasCommunicationRequirement` | `:Software` | `:CommunicationResource` | Subproperty of `:hasRequirement` |
| `:hasComputationalRequirement` | `:Software` | `:ComputationalResource` | Subproperty of `:hasRequirement` |
| `:hasFunctionalRequirement` | `:Software` | `:FunctionalResource` | Subproperty of `:hasRequirement` |
| `:physicallyLocatedAt` | `:ComputingNode` ∪ `:Resource` | `:Location` | Transitive |

### Datatype properties

| Property | Domain | Range |
| --- | --- | --- |
| `:hasLatitude` | `:GeoLocation` | `xsd:double` |
| `:hasLongitude` | `:GeoLocation` | `xsd:double` |
| `:hasUnit` | `:CommunicationResource` ∪ `:ComputationalResource` | `xsd:string` |
| `:hasValue` | — | — |

## Example

A node with a memory module and a geographic location:

```turtle
@prefix : <https://www.ia.urjc.es/ontologies/networkOntology/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

:raspberry1 a :ComputingNode ;
    :hasComputationalResource :memoria1 ;
    :physicallyLocatedAt :ubicacion1 .

:memoria1 a :Memory ;
    :hasUnit "GB" ;
    :hasValue "4"^^xsd:integer .

:ubicacion1 a :GeoLocation ;
    :hasLatitude "40.4168"^^xsd:double ;
    :hasLongitude "-3.7038"^^xsd:double .
```

SPARQL query listing the computational resources of each node:

```sparql
PREFIX : <https://www.ia.urjc.es/ontologies/networkOntology/>

SELECT ?nodo ?memoria
WHERE {
    ?nodo :hasComputationalResource ?memoria .
    ?memoria a :Memory .
}
```

## Known issues

- `:Shareable` has no properties, subclasses or relations to the rest of the ontology.
- `:hasValue` has no domain or range, so it is unclear which resources it applies to.
- `:ComputingNode` and `:Software` are not declared disjoint, nor disjoint from any other concept.
- `:ComputingNode` has no restrictions, so a node can exist without resources or a location.
- `:hasUnit` does not cover `:FunctionalResource`, although sensors often have units of measurement too.
- No class or property has an `rdfs:label` or `rdfs:comment`.
- There is no explicit alignment with other ontologies, such as SEAS or ECO.

## Files

- `networkOntology.owl`: ontology definition.
- `README.md`: this document.

## Authors and license

- **Author(s):** Alberto Fernández Gil, Iván Bernabé Sánchez.
- **License:** CC BY 4.0.
