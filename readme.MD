# Bloque anonimo

Un bloque anónimo PL/SQL es una colección de sentencias PL/SQL encerradas entre las palabras clave `DECLARE`, `BEGIN` y `END;`. Le permite agrupar múltiples sentencias PL/SQL para lograr una tarea específica o un conjunto de tareas. Un bloque anónimo no tiene nombre y se utiliza a menudo para tareas ad-hoc o con fines de prueba.

## Requerimiento 1

 ```sql 
DECLARE 
v_total_dep NUMBER(3) := 0; 
v_dep VARCHAR2(40);

BEGIN 
-- Consulta para obtener la cantidad de empleados en el departamento de finanzas
SELECT COUNT(*) INTO v_total_depcon 
FROM employees WHERE department = 'Finance'; 

-- Consulta para obtener nombre del departamento
SELECT department_name INTO v_depcon
FROM departments WHERE department_id = 'Finance'; 

-- Imprimir en pantalla
DBMS_OUTPUT.PUT_LINE('En el departamento: ' || v_depcon ||' trabajan: ' || v_total_depcon || empleados); 
END; 
/
``` 



## Requerimiento 2
```sql 
DECLARE
	-- Variable para recorrer id de los empleado
    v_emp_id NUMBER := 100;
    -- Variable para almacenar el maximo id de empleado 
    v_max_emp_id NUMBER := 130;
    -- Variable para almacenar el salario
    v_salary NUMBER;
    -- Variable para almacenar el mensaje
    v_message VARCHAR2(100);
BEGIN
	-- Recorrer los empleados
	
    WHILE v_emp_id <= v_max_emp_id LOOP
        -- Obtener el salario de los empleados
        SELECT salary INTO v_salary
        FROM employees
        WHERE employee_id = v_emp_id;

        -- Mensaje basado en el salario
        IF v_salary > 10000 THEN
            v_message := 'Es un buen sueldo';
        ELSIF v_salary >= 9000 AND v_salary <= 10000 THEN
            v_message := 'Es un salario normal';
        ELSE
            v_message := 'Es un sueldo que se debe aumentar';
        END IF;

        -- Mostrar mensaje en pantalla
        DBMS_OUTPUT.PUT_LINE('El salario actual del empleado ' || v_emp_id || 'es de' || v_salary ||' ' || v_message);
        v_emp_id := v_emp_id + 1; -- Move to the next employee
    END LOOP;
END;
/

```

## Requerimiento 3

``` sql
DECLARE
	v_msj VARCHAR2(10);
	v_prom NUMBER;
	v_nombre NUMBER(20);
	v_sum NUMBER;
BEGIN

	SELECT round(avg(salary),0), departmant_name,sum(salary)
	INTO v_prom,v_nombre,v_sum,v_count
	FROM employees JOIN department
	USING (department_id)
	GROUP BY department_name,department_id
	HAVING department_id = &Ingrese_ID_departemento;

	IF sql%found THEN
		DBMS_OUTPUT.PUT_LINE('Se seleccionaron: '||sq;%rowcount || 'Filas')
		ELSE
		DBMS_OUTPUT.PUT_LINE('No se selecciono nada')
		END IF;

v_msj := IF WHEN v_prom < 5000 THEN 'Excelente'
			WHEN v_prom < 3000 THEN 'Bueno'
			WHEN v_prom < 1000 THEN 'Inaceptable'
			ELSE '-'
			END;

DBMS_OUTPUT.PUT_LINE('Su valoracion es : '||v_msj||' ,el valor de promedio es:'
					|| (v_prom,'$999c999')
					|| ' y el nombre del departamento es: '
					||v_nombre);
```
- v_nombre deberia ser varchar2, pues esta almecando el nombre, el cual es varchar
- v_count no esta declarada y no tiene ningun valor asignado en la sentencia SELECT
- Cuando se calcula el mensaje deberia realizarse con un CASE 
	( CASE 
	WHEN v_prom < 1000 THEN v_msj := 'Inaceptable'; 
	WHEN v_prom < 3000 THEN v_msj := 'Bueno'; 
	WHEN v_prom < 5000 THEN v_msj := 'Excelente'; 
	ELSE v_msj := '-'; 
	END CASE; )
- En la impresion del mensaje, deberia usarse el metodo TO_CHAR para dar formato a v_prom (TO_CHAR(v_prom, '$999,999'))


## Requerimiento 4

```sql
DECLARE
	-- Variable para total de empleados
    v_total_employees NUMBER;
    -- Variable nombre departamento 50
    v_nom_dept departments.department_name%TYPE;
    -- Variable para empleado de menor salario (tipo igual a empleado)
    v_lowest_salary employees.salary%TYPE;
    -- Variable nombre empleado con menor sueldo
    v_lowest_name employees.name%TYPE;
    -- Variable para comision promedio
    v_avg_commission NUMBER;
    -- Variable para id maximo de departamento
    v_max_dept_id departments.department_id%TYPE;
    -- Variable para nombre del maximo departamento
    v_max_dept_name departments.department_name%TYPE;
    -- Variable para locacion de maximo departamento
    v_max_dept_location departments.location_id%TYPE;
    -- Variable para jefe 
    v_max_dept_manager employees.manager_id%TYPE;
BEGIN
    -- Total empleados en el departmento 50
    SELECT COUNT(*), department_name INTO v_total_employees, v_nom_dept
    FROM employees
    WHERE department_id = 50;

    -- Empleado con el menor salario
    SELECT MIN(salary), name INTO v_lowest_salary, v_lowest_name
    FROM employees;

    -- Comision promedio
    SELECT AVG(salary * commission_pct) INTO v_avg_commission
    FROM employees;
    
-- Departmento con mas empleados
-- Se almacenan las variables deseadas con la informacion obtenida de una subconsulta, la cual obtiene el id del departamento que mas empleados tiene
	SELECT department_id, department_name, location_id, manager_id
	INTO v_max_dept_id, v_max_dept_name, v_max_dept_location, v_max_dept_manager
	FROM departments
	WHERE department_id = (
	    SELECT department_id
	    FROM (
	        SELECT department_id, COUNT(*) AS emp_count
	        FROM employees
	        GROUP BY department_id
	        ORDER BY emp_count DESC
	    )
	    WHERE ROWNUM = 1
	);


    -- Mostrar resultados
    DBMS_OUTPUT.PUT_LINE('En el departamento : ' ||v_nom_dept|| 'trabajan ' v_total_employees||' empleados.');
    DBMS_OUTPUT.PUT_LINE(v_lowest_name||' es el empleado con salario más bajo:');
    DBMS_OUTPUT.PUT_LINE('Valor promedio de comisiones es ' || TO_CHAR(v_avg_commission, '$999,999.99'));
    DBMS_OUTPUT.PUT_LINE('ID Departamento: ' || v_max_dept_id);
    DBMS_OUTPUT.PUT_LINE('Jefe : ' || v_max_dept_manager);
    DBMS_OUTPUT.PUT_LINE('Nombre Departamento: ' || v_max_dept_name);
    DBMS_OUTPUT.PUT_LINE('Ubicación : ' || v_max_dept_location);
END;
/

```
