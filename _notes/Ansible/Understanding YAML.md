
[[Index]] 

In this section, we will learn the different ways in which the YAML data is represented.

  

## Rules for Creating YAML file

~~~~  

When you are creating a file in YAML, you should remember the following basic rules:

  • YAML is case sensitive

  • The files should have .yaml or.yml as the extension

  • YAML does not allow the use of tabs while creating YAML files; spaces are allowed instead
~~~~


## Data serialization  

  

Whenever you want to send some data structure or an object across computer networks, say the Internet, you have to turn it into a special format to read it and store it. The process is commonly known as serialization and is of enormous importance on the web. A common usage example of serialization is when reading data from databases and transferring it across the web.

  

## What is YAML?

  

 YAML is a data serialization format that stands for YAML ain’t Markup language.

The main advantage of using YAML is readability and writability. If you have a configuration file that needs to be easier for humans to read, it’s better to use YAML. YAML is not a complete substitution of JSON as JSON and XML have their places too; nevertheless, it’s useful learning YAML.

 Another benefit of YAML is its support of various data types like cases, arrays, dictionaries, lists, and scalars. It has good support for the most popular languages like JavaScript, Python, Ruby, Java, etc.


<img src="/assets/yaml.PNG"/>






  

## Understanding YAML

In this section, we will learn the different ways in which the YAML data is represented.

key-value pair

YAML uses simple key-value pair to represent the data. The dictionary is represented in key: value pair.

Note − There should be space between : and value.

Example: A student record

  

~~~~

--- - Optional YAML start syntax

james:

   name: james john

   rollNo: 34

   div: B

   sex: male

… #Optional YAML end syntax

Abbreviation

You can also use abbreviation to represent dictionaries.

~~~~

  

## Example

James: {name: james john, rollNo: 34, div: B, sex: male}

  
  

The following are the building blocks of a YAML file:

1. Key Value Pair — The basic type of entry in a YAML file is of a key value pair. After the Key and colon there is a space and then the value.

2. Arrays/Lists — Lists would have a number of items listed under the name of the list. The elements of the list would start with a -. There can be a n of lists, however the indentation of various elements of the array matters a lot.

3. Dictionary/Map — A more complex type of YAML file would be a Dictionary and Map.


<img src="/assets/yaml-type.PNG"/>




  

## Datatypes:

~~~~

Strings:

  
  

# Strings don't require quotes:

title: Introduction to YAML

  

# But you can still use them:

title-with-quotes: 'Introduction to YAML'

  

# Multiline strings start with |

execute: |

    npm ci

    npm build

    npm test

~~~~

~~~~

Numbers:

  

# Integers:

age: 35

  

# Float:

price: 18.99

  

~~~~

  

~~~~

Boolean

  

# Boolean values can be written in different ways:

published: false

published: False

published: FALSE

~~~~

  

## Null

~~~~

Null can be represented by simply not setting a value:

null-value:

  

# Or more explicitly:

null-value: null

null-value: NULL

null-value: Null

~~~~

  

## Dates & timestamps

  

~~~~

ISO-Formatted dates can be used

  

date: 2002-12-14

canonical: 2001-12-15T02:59:43.1Z

iso8601: 2001-12-14t21:59:43.10-05:00

spaced: 2001-12-14 21:59:43.10 -5

~~~~

  

## Arrays / Lists

~~~~

# A list of numbers using hyphens:

numbers:

    - one

    - two

    - three

  

# The inline version

numbers: [ one, two, three ]

~~~~

  

## Dictionaries


~~~~

# An employee record

martin:

  name: Martin D'vloper

  job: Developer

  skill: Elite

  

# The inline version

martin: {name: Martin D'vloper, job: Developer, skill: Elite}  

~~~~

## Rules for Creating YAML file

  

When you are creating a file in YAML, you should remember the following basic rules:

  • YAML is case sensitive

  • The files should have .yaml or.yml as the extension

  • YAML does not allow the use of tabs while creating YAML files; spaces are allowed instead

## Use of Variables and extra vars in Playbook

  
  

~~~~

---

- name: extra variable demo

  hosts: all

  vars:

    fruit: ""

  task:

    - name: print message

      ansible.builtin.debug:

        msg: "fruit is {{ fruit }}"

~~~~

  

~~~~

$ ansible-playbook --extra-vars="fruit=apple" -i virtualmachines/demo/inventory extra-variable/example.yml

PLAY [extra variable demo] ************************************************************************

TASK [Gathering Facts] ****************************************************************************

ok: [demo.example.com]

TASK [message] ************************************************************************************

ok: [demo.example.com] => {

    "msg": "fruit is apple"

}

PLAY RECAP ****************************************************************************************

demo.example.com           : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

ansible-pilot $

execution with a plain extra variable

$ ansible-playbook -i virtualmachines/demo/inventory --extra-vars="fruit=apple"  extra-variable/example.yml

PLAY [extra variable demo] ************************************************************************

TASK [Gathering Facts] ****************************************************************************

ok: [demo.example.com]

TASK [message] ************************************************************************************

ok: [demo.example.com] => {

    "msg": "fruit is apple"

}

PLAY RECAP ****************************************************************************************

demo.example.com           : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

ansible-pilot $

~~~~
[[Index]] 