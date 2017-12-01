## Case 1

### Initial Data Set

A table with duplicated rows. Duplicated rows only containing one unique property and except of the identifier all other properties are empty. 

| Property	| Identifier    | Property	| Image-Link    |
| ------------- | ------------- | ------------- | ------------- |
|               | A             |               | Link_1        |
| something     | A             | something     | Link_2        |
|               | A             |               | Link_3        |
| something     | B             | something     | Link_1        |
|               | B             |               | Link_2        |
|               | B             |               | Link_3        |

### Desired Result

- Coma separating image links into one column
- Removing duplicate columns

### Approach

#### Define Records 

Lets start to define records in the system. The first column of the table is the identifier of a record. All records with an empty first column are associated to the first record above with a value in the first column. 

**Sort**

- Move `Identifier` column to start of the table (`Edit column > Move column to beginning`)
- Sort by first (`Identifier`) column (`Sort > by text`) 
- Sort by `Property` (`Sort > by text`) 
- Reorder permanently (option in the top panel of the table: `Sort > Reorder rows permanently`)

**Interim Result**

| Identifier    | Property      | Property	| Image-Link    |
| ------------- | ------------- | ------------- | ------------- |
| A             | something     | something     | Link_2        |
| A             |               |               | Link_1        |
| A             |               |               | Link_3        |
| B             | something     | something     | Link_1        |
| B             |               |               | Link_2        |
| B             |               |               | Link_3        |

**Define Records**

- Remove duplicated Identifiers (`Edit cells > Blank down`) 
- Switch to records in table options. Now you should see your [records divided by color coded rows][record-row-difference]

#### Combine Image-Links

- Join cells of the Image-Link column (`Edit cells > Join multi-valued cells`)
- Define separator

**Interim Result**

| Identifier    | Property      | Property	| Image-Link             |
| ------------- | ------------- | ------------- | ---------------------- |
| A             | something     | something     | Link_2, Link_1, Link_3 |
|               |               |               |                        |
|               |               |               |                        |
| B             | something     | something     | Link_1, Link_2, Link_3 |
|               |               |               |                        |
|               |               |               |                        |

#### Remove Empty Rows

- Use facet to select all empty rows on the first column (`Facet > Customized facets > Facet by blank`)
- Switch to row view in the top panel of the table
- In the Facet/Filter on the left include `true`
- Now you should only see the rows you like to remove
- Remove all rows in the `All` row (`Edit rows > Remove all matching rows`)
- Remove Facet/Filter by clicking the `Remove All` button

**Result**

| Identifier    | Property      | Property	| Image-Link             |
| ------------- | ------------- | ------------- | ---------------------- |
| A             | something     | something     | Link_2, Link_1, Link_3 |
| B             | something     | something     | Link_1, Link_2, Link_3 |

[record-row-difference]: http://kb.refinepro.com/2012/03/difference-between-record-and-row.html