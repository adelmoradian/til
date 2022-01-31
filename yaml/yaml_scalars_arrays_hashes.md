# YAML scalars, arrays and hashes

```yaml
# Scalar types
a: 1        # integer          
a: 1.234    # float      
b: 'abc'    # string        
b: "abc"                   
b: abc                     
c: false    # boolean type 
d: 2015-04-05   # date type

---

# enfore string
b: !str 2015-04-05

---

# arrays
 array:
   - 132
   - 2.434
   - 'abc'
 my_list_of_lists:
   - [1, 2, 3]
   - [4, 5, 6]

---

# hashes

 my_hash:
   subkey:
     subsubkey1: 5
     subsubkey2: 6
   another:
     somethingelse: 'Important!'
# hash with json syntax
 my_hash: {nr1: 5, nr2: 6}
```
