---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
schema json model: {
  "@name": "KYC form",
  "@type": "schema",
  "@id": "12ab-34cd-56ef",
  "@hash": "1234124",
  "@context": [
    "raw.githubusercontent.com/ziden/us-document-id.json",
    "raw.githubusercontent.com/ziden/country-code-iso-3166.json"
  ],
  "name": {
    "id": "std-pos: val-2",
    "type": "std:string"
  },
  "address": {
    "id": "std-pos: val-1",
    "type": "usdi:address"
  },
  "gender": {
    "id": "std-pos: val-2",
    "type": "usdi:gender"
  },
  "nationality": {
    "id": "std-pos: idx-1",
    "type": "cc3166:numeric-code"
  },
  "countryOfResidence": {
    "id": "std-pos: idx-1",
    "type": "cc3166:numeric-code"
  },
  "dateOfBirth": {
    "id": "std-pos: idx-2",
    "type": "std:date"
  }
}
 ^WNjTVJx9

context json model :{
  "@name": "us-document-id",
  "@type": "context",
  "@id": "usdi",
  "@context": [
    "raw.githubusercontent.com/ziden/us-fips-subdivision-code.json"
    ],
  "address": {
    "@type": "std:object",
    "street-number": {
      "@type": "std:string",
      "value": []
    },
    "street-name": {
      "@type": "std:string",
      "value": []
    },
    "county": {
      "@type": "fips-usc:county-code",
      "value": []
    },
    "state": {
      "@type": "fips-usc:state-code",
      "value": []
    }
  },
  "gender": {
    "type": "std:string",
    "values": ["male", "female", "others"]
  },
  "id-number": {
    "type": "std:string",
    "values": []
  }
}
 ^CPtU7WTm

context json model: {
  "@name": "country-code-iso-3166",
  "@type": "context",
  "@id": "cc3166",
  "@context": [],
    "numeric-code":{
        "type":"std:integer",
        "values":[1,2,3,...,900]
    },
    "alpha-2-code":{
        "type":"std:string",
        "values":[AF,AX,...,ZW]
    },
    "alpha-3-code":{
        "type":"std:string",
        "values":[AFG,ALA,...,ZWE]
    },
    "country-name":{
        "type":"std:string",
        "values":[Afghanistan, ....]
    }
} ^wrUqjaDK

context json model: {
    "@name":"us-fips-subdivision-code",
    "@id":"fips-usc",
    "@context":[]
    "state-code":{
        "type":"std:string",
        "value":[01, 02, ..., 56]
    },
    "county-code":{
        "type":"std:string",
        "value":[01000,01001,...,56045]
    },
    "county-name":{
        "type":"std:string",
        "value":["Autauga county", ....]
    }
} ^xDR6YHSq

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://excalidraw.com",
	"elements": [
		{
			"type": "text",
			"version": 1527,
			"versionNonce": 280829240,
			"isDeleted": false,
			"id": "WNjTVJx9",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 87.9663633651935,
			"y": 30.135234816063416,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 749,
			"height": 1050,
			"seed": 1693375242,
			"groupIds": [],
			"strokeSharpness": "sharp",
			"boundElements": [],
			"updated": 1673238832543,
			"link": null,
			"locked": false,
			"fontSize": 23.93680485157777,
			"fontFamily": 1,
			"text": "schema json model: {\n  \"@name\": \"KYC form\",\n  \"@type\": \"schema\",\n  \"@id\": \"12ab-34cd-56ef\",\n  \"@hash\": \"1234124\",\n  \"@context\": [\n    \"raw.githubusercontent.com/ziden/us-document-id.json\",\n    \"raw.githubusercontent.com/ziden/country-code-iso-3166.json\"\n  ],\n  \"name\": {\n    \"id\": \"std-pos: val-2\",\n    \"type\": \"std:string\"\n  },\n  \"address\": {\n    \"id\": \"std-pos: val-1\",\n    \"type\": \"usdi:address\"\n  },\n  \"gender\": {\n    \"id\": \"std-pos: val-2\",\n    \"type\": \"usdi:gender\"\n  },\n  \"nationality\": {\n    \"id\": \"std-pos: idx-1\",\n    \"type\": \"cc3166:numeric-code\"\n  },\n  \"countryOfResidence\": {\n    \"id\": \"std-pos: idx-1\",\n    \"type\": \"cc3166:numeric-code\"\n  },\n  \"dateOfBirth\": {\n    \"id\": \"std-pos: idx-2\",\n    \"type\": \"std:date\"\n  }\n}\n",
			"rawText": "schema json model: {\n  \"@name\": \"KYC form\",\n  \"@type\": \"schema\",\n  \"@id\": \"12ab-34cd-56ef\",\n  \"@hash\": \"1234124\",\n  \"@context\": [\n    \"raw.githubusercontent.com/ziden/us-document-id.json\",\n    \"raw.githubusercontent.com/ziden/country-code-iso-3166.json\"\n  ],\n  \"name\": {\n    \"id\": \"std-pos: val-2\",\n    \"type\": \"std:string\"\n  },\n  \"address\": {\n    \"id\": \"std-pos: val-1\",\n    \"type\": \"usdi:address\"\n  },\n  \"gender\": {\n    \"id\": \"std-pos: val-2\",\n    \"type\": \"usdi:gender\"\n  },\n  \"nationality\": {\n    \"id\": \"std-pos: idx-1\",\n    \"type\": \"cc3166:numeric-code\"\n  },\n  \"countryOfResidence\": {\n    \"id\": \"std-pos: idx-1\",\n    \"type\": \"cc3166:numeric-code\"\n  },\n  \"dateOfBirth\": {\n    \"id\": \"std-pos: idx-2\",\n    \"type\": \"std:date\"\n  }\n}\n",
			"baseline": 1041,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "schema json model: {\n  \"@name\": \"KYC form\",\n  \"@type\": \"schema\",\n  \"@id\": \"12ab-34cd-56ef\",\n  \"@hash\": \"1234124\",\n  \"@context\": [\n    \"raw.githubusercontent.com/ziden/us-document-id.json\",\n    \"raw.githubusercontent.com/ziden/country-code-iso-3166.json\"\n  ],\n  \"name\": {\n    \"id\": \"std-pos: val-2\",\n    \"type\": \"std:string\"\n  },\n  \"address\": {\n    \"id\": \"std-pos: val-1\",\n    \"type\": \"usdi:address\"\n  },\n  \"gender\": {\n    \"id\": \"std-pos: val-2\",\n    \"type\": \"usdi:gender\"\n  },\n  \"nationality\": {\n    \"id\": \"std-pos: idx-1\",\n    \"type\": \"cc3166:numeric-code\"\n  },\n  \"countryOfResidence\": {\n    \"id\": \"std-pos: idx-1\",\n    \"type\": \"cc3166:numeric-code\"\n  },\n  \"dateOfBirth\": {\n    \"id\": \"std-pos: idx-2\",\n    \"type\": \"std:date\"\n  }\n}\n"
		},
		{
			"type": "rectangle",
			"version": 511,
			"versionNonce": 298453304,
			"isDeleted": false,
			"id": "Cydd6do0MKUEAUAM7EJUp",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 41.674758859093345,
			"y": 11.875416854732634,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 829.8951971026521,
			"height": 1091.235086531699,
			"seed": 1246212758,
			"groupIds": [],
			"strokeSharpness": "sharp",
			"boundElements": [],
			"updated": 1673238838366,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1706,
			"versionNonce": 339116088,
			"isDeleted": false,
			"id": "CPtU7WTm",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 36.35208724682889,
			"y": -1197.7850291605716,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 765,
			"height": 1080,
			"seed": 624093642,
			"groupIds": [],
			"strokeSharpness": "sharp",
			"boundElements": [
				{
					"id": "vfLxnboYjitPoplDvcCKc",
					"type": "arrow"
				}
			],
			"updated": 1673238877393,
			"link": null,
			"locked": false,
			"fontSize": 23.936804851577758,
			"fontFamily": 1,
			"text": "context json model :{\n  \"@name\": \"us-document-id\",\n  \"@type\": \"context\",\n  \"@id\": \"usdi\",\n  \"@context\": [\n    \"raw.githubusercontent.com/ziden/us-fips-subdivision-code.json\"\n    ],\n  \"address\": {\n    \"@type\": \"std:object\",\n    \"street-number\": {\n      \"@type\": \"std:string\",\n      \"value\": []\n    },\n    \"street-name\": {\n      \"@type\": \"std:string\",\n      \"value\": []\n    },\n    \"county\": {\n      \"@type\": \"fips-usc:county-code\",\n      \"value\": []\n    },\n    \"state\": {\n      \"@type\": \"fips-usc:state-code\",\n      \"value\": []\n    }\n  },\n  \"gender\": {\n    \"type\": \"std:string\",\n    \"values\": [\"male\", \"female\", \"others\"]\n  },\n  \"id-number\": {\n    \"type\": \"std:string\",\n    \"values\": []\n  }\n}\n",
			"rawText": "context json model :{\n  \"@name\": \"us-document-id\",\n  \"@type\": \"context\",\n  \"@id\": \"usdi\",\n  \"@context\": [\n    \"raw.githubusercontent.com/ziden/us-fips-subdivision-code.json\"\n    ],\n  \"address\": {\n    \"@type\": \"std:object\",\n    \"street-number\": {\n      \"@type\": \"std:string\",\n      \"value\": []\n    },\n    \"street-name\": {\n      \"@type\": \"std:string\",\n      \"value\": []\n    },\n    \"county\": {\n      \"@type\": \"fips-usc:county-code\",\n      \"value\": []\n    },\n    \"state\": {\n      \"@type\": \"fips-usc:state-code\",\n      \"value\": []\n    }\n  },\n  \"gender\": {\n    \"type\": \"std:string\",\n    \"values\": [\"male\", \"female\", \"others\"]\n  },\n  \"id-number\": {\n    \"type\": \"std:string\",\n    \"values\": []\n  }\n}\n",
			"baseline": 1071,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "context json model :{\n  \"@name\": \"us-document-id\",\n  \"@type\": \"context\",\n  \"@id\": \"usdi\",\n  \"@context\": [\n    \"raw.githubusercontent.com/ziden/us-fips-subdivision-code.json\"\n    ],\n  \"address\": {\n    \"@type\": \"std:object\",\n    \"street-number\": {\n      \"@type\": \"std:string\",\n      \"value\": []\n    },\n    \"street-name\": {\n      \"@type\": \"std:string\",\n      \"value\": []\n    },\n    \"county\": {\n      \"@type\": \"fips-usc:county-code\",\n      \"value\": []\n    },\n    \"state\": {\n      \"@type\": \"fips-usc:state-code\",\n      \"value\": []\n    }\n  },\n  \"gender\": {\n    \"type\": \"std:string\",\n    \"values\": [\"male\", \"female\", \"others\"]\n  },\n  \"id-number\": {\n    \"type\": \"std:string\",\n    \"values\": []\n  }\n}\n"
		},
		{
			"type": "arrow",
			"version": 1051,
			"versionNonce": 302144568,
			"isDeleted": false,
			"id": "vfLxnboYjitPoplDvcCKc",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 706.3212453844708,
			"y": 216.37729985442502,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 8.145296609140246,
			"height": 280.20140756116314,
			"seed": 723578838,
			"groupIds": [],
			"strokeSharpness": "round",
			"boundElements": [],
			"updated": 1673238852780,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "9Rv2KQQy7pgHG7xXEyEqp",
				"focus": -0.6077025474357767,
				"gap": 11.26836383525972
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-8.145296609140246,
					-280.20140756116314
				]
			]
		},
		{
			"type": "rectangle",
			"version": 679,
			"versionNonce": 236666006,
			"isDeleted": false,
			"id": "9Rv2KQQy7pgHG7xXEyEqp",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -33.783590813848605,
			"y": -1249.8022504160544,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 876.0102323718529,
			"height": 1174.7097788740566,
			"seed": 1200038026,
			"groupIds": [],
			"strokeSharpness": "sharp",
			"boundElements": [
				{
					"id": "vfLxnboYjitPoplDvcCKc",
					"type": "arrow"
				}
			],
			"updated": 1672743566309,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1098,
			"versionNonce": 1758021704,
			"isDeleted": false,
			"id": "wrUqjaDK",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -742.592676836965,
			"y": 186.0142642657089,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 406,
			"height": 660,
			"seed": 811631894,
			"groupIds": [],
			"strokeSharpness": "sharp",
			"boundElements": [],
			"updated": 1673238799437,
			"link": null,
			"locked": false,
			"fontSize": 23.936804851577765,
			"fontFamily": 1,
			"text": "context json model: {\n  \"@name\": \"country-code-iso-3166\",\n  \"@type\": \"context\",\n  \"@id\": \"cc3166\",\n  \"@context\": [],\n    \"numeric-code\":{\n        \"type\":\"std:integer\",\n        \"values\":[1,2,3,...,900]\n    },\n    \"alpha-2-code\":{\n        \"type\":\"std:string\",\n        \"values\":[AF,AX,...,ZW]\n    },\n    \"alpha-3-code\":{\n        \"type\":\"std:string\",\n        \"values\":[AFG,ALA,...,ZWE]\n    },\n    \"country-name\":{\n        \"type\":\"std:string\",\n        \"values\":[Afghanistan, ....]\n    }\n}",
			"rawText": "context json model: {\n  \"@name\": \"country-code-iso-3166\",\n  \"@type\": \"context\",\n  \"@id\": \"cc3166\",\n  \"@context\": [],\n    \"numeric-code\":{\n        \"type\":\"std:integer\",\n        \"values\":[1,2,3,...,900]\n    },\n    \"alpha-2-code\":{\n        \"type\":\"std:string\",\n        \"values\":[AF,AX,...,ZW]\n    },\n    \"alpha-3-code\":{\n        \"type\":\"std:string\",\n        \"values\":[AFG,ALA,...,ZWE]\n    },\n    \"country-name\":{\n        \"type\":\"std:string\",\n        \"values\":[Afghanistan, ....]\n    }\n}",
			"baseline": 651,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "context json model: {\n  \"@name\": \"country-code-iso-3166\",\n  \"@type\": \"context\",\n  \"@id\": \"cc3166\",\n  \"@context\": [],\n    \"numeric-code\":{\n        \"type\":\"std:integer\",\n        \"values\":[1,2,3,...,900]\n    },\n    \"alpha-2-code\":{\n        \"type\":\"std:string\",\n        \"values\":[AF,AX,...,ZW]\n    },\n    \"alpha-3-code\":{\n        \"type\":\"std:string\",\n        \"values\":[AFG,ALA,...,ZWE]\n    },\n    \"country-name\":{\n        \"type\":\"std:string\",\n        \"values\":[Afghanistan, ....]\n    }\n}"
		},
		{
			"type": "rectangle",
			"version": 726,
			"versionNonce": 1639251914,
			"isDeleted": false,
			"id": "igMbcE2Fs0urCNYKsHXZS",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -777.2183150948326,
			"y": 164.61577656051406,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 629.7659023811142,
			"height": 689.3411761751638,
			"seed": 1738964810,
			"groupIds": [],
			"strokeSharpness": "sharp",
			"boundElements": [
				{
					"id": "dmWtS5Q1XzZaidR_9Ce9q",
					"type": "arrow"
				}
			],
			"updated": 1672743566309,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 767,
			"versionNonce": 644753480,
			"isDeleted": false,
			"id": "dmWtS5Q1XzZaidR_9Ce9q",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 125.8294832214508,
			"y": 254.15031432268233,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 263.4761768216039,
			"height": 4.791337089801232,
			"seed": 729510486,
			"groupIds": [],
			"strokeSharpness": "round",
			"boundElements": [],
			"updated": 1673238847499,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "igMbcE2Fs0urCNYKsHXZS",
				"focus": -0.6976098191701723,
				"gap": 9.805719113565374
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-263.4761768216039,
					4.791337089801232
				]
			]
		},
		{
			"type": "rectangle",
			"version": 714,
			"versionNonce": 625960504,
			"isDeleted": false,
			"id": "j8R2uzED3h9r3UXOg6NCv",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -778.5066269008726,
			"y": -1187.8453614223595,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 634.4938425576315,
			"height": 574.4179680344528,
			"seed": 1911293450,
			"groupIds": [],
			"strokeSharpness": "sharp",
			"boundElements": [
				{
					"id": "G_mx-FSgj2Up0k5hK1STI",
					"type": "arrow"
				}
			],
			"updated": 1673238947190,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1018,
			"versionNonce": 871751480,
			"isDeleted": false,
			"id": "xDR6YHSq",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -749.9107952578811,
			"y": -1149.184917908614,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 453,
			"height": 510,
			"seed": 2001123222,
			"groupIds": [],
			"strokeSharpness": "sharp",
			"boundElements": [],
			"updated": 1673238947190,
			"link": null,
			"locked": false,
			"fontSize": 23.936804851577765,
			"fontFamily": 1,
			"text": "context json model: {\n    \"@name\":\"us-fips-subdivision-code\",\n    \"@id\":\"fips-usc\",\n    \"@context\":[]\n    \"state-code\":{\n        \"type\":\"std:string\",\n        \"value\":[01, 02, ..., 56]\n    },\n    \"county-code\":{\n        \"type\":\"std:string\",\n        \"value\":[01000,01001,...,56045]\n    },\n    \"county-name\":{\n        \"type\":\"std:string\",\n        \"value\":[\"Autauga county\", ....]\n    }\n}",
			"rawText": "context json model: {\n    \"@name\":\"us-fips-subdivision-code\",\n    \"@id\":\"fips-usc\",\n    \"@context\":[]\n    \"state-code\":{\n        \"type\":\"std:string\",\n        \"value\":[01, 02, ..., 56]\n    },\n    \"county-code\":{\n        \"type\":\"std:string\",\n        \"value\":[01000,01001,...,56045]\n    },\n    \"county-name\":{\n        \"type\":\"std:string\",\n        \"value\":[\"Autauga county\", ....]\n    }\n}",
			"baseline": 501,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "context json model: {\n    \"@name\":\"us-fips-subdivision-code\",\n    \"@id\":\"fips-usc\",\n    \"@context\":[]\n    \"state-code\":{\n        \"type\":\"std:string\",\n        \"value\":[01, 02, ..., 56]\n    },\n    \"county-code\":{\n        \"type\":\"std:string\",\n        \"value\":[01000,01001,...,56045]\n    },\n    \"county-name\":{\n        \"type\":\"std:string\",\n        \"value\":[\"Autauga county\", ....]\n    }\n}"
		},
		{
			"type": "arrow",
			"version": 729,
			"versionNonce": 644561224,
			"isDeleted": false,
			"id": "G_mx-FSgj2Up0k5hK1STI",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 72.14207604895364,
			"y": -1064.267786899125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 206.39070506641312,
			"height": 3.6067770797108096,
			"seed": 257078474,
			"groupIds": [],
			"strokeSharpness": "round",
			"boundElements": [],
			"updated": 1673238947190,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "j8R2uzED3h9r3UXOg6NCv",
				"gap": 9.764155325781529,
				"focus": -0.5270994387594955
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-206.39070506641312,
					3.6067770797108096
				]
			]
		},
		{
			"text": "context json model: {\n    \"contextName\":\"us-fips-subdivision-code\",\n    \"id\":\"fips-usc\",\n    \"@context\":[]\n    \"state-code\":{\n        \"@type\":\"std:string\",\n        \"value\":[01, 02, ..., 56]\n    },\n    \"county-code\":{\n        \"@type\":\"std:string\",\n        \"value\":[01000,01001,...,56045]\n    },\n    \"county-name\":{\n        \"@type\":\"std:string\",\n        \"value\":[\"Autauga county\", ....]\n    }\n}",
			"fontSize": 20,
			"fontFamily": 1,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 418,
			"id": "Eh6OEpoS",
			"type": "text",
			"x": -851.6690528506329,
			"y": -1124.9636299496606,
			"width": 426,
			"height": 425,
			"angle": 0,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"strokeSharpness": "sharp",
			"seed": 38622,
			"version": 2,
			"versionNonce": 1433757512,
			"updated": 1673238907094,
			"isDeleted": true,
			"groupIds": [],
			"boundElements": [],
			"link": null,
			"locked": false,
			"containerId": null,
			"originalText": "context json model: {\n    \"contextName\":\"us-fips-subdivision-code\",\n    \"id\":\"fips-usc\",\n    \"@context\":[]\n    \"state-code\":{\n        \"@type\":\"std:string\",\n        \"value\":[01, 02, ..., 56]\n    },\n    \"county-code\":{\n        \"@type\":\"std:string\",\n        \"value\":[01000,01001,...,56045]\n    },\n    \"county-name\":{\n        \"@type\":\"std:string\",\n        \"value\":[\"Autauga county\", ....]\n    }\n}",
			"rawText": "context json model: {\n    \"contextName\":\"us-fips-subdivision-code\",\n    \"id\":\"fips-usc\",\n    \"@context\":[]\n    \"state-code\":{\n        \"@type\":\"std:string\",\n        \"value\":[01, 02, ..., 56]\n    },\n    \"county-code\":{\n        \"@type\":\"std:string\",\n        \"value\":[01000,01001,...,56045]\n    },\n    \"county-name\":{\n        \"@type\":\"std:string\",\n        \"value\":[\"Autauga county\", ....]\n    }\n}"
		}
	],
	"appState": {
		"theme": "dark",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#000000",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 1,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 1,
		"currentItemFontSize": 20,
		"currentItemTextAlign": "left",
		"currentItemStrokeSharpness": "sharp",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"currentItemLinearStrokeSharpness": "round",
		"gridSize": null,
		"colorPalette": {}
	},
	"files": {}
}
```
%%