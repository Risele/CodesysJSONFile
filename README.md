# CodesysJSONFile
Read JSON file to CODESYS Structs

Uses the [PRO JSON lib by TVM](https://forge.codesys.com/lib/pro-json/home/Home/), version  [1.0.0.16 ](https://www.dropbox.com/scl/fi/16rzprk6dhnr6204ya5vu/PRO_JSON-1.0.0.16.library?rlkey=vohd00hny0uvm9i6a1avf16vq&dl=0)

## Limitations
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

## Requirements:
For Cyryllic symols use Win-1251 (CP1251) encoding. 
Use iconv from Linux to convert files if needed.
