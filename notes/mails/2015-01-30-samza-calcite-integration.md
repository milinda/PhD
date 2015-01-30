Hi Julian,

I did a quick scan of Calcite source including stream related tests and lingual client source. So, if we are going to reuse sqlline and quey parsing and planning infrastructure provided by Calcite:

- How can we change the execution platform while still reusing the existing JDBC infrastructure? Are there any places in code or special interfaces we should look at to understand this?
- Is Lingual a good reference implementation? 


Highly appriciate if you can provide some pointers.
