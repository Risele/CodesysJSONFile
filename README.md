# CodesysJSONFile
Read JSON file to CODESYS Structs

Uses the [PRO JSON lib by TVM](https://forge.codesys.com/lib/pro-json/home/Home/), version  [1.0.0.16 ](https://www.dropbox.com/scl/fi/16rzprk6dhnr6204ya5vu/PRO_JSON-1.0.0.16.library?rlkey=vohd00hny0uvm9i6a1avf16vq&dl=0)
## Usage

Some customization of FB required:

For each JSON object you should provide two types of structs:
- Your normal DUT (_YOUR_STRUCT_, _YOUR_SUB_STRUCT_ in example)
- JSONVAR DUTs (note! names should match JSON file exactly) (_JS_YOUR_STRUCT_, _JS_YOUR_SUB_STRUCT_ in example)

That should be done for every JSON object type.
Core:

![Structs for core object](imgs/CORE.png)

And inner:

![Structs for inner object](imgs/Sub.png)

And converting methods
- For top-most JSON object (_CORE_JS_TO_ST_ in example)
- And for inner JSON objects (_SUB_JS_TO_ST_ in example)

Also you should **specify your DUTs** in function block and maximum possible **array size** of the topmost object (see [Limitations/Arrays](#Arrays) )

![FB-tuning](imgs/FB.png)

## Limitations
### Memory
Well, it's memory-consuming.
The memory usage is specified by changing constants at GPL_JSON list in PRO_JSON library section.
### Naming
The names in JSON and in ST-based JSONVARs should match exactly.
For JSON fields of
```JSON
{"field":"value",
 "otherField":3.14
}
```
ST-struct should be:
```
TYPE JSONST
STRUCT
	field  :  		JSONVAR;
	otherField:		JSONVAR;
END_STRUCT
END_TYPE
```

### Arrays
The Array could not be topmost structure:
```JSON
[
{"obj": "value"},
{"obj": "value"}
]
``` 
This is NOT valid.

The topmost array should be named:
```JSON
{"myArr":
[
{"obj": "value"},
{"obj": "value"}
]}
```

The corresponging ST structure is
```ST
TYPE JSONST
STRUCT
	obj  :  		JSONVAR;
END_STRUCT
END_TYPE
```

And the name of the array in the ST code should match the JSON exactly:
```ST
VAR
  myArr: ARRAY[1..10] OF JSONST;
END_VAR
```
### UTF8
Do not work with non-english UTF8 symbols (or I failed to do this).

### Application name
Application name should match to specified in GPL_JSON list in PRO_JSON library section ('Application' by default)
## Requirements:
For Cyryllic symols use Win-1251 (CP1251) encoding. 
Use [Owen String Utils](https://ftp.owen.ru/CoDeSys3/04_Library/05_3.5.11.5/02_Libraries/OwenStringUtils_v3.5.4.9.compiled-library) to deal with CP1251->WStrings

Use iconv from Linux to convert file encoding if needed.


