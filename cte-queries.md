# Querying hierarchies with Recursive CTE Queries

This section uses the [iModel Console](https://imodelconsole.bentley.com) to execute queries against the `Stadium` sample iModel.

See [Aspect Queries](aspect-queries.md) for instructions on opening an iModel in the iModel Console.

Recursive Common Table Expressions can walk a tree or graph, they are a powerful tool when walking a relationship hierarchy in an iModel.  See [iTwin Learning content on CTEs](https://www.itwinjs.org/learning/commontableexp/)

## Traversing the Parent Child hierarchy

All Elements may have a single parent element and any number of child elements.  It is used for various things like [defining assemblies](https://www.itwinjs.org/bis/intro/element-fundamentals/#assemblies) and the [subject hierarchies](https://www.itwinjs.org/bis/intro/information-hierarchy/#example-information-hierarchy)

1. Walk from a leaf subject to the root subject

    ```SQL
    WITH RECURSIVE 
      subject_hierarchy (sId, sParentId, sCode) as (
        SELECT l.ECInstanceId, l.Parent.Id, l.CodeValue FROM bis.Subject l 
          WHERE l.CodeValue = 'SS_Master_Structural_Dome_Cabel_Connection.dgn, SS_Dome_Cabel_Connection' 
        UNION 
        SELECT s.ECInstanceId, s.Parent.Id, s.CodeValue FROM bis.Subject s, subject_hierarchy 
          WHERE s.ECInstanceId = subject_hierarchy.sParentId
      ) 
    SELECT * FROM subject_hierarchy
    ```

    > NOTE: The leaf code value was picked from the model tree view

1. Walk all parent child hierarchies and build a path from leaf to root

    ```SQL
    WITH RECURSIVE
      hierarchy (hId, hParentId, hCode, hPath, hCount) as (
        SELECT c.ECInstanceId, c.Parent.Id, c.CodeValue, c.CodeValue, 0 FROM bis.Element c 
          WHERE c.ECInstanceId NOT IN (
            SELECT Parent.Id FROM bis.Element WHERE Parent.Id IS NOT NULL
          ) AND c.Parent.Id IS NOT NULL 
        UNION 
        SELECT e.ECInstanceId, e.Parent.Id, e.CodeValue, hPath || '-->' || e.CodeValue, hCount + 1 FROM bis.Element e 
          JOIN hierarchy ON hParentId = e.ECInstanceId
      ) 
    SELECT hCount, hPath FROM hierarchy ORDER By hCount DESC
    ```

    > This will include all parent child hierarchies, including the Subject hierarchy which is represented as a parent child relationship.
