# Spatial Queries

This section uses the [iModel Console](https://imodelconsole.bentley.com) to execute queries against the `Stadium` sample iModel.

See [Aspect Queries](aspect-queries.md) for instructions on opening an iModel in the iModel Console.

## The Spatial Index

iModels have a special spatial index table to make spatial queries fast and builtin functions to make querying a bit easier.  See [learning docs](https://www.itwinjs.org/learning/spatialqueries/) and the [tutorial](https://www.itwinjs.org/learning/ecsqltutorial/spatialqueries/)

## Queries

1. Find the bounding box of an element

    ```SQL
    SELECT * FROM bis.SpatialIndex WHERE ECInstanceId = 0x200000063de
    ```

    > Remember find an Id from your own iModel.

1. Find Elements within a bounding box

    ```SQL
    SELECT e.ECInstanceId, c.codevalue FROM Generic.PhysicalObject e 
      JOIN bis.Category c ON c.ECInstanceId = e.Category.Id 
        WHERE e.ECInstanceId IN (
          SELECT ECInstanceId FROM bis.SpatialIndex 
            WHERE minx > 32575 AND maxx < 32690 AND miny > 31720 AND maxy < 31800 AND minz > 5.5 AND maxz < 36
        )
    ```

    > NOTE: Joining on category is not necessary but it is informative.

1. Find only the glass within the bounding box

    ```SQL
    SELECT e.ECInstanceId, c.codevalue, a.* FROM Generic.PhysicalObject e 
      JOIN bis.Category c ON c.ECInstanceId = e.Category.Id 
      JOIN DgnCustomItemTypes_SS_Steel_Structure.GlassElementAspect a ON a.Element.Id = e.ECInstanceId 
        WHERE e.ECInstanceId IN (
          SELECT ECInstanceId FROM bis.SpatialIndex 
            WHERE minx > 32575 AND maxx < 32690 AND miny > 31720 AND maxy < 31800 AND minz > 5.5 AND maxz < 36
        )
    ```

[Next: Query Hierarchies](cte-queries.md)
