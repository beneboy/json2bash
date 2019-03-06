# json2shell
Read a JSON file and output as shell variables.

## Example

In most cases you should run the `json2shell` command with the file to print.

For example:

`test.json`

```json 
{
  "username": "foo",
  "password": "bar",
  "autologin": true,
  "autologout": false,
  "extra_vars": {
    "foo": "bar",
    "baz": "bax"
  },
  "list_of_things": [1, 2, {
    "sub_dict": "sub_value"
  }, true, false]
}

```

```bash
$ ./json2shell test.json
username=foo
extra_vars_foo=bar
extra_vars_baz=bax
autologin=true
list_of_things_0=1
list_of_things_1=2
list_of_things_2_sub_dict=sub_value
list_of_things_3=true
list_of_things_4=false
autologout=false
password=bar
$
```
- dictionaries are printed as key/value pairs
- list items are printed with their main key then underscore then the their index
- recursing into structures uses their main key and then adds sub keys separated by underscores
- booleans are output as `true` or `false`

### Usage with a shell script

You can use `eval` combined with `$()` to evaluate this script's output.

```bash
$ cat test.bash
#!/bin/bash
eval $(./json2shell test.json)
echo ${list_of_things_2_sub_dict}
$ ./test.bash  
sub_value
$
```


## Requirements

Python (2 or 3) is a requirement. Most systems have this.

## Python 3

On the off chance that you don't have `python` in your path, use `json2shell3` which uses the `python3` env executable
instead. Functionality (and content wise apart from one character) they are identical.

## Why?

I needed a tool to convert JSON to shell variables so I wrote this, since I like Python an it is installed on my 
deployment environments, for me it is perfect. It does one thing only and does it fine.