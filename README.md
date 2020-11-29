# Coding Challenge - Hierarchy Markup

# Demo
## Netlify 
https://ecnopdev-coding-challenge-heirarchy-markup.netlify.app/

## Codepen.io
See Vue Pen -> https://codepen.io/ecnopdev/pen/WNGbdVg

# Problem

Below is employee data of a small company.
It represents the hierarchical relationship among employees. CEO of the company doesn't
have a manager

| Employee Name        | Id   | Manager Id |
| -------------------- |:----:| ----------:|
| Allan                | 100  | 150        |
| Martin               | 220  | 100        |
| Jamie                | 150  |            |
| Alex                 | 275  | 100        |   
| Steve                | 400  | 150        |
| David                | 190  | 400        |


Design a suitable representation of this data. Feel free to choose any database (RDBMS, inmemory database etc), file system or even a data structure like List or Map. Then write code (in any language and framework) that displays the organisation hierarchy as below:

-------------------------------
| Jamie    |        |         |
|----------|:------:|---------|
|          | Allan  |         |
|          |        | Martin  |
|          |        | Alex    |
|          | Steve  |         |
|          |        | David   |

The result can be simply displayed on the console, or HTML page or even a file; whatever
suits you. Try to cover all the possible scenarios, for example an employee with no manager, a
manager who is not valid employee; etc.
Pay more attention on writing the actual logic of representing the employee tabular data into
the hierarchical format

# Solution

## Data Representation 
### Raw Data - JSON Object
```
[
  {id:100,
   name:"Allan",
   manager_id:150
  },
  {id:220,
   name:"Martin",
   manager_id:100
  },
  {id:150,
   name:"Jamie",
   manager_id:null
  },
  {id:275,
   name:"Alex",
   manager_id:100
  },
  {id:400,
   name:"Steve",
   manager_id:150
  },
  {id:190,
   name:"David",
   manager_id:400
  },
]
```
### Heirarchy Data Structure - Hash Map using the manager id as key
```
{
  100: [{person},{person}],
  150: [{person,person}],
  400: [{person}],
  "no_manager":[{person}]
}
```

## Display
### Front-end Framework - Vue.js
### Traversal Algorithm - Recursive

```
displayHeirarchyOnConsole(topLevel,indent){
          
      topLevel.forEach(person => {
        console.log(`${indent} ${person.name}`);
   
        if(this.heirarchy[person.id]){
          this.displayHeirarchyOnConsole(this.heirarchy[person.id],`${indent}       |`)
        }
      });
      
}
```

## Local Development
1. Install node
2. Install vue-cli
3. Clone repository
4. npm install
5. npm run serve



