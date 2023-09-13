# Constructing Knowledge Graphs -  A Tutorial

Additional material need for the Knowledge Graph construction tutorial using [RML](https://rml.io/specs/rml/) at Klausurtagung 2023.



### Technical Requirements

- Download [RMLMapper.jar](https://github.com/RMLio/rmlmapper-java/releases/tag/v6.2.1)
- Download [OpenJDK](https://jdk.java.net/20/)
- Extract OpenJDK and place in the same folder as the RMLMapper.jar

## Example

Mapping of csv data using predefined rules into file result.nq

**Arguments for RMLMapper:**
- `-m` - Path to RML rule file
- `-o` - Name of output file

**How to call RMLMapper:**
````
./PATH/TO/JAVA -m ./Example1/mapping_ex1.ttl -o result.nq
````
**Expected content of result.nq:**
```Turtle
<http://example.com/10/Venus> <http://xmlns.com/foaf/0.1/name> "Venus" .
<http://example.com/10/Venus> <http://example.com/id> "10" .
<http://example.com/10/Venus> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://xmlns.com/foaf/0.1/Person> .
```

## Task 1
**Objective:** Convert a simple CSV file to RDF using RML.

**CSV Data - student_data_task1.csv:**

````CSV
id,name,age,city
1,Alice,25,New York
2,Bob,30,Los Angeles
3,Charlie,35,San Francisco
````

**Task:** Convert the CSV data into RDF triples where each person is of class ex:Person and has properties for name (ex:name), age (ex:age), and city (ex:city).

**Expected Output:**

````Turtle
<http://example.com/person/Alice> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Person>.
<http://example.com/person/Alice> <http://example.com/name> "Alice".
<http://example.com/person/Alice> <http://example.com/age> "25".
<http://example.com/person/Alice> <http://example.com/city> "New York".

<http://example.com/person/Bob> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Person>.
<http://example.com/person/Bob> <http://example.com/name> "Bob".
<http://example.com/person/Bob> <http://example.com/age> "30".
<http://example.com/person/Bob> <http://example.com/city> "Los Angeles".

<http://example.com/person/Charlie> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Person>.
<http://example.com/person/Charlie> <http://example.com/name> "Charlie".
<http://example.com/person/Charlie> <http://example.com/age> "35".
<http://example.com/person/Charlie> <http://example.com/city> "San Francisco".
````

**Background:**
- Every RML document needs a [rr:logicalSource](https://rml.io/specs/rml/#logical-source) to specify the input.
- [rr:subjectMap](https://rml.io/specs/rml/#subject-map) can be used to define a subject.
- [rr:class](https://rml.io/specs/rml/#typing) can be used to specify a class of a subject.
- [rr:template](https://rml.io/specs/rml/#from-template) can be used to specify a IRI template in subjects or objects.
- [rr:predicateObjectMap](https://rml.io/specs/rml/#predicate-object-map) can be used to define a predicate and an object.
- [rr:reference](https://rml.io/specs/rml/#reference) can be used to reference a value in the input data.

**Hint:**
<details>
  <summary>Click for hint:</summary>
  
  ```Turtle
  @prefix rr: <http://www.w3.org/ns/r2rml#> .
  @prefix ex: <http://example.com/> .
  @prefix rml: <http://semweb.mmlab.be/ns/rml#> .
  @prefix ql: <http://semweb.mmlab.be/ns/ql#> .
  @base <http://example.com/base/> .
  
<#PersonMapping> a rr:TriplesMap;
    rml:logicalSource [
        rml:source "./Task1/student_data_task1.csv";
        rml:referenceFormulation ql:CSV
    ];
      rr:subjectMap [
          rr:template "_______________";
          rr:class _______________
      ];
      rr:predicateObjectMap [
          rr:predicate _______________;
          rr:objectMap [ rml:reference _______________ ]
      ];
      rr:predicateObjectMap [
          rr:predicate _______________;
          rr:objectMap [ rml:reference _______________ ]
      ];
      rr:predicateObjectMap [
          rr:predicate ex:city;
          rr:objectMap [ rml:reference "city" ]
      ].
  ```
</details>

**Solution:**
<details>
  
<summary>Click for solution:</summary>

  ```Turtle
@prefix rr: <http://www.w3.org/ns/r2rml#> .
@prefix ex: <http://example.com/> .
@prefix rml: <http://semweb.mmlab.be/ns/rml#> .
@prefix ql: <http://semweb.mmlab.be/ns/ql#> .
@base <http://example.com/base/> .

<#PersonMapping> a rr:TriplesMap;
    rml:logicalSource [
        rml:source "./Task1/student_data_task1.csv";
        rml:referenceFormulation ql:CSV
    ];
    rr:subjectMap [
        rr:template "http://example.com/person/{name}";
        rr:class ex:Person
    ];
    rr:predicateObjectMap [
        rr:predicate ex:name;
        rr:objectMap [ rml:reference "name" ]
    ];
    rr:predicateObjectMap [
        rr:predicate ex:age;
        rr:objectMap [ rml:reference "age" ]
    ];
    rr:predicateObjectMap [
        rr:predicate ex:city;
        rr:objectMap [ rml:reference "city" ]
    ].
  ```
</details>

## Task 2
**Objective:** Extend the mapping rules from Task 1.

**CSV Data - student_data_task2.csv:**

````CSV
id,name,age,city
1,Alice,25,New York
2,Bob,30,Los Angeles
3,Charlie,35,San Francisco
````

**Task:** Extend the mapping rules so that each person is also of class ex:Human, each person knows someone (ex:knows _:bn), and the city is identified by an IRI (e.g., "http://example.com/USA/New%20York").

**Expected Output:**

````Turtle
<http://example.com/person/Alice> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Human>.
<http://example.com/person/Alice> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Person>.
<http://example.com/person/Alice> <http://example.com/name> "Alice".
<http://example.com/person/Alice> <http://example.com/age> "25".
<http://example.com/person/Alice> <http://example.com/city> <http://example.com/USA/New%20York>.
<http://example.com/person/Alice> <http://example.com/knows> _:0.

<http://example.com/person/Bob> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Human>.
<http://example.com/person/Bob> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Person>.
<http://example.com/person/Bob> <http://example.com/name> "Bob".
<http://example.com/person/Bob> <http://example.com/age> "30".
<http://example.com/person/Bob> <http://example.com/city> <http://example.com/USA/Los%20Angeles>.
<http://example.com/person/Bob> <http://example.com/knows> _:1.

<http://example.com/person/Charlie> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Human>.
<http://example.com/person/Charlie> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Person>.
<http://example.com/person/Charlie> <http://example.com/name> "Charlie".
<http://example.com/person/Charlie> <http://example.com/age> "35".
<http://example.com/person/Charlie> <http://example.com/city> <http://example.com/USA/San%20Francisco>.
<http://example.com/person/Charlie> <http://example.com/knows> _:2.
````

**Background:**
- [rr:termType](https://rml.io/specs/rml/#termtype) can be used to specify the term type (IRI, Blank Node, Literal) of a node.

**Hint:**
<details>
  <summary>Click for hint:</summary>
  
  ```Turtle
@prefix rr: <http://www.w3.org/ns/r2rml#> .
@prefix ex: <http://example.com/> .
@prefix rml: <http://semweb.mmlab.be/ns/rml#> .
@prefix ql: <http://semweb.mmlab.be/ns/ql#> .
@base <http://example.com/base/> .

<#PersonMapping> a rr:TriplesMap;
    rml:logicalSource [
        rml:source "./Task2/student_data_task2.csv";
        rml:referenceFormulation ql:CSV
    ];
    rr:subjectMap [
        rr:template "http://example.com/person/{name}";
        rr:class ex:Person;
		rr:class _______________
    ];
    rr:predicateObjectMap [
        rr:predicate ex:name;
        rr:objectMap [ rml:reference "name" ]
    ];
    rr:predicateObjectMap [
        rr:predicate ex:age;
        rr:objectMap [ rml:reference "age" ]
    ];	
    rr:predicateObjectMap [
        rr:predicate ex:city;
        rr:objectMap [ rr:template _______________ ]
    ];
	rr:predicateObjectMap [
        rr:predicate ex:knows;
        rr:objectMap [  rr:termType _______________ ]
    ].

  ```
</details>

**Solution:**
<details>
  
<summary>Click for solution:</summary>

  ```Turtle
@prefix rr: <http://www.w3.org/ns/r2rml#> .
@prefix ex: <http://example.com/> .
@prefix rml: <http://semweb.mmlab.be/ns/rml#> .
@prefix ql: <http://semweb.mmlab.be/ns/ql#> .
@base <http://example.com/base/> .

<#PersonMapping> a rr:TriplesMap;
    rml:logicalSource [
        rml:source "./Task2/student_data_task2.csv";
        rml:referenceFormulation ql:CSV
    ];
    rr:subjectMap [
        rr:template "http://example.com/person/{name}";
        rr:class ex:Person;
		rr:class ex:Human
    ];
    rr:predicateObjectMap [
        rr:predicate ex:name;
        rr:objectMap [ rml:reference "name" ]
    ];
    rr:predicateObjectMap [
        rr:predicate ex:age;
        rr:objectMap [ rml:reference "age" ]
    ];	
    rr:predicateObjectMap [
        rr:predicate ex:city;
        rr:objectMap [ rr:template "http://example.com/USA/{city}" ]
    ];
	rr:predicateObjectMap [
        rr:predicate ex:knows;
        rr:objectMap [  rr:termType rr:BlankNode ]
    ].

  ```
</details>

## Task 3
**Objective:** Extend the mapping rules from Task 2 using join.

**CSV Data - student_data_task3.csv:**

````
id,name,age,city
1,Alice,25,New York
2,Bob,30,Los Angeles
3,Charlie,35,San Francisco
````

**CSV Data - sport_data_task3.csv:**
````
id,sport
1,tennis
2,fencing
3,fishing
````

**Task:** Extend the mapping rules to join both csv datasets using the id entry with the predicate <http://example.com/ontology/practises> and id's for the sport.

**Expected Output:**

````Turtle
<http://example.com/person/Alice> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Human>.
<http://example.com/person/Alice> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Person>.
<http://example.com/person/Alice> <http://example.com/ontology/practises> <http://example.com/resource/sport_1>.
<http://example.com/person/Alice> <http://example.com/name> "Alice".
<http://example.com/person/Alice> <http://example.com/age> "25".
<http://example.com/person/Alice> <http://example.com/city> <http://example.com/USA/New%20York>.
<http://example.com/person/Alice> <http://example.com/knows> _:0.

<http://example.com/person/Bob> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Human>.
<http://example.com/person/Bob> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Person>.
<http://example.com/person/Bob> <http://example.com/ontology/practises> <http://example.com/resource/sport_2>.
<http://example.com/person/Bob> <http://example.com/name> "Bob".
<http://example.com/person/Bob> <http://example.com/age> "30".
<http://example.com/person/Bob> <http://example.com/city> <http://example.com/USA/Los%20Angeles>.
<http://example.com/person/Bob> <http://example.com/knows> _:1.

<http://example.com/person/Charlie> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Human>.
<http://example.com/person/Charlie> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://example.com/Person>.
<http://example.com/person/Charlie> <http://example.com/ontology/practises> <http://example.com/resource/sport_3>.
<http://example.com/person/Charlie> <http://example.com/name> "Charlie".
<http://example.com/person/Charlie> <http://example.com/age> "35".
<http://example.com/person/Charlie> <http://example.com/city> <http://example.com/USA/San%20Francisco>.
<http://example.com/person/Charlie> <http://example.com/knows> _:2.

<http://example.com/resource/sport_1> <https://www.w3.org/TR/rdf11-schema/label> "tennis".
<http://example.com/resource/sport_2> <https://www.w3.org/TR/rdf11-schema/label> "fencing".
<http://example.com/resource/sport_3> <https://www.w3.org/TR/rdf11-schema/label> "fishing".
````

**Background:**
- Multiple tripleMaps can be connect via [joins](https://rml.io/specs/rml/#logical-join).

**Hint:**
<details>
  <summary>Click for hint:</summary>
  
  ```Turtle
@prefix rr: <http://www.w3.org/ns/r2rml#> .
@prefix ex: <http://example.com/> .
@prefix rml: <http://semweb.mmlab.be/ns/rml#> .
@prefix ql: <http://semweb.mmlab.be/ns/ql#> .
@prefix rdfs: <https://www.w3.org/TR/rdf11-schema/> .
@base <http://example.com/base/> .

<#PersonMapping> a rr:TriplesMap;
    rml:logicalSource [
        rml:source "./Task3/student_data_task3.csv";
        rml:referenceFormulation ql:CSV
    ];
    rr:subjectMap [
        rr:template "http://example.com/person/{name}";
        rr:class ex:Person;
		rr:class ex:Human
    ];
    rr:predicateObjectMap [
        rr:predicate ex:name;
        rr:objectMap [ rml:reference "name" ]
    ];
    rr:predicateObjectMap [
        rr:predicate ex:age;
        rr:objectMap [ rml:reference "age" ]
    ];	
    rr:predicateObjectMap [
        rr:predicate ex:city;
        rr:objectMap [ rr:template "http://example.com/USA/{city}" ]
    ];
	rr:predicateObjectMap [
        rr:predicate ex:knows;
        rr:objectMap [  rr:termType rr:BlankNode ]
    ];
	rr:predicateObjectMap [ 
		rr:predicate _______________ ;
		rr:objectMap [ 
		  a rr:RefObjectMap ;
		  rr:parentTriplesMap <TriplesMap2>;
		  rr:joinCondition [
			rr:child _______________ ;
			rr:parent _______________ ;
		  ]
		]
	  ] .
	

<TriplesMap2> a rr:TriplesMap;
  rml:logicalSource [ 
    rml:source "./Task3/sport_data_task3.csv";
    rml:referenceFormulation ql:CSV
  ];

  rr:subjectMap [ rr:template _______________ ]; 
	
  rr:predicateObjectMap [ 
    rr:predicate rdfs:label ; 
    rr:objectMap [ rml:reference _______________ ];
  ].
  ```
</details>

**Solution:**
<details>
  
<summary>Click for solution:</summary>

  ```Turtle
@prefix rr: <http://www.w3.org/ns/r2rml#> .
@prefix ex: <http://example.com/> .
@prefix rml: <http://semweb.mmlab.be/ns/rml#> .
@prefix ql: <http://semweb.mmlab.be/ns/ql#> .
@prefix rdfs: <https://www.w3.org/TR/rdf11-schema/> .
@base <http://example.com/base/> .

<#PersonMapping> a rr:TriplesMap;
    rml:logicalSource [
        rml:source "./Task3/student_data_task3.csv";
        rml:referenceFormulation ql:CSV
    ];
    rr:subjectMap [
        rr:template "http://example.com/person/{name}";
        rr:class ex:Person;
		rr:class ex:Human
    ];
    rr:predicateObjectMap [
        rr:predicate ex:name;
        rr:objectMap [ rml:reference "name" ]
    ];
    rr:predicateObjectMap [
        rr:predicate ex:age;
        rr:objectMap [ rml:reference "age" ]
    ];	
    rr:predicateObjectMap [
        rr:predicate ex:city;
        rr:objectMap [ rr:template "http://example.com/USA/{city}" ]
    ];
	rr:predicateObjectMap [
        rr:predicate ex:knows;
        rr:objectMap [  rr:termType rr:BlankNode ]
    ];
	rr:predicateObjectMap [ 
		rr:predicate <http://example.com/ontology/practises> ;
		rr:objectMap [ 
		  a rr:RefObjectMap ;
		  rr:parentTriplesMap <TriplesMap2>;
		  rr:joinCondition [
			rr:child "ID" ;
			rr:parent "ID" ;
		  ]
		]
	  ] .
	

<TriplesMap2> a rr:TriplesMap;
  rml:logicalSource [ 
    rml:source "./Task3/sport_data_task3.csv";
    rml:referenceFormulation ql:CSV
  ];

  rr:subjectMap [ rr:template "http://example.com/resource/sport_{ID}" ]; 
	
  rr:predicateObjectMap [ 
    rr:predicate rdfs:label ; 
    rr:objectMap [ rml:reference "sport" ];
  ].
  ```
</details>