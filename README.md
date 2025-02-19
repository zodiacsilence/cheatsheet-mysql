# **📝 SQL Server Cheat Sheet - AdventureWorks**

## **1️⃣ Consultas Básicas - `SELECT`, `WHERE`, `ORDER BY`**
### **📌 `SELECT` - Obtener datos de una tabla**
```sql
SELECT * FROM Production.Product;
```
Seleccionar columnas específicas:
```sql
SELECT Name, ListPrice FROM Production.Product;
```

### **📌 `WHERE` - Filtrar resultados**
```sql
SELECT Name, ListPrice 
FROM Production.Product
WHERE ListPrice > 1000;
```

📌 **Operadores en `WHERE`**  
| Operador | Significado |
|----------|------------|
| `=`      | Igual a    |
| `!=` o `<>` | Diferente de |
| `>`      | Mayor que  |
| `<`      | Menor que  |
| `BETWEEN x AND y` | Dentro de un rango |
| `LIKE`   | Buscar con patrón |
| `IN`     | Buscar dentro de una lista |

Ejemplo con `LIKE`:
```sql
SELECT Name FROM Production.Product
WHERE Name LIKE '%Bike%';
```

### **📌 `ORDER BY` - Ordenar resultados**
```sql
SELECT Name, ListPrice
FROM Production.Product
ORDER BY ListPrice DESC;
```

---

## **2️⃣ `JOINs` - Uniendo Tablas**
### **📌 `INNER JOIN` - Solo coincidencias en ambas tablas**
```sql
SELECT soh.SalesOrderID, c.CustomerID, p.FirstName, p.LastName, soh.TotalDue
FROM Sales.SalesOrderHeader soh
INNER JOIN Sales.Customer c ON soh.CustomerID = c.CustomerID
INNER JOIN Person.Person p ON c.PersonID = p.BusinessEntityID;
```

### **📌 `LEFT JOIN` - Todos los registros de la tabla izquierda**
```sql
SELECT c.CustomerID, p.FirstName, p.LastName, soh.SalesOrderID, soh.TotalDue
FROM Sales.Customer c
LEFT JOIN Person.Person p ON c.PersonID = p.BusinessEntityID
LEFT JOIN Sales.SalesOrderHeader soh ON c.CustomerID = soh.CustomerID;
```

---

## **3️⃣ Funciones de Agregación - `SUM()`, `AVG()`, `MIN()`, `MAX()`**
```sql
SELECT SUM(TotalDue) AS TotalVentas FROM Sales.SalesOrderHeader;
SELECT AVG(ListPrice) AS PrecioPromedio FROM Production.Product;
SELECT MIN(ListPrice) AS PrecioMinimo, MAX(ListPrice) AS PrecioMaximo FROM Production.Product;
```

---

## **4️⃣ `GROUP BY` y `HAVING`**
```sql
SELECT TerritoryID, SUM(TotalDue) AS VentasPorTerritorio
FROM Sales.SalesOrderHeader
GROUP BY TerritoryID;
```

```sql
SELECT CustomerID, SUM(TotalDue) AS TotalGastado
FROM Sales.SalesOrderHeader
GROUP BY CustomerID
HAVING SUM(TotalDue) > 100000;
```

---

## **5️⃣ Creación y Modificación de Tablas - `CREATE TABLE`, `ALTER TABLE`**
```sql
CREATE TABLE ClientesTop (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    CustomerID INT,
    TotalGasto DECIMAL(18,2)
);
```

```sql
ALTER TABLE ClientesTop ADD PrimeraCompra DATE, UltimaCompra DATE;
```

---

## **6️⃣ Insertar Datos desde una Consulta - `INSERT INTO ... SELECT`**
```sql
INSERT INTO ClientesTop (CustomerID, TotalGasto)
SELECT CustomerID, SUM(TotalDue) AS TotalGasto
FROM Sales.SalesOrderHeader
GROUP BY CustomerID
HAVING SUM(TotalDue) > 100000;
```

---

## **7️⃣ Actualizar Datos - `UPDATE` con `JOIN`**
```sql
UPDATE ct
SET ct.PrimeraCompra = sub.MinOrderDate,
    ct.UltimaCompra = sub.MaxOrderDate
FROM ClientesTop ct
JOIN (
    SELECT CustomerID, MIN(OrderDate) AS MinOrderDate, MAX(OrderDate) AS MaxOrderDate
    FROM Sales.SalesOrderHeader
    GROUP BY CustomerID
) sub ON ct.CustomerID = sub.CustomerID;
```

---

## **8️⃣ Uso de Variables en SQL Server - `DECLARE`**
```sql
DECLARE @PromedioGasto DECIMAL(18,2);

SELECT @PromedioGasto = AVG(TotalGasto) FROM ClientesTop;

SELECT * FROM ClientesTop WHERE TotalGasto > @PromedioGasto;
```

---

## **🔥 Resumen Rápido**
| Comando | Descripción |
|---------|------------|
| `SELECT` | Consulta datos |
| `WHERE` | Filtrar datos |
| `ORDER BY` | Ordenar datos |
| `JOIN` | Unir tablas |
| `SUM()`, `AVG()`, `MIN()`, `MAX()` | Funciones de agregación |
| `GROUP BY` | Agrupar datos |
| `HAVING` | Filtrar datos agrupados |
| `CREATE TABLE` | Crear tabla |
| `ALTER TABLE` | Modificar tabla |
| `INSERT INTO ... SELECT` | Insertar datos de otra tabla |
| `UPDATE` | Modificar datos |
| `DECLARE` | Crear variables |

🚀 **¡Listo para consultas avanzadas en AdventureWorks!** 😃
